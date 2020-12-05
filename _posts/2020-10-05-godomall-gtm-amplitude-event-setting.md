---
layout: post
title: '구글 태그 매니저로 고도몰에 앰플리튜드 구매 여정 이벤트 설정하기'
date: 2020-10-05 00:00:00 +09:00
author: "Ogaeng"
permalink: /godomall-gtm-amplitude-event-setting/
content_id: 23
image: /images/thumbnail/godomall-gtm-amplitude-event-setting.jpg
categories: [고도몰,Amplitude,GTM]
tags:
  - 고도몰
  - 이벤트
  - GTM
  - Amplitude
description: GTM을 이용해 고도몰 5로 만들어진 쇼핑몰에 앰플리튜드(Amplitude)를 이용해 상품 상세 보기부터 구매까지 여정에 대한 이벤트 설정하기
---

앰플리튜드는 최근 인기를 끌고 있는 분석 도구로 이벤트를 기반으로 한 내부 행동 분석과 코호트, 퍼널 분석 등이 강점이다. 무료 플랜에서 제공하는 기능도 충분히 강력하기 때문에 사용해보길 추천한다.(앱을 다루다가 웹으로 넘어와 적응하고 있는 분들에게 특히 추천)

이번 글에서는 카페24와 함께 많이 사용하는 임대몰 서비스인 고도몰 5를 이용해 앰플리튜드(Amplitude) 설치와 쇼핑몰에 필요한 기본적인 구매 여정에 대한 이벤트를 설정해 볼 건데 고도몰은 불가피하게 내부 코드를 만져야 하니 유심히 보고 따라 하도록 하자.

이번에도 내용이 길어 필요한 내용으로 바로 넘어갈 수 있게 목차를 준비했다.

## 목차

- <a onclick="window.open('#1', '_self');">User ID 설정과 앰플리튜드 설치</a>
- <a onclick="window.open('#2', '_self');">상품 상세 보기와 장바구니 추가 이벤트</a>
- <a onclick="window.open('#3', '_self');">장바구니 보기 이벤트</a>
- <a onclick="window.open('#4', '_self');">주문서 작성 이벤트</a>
- <a onclick="window.open('#5', '_self');">주문 완료 이벤트</a>

<div id="1"></div>
## 1. User ID 설정과 앰플리튜드(Amplitude) 설치

### User ID 정보 가져오기

가장 먼저 앰플리튜드의 이점인 User ID 기능을 설정해보자. User ID를 불러오기 위해 소스를 삽입해야 한다. 고도몰 쇼핑몰 관리자에 접속하자.

[디자인] 메뉴에 접속하면 왼쪽에 폴더와 파일 리스트가 나오는 것을 볼 수 있다. 여기서 [전체 레이아웃] > [상단 레이아웃]을 눌러 상단 레이아웃 파일을 열자.

![고도몰 디자인 화면](/images/post/23/00.png)

상단 레이아웃은 모든 페이지에서 실행되는 공통 영역으로 `<head>` 태그를 담고 있다. `<head>` 태그 안에 아래 User ID 코드를 삽입한다.

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

위 코드는 사용자가 로그인했을 때 로그인 한 사용자의 일련번호를 userId라는 데이터 영역 변수로 만들어 구글 태그 매니저에서 사용할 수 있게 한다.(여기에서는 회원의 일련번호를 이용했는데 일련번호나 아이디 등을 해시해서 사용하는 것이 좋다.)

### 구글 태그 매니저 설치

이제 User ID 코드 바로 아래에 GTM 스니펫 설치 코드를 넣는다. 이때 GTM에서 제공하는 스니펫을 그대로 넣으면 컨테이너 로드가 되지 않는 문제가 발생하니 아래 코드에서 `'GTM-XXXXXX'` 부분을 내 GTM ID로 변경해서 넣도록 하자.

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

User ID를 GTM에서 사용할 수 있도록 변수를 만들어보자. GTM에서 [변수] → [새로 만들기]를 눌러 변수명은 `User ID`로 하고 변수 유형을 [데이터 영역 변수]로 선택한다. 그리고 아래 데이터 영역 변수 이름 입력 창에 `userId` 를 입력하고 저장한다.

