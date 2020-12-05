---
content_id: 9
layout: post
title: 페이스북 픽셀로 유입 키워드를 수집하고 맞춤타겟 만들기
date: 2017-11-08 20:16:00 +09:00
author: "Ogaeng"
permalink: /facebook-pixel-search-keywords/
image: /images/thumbnail/facebook-pixel-search-keywords_thumbnail.png
categories: Facebook
tags:
  - 페이스북픽셀
  - 키워드
  - GTM
  - facebookanalytics
  - 맞춤타겟
description: 유입분석을 할 때 가장 중요하게 확인하는 부분 중 하나가 어떤 키워드로 검색을 해서 우리 서비스에 접속했는지다. 검색한 키워드에 따라서 잠재고객의 관심도를 파악할 수 있고 키워드에 따라 다양한 전략을 세울 수도 있다. 그래서 이번 글에선 페이스북 픽셀 맞춤이벤트를 만들어 검색엔진에서 유입된 사용자의 검색어를 수집하고 맞춤타겟을 만드는 것까지 이야기하려고 한다.
---

유입분석을 할 때 가장 중요하게 확인하는 부분 중 하나가 어떤 키워드로 검색을 해서 우리 서비스에 접속했는지다. 검색한 키워드에 따라서 잠재고객의 관심도를 파악할 수 있고 키워드에 따라 다양한 전략을 세울 수도 있다. 그래서 이번 글에선 페이스북 픽셀 맞춤이벤트를 만들어 검색엔진에서 유입된 사용자의 검색어를 수집하고 맞춤타겟을 만드는 것까지 이야기하려고 한다.

먼저, 페이스북 애널리틱스를 사용하면서 가지고 있던 가장 큰 불만은 국내 검색엔진의 검색 키워드 정보가 넘어오질 않는다는 것이다. 구글은 자체적으로 검색엔진으로 잡아내지만, 네이버나 다음은 검색엔진으로 잡질 못하고 추천(Referral)로 잡아버리기 때문에 검색어에 대한 정보가 수집되잘 않는다. 이런 불편이 온전히 페이스북 애널리틱스를 사용하지 않고 구글 애널리틱스를 사용하게 만드는 이유가 된다. (게다가 구글은 정책상 검색어 정보가 넘어오질 않는다)

검색어 정보를 받을 수 있는 편법으로는 키워드 정보가 들어간 UTM태그를 작성하는 방법인데 검색 광고에서는 UTM태그를 사용할 수 있지만, 자연검색은 할 방법이 없다. 그래서 생각한 방법이 우리 서비스로 접속하기 전 검색엔진의 리퍼러 정보에서 검색어를 추출해서 저장하는 방법인데 지금부터 그 방법을 소개하려고 한다.

~~~
[업데이트] 2020.10.12 - 변수 간소화 및 _removed_ 현상 해결을 위한 매개변수명 변경
~~~

## 이 글에서 다루는 도구 ##

- Google Tag Manager
- Facebook Pixel
- Facebook Analytics



## Google Tag Manager로 Facebook Pixel 맞춤이벤트 만들기 ##

이번에도 마찬가지로 다른 도구 없이 구글 태그 매니저만을 이용해서 맞춤이벤트를 만들어보자.

### 1. 리퍼러 호스트 변수 만들기 ###

먼저 어떤 검색엔진에서 유입됐는지를 확인하기 위한 유입 경로를 저장하는 변수를 만들어보자.

GTM에 접속한 후 [변수] → [새로만들기]를 선택하고 [변수 구성]에서 [변수 유형]을 [HTTP 리퍼러]로 선택한다. 그리고 아래의 [구성요소 유형]을 눌러 [호스트 이름]을 선택하고 'www.' 생략에 체크한 후 이름을 <code>referrerHost</code>로 하여 [저장]한다.

![변수-referrerHost](/images/post/9/gtm-var-referrerhost.png)

### 2. 검색어 변수 만들기 ###

