---
layout: post
title: '고도몰에 구글 애널리틱스 4(GA4) 전자상거래 이벤트 설정하기'
date: 2020-10-28 14:18:00 +09:00
author: "Ogaeng"
permalink: /godomall-ga4-ecommerce-setting/
content_id: 24
image: /images/thumbnail/godomall-ga4-ecommerce-setting.jpg
categories: [고도몰,GA,GTM]
tags:
  - 고도몰
  - 이벤트
  - GTM
  - GA
  - GA4
description: GTM을 이용해 고도몰 5로 만들어진 쇼핑몰에 GA4 전자상거래 이벤트 설정하기
---

구글 애널리틱스 앱+웹 속성이 1년이 조금 넘는 기간의 베타를 거쳐 구글 애널리틱스 4라는 이름으로 새로운 속성이 정식으로 출시되었다. 기존 무료 버전의 GA에서 사용할 수 없었던 기능들이 추가되고 특히 단점으로 지적됐던 이벤트와 관련된 부분이 대폭 개선되었다. 이벤트 카테고리, 액션, 라벨 구조 때문에 다른 도구들과 다르게 이벤트를 설정해 사용했던 분들에게는 희소식이다.

이번 글에서는 고도몰 5 쇼핑몰을 이용해 상품 상세 보기 부터 주문 완료까지 기본적인 구매 과정 몇 가지를 뽑아 구글 태그 매니저를 활용해 구글 애널리틱스 4 이벤트로 기록하는 과정을 진행하려고 한다. 이번에도 불가피하게 내부 코드를 만져야 하니 잘 보고 따라 해보자.

이 글에서 사용한 이벤트 이름과 매개변수 이름은 아래 가이드를 참고했다.

> <a onclick="window.open('https://support.google.com/analytics/answer/9268036?hl=en&ref_topic=9756175', '_blank');">Analytics Help - Google Analytics Events: Retail/Ecommerce</a>

## 목차

- <a onclick="window.open('#1', '_self');">User ID 설정과 구글 태그 매니저 설치</a>
- <a onclick="window.open('#2', '_self');">상품 상세 보기와 장바구니 추가 이벤트</a>
- <a onclick="window.open('#3', '_self');">주문서 작성 이벤트</a>
- <a onclick="window.open('#4', '_self');">주문 완료 이벤트</a>

<div id="1"></div>
## 1. User ID 설정과 구글 태그 매니저 설치

### User ID 정보 가져오기

가장 먼저 고도몰에서 사용자가 로그인 했을 때 User ID를 GTM에서 수집할 수 있도록 설정해보자. 소스를 삽입해야 하니 우선 고도몰 쇼핑몰 관리자에 접속하자.

[디자인] 메뉴에 접속하면 왼쪽에 폴더와 파일 리스트를 볼 수 있다. 여기에서 [전체 레이아웃] > [상단 레이아웃]을 눌러 상단 레이아웃 파일을 열자.

![고도몰 디자인 화면](/images/post/24/00.png)

상단 레이아웃은 모든 페이지에서 실행되는 공통 영역으로 <head> 태그를 담고 있다. <head> 태그 안에 아래 User ID 코드를 넣는다.

{% highlight html %}
{% raw %}

<!-- User ID -->
<script type="text/javascript">
  var loginCheck = '{=gd_is_login()}';
  if (loginCheck == '1'){
    dataLayer = [{
      'userId': '{=gSess.memNo}'
    }];
  }
</script>
<!-- End User ID -->

{% endraw %}
{% endhighlight %}

이 코드는 사용자가 로그인했을 때 로그인 한 사용자의 일련번호를 userId라는 데이터 영역 변수(dataLayer)로 만들어 구글 태그 매니저에서 사용할 수 있게 한다.(여기에서는 회원의 일련번호를 이용했는데 일련번호나 ID 등을 해시하여 사용하는 것이 좋다.)

### 구글 태그 매니저 설치

이제 User ID 코드 바로 아래에 GTM 스니펫 설치 코드를 넣는다. 이때 GTM에서 제공하는 스니펫을 그대로 넣으면 컨테이너 로드가 되지 않는 문제가 발생하니 아래 코드에서 `'GTM-XXXXXX'` 부분을 내 'GTM ID'로 변경해서 넣도록 하자.

{% highlight html %}
{% raw %}