![GTM User ID 변수](/images/post/23/01.png)

### 앰플리튜드(Amplitude) SDK 태그 만들기

구글 태그 매니저를 이용해 앰플리튜드 설치하는 방법은 아래 글 링크를 참고하여 따라 하도록 하자.

> <a onclick="window.open('https://ogaeng.com/amplitude-setup/', '_blank');">구글 태그 매니저를 이용해 앰플리튜드(Amplitude) 설치하기</a>

위 글 내용을 보고 따라 했다면 앰플리튜드 SDK 태그 하단에 아래와 같은 코드가 있을 것이다.

{% highlight html %}
{% raw %}

amplitude.getInstance().init("API 키", null, {
  includeGclid: true,
  includeUtm: true,
  includeReferrer: true
});
</script>

{% endraw %}
{% endhighlight %}

여기서 `"API 키"` 다음에 있는 `null` 부분에 우리가 만든 User ID 변수를 넣어야 한다. 아래와 같이 수정한 다음 저장하자.

{% highlight html %}
{% raw %}

amplitude.getInstance().init("API 키", {{User ID}}, {
  includeGclid: true,
  includeUtm: true,
  includeReferrer: true
});
</script>

{% endraw %}
{% endhighlight %}

![GTM 앰플리튜드 SDK 태그](/images/post/23/02.png)

이렇게 User ID를 사용하면 사용자가 로그인했을 때, 앰플리튜드에 User ID를 매핑해 사용자가 로그인을 한다면 여러 기기를 사용하더라도 같은 사용자로 보고 데이터를 연결할 수 있다.

<div id="2"></div>
## 2. 상품 상세 보기와 장바구니 추가 이벤트

### 상품 상세 보기 페이지 삽입 코드

이제 상품 상세 페이지를 작업해보자. 고도몰 디자인 메뉴에서 [상품] > [상품상세화면]을 눌러 상품 상세 보기 파일을 열자.(경로: goods/goods_view.html) 편집창이 나오면 맨 밑에 아래 코드를 넣고 저장하자. (이 글에서는 상품 상세 페이지에서 단일 상품만 노출되는 경우를 예로 들었다.)

{% highlight html %}
{% raw %}

<script type="text/javascript">
  dataLayer.push({
    'event': 'detail',
    'goodsInfo': [{
      'id': '{goodsView.goodsNo}',
      'name': '{=goodsView['goodsNm']}',
      'price': '{=gd_isset(goodsView['goodsPrice'],0)}'
    }]
  });
</script>

{% endraw %}
{% endhighlight %}

위 코드는 상품 상세 화면에서 `detail`이라는 맞춤 이벤트를 발동시키고 `goodsInfo`에 상품 번호, 이름, 가격 정보를 수집해 GTM에서 활용할 수 있게 한다.

이제 위 코드에서 전송한  `goodsInfo` 데이터 영역 변수를 GTM 변수로 만들 차례다. GTM에서 [변수] → [새로 만들기]를 눌러 변수명은 `상품 정보`로 하고 변수 유형을 [데이터 영역 변수]로 선택한다. 그리고 아래 데이터 영역 변수 입력창에 `goodsInfo` 를 입력하고 저장한다.

![상품 정보 변수](/images/post/23/03.png)

상품 정보 리스트를 변수로 만들었으니 앞으로 등장할 이벤트에서 모두 사용될 상품 번호와 이름같이 각각의 정보를 따로 담을 변수를 만들어 보자.(앰플리튜드는 이벤트의 속성의 값으로 여러 개의 값이 들어갈 때 각각 배열의 형태로 만들어주어야 한다.)

### 상품 번호 변수

[변수] → [새로 만들기]를 눌러 변수명은 `상품 번호`로 하고 변수 유형을 [맞춤 자바스크립트]로 선택한다. 그리고 아래 입력창에 아래의 스크립트를 넣고 저장한다.

{% highlight javascript %}
{% raw %}

function() {
  var key = {{상품 정보}};
  var list = [];
  for(var i = 0; i < key.length; i++) {
      var name = key[i].id;
      list.push(name);
  }
  return list;
}

{% endraw %}
{% endhighlight %}

### 상품 이름 변수

