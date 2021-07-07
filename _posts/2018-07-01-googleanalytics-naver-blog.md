---
content_id: 11
layout: post
title: 구글애널리틱스로 유입된 네이버블로그의 주소와 검색키워드 가져오기
date: 2018-07-01 13:00:00 +09:00
author: "Ogaeng"
permalink: /googleanalytics-naver-blog/
image: /images/thumbnail/googleanalytics-naver-blog_thumbnail.png
categories: [Naver,GA,GTM]
tags:
  - GA
  - 네이버블로그
  - 유입분석
  - 검색어
description: GA와 GTM을 이용해 우리 웹사이트에 유입된 네이버 블로그의 아이디와 글주소, 그리고 해당 블로그 글에 접속하기 전에 검색했던 검색어까지 맞춤측정기준을 통해 가져오는 방법을 소개하려고 한다.
---

`[업데이트] 2021.07.07 - 네이버 블로그의 아이디와 글 번호를 담는 변수 변경`

우리나라에서 가장 많은 사람들이 사용하는 블로그가 무엇이냐고 물어보면 모든 사람들은 네이버 블로그라고 이야기한다. 그래서 많은 기업들이 네이버 블로그를 만들어 운영하고, 흔히 파워블로거라고 말하는 네이버 블로그를 운영하는 블로거에게 돈을 주고 광고를 의뢰하곤 한다. 하지만 마케팅 담당자 입장에선 비용을 지불한 블로그의 성과를 측정하는 것이 굉장히 어렵다.

구글 애널리틱스(GA)에서 네이버 블로그 유입을 살펴보면 아래와 같은 화면을 마주한다.

![구글애널리틱스-추천보고서-네이버블로그](/images/post/11/ga-referral-report-01.png)

추천 경로가 /PostView.nhn이라고 나오면서 어떤 블로그에서 유입되었는지를 보여주지 않는다. 이런 문제를 해결하기 위해 많은 마케터가 UTM 태그가 달린 URL이나 단축 URL을 블로거에게 제공하려고 하지만 블로거들도 그 주소를 통해 자신의 블로그에 대한 성과를 측정한다는 것을 알기 때문에 대부분 해당 URL을 포스팅에 넣는 걸 거절한다.

과연 이대로 네이버 블로그의 유입과 바이럴의 성과는 측정이 불가능한 것일까? 이번 글에서는 GA와 구글태그매니저(GTM)을 이용해 우리 웹사이트에 유입된 네이버 블로그의 아이디와 글 주소, 그리고 해당 블로그 글에 접속하기 전에 검색했던 검색어까지 맞춤측정기준을 통해 가져오는 방법을 소개하려고 한다.

## 이 글에서 다루는 도구 ##

* Google Analytics
* Google Tag Manager

## 작동 원리 이해하기 ##

기본적으로 네이버 블로그의 주소는 'blog.naver.com/아이디/글번호' 의 형태로 이루어져 있다. 브라우저에도 이런 주소의 형태로 보인다. 그런데 왜 우리의 GA에서는 추천 경로가 ''/PostView.naver'으로 나오는 것일까? 실제로 우리 눈에 보이는 주소는 'blog.naver.com/아이디/글 주소'의 형태이지만 진짜 주소는 아래와 같은 형태로 이루어져 있다.

> blog.naver.com/PostView.naver?blogId=네이버아이디&logNo=글번호

위 주소를 보면 GA에서 추천 경로가 ''/PostView.nhn'으로 나오는 이유가 수긍이 간다. 그럼 이제 저 주소를 이용해 우리가 원하는 네이버 블로그를 통한 유입의 정보를 알아보자.

먼저, 'blog.naver.com/PostView.nhn' 뒤에 있는 매개변수를 이용해 네이버 아이디와 글번호, 그리고 블로그 유입 검색어까지 3가지의 정보를 GA의 맞춤측정기준 기능을 이용해 GA 보고서 상에서 볼 수 있도록 해보자.