<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i,j){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:j+'.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-XXXXXX','gtm');</script>
<!-- End Google Tag Manager -->

{% endraw %}
{% endhighlight %}

이때 주의해야 할 점은 아래 예시와 같이 User ID 코드가 위, GTM 스니펫이 아래에 위치해야 한다.

{% highlight html %}
{% raw %}

<!-- User ID -->
<script type="text/javascript">
  var loginCheck = '{=gd_is_login()}';
  if (loginCheck == '1'){
    dataLayer = [{
      'userId': '{=gSess.memNo}'
    }];
  }
</script>
<!-- End User ID -->

<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i,j){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:j+'.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-XXXXXX','gtm');</script>
<!-- End Google Tag Manager -->

{% endraw %}
{% endhighlight %}

### User ID 변수 만들기

User ID를 구글 태그 매니저에서 사용할 수 있도록 변수를 만들어보자. GTM에서 [변수] → [새로 만들기]를 눌러 변수명은 `User ID`로 하고 변수 유형을 [데이터 영역 변수]로 선택한다. 그리고 아래 데이터 영역 변수 이름 입력 창에 `userId` 를 입력하고 저장한다.

![GTM User ID 변수](/images/post/24/01.png)

### GA4 태그 만들기

GA4의 태그는 우선 구글 애널리틱스에 접속해서 [속성 만들기]를 통해 새로운 GA4 속성을 생성한다. 그리고 [데이터 스트림]에서 플랫폼을 [웹]으로 선택한 후 웹사이트의 정보를 입력한 다음 [스트림 만들기]를 누른다.

![데이터 스트림 설정](/images/post/24/02.png)

그리고 등장하는 [웹 스트림 세부정보]에서 [측정 ID]를 복사한다.

![웹 스트림 세부정보](/images/post/24/03.png)

이제 GTM으로 돌아가서 [태그] → [새로만들기]를 선택하고 [태그 구성]에서 [Google 애널리틱스: GA4 구성]을 선택한다. [측정 ID]에 위에서 복사한 ID를 입력한다.

그리고 User ID를 설정하기 위해 아래에 있는 [설정할 필드]를 눌러 필[행 추가]를 누른다음 [필드 이름]에는 `user_id`를 입력하고 값에는 블록 아아이콘을 눌러 위에서 생성한 `User ID` 변수를 선택한다. 트리거는 `All Pages`를 선택한 다음 태그의 이름을 입력하고 저장한다.

![GA4 태그](/images/post/24/04.png)

<div id="2"></div>
## 2. 상품 상세 보기와 장바구니 추가 이벤트

### 상품 상세 보기 페이지 삽입 코드

다음으로 상품 상세 페이지를 작업하자. 고도몰 디자인 메뉴에서 [상품] > [상품상세화면]을 눌러 상품 상세 보기 파일을 열자.(경로: goods/goods_view.html) 편집창이 나오면 맨 밑에 아래 코드를 넣고 저장하자. (여기서는 상품 상세 페이지에서 단일 상품만 노출되는 경우를 예로 들었다.)

{% highlight html %}
{% raw %}

<script type="text/javascript">
  dataLayer.push({
    'event': 'detail',
    'goodsInfo': [{
      'item_id': '{goodsView.goodsNo}',
      'item_name': '{=goodsView['goodsNm']}',
      'price': '{=gd_isset(goodsView['goodsPrice'],0)}'
    }]
  });
</script>

{% endraw %}
{% endhighlight %}

위 코드는 상품 상세 화면에서 `detail`이라는 맞춤 이벤트를 발동시키고 `goodsInfo`에 상품 번호, 이름, 가격 정보를 수집해 GTM에서 활용할 수 있게 한다.

이제 위 코드에서 전송한  `goodsInfo` 데이터 영역 변수를 GTM 변수로 만들 차례다. GTM에서 [변수] → [새로 만들기]를 눌러 변수명은 `상품 정보`로 하고 변수 유형을 [데이터 영역 변수]로 선택한다. 그리고 아래 데이터 영역 변수 입력창에 `goodsInfo` 를 입력하고 저장한다.

![상품 정보 변수](/images/post/24/05.png)

### 상품 상세 보기 이벤트 태그