[변수] → [새로 만들기]를 눌러 변수명은 `상품 이름`으로 하고 변수 유형을 [맞춤 자바스크립트]로 선택한다. 그리고 아래 입력창에 아래의 스크립트를 넣고 저장한다.

{% highlight javascript %}
{% raw %}

function() {
  var key = {{상품 정보}};
  var list = [];
  for(var i = 0; i < key.length; i++) {
      var name = key[i].name;
      list.push(name);
  }
  return list;
}

{% endraw %}
{% endhighlight %}

### 상품 가격 변수

[변수] → [새로 만들기]를 눌러 변수명은 `상품 가격`으로 하고 변수 유형을 [맞춤 자바스크립트]로 선택한다. 그리고 아래 입력창에 아래의 스크립트를 넣고 저장한다.

{% highlight javascript %}
{% raw %}

function() {
  var key = {{상품 정보}};
  var list = [];
  for(var i = 0; i < key.length; i++) {
      var name = key[i].price;
      list.push(name);
  }
  return list;
}

{% endraw %}
{% endhighlight %}

### 상품 수량 변수

[변수] → [새로 만들기]를 눌러 변수명은 `상품 수량`으로 하고 변수 유형을 [맞춤 자바스크립트]로 선택한다. 그리고 아래 입력창에 아래의 스크립트를 넣고 저장한다.

{% highlight javascript %}
{% raw %}

function() {
  var key = {{상품 정보}};
  var list = [];
  for(var i = 0; i < key.length; i++) {
      var name = key[i].quantity;
      list.push(name);
  }
  return list;
}

{% endraw %}
{% endhighlight %}

위 4가지 변수는 앞으로 모든 이벤트에서 이벤트 속성 값으로 사용될 예정이니 잘 만들어놓자.

### 상품 상세 보기 앰플리튜드 이벤트 태그

이제 상품을 조회할 때 발동하는 앰플리튜드 이벤트를 만들어보자. [태그] → [새로 만들기]를 눌러 새로운 태그를 만든다. 이름은 자유롭게 입력하고 태그 유형은 [맞춤 HTML]로 선택한다. 그리고 입력창에 아래 코드를 입력한다.

{% highlight html %}
{% raw %}

<script type="text/javascript">
  amplitude.getInstance().logEvent('View Detail', {
    productId: {{상품 번호}},
    name: {{상품 이름}},
    price: {{상품 가격}}
  });
</script>

{% endraw %}
{% endhighlight %}

위 코드는 아래의 이벤트 정보를 가진다.

<table>
  <thead>
    <th style="width:30%">이벤트</th>
    <th style="width:30%">속성</th>
    <th>속성 값</th>
  </thead>
  <tbody>
    <tr>
      <th rowspan="4">View Detail</th>
    </tr>
    <tr>    
      <td>productId</td>
      <td>상품 번호</td>
    </tr>
    <tr>
      <td>name</td>
      <td>상품 이름</td>
    </tr>
    <tr>
      <td>price</td>
      <td>상품 가격</td>
    </tr>
  </tbody>
</table>

이제 이 태그의 트리거를 만들자. 트리거 유형을 [맞춤 이벤트]로 선택하고 이벤트 이름을 `detail` 로 입력하고 저장한다.

![상품 상세 보기 트리거](/images/post/23/04.png)

트리거 지정이 완료되었으면 태그를 저장하자.

![상품 상세 보기 이벤트 태그](/images/post/23/05.png)

### 장바구니 추가 코드

이번에는 상품 상세 보기 화면에서 [장바구니]를 눌러 상품을 장바구니에 담는 액션을 이벤트로 기록할 차례다. 위에서 작업하던 고도몰 관리자의 디자인 메뉴에서 [상품상세화면]에 입력한 상품상세보기 코드 밑에 아래 코드를 넣고 저장한다.

{% highlight html %}
{% raw %}

<script type="text/javascript">
  var addToCart = function () {
    dataLayer.push({
      'event': 'addToCart',
      'goodsInfo': [{
        'id': '{goodsView.goodsNo}',
        'name': '{=goodsView['goodsNm']}',
        'price': '{=gd_isset(goodsView['goodsPrice'],0)}',
        'quantity': goodsTotalCnt
      }]
    });
  }

  $('#cartBtn').click(addToCart);
