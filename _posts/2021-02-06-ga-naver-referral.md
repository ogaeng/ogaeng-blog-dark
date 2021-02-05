---
layout: post
title: 'GA에서 naver.com/referral은 무엇일까? 브라우저의 추적 방지 살펴보기'
date: 2021-02-06 02:00:00 +09:00
author: "Ogaeng"
permalink: /ga-naver-referral/
content_id: 25
image: /images/thumbnail/ga-naver-referral.png
categories: 분석
tags:
  - referrer
  - 유입분석
  - GA
description: '구글 애널리틱스 소스/매체에 수집되는 naver.com / referral의 정체에 대해 알아보자.'
---

국내에서 웹사이트를 운영한다면 언젠가부터 아래 이미지와 같이 구글 애널리틱스에 `naver.com/referral`이라는 소스/매체가 쌓이기 시작하는 걸 보았을 것이다. 우리가 알고 있는 네이버의 자연 유입은 `naver/organic`인데 과연 `naver.com`은 무엇일까?

![naver.com 이미지](/images/post/25/01.png)

이탈률도 적고 세션 시간도 기니 네이버 자연 유입이지 않을까라는 생각을 가지고 키워드 정보를 한 번 살펴보니.. 역시나 (not set)으로 정보가 없는걸 볼 수 있다.

![GA 키워드 이미지](/images/post/25/02.png)

어디서 들어온 것인지 조금 더 살펴보기 위해 운영체제 정보를 살펴보니 대부분 아이폰과 맥에서 들어온 것이라는 것을 알 수 있다.

![GA 운영체제 이미지](/images/post/25/03.png)

그럼 아이폰과 맥에 무슨 일이 있었던 걸까?

결론부터 이야기하자면 iOS 13부터 적용된 애플 사파리의 `크로스 사이트 추적 방지` 때문에 발생하는 현상이다. 애플은 사파리의 기반이 되는 Webkit에서 '크로스 사이트 요청 시 리퍼러를 페이지의 출처로만 다운그레이드 하겠다'라고 했는데 이 말을 쉽게 풀어보자.

> <a href="https://webkit.org/blog/9661/preventing-tracking-prevention-tracking/" target="_blank">Preventing Tracking Prevention Tracking - Webkit</a>

예를 들어 네이버에 접속해서 `ogaeng` 키워드를 입력하고 검색을 하고 나면 네이버 통합검색 결과 페이지로 이동하게 된다. 이때 검색 결과의 페이지의 주소는 다음과 같다.(모바일 웹 기준)

> <span style="font-size:1.3rem">https://m.search.naver.com/search.naver?where=nexearch&sm=top_hty&fbm=1&ie=utf8&query=ogaeng</span>

여기에 노출된 ogaeng 사이트 링크를 눌러 접속하면 ogaeng 사이트의 리퍼러는 접속 이전 페이지의 주소인 다음의 주소가 된다.

> <span style="font-size:1.3rem">https://m.search.naver.com/search.naver?where=nexearch&sm=top_hty&fbm=1&ie=utf8&query=ogaeng</span>

하지만 사파리에서는 저 긴 주소를 그대로 리퍼러로 전달하지 않고 `https://m.search.naver.com`까지만 전달하겠다는 내용이다. 그래서 `query=ogaeng`과 같은 키워드 정보를 포함하고 있지 않는다.

리퍼러에 대해 알고 싶다면 아래 글을 읽어보자.

> <a href="https://ogaeng.com/http-referrer/" target="_blank">유입 분석을 위한 HTTP 리퍼러(Referrer) 이해하기</a>

그럼 사파리로 접속하면 모두 그런 건가? 그렇진 않다. `크로스 사이트 추적 방지` 옵션이 꺼져있다면 정상적으로 추적이 가능하다. iOS의 설정에 들어가면 `크로스 사이트 추적 방지` 기능을 켜고 끌 수 있다. 하지만 안타깝게도 기본 옵션은 추적 방지 On이고 아이폰은 OS를 자동 업데이트한다.