이제 `detail` 맞춤 이벤트가 발동되는 시점에 실행되는 상품 상세 보기의 GA4 이벤트 태그를 만들 차례다. [태그] → [새로 만들기]를 눌러 새로운 태그를 만든다. 이름은 자유롭게 입력하고 태그 유형은 [Google 애널리틱스: GA4 이벤트]를 선택한다.

구성 태그에는 위에서 만든 GA4 구성 태그를 선택하고 이벤트 이름은 `view_item`으로 한다. 아래에 있는 이벤트 매개변수 화살표를 열어 [행 추가]를 두 번 눌러 2개의 행을 만든다.

첫 번째 행의 매개변수 이름에 `items` 를 입력하고 값에는 위에서 만든 `상품 정보` 변수를 선택한다.(이 매개변수에는 상품의 정보가 들어간다.)

두 번째 행 매개변수 이름에는 `currency`를 입력하고 값은 `KRW`를 입력한다.(이 부분은 통화 정보를 입력하는 것으로 해외 통화를 사용하는 경우 해외 통화의 코드를 넣으면 된다.)

이제 트리거에서 +를 눌러 새 트리거를 만들어야 한다. 새 트리거는 유형을 [맞춤 이벤트]로 선택하고 이벤트 이름에 `detail` 을 입력하고 저장한다.

![상품 상세보기 이벤트 트리거](/images/post/24/06.png)

트리거까지 지정이 완료되었으면 아래와 같이 빠진 내용이 없는지 확인한 후 태그를 저장한다.

![상품 상세보기 이벤트 태그](/images/post/24/07.png)

### 장바구니 추가 코드

이제 상품 상세 보기 화면에서 [장바구니]를 눌러 상품을 장바구니에 담는 액션을 이벤트로 작성할 차례다. 위에서 작업하던 고도몰 관리자의 디자인 메뉴에서 [상품상세화면]에 입력한 상품상세보기 코드 밑에 아래 코드를 넣고 저장한다.

{% highlight html %}
{% raw %}

<script type="text/javascript">
  var addToCart = function () {
    dataLayer.push({
      'event': 'addToCart',
      'goodsInfo': [{
        'item_id': '{goodsView.goodsNo}',
        'item_name': '{=goodsView['goodsNm']}',
        'price': '{=gd_isset(goodsView['goodsPrice'],0)}',
        'quantity': goodsTotalCnt
      }]
    });
  }
  $('#cartBtn').click(addToCart);
</script>

{% endraw %}
{% endhighlight %}

위 코드는 ID가 `cartBtn`인 장바구니 담기 버튼을 눌렀을 때 상품 정보가 담긴 `addToCart` 함수를 실행시킨다.(쇼핑몰 스킨마다 장바구니 담기 버튼의 형태와 상품을 담는 방식이 다를 수 있기 때문에 사용 중인 스킨에 알맞게 수정해야 한다.)

### 장바구니 추가 이벤트 태그

장바구니에 추가할 때 작동하는 GA4 이벤트를 만들어보자. [태그] → [새로 만들기]를 눌러 새로운 태그를 만든다. 이름은 자유롭게 입력하고 태그 유형은 [Google 애널리틱스: GA4 이벤트]를 선택한다.

구성 태그에는 위에서 만든 GA4 구성 태그를 선택하고 이벤트 이름은 `add_to_cart`로 한다. 상품 상세보기와 마찬가지로 아래 이벤트 매개변수 화살표를 열어 [행 추가]를 두 번 눌러 2개의 행을 만든다.

첫 번째 행의 매개변수 이름에 `items` 를 입력하고 값은 상품 상세보기와 같이 `상품 정보` 변수를 선택한다.

두 번째 행 매개변수 이름에는 `currency`를 입력하고 값은 `KRW`를 입력한다.

이제 이 태그의 트리거를 만들자. 트리거 유형을 [맞춤 이벤트]로 선택하고 이벤트 이름을 `addToCart` 로 입력하고 저장한다.

![장바구니 추가 트리거](/images/post/24/08.png)

트리거 지정 후 태그를 저장하자.

![장바구니 추가 태그](/images/post/24/09.png)

<div id="3"></div>
## 3. 주문서 작성 이벤트

이제 사용자가 배송이나 결제 등의 정보를 입력하는 주문서 작성 단계다.

### 주문서 작성 페이지 삽입 코드