</script>

{% endraw %}
{% endhighlight %}

위 코드는 ID가 `cartBtn`인 장바구니 담기 버튼을 눌렀을 때 상품 정보가 담긴 `addToCart` 함수를 실행시킨다.(쇼핑몰 스킨마다 장바구니 담기 버튼의 형태와 상품을 담는 방식이 다를 수 있기 때문에 사용 중인 스킨에 알맞게 수정이 필요하다.)

## 장바구니 추가 이벤트 태그

장바구니에 추가할 때 작동하는 앰플리튜드 이벤트를 만들어보자. GTM에서 [태그] → [새로 만들기]를 눌러 새로운 태그를 만든다. 이름은 자유롭게 입력하고 태그 유형은 [맞춤 HTML]로 선택한다. 그리고 입력창에 아래 코드를 입력한다.

{% highlight html %}
{% raw %}

<script type="text/javascript">
  amplitude.getInstance().logEvent('Add To Cart', {
    productId: {{상품 번호}},
    name: {{상품 이름}},
    $quantity: {{상품 수량}},
    price: {{상품 가격}}
  });
</script>

{% endraw %}
{% endhighlight %}

위 코드는 다음과 같은 이벤트 정보를 갖는다.

<table>
  <thead>
    <th style="width:30%">이벤트</th>
    <th style="width:30%">속성</th>
    <th>속성 값</th>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5">Add To Cart</th>
    </tr>
    <tr>    
      <td>productId</td>
      <td>상품 번호</td>
    </tr>
    <tr>
      <td>name</td>
      <td>상품 이름</td>
    </tr>
    <tr>
      <td>$quantity</td>
      <td>상품 수량</td>
    </tr>
    <tr>
      <td>price</td>
      <td>상품 가격</td>
    </tr>
  </tbody>
</table>

이제 이 태그의 트리거를 만들자. 트리거 유형을 [맞춤 이벤트]로 선택하고 이벤트 이름을 `addToCart` 로 입력하고 저장한다.

![장바구니 추가 이벤트 트리거](/images/post/23/06.png)

트리거 지정 후 태그를 저장하자.

![장바구니 추가 이벤트 태그](/images/post/23/07.png)

<div id="3"></div>
## 3. 장바구니 보기 이벤트

여러 상품을 구매할 때에는 장바구니 페이지를 거치게 되는데 장바구니 페이지에 접속했을 때 어떤 상품이 담겨 있는지도 수집하도록 설정해보자.

### 장바구니 보기 페이지 삽입 코드

고도몰 디자인 메뉴에서 [주문] > [장바구니]를 눌러 장바구니 파일을 열자.(경로: order/cart.html) 편집창이 나오면 맨 밑에 아래 코드를 넣고 저장하자.

{% highlight html %}
{% raw %}

<script type="text/javascript">
  var goodsInfo = [
    <!--{ @ cartInfo }-->
      <!--{ @ .value_ }-->
        <!--{ @ ..value_ }-->
          {
            'id': '{=...goodsNo}',
            'name': '{=...goodsNm}',
            'quantity': {=...goodsCnt},
            'price': {=...price['goodsPrice']}
          },
        <!--{ / }-->
      <!--{ / }-->
    <!--{ / }-->
  ];

  dataLayer.push({
    'event': 'viewCart',
    'goodsInfo': goodsInfo
  });
</script>

{% endraw %}
{% endhighlight %}

이 코드에서 <!--{ @ cartInfo }--> 부분은 상품 정보를 반복하여 가져올 수 있도록 해주는 고도몰의 치환코드이다. 그리고 아래 dataLayer.push를 통해 장바구니 페이지에서 viewCart라는 맞춤 이벤트를 발동시키고 상품 상세 보기와 마찬가지로 goodsInfo에 상품 정보를 넣어 GTM에서 활용할 수 있게 한다.

### 장바구니 보기 이벤트 태그

필요한 상품 정보 변수는 위에서 이미 만들었으니 바로 앰플리튜드 이벤트를 만들자. [태그] → [새로 만들기]를 눌러 새로운 태그를 만든다. 이름은 자유롭게 입력하고 태그 유형은 [맞춤 HTML]로 선택한다. 그리고 입력창에 아래 코드를 입력한다.

