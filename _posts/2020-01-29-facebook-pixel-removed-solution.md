---
layout: post
title: 페이스북 픽셀 이벤트와 매개변수의 _removed_ 문제 해결하기
date: 2020-01-29 14:00:00 +09:00
author: "Ogaeng"
permalink: /facebook-pixel-removed-solution/
content_id: 16
image: /images/thumbnail/facebook-pixel-removed-solution.png
categories: Facebook
tags:
  - 페이스북
  - 페이스북픽셀
  - 광고
  - 이벤트
description: 페이스북 픽셀 맞춤 이벤트를 만들 때 발생하는 _removed_ 문제를 표준 매개변수를 이용해 손쉽게 해결하기
---

많은 마케터들이 웹사이트 사용자의 행동을 더 세밀하게 측정하고 광고에 활용하기 위해 페이스북 픽셀로 맞춤 이벤트를 만들어 활용한다.

하지만 최근 들어 문제가 하나 발생했는데, 매개변수의 데이터가 \_removed\_로 지워진다는 것이다. 매개변수의 데이터가 지워지면 이벤트만으로 행동을 추적해야 하는데 이벤트만을 사용한다면 이벤트의 종류가 많아지고 세부적인 행동을 분리하기가 어렵다.

![페이스북 픽셀 _removed_ 현상](/images/post/16/01-pixel-removed.png)

## 페이스북 픽셀 이벤트

페이지의 스크롤을 측정하는 이벤트로 간단한 예를 든다면

<table>
  <thead>
    <th style="width:20%">구분</th>
    <th style="width:30%">이름</th>
    <th>값</th>
  </thead>
  <tbody>
    <tr>
      <th>이벤트</th>
      <td>ScrollTracking</td>
      <td>-</td>
    </tr>
    <tr>
      <th rowspan="2">매개변수</th>
      <td>content_title</td>
      <td>페이지 제목</td>
    </tr>
    <tr>
      <td>scroll_depth</td>
      <td>스크롤 깊이</td>
    </tr>
  </tbody>
</table>

이런 식으로 이벤트와 매개변수가 기록된다. 하지만 요즘의 페이스북 픽셀은 <code>scroll_depth</code>와 <code>content_title</code> 매개변수는 지워진다. 매개변수가 지워지는 걸 보완하기 위한 간단한 방법은 이벤트를 <code>Scroll_50%</code>, <code>Scroll_75%</code>처럼 이름을 지어 세부적으로 나눠 기록하는 것이다.

하지만 이런 방식으로 이벤트명을 나누는 것에는 한계가 있다. 예시로 든 스크롤 이벤트도 몇 %까지 내렸는지 정도만 이벤트에 기록하지 매개변수 없이 어느 페이지에서 스크롤을 어디까지 내렸는지에 대한 것을 이벤트로 기록하는 것은 어렵다. 이런 간단한 이벤트가 아닌 더 많은 매개변수가 필요할 때에는 문제가 심각해진다.

## 표준 매개변수 활용

이 문제를 해결하기 위해 테스트를 진행해보니 한 가지 안정적으로 활용할 수 있는 매개변수들이 있다는 걸 확인했는데 페이스북 픽셀에서 제공하는 표준 매개변수를 활용하는 방법이다.

페이스북 개발자 사이트에는 페이스북 픽셀의 표준 이벤트를 설명하고 있는데, 그 내용 중 표준 매개변수(개체 속성)도 함께 설명하고 있다. 표준 매개변수의 원래 쓰임새는 정해져 있지만 우리가 맞춤 이벤트를 만들 때에도 표준 매개변수의 이름을 이용하면 데이터가 지워지지 않고 문제없이 기록되는 걸 확인했다.(물론 몇몇 표준 매개변수가 아닌 이름도 지워지지 않는 것을 확인했지만 언제 지워질지 모른다 ㅠㅠ) 페이스북에서 제공하는 표준 매개변수는 다음과 같다.

