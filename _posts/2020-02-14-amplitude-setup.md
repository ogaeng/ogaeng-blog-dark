---
layout: post
title: "구글 태그 매니저를 이용해 앰플리튜드(Amplitude) 설치하기"
date: 2020-02-14 02:00:00 +09:00
author: "Ogaeng"
permalink: /amplitude-setup/
content_id: 17
image: /images/thumbnail/amplitude-setup-thumbnail.jpg
categories:
  - 마케팅
  - 분석
  - Amplitude
tags:
  - 웹분석
  - Amplitude
description: 앰플리튜드를 이용해 웹 분석을 하기 위한 과정의 첫 단계인 설치와 유입 분석을 위한 세팅 방법을 소개합니다
---

최근 앰플리튜드(Amplitude)와 브레이즈(Braze)를 함께 도입하는 회사가 늘고 있다. 주로 마케팅 자동화와 서비스 개선에 관심이 많은 조직에서 도입을 하고 있는데 사용자가 적거나 정식으로 도입을 하기 이전에 테스트를 해보기를 원한다면 앰플리튜드의 무료 플랜을 이용해보면 좋다.

이번에는 웹 기반의 서비스에서 구글 태그 매니저를 이용해 손쉽게 앰플리튜드를 설치하는 방법에 대해서 이야기하고자 한다. 사실 앰플리튜드 설치는 페이스북 픽셀 설치와 거의 동일한 과정으로 할 수 있다.

다만, 기본적으로 제공하는 코드로 설치하면 많은 마케터들이 필요로 하는 유입 분석이 불가능하기 때문에 기본적인 유입이나 UTM을 활용하고 있다면 살짝 손을 봐줘야 한다.(이 과정은 설치 마지막에 설명하겠다.)

## 앰플리튜드(Amplitude) 가입 및 SDK 코드 발급받기

먼저 앰플리튜드를 설치하기 위해서는 회원 가입을 해야 한다. [앰플리튜드 웹사이트](https://amplitude.com/)에 방문해서 먼저 회원 가입을 하자. 회원 가입을 하면 데모 계정을 볼 수 있는데 데모 계정 대시보드에서 [Set up on the Free Plan]을 눌러 새로운 조직과 접속할 주소를 설정한다.

![앰플리튜드 등록](/images/post/17/02-create-ogranization.png)

조직과 URL을 생성하면 Manage Data 화면으로 넘어가는데, 여기에서 [Create Project]를 눌러 새 프로젝트를 생성한다.

![프로젝트 생성](/images/post/17/03-dashboard.png)

프로젝트를 생성한 후 타임존을 내 지역에 맞게 변경한 뒤 아래쪽에 있는 [Add Data Source] 버튼을 눌러 SDK를 발급받자.

![데이터 소스 발급](/images/post/17/04-setup-start.png)

SDK 선택 창이 등장하면 우리는 웹용 앰플리튜드를 설치할 것이기 때문에 JavaScript SDK를 선택하고 넘어간다.

![자바스크립트 SDK 선택](/images/post/17/05-setup-select-sdk.png)

이제 우리가 설치할 코드가 등장했다. 이제 이 코드를 GTM을 이용해 태그로 만들어야 하니 화면에 나온 코드를 복사하자.

![앰플리튜드 설치 코드](/images/post/17/06-setup-code-view.png)

## GTM으로 앰플리튜드 설치하기

이제 GTM에 접속한 후 [태그] → [새로만들기]를 선택하고 [태그 구성]에서 [맞춤 HTML]을 선택한다. 그리고 복사한 앰플리튜드 SDK 소스를 붙여 넣자.

그리고 아래의 [트리거]를 눌러 위에서 모든 페이지에서 실행되도록 [All Pages]를 선택하고 [저장]한다.

![GTM 태그 설정](/images/post/17/07-create-gtm-tag.png)

이제 앰플리튜드의 설치가 완료됐으니 GTM의 [제출]을 눌러 발행하면 웹사이트에 적용된다. 하지만, 여기까지만 작업을 하면 어디에서 우리 웹사이트를 접속했는지 유입 정보를 확인할 수가 없다. 유입 확인이 필요하다면 아래 내용을 더 읽어보자.

## 앰플리튜드에서 유입 정보 데이터 수집하기

앰플리튜드에서 유입 정보와 UTM 정보를 확인하려면 JavaScript SDK에서 추가로 코드를 작성해주어야 한다. 먼저, 위에서 만든 앰플리튜드 태그를 확인하자. 소스 맨 하단에 아래와 같은 부분이 있을 것이다. (init 뒤에 있는 'API 키'는 사용자마다 다르다.)

{% highlight javascript %}
{% raw %}

amplitude.getInstance().init("API 키");
</script>

{% endraw %}
{% endhighlight %}

위처럼 되어 있는 부분을 아래와 같이 수정하고 [저장]한다.

{% highlight javascript %}
{% raw %}

amplitude.getInstance().init("API 키", null, {
      includeGclid: true,
      includeUtm: true,
      includeReferrer: true
  });
</script>

{% endraw %}
{% endhighlight %}

이렇게 작업을 해주면 앰플리튜드 보고서에서 UTM 정보와 유입 정보 확인이 가능하다.

> 참고자료: [Amplitude - Setting Configuration Option](https://help.amplitude.com/hc/en-us/articles/115001361248#settings-configuration-options)

## 정리

사실 웹에서의 앰플리튜드 설치는 매우 간단하다. 순서를 정리하면 아래와 같다.

1. 앰플리튜드 회원 가입
2. 조직과 URL 생성
3. Javascript SDK 발급
4. GTM 태그 생성

마지막으로 최근 들어 특정 도구를 맹신하는 사람들이 늘어나고 있는데, 이런 도구들은 보조 수단에 불과하다는 것을 기억했으면 좋겠다.

그래도 여러 도구를 활용해 보는 것은 좋은 경험이 되므로 이 글을 보는 분들도 다양한 도구를 한 번씩 활용해보기를 바란다.

다음 글에서는 기본 SDK 설치를 바탕으로 사용자의 행동을 추적할 수 있는 앰플리튜드 이벤트 태깅에 대해서 설명하겠다.
