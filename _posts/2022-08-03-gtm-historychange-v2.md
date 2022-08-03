---
layout: post
title: '구글 태그 관리자(GTM)에서 기록 변경 V2(gtm.historyChange-v2) 활용하기'
date: 2022-08-03 21:00:00 +09:00
author: "Ogaeng"
permalink: /gtm-historychange-v2/
content_id: 26
image: /images/thumbnail/gtm-historychange-v2.jpg
categories: 분석
tags:
  - GTM
  - GA
description: '새로운 기록 변경 이벤트인 gtm.historyChange-v2에 대해서 알아보자.'
---

언젠가부터 구글 태그 매니저에서 미리보기를 실행시키면 특정 계정에서 아래와 같이 History 트리거가 2개씩 실행되는 현상을 볼 수 있다.

![GTM 미리보기 - 트리거](/images/post/26/01.png)

트리거를 하나씩 눌러 살펴보면 다음과 같이 dataLayer에 푸시되는 이벤트의 이름이 비슷해 보이지만 살짝 다른 것을 볼 수 있는데

![GTM 미리보기 - 데이터 영역 1](/images/post/26/02.png)

![GTM 미리보기 - 데이터 영역 2](/images/post/26/03.png)

하나는 gtm.historyChange 또 다른 하나는 gtm.historyChange-v2라고 v2가 꼬리에 붙어있는 걸 볼 수 있다. 이 둘은 무엇이 다르고 어떤 역할을 하는지 한 번 살펴보자.

<div id="historyChange">
  <h2>기록 변경(gtm.historyChange) 트리거</h2>
</div>

GTM에서 기록 변경 트리거는 URL 조각(Fragment)이 변경(hashchange) 되거나 History API를 이용해 URL이 변경(pushstate, replacestate, popstate) 될 때 발동되는 트리거다.

주로 페이지 내 HTML에 ID(#)를 이용해 해당 섹션으로 이동시킬 때나 React, vue.js 등을 활용해 제작된 SPA 환경에서 페이지 이동할 때 실행되는 것을 볼 수 있다.(URL 조각이 궁금하다면 [여기](#historyChange)를 눌러보자. 그럼 URL 뒤에 `#historyChange`가 생긴걸 볼 수 있다.)

결론적으로 GTM에서의 기록 변경은 내장 트리거로 `gtm.historyChange` 이벤트가 실행되는 것을 의미한다.

> 참고: <a href="https://support.google.com/tagmanager/answer/7679322" target="_blank">구글 태그 관리자 고객센터 - 기록 변경</a>

## 기록 변경 V2(gtm.historyChange-v2) 트리거

기록 변경 V2 트리거가 언제부터 실행됐는지 눈치가 빠르다면 바로 느낌이 왔을 것이다. 아마 GA4를 설치한 이후부터 보이기 시작했을 텐데 기록 변경 V2는 GTM의 내장 트리거가 아닌 GA4에서 실행되는 이벤트다.

이 녀석은 GA4의 향상된 측정에 의해 생성되는 이벤트로 GA4의 관리 → 속성 → 데이터 스트림 → 향상된 측정 → 페이지 조회 → 고급 설정 표시에서 해당 설정을 확인할 수 있다.

![GA4 향상된 측정](/images/post/26/04.png)

GA4는 UA와 다르게 기본적으로 SPA 환경에서도 문제없이 `page_view` 이벤트를 자동으로 발동시키는데 그 이유가 바로 `브라우저 방문 기록 이벤트를 토대로 한 페이지 변경사항` 항목이 활성화되어있기 때문이다.

이 항목을 비활성화하면 UA처럼 히스토리가 변경될 때 page_view 이벤트를 자동으로 발동시키지 않는 것을 볼 수 있는데

이 항목을 활성화하면 SPA 환경에서도 아래 스크린샷처럼 GA4 구성 태그에 추가 설정 없이 `All Pages` 트리거 하나만 넣어두면 페이지 이동 시 page_view 이벤트를 알아서 척척 찍어준다.

![GTM 태그](/images/post/26/05.png)

기록 변경 V2 트리거를 만들어 사용하고 싶을 때에는 다음과 같이 트리거를 생성하면 된다. 필요에 따라 여기에 조건을 추가하여 사용하자.

![GTM 트리거](/images/post/26/06.png)

## 기록 변경 V2의 활용처

일반적으로 SPA 환경에서 유니버설 애널리틱스나 앰플리튜드 등에서 페이지뷰를 수집할 때에는 기록 변경 트리거를 활용한다. 이때 어떤 웹사이트에서는 한 번의 페이지 이동을 했지만 여러 개의 기록 변경 트리거가 발동되는 상황이 벌어질 수 있는데 이때 중복으로 발동되는 트리거를 정리하기 위해 History Source 변수에 pushState나 popState, replaceState의 조건을 넣어 추가 설정을 한다.

하지만 기록 변경 V2를 이용하면 `History Source` 조건을 설정하지 않고도 원활하게 트리거를 사용할 수 있으니 GA4를 사용하고 있다면 이번 기회에 활용해 보자.