<table>
  <thead>
    <th style="width:20%">매개변수 이름</th>
    <th>매개변수 설명</th>
  </thead>
  <tbody>
    <tr>
      <td>

          content_category

      </td>
      <td>
        페이지/제품의 카테고리
      </td>
    </tr>
    <tr>
      <td>
        content_ids
      </td>
      <td>
        이벤트와 연결된 제품 ID(예: SKU([&#039;ABC123&#039;, &#039;XYZ789&#039;]))
      </td>
    </tr>
    <tr>
      <td>
        content_name
      </td>
      <td>
        페이지/제품의 이름
      </td>
    </tr>
    <tr>
      <td>
        content_type
      </td>
      <td>
        전달된 content_ids 또는 contents를 기반으로 한 product 또는 product_group. content_ids 또는 contents 매개변수에서 전달되는 ID가 제품 ID인 경우 값은 product가 되어야 합니다. 제품 그룹 ID가 전달되면 값은 product_group이 되어야 합니다.
      </td>
    </tr>
    <tr>
      <td>
        contents
      </td>
      <td>
        해당하는 경우 수량과 EAN(국제 상품 번호)가 포함되거나 다른 제품 또는 콘텐츠 ID가 포함된 JSON 개체 배열. id 및 quantity는 필수 필드입니다(예: &quot;[&#123;&#039;id&#039;: &#039;ABC123&#039;, &#039;quantity&#039;: 2&#125;, &#123;&#039;id&#039;: &#039;XYZ789&#039;, &#039;quantity&#039;: 2&#125;]&quot;).
      </td>
    </tr>
    <tr>
      <td>
        currency
      </td>
      <td>
        value에 지정된 통화
      </td>
    </tr>
    <tr>
      <td>
        num_items
      </td>
      <td>
        InitiateCheckout 이벤트와 함께 사용. 결제를 초기화한 품목 수.
      </td>
    </tr>
    <tr>
      <td>
        predicted_ltv
      </td>
      <td>
        광고주가 정의한 구독자의 생애 가치(LTV) 또는 정확한 값으로 표현된 생애 가치
      </td>
    </tr>
    <tr>
      <td>
        search_string
      </td>
      <td>
        Search 이벤트와 함께 사용. 사용자가 검색을 위해 입력한 문자열.
      </td>
    </tr>
    <tr>
      <td>
        status
      </td>
      <td>
        CompleteRegistration 이벤트와 함께 사용하여 등록 상태 표시
      </td>
    </tr>
    <tr>
      <td>
        value
      </td>
      <td>
        비즈니스에서 이 이벤트를 실행한 사용자의 가치
      </td>
    </tr>
  </tbody>
</table>

> 표준 이벤트 더 알아보기: <a onclick="window.open('https://developers.facebook.com/docs/facebook-pixel/reference', '_blank');">facebook for developers</a>

## 활용 예시

그럼 실제 예시로 이 블로그에 있는 페이스북 픽셀 스크롤 측정 이벤트 내용을 가져와보자

{% highlight javascript %}
{% raw %}

fbq('trackCustom', 'ScrollTracking', {
	content_title: document.title,
	scroll_depth: '{{Scroll Depth Threshold}}%'
});

{% endraw %}
{% endhighlight %}

위에는 페이지의 제목을 수집하는 <code>content_title</code>과 스크롤 깊이를 수집하는 <code>scroll_depth</code> 매개변수가 있다. 이 두 매개변수 이름을 아래와 같이 변경하면 된다.

{% highlight javascript %}
{% raw %}

fbq('trackCustom', 'ScrollTracking', {
	content_name: document.title,
	value: '{{Scroll Depth Threshold}}'
});

{% endraw %}
{% endhighlight %}

이렇게 표준 매개변수인 <code>content_name</code>과 <code>value</code>로 이름을 바꿔주면 매개변수 데이터가 지워지는 현상을 해결할 수 있다.

예시를 하나만 들면 정 없으니 하나만 더 들어보자. 이번엔 검색 엔진의 유입 키워드를 수집하는 이벤트를 소환한다.

{% highlight javascript %}
{% raw %}

fbq('trackCustom', 'SearchKeywords', {
    keyword: {{referrerKeyword}}
});

{% endraw %}
{% endhighlight %}

위 이벤트에서는 키워드를 수집하는 <code>keyword</code> 매개변수를 표준 매개변수로 바꿔주면 된다.

{% highlight javascript %}
{% raw %}

fbq('trackCustom', 'SearchKeywords', {
    search_string: {{referrerKeyword}}
});

{% endraw %}
{% endhighlight %}

이렇게 표준 매개변수인 <code>search_string</code>으로 변경을 하면 데이터가 지워지지 않고 잘 수집된다.

특히, 이 검색어 수집 이벤트는 페이스북 코리아에서도 2018년 F8 행사를 시작으로 많은 광고주에게 교육한 것으로 알고 있는데, 그곳에서도 이 블로그의 글을 참고했기 때문에 그 방식을 이용하는 분들도 매개변수 명에 문제가 있으니 꼭 매개변수 명을 고쳐주어야 한다.

지금 이 글을 보았다면 어서 페이스북 픽셀 이벤트 관리자에 접속해서 우리 웹사이트의 데이터가 제대로 수집되고 있는지를 꼭 확인해보길 바란다.

문제가 있다면 빠르게 수정하자.
