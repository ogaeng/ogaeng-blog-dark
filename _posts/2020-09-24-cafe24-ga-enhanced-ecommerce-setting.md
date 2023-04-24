---
layout: post
title: '구글 태그 매니저로 카페24 쇼핑몰에 GA 향상된 전자상거래 설정하기'
date: 2020-09-24 17:00:00 +09:00
author: "Ogaeng"
permalink: /cafe24-gtm-enhanced-ecommerce-setting/
content_id: 22
image: /images/thumbnail/cafe24-gtm-enhanced-ecommerce-setting.jpg
categories: [카페24,GA,GTM]
tags:
  - 카페24
  - 전자상거래
  - GA
  - GTM
description: 카페24 쇼핑몰에 구글 태그 매니저를 이용해 구글애널리틱스 향상된 전자상거래 설정을 해보자.
---
우리나라에서 가장 많이 사용되는 임대형 쇼핑몰에는 카페24가 있다. 그런데 임대몰 사용자는 개발 인력이 부족한 경우가 많아 마케터는 GA를 통한 분석을 더 자세하게 하고 싶어도 설정이 쉽지 않아 포기하곤 한다.

물론 카페24 앱스토어 등을 통해 전자상거래 설정을 제공하는 서비스가 있지만 단순히 전자상거래 설정만이 아닌 더 다양한 설정을 GTM으로 하고 싶을 때에는 사용하기가 쉽지 않다.

그래서 이번 글에서는 GA와 GTM을 조금이라도 다루는 마케터를 위해 카페24에서 쇼핑몰 내부 코드 수정 없이 GTM만을 이용해 간단하게 향상된 전자상거래 세팅하는 방법을 이야기하려고 한다. 막상 모두 완료하고 나면 적은 양이지만 설명을 위해 내용이 길어질 수 있어 필요한 부분만 눌러 이동할 수 있도록 목차를 준비했다.