{% highlight html %}
{% raw %}

<script type="text/javascript">
  amplitude.getInstance().logEvent('View Cart', {
    productId: {{상품 번호}},
    name: {{상품 이름}},
    $quantity: {{상품 수량}},
    price: {{상품 가격}}
  });
</script>

{% endraw %}
{% endhighlight %}

위 코드는 아래 이벤트 정보를 갖는다.

<table>
  <thead>
    <th style="width:30%">이벤트</th>
    <th style="width:30%">속성</th>
    <th>속성 값</th>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5">View Cart</th>
    </tr>
    <tr>    
      <td>productId</td>
      <td>상품 번호</td>
    </tr>
    <tr>
      <td>name</td>
      <td>상품 이름</td>
    </tr>
    <tr>
      <td>$quantity</td>
      <td>상품 수량</td>
    </tr>
    <tr>
      <td>price</td>
      <td>상품 가격</td>
    </tr>
  </tbody>
</table>

트리거는 트리거 유형을 [맞춤 이벤트]로 선택하고 이벤트 이름을 `viewCart`로 입력하고 저장하자.

![장바구니 보기 트리거](/images/post/23/08.png)

트리거 설정을 완료했으면 태그도 저장하자.

![장바구니 이벤트 태그](/images/post/23/09.png)

<div id="4"></div>
## 4. 주문서 작성 이벤트

이제 사용자가 배송이나 결제 등의 정보를 입력하는 주문서 작성 단계다. 이 단계의 설정은 장바구니 보기와 거의 똑같으니 빠르게 넘어가도록 하자.

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
          'id': '{=...goodsNo}',
          'name': '{=...goodsNm}',
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

그렇다. 맞춤 이벤트명인 `checkout` 빼고는 코드가 똑같다. 바로 이어서 이벤트 태그를 만들어보자.

### 주문서 작성 이벤트 태그

GTM의 [태그] → [새로 만들기]를 눌러 새로운 태그를 만든다. 이름은 자유롭게 입력하고 태그 유형은 [맞춤 HTML]로 선택한다. 그리고 입력창에 아래 코드를 입력한다.

{% highlight html %}
{% raw %}

<script type="text/javascript">
  amplitude.getInstance().logEvent('Checkout', {
    productId: {{상품 번호}},
    name: {{상품 이름}},
    $quantity: {{상품 수량}},
    price: {{상품 가격}}
  });
</script>

{% endraw %}
{% endhighlight %}

이 코드는 아래 이벤트 정보를 가진다.

<table>
  <thead>
    <th style="width:30%">이벤트</th>
    <th style="width:30%">속성</th>
    <th>속성 값</th>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5">Checkout</th>
    </tr>
    <tr>    
      <td>productId</td>
      <td>상품 번호</td>
    </tr>
    <tr>
      <td>name</td>
      <td>상품 이름</td>
    </tr>
    <tr>
      <td>$quantity</td>
      <td>상품 수량</td>
    </tr>
    <tr>
      <td>price</td>
      <td>상품 가격</td>
    </tr>
  </tbody>
</table>

트리거는 트리거 유형을 [맞춤 이벤트]로 선택하고 이벤트 이름을 `checkout` 으로 입력하고 저장하자. 그리고 태그도 저장하자.

![주문서 작성 트리거](/images/post/23/10.png)

![주문서 작성 이벤트 태그](/images/post/23/11.png)

<div id="5"></div>
## 5. 주문 완료 이벤트

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
      'id': '{=.goodsNo}',
      'name': '{=.goodsNm}',
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

주문 정보라는 새로운 정보가 등장했기 때문에 GTM에서도 변수를 추가해주어야 한다. GTM에서 [변수] → [새로 만들기]를 눌러 변수명은 `주문 정보`로 하고 변수 유형을 [데이터 영역 변수]로 선택한다. 그리고 아래 데이터 영역 변수 입력창에 `txInfo` 를 입력하고 저장한다.

![주문 정보 GTM 변수](/images/post/23/12.png)