그다음 과정으로는 유입경로가 검색엔진일 때 리퍼러 URL에서 검색어를 추출하여 저장하는 변수를 만들어 볼 차례다.

![검색엔진URL](/images/post/9/browser-url.png)

예를 들어 네이버에서 '페이스북 픽셀'이라고 검색을 하면 검색 결과 창의 주소는 위 이미지처럼 나오는데 저 안에 검색어에 대한 정보가 들어있다. 바로 그 정보를 뽑아서 변수로 저장하는 것이다.

[변수] → [새로만들기]를 눌러 [변수 유형]을 [맞춤 자바스크립트]로 선택한다. 그리고 입력창에 아래의 소스코드 링크를 눌러 나온 소스를 복사하여 붙여넣고 변수의 이름을 <code>referrerKeyword</code>로 하여 [저장]한다.

{% highlight javascript %}
{% raw %}

function() {
  var referrer_hostname = {{referrerHost}};
  var url_string = {{Referrer}};
  // referrerKeyword by ogaeng.com, CC BY - SA
  if (referrer_hostname == "search.naver.com" || referrer_hostname =="m.search.naver.com"){
    var url = new URL(url_string);
    var result = url.searchParams.get('query');
    return result;
  } else {
    var url = new URL(url_string);
    var result = url.searchParams.get('q');
    return result;
  }
}

{% endraw %}
{% endhighlight %}

![변수-referrerKeyword](/images/post/9/gtm-var-referrerkeyword.png)

### 3. 트리거 만들기 ###

검색엔진에서 우리 사이트로 넘어올 때만 검색어 이벤트가 작동되게 하기 위해선 트리거를 만들어주어야 한다. 위에서 만든 리퍼러 호스트가 검색엔진과 일치 할 때만 이벤트가 동작하도록 트리거를 만들자.

[트리거] → [새로만들기]를 선택하고 [트리거 구성]에서 [페이지 뷰]를 선택한다. 그리고 아래로 가서 [일부 페이지뷰]에 체크한다. 그리고 조건을 아래의 이미지와 같이 구성하고 이름은 자유롭게 지정해서 [저장]한다.

![트리거 설정](/images/post/9/gtm-trigger-referrerhost.png)

### 4. 페이스북 픽셀 이벤트 태그 만들기 ###

설정의 마지막이면서 가장 중요한 단계인 픽셀 맞춤이벤트 태그를 추가할 차례다. 맞춤이벤트 태그를 만들기 전에 페이스북 픽셀 기본 코드 태그가 있어야 하니 아직 없다면 아래 링크를 참고해서 추가하자.