> 카페24 쇼핑몰의 GA4 전자상거래 이벤트 태깅에 대해 궁금하다면 [여기를 눌러](https://osoma.kr/blog/gtm-template-cafe24-ga4/){:target="_blank"} 오소마 웹사이트로 이동하길 바란다.

## 목차

- <a onclick="window.open('#1', '_self');">카페24에 구글 태그 매니저 설치하기</a>
- <a onclick="window.open('#2', '_self');">상품 상세 보기</a>
- <a onclick="window.open('#3', '_self');">장바구니 담기</a>
- <a onclick="window.open('#4', '_self');">주문서 작성</a>
- <a onclick="window.open('#5', '_self');">주문 완료</a>
- <a onclick="window.open('#6', '_self');">User ID 및 추적 코드 설정</a>
- <a onclick="window.open('#7', '_self');">전자상거래 이벤트 태그</a>

<div id="1"></div>
## 1. 카페24에 구글 태그 매니저 설치하기

먼저 GTM에서 새로운 컨테이너를 만들고 아래와 같이 GTM 스니펫 설치 코드가 등장하면 브라우저의 새 탭을 열고 카페24 쇼핑몰 관리자로 접속한다.

![GTM 스니펫](/images/post/22/01-gtm-cafe24.png)

관리자에서 상점관리 → 운영관리 → 검색엔진최적화(SEO)로 접속한 후 [고급 설정] 탭을 누른다.

![카페24에 GTM 넣기](/images/post/22/02-cafe24-admin.png)

HTML 태그 직접 입력에서 PC 쇼핑몰과 모바일 쇼핑몰에 있는 [Head 태그 소스], [Body 태그 소스]에 각각 알맞은 GTM 스니펫 코드를 입력하고 하단에 [저장]을 누른다.

다시 GTM으로 돌아와서 GA 페이지뷰 태그를 만든다.(GA 태그를 만드는 방법은 생략하겠다. 만약 GA 태그 만드는 방법을 모른다면 이 글은 오늘은 그만 보고 다음에 다시 보러 오자)

### GA 향상된 전자상거래 설정

향상된 전자상거래를 사용하려면 전자상거래 기능을 작동시켜야 한다. GA에서 [관리] → [보기] → [전자상거래 설정]에서 [전자상거래 사용]을 설정으로 변경하고 아래에 등장하는 [향상된 전자상거래 보고서 사용 설정]도 설정으로 변경한다. 그리고 저장한다.

![GA 전자상거래 설정](/images/post/22/03-ga-enhanced-ecommerce.png)

이제 기초적인 준비는 끝났으니 본격적으로 설정을 시작해보자.

<div id="2"></div>
## 2. 상품 상세 보기

쇼핑몰 방문자가 가장 많이 방문하고 오래 머무는 페이지가 바로 상품 상세 보기 페이지이다. 이제 여기서 상품 정보를 담고 있는 변수를 찾아 GTM으로 가져오면 된다. 가장 먼저 할 일은 상품 상세 보기 페이지에서 실행되는 어떤 변수에 우리가 원하는 값이 들어있는지 찾아보는 것인데, 찾는 과정은 생략하고 어떤 변수에 값이 들어있는지 나열하면 아래와 같다.

- 상품 번호: iProductNo
- 상품명: product_name
- 판매가: product_sale_price
- 카테고리 번호: iCategoryNo

```현재 운영 중인 쇼핑몰 스킨에 따라 사용하는 변수는 달라질 수 있다.```

### 상품 정보 변수 만들기

이제 이 상품 정보를 가져와 GTM에서 변수를 만들어보자. GTM에서 [변수] → [새로 만들기]를 눌러 변수명은 `상품 상세 보기 - 상품 배열`로 하고 변수 유형을 [맞춤 자바스크립트]로 선택한다. 그리고 아래 입력창에 아래의 스크립트를 넣고 저장한다.

{% highlight javascript %}
{% raw %}

function() {
  var productInfo = [{
    'id': iProductNo,
    'name': product_name,
    'price': product_sale_price,
    'category': iCategoryNo
  }];
  return productInfo;
}

{% endraw %}
{% endhighlight %}

![GTM 상세보기 변수](/images/post/22/04-gtm-var-detail.png)

### 전자상거래 데이터 영역 태그 만들기

[태그] → [새로 만들기]를 눌러 새로운 태그를 만든다. 이름은 자유롭게 입력하고 태그 유형은 [맞춤 HTML]로 선택한다. 그리고 입력창에 아래 코드를 입력한다.

{% highlight html %}
{% raw %}

<script>
  dataLayer.push({
    'event': 'detail',
    'ecommerce': {
      'detail': {
        'actionField': {'step': 1},
        'products': {{상품 상세 보기 - 상품 배열}}
      }
    }
  });
</script>

{% endraw %}
{% endhighlight %}

위 코드는 GTM에서 사용하는 변수인 dataLayer에 상품 상세 보기 액션이 실행되었다는 정보를 보내주는 코드로 `'event': 'detail'` 부분은 `detail`이라는 이름을 가진 맞춤 이벤트를 실행시키는 코드이다. 이 코드가 실행되면 detail 맞춤 이벤트를 트리거로 하여 새로운 태그를 실행할 수 있다. 아래 `ecommerce`가 전자상거래를 뜻하고 그 아래에 있는 `detail`은 상품 상세 보기 액션을 말한다. 그리고 `'products': {상품 상세 보기 - 상품 배열}` 은 어떤 상품을 봤는지 보내주는 코드인데 `상품 상세 보기 - 상품 배열` 가 위에서 만든 상품 정보 변수이다.(위에서 GTM 변수를 만들 때 이름을 다르게 했다면 `{`를 두 번 타이핑하면 변수 목록이 나오는데 거기서 선택하면 된다.)

이제 트리거를 만들 차례인데 카페24의 상품 상세 보기 페이지는 다음과 같은 주소 형태를 가지고 있다.

`https://호스트네임/product/detail.html?product_no=상품번호...`

`https://호스트네임/product/제품명/상품번호/category/카테고리번호...`

바로 위 주소를 가진 페이지에서 DOM 준비가 되는 시점으로하는 트리거를 아래와 같이 만들고 태그에 등록하면 된다.(쇼핑몰의 상품 상세 보기 페이지의 구조가 위 내용과 다르다면 자신의 쇼핑몰 구조에 맞게 조건을 생성하면 된다.)

아래 예제에서는 `Page Path`를 `정규표현식과 일치` 조건으로하여 다음과 같이 설정했다.

`/product/detail.html|/product/.*/category/.*/display/.*`

![상세 트리거](/images/post/22/05-gtm-trg-dom-detail-n.png)

이렇게 설정하면 태그는 다음 이미지처럼 설정이 된다.

![상세 태그](/images/post/22/06-gtm-tag-dom-detail.png)

### 이벤트 트리거 만들기

이제 상품 상세 보기 이벤트를 실행할 수 있도록 트리거를 만들 차례다.

트리거 메뉴에서 [새로 만들기]를 눌러 새 트리거를 만들자. 새 트리거는 유형을 [맞춤 이벤트]로 선택하고 이벤트 이름을 `detail` 이라고 입력하고 저장한다.

![이벤트 트리거](/images/post/22/07-gtm-trg-event-detail.png)

<div id="3"></div>

## 3. 장바구니 담기

이제 상품을 담는 액션을 추적하기 위한 장바구니 담기를 설정할 차례다. 장바구니 담기의 경우 상황에 따라 설정 방법이 다양하게 바뀌는데 이번에는 상품 상세페이지에서 장바구니 버튼을 누를 때 추적하는 방식으로 함께 설정해보자.(그냥 따라하기보다는 우리 서비스에 맞는 방법을 찾아야 한다.)

### 트리거 만들기

먼저 트리거를 만들어야 하는데 장바구니 담기 버튼을 클릭하는 시점을 트리거로 설정하려고 한다.

장바구니 버튼의 HTML 속성으로 ID가 있다면 ID를 사용하는게 가장 쉬운 방법이다. 이번에는 `actionCart`라는 ID가 있다고 가정하고 ID 정보를 이용해 트리거를 만들어보자.

트리거를 만들기 전에 변수 메뉴로 들어가 [구성]을 누르고 `Click ID`에 체크한다.

![장바구니 변수](/images/post/22/09-gtm-var-cart-n.png)

이제 트리거 메뉴로 가서 [새로 만들기]를 누르고 유형을 [모든 요소]로 선택하자. 그리고 일부 클릭에 체크를 한 후 변수는 Click ID, 조건은 같음 혹은 포함을 선택하고 값에는 장바구니 버튼의 ID를 입력하고 저장한다.

![장바구니 트리거](/images/post/22/10-gtm-trg-dom-cart-n.png)

### 전자상거래 태그 만들기

[태그] → [새로 만들기]를 눌러 새로운 태그를 만든다. 이름은 자유롭게 입력하고 태그 유형은 [맞춤 HTML]로 선택한다. 그리고 입력창에 아래 코드를 입력한다.

{% highlight html %}
{% raw %}

<script>
  dataLayer.push({
  'event': 'addToCart',
    'ecommerce': {
      'currencyCode': 'KRW',
      'add': {
        'actionField': {'step': 2},
        'products': {{상품 상세 보기 - 상품 배열}}
      }
    }
  });
</script>

{% endraw %}
{% endhighlight %}

이 태그의 내용은 `addToCart`라고 하는 맞춤 이벤트를 실행시키고 `ecommerce`의 `add`를 통해 장바구니에 상품이 담겼다는 액션을 쏜다. `products`에는 장바구니에 담긴 상품의 정보가 담긴다.

상품 상세페이지에서 액션이 일어나기 때문에 상품 정보는 상품 상세 보기에서 쓰인 변수를 그대로 가져왔다.

이 태그의 트리거는 위에서 설정한 장바구니 담기 클릭 트리거로 설정해주면 된다.

![장바구니 태그](/images/post/22/11-gtm-tag-dom-cart-n.png)

### 이벤트 트리거 만들기

이제 이벤트에서 사용할 트리거를 만들기 위해 트리거를 만들어야 한다. 새 트리거는 유형을 [맞춤 이벤트]로 선택하고 이벤트 이름을 `addToCart`라고 입력하고 저장한다.

![장바구니 이벤트 트리거](/images/post/22/12-gtm-trg-event-cart-n.png)

<div id="4"></div>

## 4. 주문서 작성

사용자가 배송 정보나 결제 정보를 입력하는 주문서 작성 단계를 작업 할 차례다. 주문서 작성 단계 페이지에서는 주문하고자 하는 상품 정보를 담고 있는 `aBasketProductOrderData` 변수가 있다. 이 변수에도 장바구니처럼 배열 형태로 상품의 정보가 담겨있는데 그중 필요한 정보를 GTM 변수로 만들어보자.

### 주문서 작성 단계의 상품 변수 만들기

[변수] → [새로 만들기]를 눌러 변수명은 `주문서 작성 - 상품 배열`로 하고 변수 유형을 [맞춤 자바스크립트]로 선택한다. 그리고 아래 입력창에 아래의 스크립트를 넣고 저장한다.

{% highlight javascript %}
{% raw %}

function() {
  var source = aBasketProductOrderData;
  var productInfo = [];
  for(var i = 0 ; i < source.length ; i++) {
      var productId = source[i].product_no;
      var productPrice = source[i].product_sale_price;
      var productQty = source[i].quantity;
      var productCate = source[i].main_cate_no;

      productInfo.push({
        'id': productId,
        'price': productPrice,
        'quantity': productQty,
        'category': productCate
      });
  }
  return productInfo;
}

{% endraw %}
{% endhighlight %}

이 변수에는 주문서 작성 단계에 있는 상품의 상품 번호, 판매가, 수량, 옵션, 카테고리 번호가 담긴다.(이상하게도 변수 내에 상품명은 없었다.)

![주문서 작성 변수](/images/post/22/14.png)

### 전자상거래 태그 만들기

[태그] → [새로 만들기]를 눌러 새로운 태그를 만든다. 이름은 자유롭게 입력하고 태그 유형은 [맞춤 HTML]로 선택한다. 그리고 입력창에 아래 코드를 입력한다.

{% highlight html %}
{% raw %}

<script>
  dataLayer.push({
    'event': 'checkout',
    'ecommerce': {
      'checkout': {
        'actionField': {'step': 3},
        'products': {{주문서 작성 - 상품 배열}}
      }
    }
  });
</script>

{% endraw %}
{% endhighlight %}

이 태그의 내용은 `checkout`라고 하는 맞춤 이벤트를 실행시키고 `ecommerce`의 `checkout`를 통해 주문서 작성 단계에 왔다는 액션을 쏜다. `products`에는 위에서 만든 `주문서 작성 - 상품 배열`변수를 값으로 넣는다.

이제 트리거를 만들 차례인데 주문서 작성 페이지는 다음과 같은 주소 형태를 가지고 있다.

> /order/orderform.html

바로 위 주소를 가진 페이지에서 DOM 준비가 되는 시점으로하는 트리거를 아래와 같이 만들고 태그에 등록하자.

![주문서 작성 트리거](/images/post/22/15.png)

이렇게 설정하면 태그는 다음과 같다.

![주문서 작성 태그](/images/post/22/16.png)

### 이벤트 트리거 만들기

이제 주문서 작성 이벤트가 실행될 트리거를 만들자. 새 트리거를 만들고 유형을 [맞춤 이벤트]로 선택한 뒤 이벤트 이름을 `checkout` 이라고 입력하고 저장한다.

![주문서 작성 트리거](/images/post/22/17.png)

계속 비슷한 작업을 반복해서 이제는 쉽다고 느낄 것이다.

<div id="5"></div>

## 5. 주문 완료

이제 대망의 주문 완료 단계다. 카페24는 주문을 완료했을 때 등장하는 완료 페이지가 있는데 그 페이지에 주문 정보를 담고 있는 `EC_FRONT_EXTERNAL_SCRIPT_VARIABLE_DATA` 변수가 있다 이 변수 안에 `order_product` 를 보면 주문 정보와 주문한 상품의 정보를 담고 있다. 이 정보를 이용해보자

### 주문 번호 변수 만들기

[변수] → [새로 만들기]를 눌러 변수명은 `주문 완료 - 주문 번호`로 하고 변수 유형을 [맞춤 자바스크립트]로 선택한다. 그리고 아래 입력창에 아래의 스크립트를 넣고 저장한다.

{% highlight javascript %}
{% raw %}

function() {
  var source = EC_FRONT_EXTERNAL_SCRIPT_VARIABLE_DATA;
  var orderId = source.order_id;
	return orderId;
}

{% endraw %}
{% endhighlight %}

![주문번호](/images/post/22/19.png)

### 결제 금액 변수 만들기

변수명을`주문 완료 - 결제 금액`으로 하고 변수 유형을 [맞춤 자바스크립트]로 선택한다. 그리고 아래 입력창에 아래의 스크립트를 넣고 저장한다.

{% highlight javascript %}
{% raw %}

function() {
  var source = EC_FRONT_EXTERNAL_SCRIPT_VARIABLE_DATA;
  var revenue = source.payed_amount;
	return revenue;
}

{% endraw %}
{% endhighlight %}

![결제 금액](/images/post/22/20.png)

### 배송비 변수 만들기

변수명을`주문 완료 - 배송비`로 하고 변수 유형을 [맞춤 자바스크립트]로 선택한다. 그리고 아래 입력창에 아래의 스크립트를 넣고 저장한다.

{% highlight javascript %}
{% raw %}

function() {
  var source = EC_FRONT_EXTERNAL_SCRIPT_VARIABLE_DATA;
  var shippingFee = source.total_basic_ship_fee;
	return shippingFee;
}

{% endraw %}
{% endhighlight %}

![배송비](/images/post/22/21.png)

### 주문한 상품 정보 변수 만들기

변수명을`주문 완료 - 상품 배열`로 하고 변수 유형을 [맞춤 자바스크립트]로 선택한다. 그리고 아래 입력창에 아래의 스크립트를 넣고 저장한다.

{% highlight javascript %}
{% raw %}

function() {
  var source = EC_FRONT_EXTERNAL_SCRIPT_VARIABLE_DATA.order_product;
  var productInfo = [];
  for(var i = 0 ; i < source.length ; i++) {
      var productId = source[i].product_no;
      var productName = source[i].product_name;
      var productPrice = source[i].product_price;
      var productQty = source[i].quantity;
      var productCate = source[i].category_no_2;

      productInfo.push({
        'id': productId,
        'name': productName,
        'price': productPrice,
        'category': productCate,
        'quantity': productQty
      });
  }
  return productInfo;
}

{% endraw %}
{% endhighlight %}

이 변수에는 주문 완료한 상품의 상품 번호, 판매가, 수량, 카테고리 번호가 담긴다.

![상품 정보](/images/post/22/22.png)

### 전자상거래 태그 만들기

[태그] → [새로 만들기]를 눌러 새로운 태그를 만든다. 이름은 자유롭게 입력하고 태그 유형은 [맞춤 HTML]로 선택한다. 그리고 입력창에 아래 코드를 입력한다.

{% highlight html %}
{% raw %}

<script>
  dataLayer.push({
    'event': 'purchase',
    'ecommerce': {
      'purchase': {
        'actionField': {
          'id': {{주문 완료 - 주문 번호}},
          'revenue': {{주문 완료 - 결제 금액}},
          'shipping': {{주문 완료 - 배송비}},
        },
        'products': {{주문 완료 - 상품 배열}}
      }
    }
  });
</script>

{% endraw %}
{% endhighlight %}

이 태그의 내용은 `purchase`라고 하는 맞춤 이벤트를 실행시키고 `ecommerce`의 `purchase`를 통해 주문 완료했다는 액션을 쏜다. `actionField`의 `id`, `revenue`, `shipping`에는 위에서 만든 주문 번호, 결제 금액, 배송비 정보가 담긴 변수를 값으로 넣는다. `products`에는 위에서 만든 `주문 완료 - 상품 배열`변수를 값으로 넣는다.

이제 트리거를 만들 차례인데 주문 완료 페이지는 다음과 같은 주소 형태를 가지고 있다.

> /order/order_result.html

바로 위 주소를 가진 페이지에서 DOM 준비가 되는 시점으로하는 트리거를 아래와 같이 만들고 태그에 등록하자.

![DOM 트리거](/images/post/22/23.png)

이렇게 설정한 태그는 다음과 같다.

![주문완료 태그](/images/post/22/24.png)

### 이벤트 트리거 만들기

이제 트리거에서 주문 완료 이벤트 트리거를 만들자. 새 트리거는 유형을 [맞춤 이벤트]로 선택하고 이벤트 이름을 `purchase` 라고 입력하고 저장한다.

![주문완료 트리거](/images/post/22/25.png)

<div id="6"></div>

## 6. User ID 및 추적 코드 설정

User ID 설정을 위해 가장먼저 해야할 일은 User ID 기능을 작동시키는 것이다. GA에서 [관리] → [속성] → [추적 정보] → [User-ID]를 누르고 모든 항목에 [설정]으로 체크한다음 [만들기]를 눌러 User ID 보기를 만든다.

![User ID 설정](/images/post/22/27.png)

고맙게도 카페24는 로그인한 사용자의 ID를 암호화하여 EC_FRONT_EXTERNAL_SCRIPT_VARIABLE_DATA 변수 안에 값을 가지고 있다. 이 정보를 이용하기 위해 GTM으로 돌아와서 [변수] → [새로 만들기]를 눌러 변수명은 User ID로 하고 변수 유형을 [맞춤 자바스크립트]로 선택한다. 그리고 아래 입력창에 아래의 스크립트를 넣고 저장한다.

{% highlight javascript %}
{% raw %}

function(){
  var uid = EC_FRONT_EXTERNAL_SCRIPT_VARIABLE_DATA.common_member_id_crypt;
  return uid;
}

{% endraw %}
{% endhighlight %}

![User ID 변수](/images/post/22/28.png)

이미 만들어둔 GA 추적 ID 변수를 수정하기 위해 변수를 눌러 들어간다. 하단에 [기타 설정]을 누르고 [설정할 필드]에 [+ 입력란]을 누른 후 [입력란 이름]을 userId로 [값]은 오른쪽 +블록 버튼을 눌러 위에서 만든 User ID 변수를 선택하고 저장하자.

추가로 앞서 만든 전자상거래 태그의 정보를 사용하기 위해 [전자상거래]에 [향상된 전자상거래 기능 사용]에 체크하고 [데이터 영역 사용]에 체크를 해준다.

![User ID 추적 코드](/images/post/22/29.png)

이 작업을 통해 User ID 변수에 기록된 암호화 된 사용자 ID 값을 GA에 입히게 된다.

<div id="7"></div>

## 7. 전자상거래 이벤트 태그 설정

마지막으로 앞서 만든 전자상거래 이벤트 트리거를 이용해 상품 상세 보기부터 주문 완료까지의 GA 이벤트를 만들 차례다.

이번에는 여러개의 태그를 만들지 않고 조금 더 효율적인 태그 구성을 위해 [참고표] 변수를 활용해보자.

### 참고표 만들기

동적으로 이벤트의 이름을 활용하기 위해 참고표를 이용해보자. 먼저 변수 메뉴로 들어가 [새로 만들기]를 눌러 새 변수를 만들고 변수의 이름은 `참고표 - 전자상거래 이벤트 액션`으로 하고 변수 유형을 [참고표]로 선택한다.

그리고 입력 변수를 {% raw %}`{{Event}}`{% endraw %}로 선택한다. 그리고 참고표에는 다음 이미지를 참고해 입력한다.

입력 | 출력
detail | 상품 상세 보기
addToCart | 장바구니 담기
checkout | 주문서 작성
purchase | 주문 완료

![참고표](/images/post/22/30.png)

이 참고표는 실행되는 맞춤이벤트 이름에 따라 출력 값이 바뀌는 변수가 된다.(javascript의 switch문을 생각하면 된다.)

### GA 이벤트 태그 만들기

앞서 만든 전자상거래 맞춤 이벤트가 발동되는 시점에 실행되는 GA 이벤트 태그를 만들자. [태그] → [새로 만들기]를 눌러 새로운 태그를 만든다. 이름은 자유롭게 입력하고 태그 유형은 [Google 애널리틱스: 유니버설 애널리틱스]로 선택한다. 그리고 추적 유형은 [이벤트]로 한다.

- 카테고리: 전자상거래
- 작업: {% raw %}{{참고표 - 전자상거래 이벤트 액션}}{% endraw %}
- 비 상호작용 조회: 참
- Google 애널리틱스 설정: 내 GA 추적 ID 변수 선택

그리고 트리거는 위에서 만들었던 `상품 상세 보기`, `장바구니 담기`, `주문서 작성`, `주문 완료` 이벤트를 모두 선택하고 저장한다.

![전자상거래 태그](/images/post/22/31.png)

이렇게 하면 태그는 하나지만 상황에 맞게 작업(액션)에 알맞는 값이 들어가게 되어 태그 4개와 동일한 역할을 한다.

모든 작업이 끝났다면 [미리보기]를 이용해 테스트를 한 후 정상적으로 작동한다면 [제출]하여 마무리하자.

## 마치며

여기에서 만든 변수를 응용하면 여러 광고 스크립트는 물론 앰플리튜드와 같은 다른 분석 도구의 설치도 쉽게 할 수 있으니 이 글을 통해 GTM 변수를 공부하는데 도움이 되었으면 좋겠다.

마지막으로 이 글에서 다루지 않은 액션이 많은데 위 내용을 토대로 향상된 전자상거래의 다른 액션도 어떻게 하면 설정할 수 있을지 스스로 연구해보길 바란다.

> 카페24 쇼핑몰 GA4 세팅 컨설팅이 필요하다면 [오소마 웹사이트](https://osoma.kr/data-consulting/)를 방문하세요.