> [측정기준과 측정항목 - 구글 애널리틱스 도움말](https://support.google.com/analytics/answer/1033861)

> [맞춤 측정기준 및 측정항목 - 구글 애널리틱스 도움말](https://support.google.com/analytics/answer/2709828?hl=ko&ref_topic=2709827)

## 구글애널리틱스 맞춤측정기준 추가 ##

가장 먼저, 맞춤측정기준을 만들기 위해서 GA의 [관리] → [속성] → [맞춤정의] → [맞춤측정기준] 순서로 접속하자.

![관리-속성-맞춤측정기준](/images/post/11/ga-custom-dimension-01.png)

그리고 [+ 새 맞춤 측정기준]을 눌러 이름을 <code>네이버 블로그 주소</code>, 범위는 '세션', 사용중에 체크하고 [만들기]를 누르자.

![맞춤측정기준 만들기 01](/images/post/11/ga-custom-dimension-02.png)

그리고 같은 조건의 <code>네이버 블로그 유입 키워드</code>의 이름으로 맞춤측정기준을 하나 더 만들자.

![맞춤측정기준 만들기-02](/images/post/11/ga-custom-dimension-03.png)

이제 필요한 맞춤측정기준이 생성되었으니 GTM에서 이 맞춤측정기준을 사용하도록 하자.

## 구글태그매니저로 블로그의 정보를 담는 변수 만들기 ##

### 1. 리퍼러 호스트 변수 만들기 ###

네이버 블로그를 통한 유입을 구분하기 위해 GTM에서 호스트 이름을 저장하는 변수를 만들어보자.

GTM에 접속한 후 [변수] → [새로만들기]를 선택하고 [변수 구성]에서 [변수 유형]을 [HTTP 리퍼러]로 선택한다. 그리고 아래의 [구성요소 유형]을 눌러 [호스트 이름]을 선택하고 'www.' 생략에 체크한 후 이름을 <code>referrerHost</code>로 하여 [저장]한다.

![변수-referrerHost](/images/post/11/gtm-var-referrerhost.png)

### 2. 네이버 블로그의 아이디와 글 번호를 담는 변수 만들기 ###

이제 우리가 원하는 네이버 블로그의 아이디와 글 번호를 담는 변수를 만들 차례다. GTM의 미리보기 기능을 이용해 네이버 블로그에서 우리 웹사이트로 넘어오는 Referrer를 확인하면 아래와 같은 형태로 이루어져 있다는 걸 확인할 수 있다.

> https://blog.naver.com/PostView.naver?blogId=naverid&logNo=123456789000&beginTime=0&jumpingVid=&from=search&redirect=Log&widgetTypeCall=true&topReferer=https%3A%2F%2Fsearch.naver.com%2Fsearch.naver%3Fwhere%3Dnexearch%26sm%3Dtop_hty%26fbm%3D1%26ie%3Dutf8%26query%3D%EC%95%8C%ED%8C%8C%EC%BD%98&directAccess=false

무언가 알 수 없는 말들로 길지만 여기서 우리가 필요한 부분만 뽑아내면 된다. 먼저, GTM으로 이 Referrer 정보에서 블로그 작성자의 네이버 ID와 글 번호를 뽑아내고 '네이버 아이디/글 번호'의 형식으로 변수에 저장한다.

[변수] → [새로만들기]를 눌러 [변수 유형]을 [맞춤 자바스크립트]로 선택한다. 그리고 입력창에 아래의 소스 코드를 복사하여 붙여넣고 변수의 이름을 <code>naverBlogPost</code>로 하여 [저장]한다.

{% highlight javascript %}
{% raw %}
function() {
  var referrer_hostname = {{referrerHost}};
  var url_string = {{Referrer}};
  var url = new URL(url_string);
  // naverBlogPost by ogaeng.com, CC BY - SA
  if (referrer_hostname == "blog.naver.com" && url.pathname == "/PostView.naver"){
    var blogId = url.searchParams.get('blogId');
    var logNo = url.searchParams.get('logNo');
    var result = referrer_hostname + "/" + blogId + "/" + logNo;
    return result;
  } else if (referrer_hostname == "blog.naver.com"){
    var blog = url["pathname"].substring(1);
    var result = "blog.naver.com/" + blog;
    return result;
  } else if (referrer_hostname == "m.blog.naver.com" && url.pathname == "/PostView.nhn") {
    var blogId = url.searchParams.get('blogId');
    var logNo = url.searchParams.get('logNo');
    var result = referrer_hostname + "/" + blogId + "/" + logNo;
    return result;
  } else if (referrer_hostname == "m.blog.naver.com" && url.pathname == "/PostView.naver"){
    var blogId = url.searchParams.get('blogId');
    var logNo = url.searchParams.get('logNo');
    var result = referrer_hostname + "/" + blogId + "/" + logNo;
    return result;
  } else if (referrer_hostname == "m.blog.naver.com"){
    var blog = url["pathname"].substring(1);
    var result = "m.blog.naver.com/" + blog;
    return result;
  }
}
{% endraw %}
{% endhighlight %}

![GTM 변수 네이버 아이디, 글번호](/images/post/11/gtm-var-01.png)

### 3. 네이버 블로그의 검색 키워드를 담는 변수 만들기 ###

어떤 링크를 타고 혹은 연관 글을 타고 네이버 블로그에 방문한다면 검색 키워드 정보가 없겠지만 네이버에서 검색을 하고 검색에 노출된 블로그 글을 통해 우리 웹사이트에 접속했다면 네이버에서 검색했던 검색어 정보를 가져올 수 있다.

이번에는 검색을 통해 블로그에 방문했을 때, 그 검색 키워드를 GTM의 변수로 저장해보도록 하자.

[변수] → [새로만들기]를 눌러 [변수 유형]을 [맞춤 자바스크립트]로 선택한다. 그리고 입력창에 아래의 소스코드 링크의 소스를 복사하여 붙여넣고 변수의 이름을 <code>naverBlogKeyword</code>로 하여 [저장]한다.

{% highlight javascript %}
{% raw %}
function() {
  var referrer_hostname = {{referrerHost}};
  // naverBlogKeyword by ogaeng.com, CC BY - SA
  if (referrer_hostname == "blog.naver.com"){
    var url_string = {{Referrer}};
    var url = new URL(url_string);
    var topReferer = url.searchParams.get('topReferer');
    var url2 = new URL(topReferer);
    var result = url2.searchParams.get('query');
    return result;
  }
}
{% endraw %}
{% endhighlight %}

![GTM 변수 네이버블로그 키워드](/images/post/11/ga-var-naverblogketword.png)

## 구글애널리틱스 태그에 맞춤측정기준 추가하기 ##

이제 위에서 만든 2개의 변수를 GA의 맞춤측정기준과 연결해줄 차례다. 지금 이 글을 보고 있는 수준이라면 이미 GTM으로 GA태그를 사용하고 있을 테니 가지고 있는 GA 태그를 열어서 수정하면 된다.

[태그] 메뉴로 가서 가지고 있는 [GA 태그]로 들어간다. 그리고 조금 내려가서 [이 태그의 설정 재정의 사용]에 체크를 한다. 그러면 아래에 [> 기타 설정]과 [> 고급 설정]이 새로 생기는데 [> 기타 설정]을 눌러 나온 하위 메뉴에서 [> 맞춤 측정기준]을 누른 후 [+ 맞춤 측정기준]을 누른다. 이제 [지수]와 [측정기준 값]을 입력할 차례다. [지수]에 입력할 값을 알기 위해서 다시 GA로 돌아가서 [관리] → [속성] → [맞춤정의] → [맞춤 측정기준]으로 들어가 먼저 만들었던 맞춤 측정기준의 지수를 확인한다.

![GA 맞춤측정기준 확인](/images/post/11/ga-custom-dimension-03-3.png)

위 이미지에서는 '네이버 블로그 주소'가 지수 '1', '네이버 블로그 유입 키워드'가 지수 '2'로 나와있는데, 여기 나온 지수의 숫자를 그대로 써넣으면 된다. 이미 만들어놓은 맞춤측정기준이 있다면 지수가 다를 수 있으니 본인의 GA에 나온 지수를 다시 한 번 확인해야 한다.

지수를 확인했으니 다시 GTM으로 돌아와 [지수]란에 해당 번호를 입력한다. 이제 [측정기준 값]을 입력할 차례인데 측정기준 값 입력란 오른쪽에 있는 +블록 모양을 눌러 해당 지수에 맞는 naverBlogPost 변수와 naverBlogKeyword 변수를 선택한다. 그리고 [저장]을 눌러 수정을 완료한다.

![GTM 태그 설정](/images/post/11/ga-tag-gacode.png)

## 작동 확인 ##

이제 우리가 설정한 맞춤측정기준이 잘 작동되는지 확인할 단계다. 먼저 GTM 상단에 있는 [미리보기]를 이용해서 확인해보자. [미리보기] 버튼을 누르고 아래와 같은 메시지가 나온다면 미리보기 상태가 된다.

![GTM미리보기 메시지](/images/post/11/gtm-preview-button.png)

이 상태에서 네이버 블로그를 통해 정보가 잘 들어오는지 확인을 해야 하는데, 네이버에서 우리 웹사이트와 관련된 키워드로 검색을 하고 노출된 네이버 블로그 중 우리의 웹사이트의 정보를 담은 포스팅을 찾아 그 안에 있는 링크를 타고 우리 웹사이트로 접속해보자.

![GTM 미리보기 01](/images/post/11/gtm-preview-01.png)

미리보기 모드를 켜고 웹사이트에 접속하면 하단에 GTM 창이 하나 뜬다. 여기에서 'Tags Fired On This Page' 부분에 현재 작동 중인 GA 태그를 선택해보자.

![GTM 미리보기 맞춤측정기준](/images/post/11/gtm-preview-02.png)

GA태그의 자세한 내용이 나오는데 하단에 '맞춤측정기준'이 있는걸 볼 수 있다. 그 안에 우리가 방금 설정한 지수 1에 '네이버아이디/글번호', 지수 2에 '검색 키워드'가 나오는 걸 볼 수 있다. 이제 작동이 잘 되는 걸 확인했으니 GTM으로 돌아가 [제출]을 눌러 작업을 게시하자.

이제 GA에서도 우리가 만든 맞춤측정기준이 잘 들어오는지 확인할 차례다. 실제로 처음 맞춤측정기준을 만들면 보고서에 실시간으로 반영되진 않고 일정 시간이 지난 후부터 보이기 시작하기 때문에 GA에서의 확인은 다음날 하는 것을 추천한다.

GA에서 [획득] → [전체 트래픽] → [소스/매체] 혹은 [추천] 보고서에 들어가자. 그리고 보고서 표 상단에 있는 '두 번째 측정기준'을 눌러 '네이버 블로그 주소'를 검색하고 선택하자. 그러면 아래와 같은 보고서를 볼 수 있다.

![GA 맞춤측정기준 보고서 01](/images/post/11/ga-report-01.png)

우리 웹사이트에 유입된 네이버 블로그 글의 아이디와 글 번호가 찍혀 나와 블로그 마케팅의 성과를 더욱 자세하게 측정할 수 있다.

이번에는 블로그에 접속할 때 검색한 키워드를 확인해보자. '두 번째 측정기준'을 눌러 '네이버 블로그 유입 키워드'를 선택하자.

![GA 맞춤측정기준 보고서 02](/images/post/11/ga-report-02.png)

위 보고서처럼 검색 키워드가 보이는걸 확인할 수 있다. 이 데이터를 이용해 검색엔진을 통해 우리 웹사이트에 바로 접속하지 않고 다른 블로그의 글을 보고 들어왔다고 하더라도 검색 키워드 정보를 통해 어떤 키워드가 인기 있는지, 효과가 좋은지 등을 알 수 있어 다른 마케팅 활동에도 참고할 수 있다.

## 정리 ##

GA의 맞춤측정기준은 사용하는 사람의 능력에 따라 굉장히 많은 것을 할 수 있도록 도와주는 좋은 기능이다. 하지만 여기서 우리가 짚고 넘어가야 할 것은 GA와 같은 툴은 어디까지나 보조자료일 뿐이라는 것이다. 이런 보조자료가 100% 완벽할 수는 없기 때문에 의사결정에 참고하는 데에만 사용해야 하지, 맹목적인 믿음으로 데이터 하나하나의 싱크를 맞추려고 할 필요는 없다.

어쨌든 이번 글을 통해 그동안 측정이 어려웠던 '네이버 블로그를 통한 바이럴 마케팅의 효과'를 조금이나마 측정할 수 있게 되었다는데에 의의를 두려고 한다.

마지막으로 GA를 이용해 네이버 블로그 유입을 추적하는 방법을 정리하면 아래와 같다.

1. GA 맞춤측정기준 만들기
2. GTM으로 네이버 블로그 주소와 검색 키워드 변수 생성
3. GTM의 GA 태그에 맞춤측정기준 설정
4. GTM 미리보기와 GA로 동작 확인

이번 내용을 통해 많은 마케터들이 효율적인 마케팅을 실행할 수 있기를 바라며 막히는 부분이 생긴다면 언제든 댓글을 남겨주길 바란다.