고도몰 디자인 메뉴에서 [주문] > [주문서 작성 / 결제]를 눌러 주문서 작성 파일을 열자.(경로: order/order.html) 편집창이 맨 밑에 아래 코드를 넣고 저장하자.

{% highlight html %}
{% raw %}

<script type="text/javascript">
  var goodsInfo = [
    <!--{ @ cartInfo }-->
      <!--{ @ .value_ }-->
        <!--{ @ ..value_ }-->
        {
          'item_id': '{=...goodsNo}',
          'item_name': '{=...goodsNm}',
          'quantity': {=...goodsCnt},
          'price': {=...price['goodsPrice']}
        },
        <!--{ / }-->
      <!--{ / }-->
    <!--{ / }-->
  ];

  dataLayer.push({
    'event': 'checkout',
    'goodsInfo': goodsInfo
  });
</script>

{% endraw %}
{% endhighlight %}

이 코드에서 `<!--{ @ cartInfo }-->` 부분은 상품 정보를 반복하여 가져올 수 있도록 해주는 고도몰의 치환코드이다. 그리고 아래 `dataLayer.push`를 통해 장바구니 페이지에서 `checkout`이라는 맞춤 이벤트를 발동시키고 상품 상세 보기와 마찬가지로 `goodsInfo`에 상품 정보를 넣어 GTM에서 활용할 수 있게 한다.

### 주문서 작성 이벤트 태그

GTM의 [태그] → [새로 만들기]를 눌러 새로운 태그를 만든다. 이름은 자유롭게 입력하고 태그 유형은 [Google 애널리틱스: GA4 이벤트]로 선택한다.

앞에서 설명한 이벤트와 마찬가지로 위에서 만든 GA4 구성 태그를 선택하고 이벤트 이름은 `begin_checkout`로 한다. 상품 상세보기와 마찬가지로 아래 이벤트 매개변수 화살표를 열어 [행 추가]를 두 번 눌러 2개의 행을 만든다.

첫 번째 행의 매개변수 이름에 `items` 를 입력하고 값은  `상품 정보` 변수를 선택한다.

두 번째 행 매개변수 이름에는 `currency`를 입력하고 값은 `KRW`를 입력한다.

트리거는 유형을 [맞춤 이벤트]로 선택하고 이벤트 이름을 `checkout` 으로 입력하고 저장하고 태그도 저장하자.

![주문서 작성 트리거](/images/post/24/10.png)

![주문서 작성 태그](/images/post/24/11.png)

<div id="4"></div>
## 4. 주문 완료 이벤트

이제 구매 여정의 마지막 단계인 주문 완료 단계이다. 주문을 완료 했을 때 등장하는 주문 완료 페이지에서 이벤트가 실행되도록 작업을 시작해보자.

### 주문 완료 페이지 삽입 코드

고도몰 디자인 메뉴에서 [주문] > [주문 완료]를 눌러 주문서 작성 파일을 열자.(경로: order/order_end.html) 편집창이 맨 밑에 아래 코드를 넣고 저장하자.

{% highlight html %}
{% raw %}

<script type="text/javascript">
  var txInfo = {
    'transactionId': '{orderInfo.orderNo}',
    'totalRevenue': {=orderInfo.settlePrice},
    'shippingFee': {=gd_isset(orderInfo.totalDeliveryCharge)},
    'firstOrder': '{=orderInfo.firstSaleFl}'
  };

  var goodsInfo = [
    <!--{ @ orderInfo.goods }-->
      {
      'item_id': '{=.goodsNo}',
      'item_name': '{=.goodsNm}',
      'quantity': {=.goodsCnt},
      'price': {=.goodsPrice}
      },
    <!--{ / }-->
  ];

  dataLayer.push({
    'event': 'purchase',
    'txInfo': txInfo,
    'goodsInfo': goodsInfo
  });
</script>

{% endraw %}
{% endhighlight %}

주문 완료에서는 상품 정보와 주문 정보가 함께 필요하다. 먼저 `txInfo`에 들어가 있는 항목을 살펴보면 transactionId(주문 번호), totalRevenue(결제 금액),  shippingFee(배송비), firstOrder(첫 구매 여부)가 있다. 이 중 첫 구매 여부는 y(첫 구매 O)와 n(첫 구매 X) 두 가지 값을 가지고 있다.

### 주문 정보 GTM 변수