### 주문 완료 이벤트 태그

드디어 마지막 과정이다. GTM의 [태그] → [새로 만들기]를 눌러 새로운 태그를 만든다. 이름은 자유롭게 입력하고 태그 유형은 [맞춤 HTML]로 선택한다. 그리고 입력창에 아래 코드를 입력한다.

{% highlight html %}
{% raw %}

<script type="text/javascript">
  var txInfo = {{주문 정보}};
  amplitude.getInstance().logEvent('Purchase', {
    transactionId: txInfo.transactionId,
    $revenue: txInfo.totalRevenue,
    revenueType: 'income',
    shippingFee: txInfo.shippingFee,
    firstOrder: txInfo.firstOrder,
    productId: {{상품 번호}},
    name: {{상품 이름}},
    $quantity: {{상품 수량}},
    price: {{상품 가격}}
  });
</script>

{% endraw %}
{% endhighlight %}

위 코드는 다음 이벤트 정보를 가지고 있다.

<table>
  <thead>
    <th style="width:30%">이벤트</th>
    <th style="width:30%">속성</th>
    <th>속성 값</th>
  </thead>
  <tbody>
    <tr>
      <th rowspan="10">Purchase</th>
    </tr>
    <tr>    
      <td>transactionId</td>
      <td>주문 번호</td>
    </tr>
    <tr>    
      <td>$revenue</td>
      <td>결제 금액</td>
    </tr>
    <tr>    
      <td>revenueType</td>
      <td>income</td>
    </tr>
    <tr>    
      <td>shippingFee</td>
      <td>배송비</td>
    </tr>
    <tr>    
      <td>firstOrder</td>
      <td>첫 구매 여부(y/n)</td>
    </tr>
    <tr>    
      <td>productId</td>
      <td>상품 번호</td>
    </tr>
    <tr>
      <td>name</td>
      <td>상품 이름</td>
    </tr>
    <tr>
      <td>$quantity</td>
      <td>상품 수량</td>
    </tr>
    <tr>
      <td>price</td>
      <td>상품 가격</td>
    </tr>
  </tbody>
</table>

마지막 트리거도 만들어보자. 트리거 유형을 [맞춤 이벤트]로 선택하고 이벤트 이름을 `purchase` 으로 입력하고 저장하고 태그도 저장하자.

![주문 완료 트리거](/images/post/23/13.png)

![주문 완료 이벤트 태그](/images/post/23/14.png)

앰플리튜드 구매 이벤트의 이벤트 속성의 이름은 아래 가이드를 참고해서 적용했다.

> <a onclick="window.open('https://help.amplitude.com/hc/en-us/articles/115003116888-Tracking-Revenue', '_blank');">Tracking Revenue - Amplitude</a>

그리고 앰플리튜드에서 제공하는 가이드를 보면 많은 정보가 있으니 <a onclick="window.open('https://developers.amplitude.com/docs', '_blank');">Developer Docs</a>를 꼭 읽어보도록 하자.

### 앰플리튜드 크롬 확장 프로그램

페이스북 픽셀 헬퍼와 같이 웹사이트에서 실행되는 앰플리튜드를 디버깅하기 위한 도구로 Amplitude Instrumentation Explorer가 있는데 크롬 웹스토어에서 설치가 가능하니 꼭 사용하도록 하자.

> <a onclick="window.open('https://chrome.google.com/webstore/detail/amplitude-instrumentation/acehfjhnmhbmgkedjmjlobpgdicnhkbp', '_blank');">Amplitude Instrumentation Explorer 크롬 웹스토어 바로가기</a>

## 마치며

이번 글에서는 상품 보기부터 주문 완료까지 구매 여정에 필요한 기본적인 이벤트를 함께 설정해봤다. 이 외에도 우리 서비스를 이용하는 사용자의 행동을 수집하기 위해 만들 수 있는 이벤트가 많이 있을 것이다. 어느 페이지를 방문했는지도 중요하지만 그 페이지 안에서 어떤 행동을 했는지도 굉장히 중요하기 때문에 이번 기회에 우리 서비스의 이벤트 구조를 어떻게 짜면 좋을지 생각해볼 수 있는 기회가 됐으면 한다.