> 참고: [Google 태그 관리자에서 Facebook 픽셀 사용하기](https://www.facebook.com/business/help/1021909254506499)

이제 [태그] 메뉴로 가서 [새로만들기]를 선택하고 [태그 구성]에서 [맞춤 HTML]을 선택한다. 그리고 아래의 소스 코드 링크를 눌러 나온 소스를 복사하여 붙여넣자.

{% highlight html %}
{% raw %}
<script>
  fbq('trackCustom', 'SearchKeywords', {
    search_string: {{referrerKeyword}}
  });
</script>

{% endraw %}
{% endhighlight %}

소스 코드를 붙여넣고 아래의 [트리거]를 눌러 위에서 만든 [Referrer 트리거]를 선택하고 [저장]한다.

![태그-SearchKeywords](/images/post/9/gtm-tag-searchkeywords.png)

이 맞춤이벤트가 작동하면 페이스북 애널리틱스에서 SearchKeywords를 확인할 수 있고 매개변수 search_string로 검색어를 저장한다.

### 5. 작동 확인 ###

이제 GTM의 설정을 모두 마쳤으니 제대로 작동하는지 확인을 할 단계다. 먼저 GTM의 [미리 보기] 기능과 [Facebook Pixel Helper](https://chrome.google.com/webstore/detail/facebook-pixel-helper/fdgfkebogiimcoedlicjlajpkdmockpc)를 이용해 태그가 잘 동작하는지 확인해보자.

> 참고: [Facebook Pixel Helper 안내](https://developers.facebook.com/docs/facebook-pixel/pixel-helper)

확인하는 방법은 간단하다. [미리 보기] 상태에서 네이버나 다음과 같은 검색엔진에 접속해 우리의 사이트가 나올만한 키워드로 검색을 하고 검색 결과에서 사이트를 눌러 접속하면 된다.

![테스트-네이버검색](/images/post/9/test-naver-search.png)

테스트로 네이버에서 '페이스북 픽셀 스크롤'로 검색을 해봤다. 검색 결과에 우리 블로그의 글인 '페이스북 픽셀로 스크롤 측정하고 맞춤타겟 만들기' 가 보이니 이걸 눌러 블로그로 이동해보겠다.

![테스트-픽셀헬퍼](/images/post/9/test-pixel-helper.png)



검색엔진을 통해 블로그에 접속한 후 Facebook Pixel Helper로 확인해보니 [SearchKeywords] 이벤트가 생긴 걸 확인할 수 있다. 이벤트를 눌러보면 [search_string] 매개변수에 방금 검색한 검색어가 기록된 것이 보인다. 정상적으로 동작하는 걸 확인했으니 다시 GTM으로 돌아가 변경사항을 [제출]하자.

검색어 데이터가 쌓인 뒤에 [Facebook Analytics](https://www.facebook.com/analytics)에 접속해서 [이벤트] 메뉴에 들어가면 검색어 통계를 확인할 수 있다.

## 검색 키워드를 이용하여 페이스북 맞춤타겟 만들기 ##

이제 데이터가 쌓이면 우리가 만든 SearchKeywords 이벤트를 활용해 원하는 검색어로 맞춤타겟을 만들 수 있다. 함께 맞춤타겟을 만들어보자.

페이스북의 [타겟] 메뉴로 들어가 [타겟 만들기]를 눌러 [웹사이트 트래픽]을 선택한다.

![페이스북 타겟 생성](/images/post/9/facebook-target-1.png)

다음 화면에서 [모든 웹사이트 방문자 ▾]을 누르고 [SearchKeywords]를 선택한다. 그리고 아래의 [세분화 기준]을 눌러 [URL/매개변수]를 선택한다. 그리고 [URL ▾]을 눌러 [search_string]를 선택하고 아래의 타겟을 만들고자 하는 유입키워드를 입력하고 타겟을 만든다.

![페이스북 타겟 생성](/images/post/9/facebook-target-create.png)

맞춤타겟을 만들 때 키워드를 하나만 입력하기 보다는 같은 그룹의 여러 키워드를 포함해 더 큰 타겟을 만드는 것이 좋다.

## 정리 ##

구글 태그 매니저를 이용하면 개발자에게 따로 요청하지 않고도 다양한 태그를 넣을 수 있어 구글 애널리틱스나 페이스북 픽셀에 필요한 이벤트를 손쉽게 만들 수 있다. 이 블로그에 있는 몇 가지의 픽셀 이벤트를 따라서 만들다 보면 자신에게 필요한 맞춤이벤트를 스스로 테스트해보며 만들 수 있는 수준이 될 수 있을 거라고 생각한다.

마지막으로 검색어를 수집하는 이벤트의 작업 순서를 정리하면 아래와 같다.

1. 리퍼러 호스트와 키워드 수집 변수 생성
2. 검색엔진 리퍼러를 확인하는 트리거 생성
3. 페이스북 픽셀 이벤트 생성
4. Facebook Pixel Helper로 동작 확인

오늘 만든 검색어 맞춤타겟으로 더욱 효율적인 광고를 할 수 있기를 기원하며 따라 하다가 막히는 부분이 생긴다면 언제든 댓글을 남겨주기 바란다.