주문 정보라는 새로운 정보가 등장했기 때문에 GTM에서도 변수를 추가해주어야 한다. 주문 번호, 결제 금액, 배송비, 첫 구매 여부의 정보를 담은 변수를 각각 만들어보자.

### 주문 번호 변수

GTM에서 [변수] → [새로 만들기]를 눌러 변수명은 `주문 정보 - 주문 번호`로 하고 변수 유형을 [데이터 영역 변수]로 선택한다. 그리고 아래 데이터 영역 변수 입력창에 `txInfo.transactionId` 를 입력하고 저장한다.

![주문번호 변수](/images/post/24/12.png)

### 결제 금액 변수

[변수] → [새로 만들기]를 눌러 변수명은 `주문 정보 - 결제 금액`로 하고 변수 유형을 [데이터 영역 변수]로 선택한다. 그리고 아래 데이터 영역 변수 입력창에 `txInfo.totalRevenue` 를 입력하고 저장한다.

![결제금액 변수](/images/post/24/13.png)

### 배송비 변수

[변수] → [새로 만들기]를 눌러 변수명은 `주문 정보 - 배송비`로 하고 변수 유형을 [데이터 영역 변수]로 선택한다. 그리고 아래 데이터 영역 변수 입력창에 `txInfo.shippingFee` 를 입력하고 저장한다.

![배송비 변수](/images/post/24/14.png)

### 첫 구매 여부 변수

[변수] → [새로 만들기]를 눌러 변수명은 `주문 정보 - 첫 구매 여부`로 하고 변수 유형을 [데이터 영역 변수]로 선택한다. 그리고 아래 데이터 영역 변수 입력창에 `txInfo.firstOrder` 를 입력하고 저장한다.

![첫구매 변수](/images/post/24/15.png)

### 주문 완료 이벤트 태그

주문 과정의 마지막 단계인 주문 완료 이벤트다. [태그] → [새로 만들기]를 눌러 새로운 태그를 만든다. 이름은 자유롭게 입력하고 태그 유형은 [Google 애널리틱스: GA4 이벤트]로 선택한다.

앞에서 설명한 이벤트와 마찬가지로 위에서 만든 GA4 구성 태그를 선택하고 이벤트 이름은 `purchase`로 한다. 이벤트 매개변수에는 아래 항목들을 추가한다.

- 매개변수 이름: `items` ,  값:  `상품 정보` 변수를 선택
- 매개변수 이름: `currency` ,  값:  `KRW` 입력
- 매개변수 이름: `transaction_id` ,  값:  `주문 정보 - 주문 번호` 변수 선택
- 매개변수 이름: `value` ,  값:  `주문 정보 - 결제 금액` 변수 선택
- 매개변수 이름: `shipping` ,  값:  `주문 정보 - 배송비` 변수 선택
- 매개변수 이름: `first_order` ,  값:  `주문 정보 - 첫 구매 여부` 변수 선택

트리거는 유형을 [맞춤 이벤트]로 선택하고 이벤트 이름을 `purchase` 로 입력하고 저장하고 태그도 저장하자.

![주문완료 트리거](/images/post/24/16.png)

![주문완료 태그](/images/post/24/17.png)

### 맞춤 측정 기준 추가

주문 완료 이벤트에 삽입된 이벤트 매개변수 중 첫 구매 여부 항목은 볼 수 있도록 설정을 해주어야 GA4에서 확인이 가능하다. 추가를 위해서 GA4 메뉴 중 [모든 이벤트] → [맞춤 정의 관리]로 들어가서 [맞춤 측정기준 만들기]를 누르자.

이벤트 매개변수 이름에 위에서 설정한`first_order` 를 넣고 맞춤 측정기준 이름은 `첫 구매 여부` 로 설정한 다음 [저장]하자. 이렇게 넣어두면 이벤트 보고서에서 첫 구매 정보를 확인할 수 있다.

![맞춤측정기준 추가](/images/post/24/18.png)

## 마치며

GA4가 등장하면서 GA에서도 드디어 제대로 된 이벤트 기록을 할 수 있게 됐다. 이번 글에서 다룬 전자상거래 관련 이벤트를 기초로 해서 우리 웹사이트를 방문하는 사용자의 행동을 이번 기회를 통해 좀 더 잘 이해할 수 있도록 이벤트 구조를 생각해보는 시간을 가지는 계기가 되었으면 좋겠다.