그럼 이제 기본 옵션인 크로스 사이트 추적 방지가 켜져 있는 상태에서 GA 상에 네이버 자연 유입이 어떻게 표시되는지 살펴보자.

<table>
  <thead>
    <th style="width:25%">접속 방법</th>
    <th>리퍼러</th>
    <th>소스/매체</th>
  </thead>
  <tbody>
    <tr>
      <th>
        맥용 Safari
      </th>
      <td>
        https://search.naver.com
      </td>
      <td>naver / organic</td>
    </tr>
    <tr>
      <th>
        iOS Safari
      </th>
      <td>
        https://m.search.naver.com
      </td>
      <td>m.search.naver.com / referral</td>
    </tr>
    <tr>
      <th>
        iOS 네이버 앱
      </th>
      <td>
        https://naver.com
      </td>
      <td>naver.com / referral</td>
    </tr>
    <tr>
      <th>
        iOS Chrome 앱
      </th>
      <td>
        https://m.search.naver.com
      </td>
      <td>m.search.naver.com / referral</td>
    </tr>
  </tbody>
</table>

표를 보면 한 가지 놀라운 사실이 있는데 사파리뿐 아니라 iOS의 크롬 앱도 기본으로 `크로스 사이트 추적`을 허용하지 않는다는 것이다.

내 아이폰의 크로스 사이트 추적 방지 정보를 보려면 [설정] 앱에서 Safari와 Chrome을 살펴보면 된다.

![아이폰 크로스 사이트 추적 설정 이미지](/images/post/25/04.png)

## 대처 방안

그렇다면 이제 어떡해야 할까? 사실 우리는 이런 일을 처음 겪는 것은 아니다. Google 검색에서 이미 같은 일을 겪고 있다. Google 자연 검색 유입에서 방문자의 키워드 정보가 `not provided`로 나오는 것과 거의 같은 상황이다. Google 검색 결과에서 링크를 눌러 유입될 때 `https://google.com`만 남기고 뒷부분은 모두 제거되어 있기 때문이다.

그럼 맥과 iOS에서 사파리와 네이버 앱을 통한 유입의 키워드 정보는 영영 볼 수 없는 것일까? 이 문제도 100%는 아니지만 구글과 같은 방법으로 어느 정도는 해결이 가능하다. <a href="https://searchadvisor.naver.com/" target="_blank">네이버 웹마스터 도구</a>를 활용하면 된다.

네이버 웹마스터 도구에 사이트를 등록하고 리포트 → 콘텐츠 노출/클릭 메뉴에 접속하면 네이버 검색 엔진에서 접속한 키워드의 노출/클릭 정보를 볼 수 있다.

![네이버 웹마스터 도구 키워드 이미지](/images/post/25/05.png)

아직은 TOP 10 정도만 볼 수 있지만 이 기능도 생긴지 얼마 안 되었으니 언젠가는 구글 서치 콘솔처럼 더 많은 정보를 볼 수 있지 않을까 한다.

## 네이버 검색 광고는?

그렇다면 네이버 검색 광고를 통한 유입도 마찬가지인 걸까?

안타깝게도 그렇다. 하지만 검색 광고 연결 URL에 UTM을 잘 달아두었다면 UTM에 달린 정보로 들어올 테니 걱정하지 않아도 된다. 검색 광고에 UTM을 잘 달아두었는지 다시 한번 확인하도록 하자

## 한줄요약

`naver.com / referral`은 대부분 아이폰과 아이패드의 네이버 앱에서 유입된 방문이다.

## 마치며

애플이 충전 어댑터를 제공하지 않으니 다른 기업도 따라 하는 걸 우리는 봐왔기 때문에 그동안 눈치 보던 다른 브라우저에서도 추적 정보를 차단하는건 시간문제로 보인다.

최근 몇 년간 애플의 개인정보보호 정책을 보니 앞으로는 광고보다는 SEO나 CRM에 해박한 디지털 마케터가 더 각광받지 않을까? 하는 생각이 든다.
