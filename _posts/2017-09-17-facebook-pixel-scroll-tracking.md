---
content_id: 6
layout: post
title: 페이스북 픽셀로 스크롤 측정하고 맞춤타겟 만들기
date: 2017-09-17 01:00:00 +09:00
author: "Ogaeng"
permalink: /facebook-pixel-scroll-tracking/
image: /images/thumbnail/facebook-pixel-scroll-tracking_thumbnail.png
categories:
  - 마케팅
  - Facebook
tags:
  - 페이스북픽셀
  - 스크롤
  - GTM
  - facebookanalytics
description: 언론사나 블로그 등 글로 작성된 콘텐츠 위주의 웹사이트에서는 글을 어디까지 읽었는지가 꽤 중요한데, 많은 분이 Google Tag Manager를 이용해 Google Analytics 이벤트로 페이지 스크롤 깊이를 측정하고 있다. 이 때 Google Analytics의 세그먼트를 만들어서 스크롤 깊이를 이용한 광고를 Adwords에서 할 수 있지만 뭐니뭐니해도 광고는 역시 페이스북이 아닌가? 그래서 이번 글에선 페이스북 픽셀을 활용해 스크롤 깊이를 측정하고 그것을 이용해 맞춤타겟을 만드는 과정을 이야기해보려고 한다.
---

언론사나 블로그 등 글로 작성된 콘텐츠 위주의 웹사이트에서는 글을 어디까지 읽었는지가 꽤 중요한데, 많은 분이 Google Tag Manager를 이용해 Google Analytics 이벤트로 페이지 스크롤 깊이를 측정하고 있다. 이 때 Google Analytics 안에서 세그먼트를 만들어 스크롤 깊이를 이용한 광고를 Adwords에서 할 수 있지만 뭐니뭐니해도 광고는 역시 페이스북이 아닌가? 그래서 이번 글에선 페이스북 픽셀을 활용해 스크롤 깊이를 측정하고 그것을 이용해 맞춤타겟을 만드는 과정을 이야기해보려고 한다.

> 참고: [Tracking Scroll Depth with Google Tag Manager](https://www.advancedwebranking.com/blog/how-to-track-scroll-depth-with-google-tag-manager/) (Google Analytics 스크롤 측정 이벤트 만들기)

~~~
[업데이트] 2017.10.19 - GTM의 스크롤 트리거 지원으로 글 내용 수정
~~~



## 이 글에서 다루는 도구 ##

* Google Tag Manager
* Facebook Pixel
* Facebook Analytics


## 구글 태그 매니저를 이용하여 페이스북 픽셀 이벤트 설정하기 ##

먼저 Google Tag Manger(줄여서 GTM이라고 하겠다.)를 이용해 스크롤을 측정하고 이벤트를 내보내는 Facebook Pixel을 설정해야 한다.

### 1. 스크롤 트리거 만들기 ###

첫번째로 스크롤 깊이로 트리거를 만들어야 한다. 최근 GTM에 동영상 행동과 함께 추가된 스크롤 깊이 유형을 사용하면 쉽다. GTM에 접속한 후 [트리거] → [새로만들기]를 선택하고 [트리거 구성]에서 [스크롤 깊이]를 선택한다. 트리거 이름은 쉽게 구분할 수 있는 이름을 설정하자. 이 글에서는<code>ScrollDepth</code>로 설정했다.

스크롤 깊이로 트리거 유형을 선택하면 어떤 스크롤 깊이를 측정할지 체크하는 부분이 생겨난다. 세로와 가로 스크롤을 측정할 수 있는데, 이번에는 세로 스크롤 측정을 기준으로 설명하겠다. [세로 스크롤 깊이]에 체크하고 [비율]을 선택, 측정하고자 하는 기준을 콤마(,)로 구분하여 입력하면 된다. 여기서는 <code>25,50,75,100</code>으로 설정했다. 이제 [저장]을 누르자.

![트리거-ScrollDepth](https://lh3.googleusercontent.com/g5TnX-6RGKXfZxPI187asOvQOC1WZLrRZgGfLgPxylMu_X3tnEp4oMwqZ1W1Ji3f_wCcat6jnZ44ClJrb3m2ywM0ZM03VsuZudoPd5yY74gh_oe_kwfWEtoU4OGwaopJkkkhiEg_xDTd6WSrFCbi8CVmXbj2VUzUsx0Fury24aznrC3qF5b8H1OvpZ--OaHIwsYEBrvbJypLX1KKbSuvhVQRbKcd89B9ExVdR_6Fmb7f9dnXTdmVxy2D4UO76XcE95uc1gVq7-fWtu8tSLl_nRa5MyVN45_AcbNe8f4CbRdZk3YNAizarLocp2IT-Z18Lp6HufqZm6sfN6v0ZZAhGSkPninMZ4xEOtGOjjAYemNkcAxB6oXWmIUk0EC-Cop8s63XaQPTBMSGa3yTJeVbE4shuHcQQPrUngqt4HfXhN7zGH0nHiz_FTxkPpJFPyfHguaoRxFWH-d59YeKRZl0gAvPXeK4oolqLnmOvvZQDzoulcty-FNWPrN2U7BAj2kxwfsVRJ0o8eV-fKOpCxs8aUE0Y7AFhcgXowH3nzW7G3NZRDiV2t_cM4BTmACSqZphZsqefjBxYJPH6L5Nhofu5a1Pjs8d____j5E4aBd9kMr_4pe2NeG23nqTpHPtleJH9vP6mgSKDedBKsmFZ0CCKXd9PlQuJXcFdh4=w1647-h1052-no)

### 2. 스크롤 관련 변수 만들기 ###

이번에는 위에서 만든 스크롤 트리거가 실행될 때마다 25%, 50%, 75%, 100% 이런 식으로 측정된 값을 GTM에서 저장하는 변수를 만들 차례다.

[변수] → [구성]을 선택하고 스크롤을 내려보면 [스크롤] 항목이 나온다. 이 변수들을 모두 체크하자. 각 변수의 설명은 아래와 같다.

* Scroll Depth Threshold: 스크롤 깊이(25, 50, 75, 100)
* Scroll Depth Units: 표시 기준(픽셀, 퍼센트)
* Scroll Depth Direction: 스크롤 방향(세로, 가로)

![변수-스크롤](https://lh3.googleusercontent.com/G3vJKChbvK8LCZyWuQ3ZhY2KskVEaLeJJqwYOyjKmB_I780kKuV3YL3A3d7oNuXOv6HcnfHy77bBACvl2ObR9Y-i4ZtEZO1VrqV3lq5UthDa8u0mGeZR0_V7r7fThnE9mOTjXPmY8axiSl1S_c0PmsP39Vr0ofUTqtoReU8ZW-XqHSDPrC3hblSYksaOAzj0_cOSbkeUjk9KAog0NiCLz_75C6fvZsZLiYP2ASO5k94CQOWfelIaaswtq1-bA6zhLYTkzVG5BEdGnC1hogEzQz0sPWvVpPpHm21E8nl6J2CpIt_yOx0__yFjRxXwJpj6swojQp2vfjqQMGdqBgIpBtozpQwDauwx7v-eMyf83Ubm2q_L95gO_OAhZKoy6MjXfyV3aFpms7o04S_wf9R_5Zsh5tk5v2l9tvH0vsm1kFEUbBp4WpvuDi9Kf_0udtHiXDvEd59sPg3ARZZdudiPHI1ZumnmBxB8l161Y1BSxvkY_TI3dvyn1G3iifzpR-2oLZteRJLLJM5Dge_f_xRhxTrSWmLx-_EzZM-NbaD8rBI7Woq6VRO_VRLnqGMDTRWidbBrIT06IkP_auYqhB80HdED4NWzNbn3fX8aibg1YeLK1NgkFxDurbzSvs9AoKnxCno-rulaV0DdkAsC1vPmEKVXH0N5utGsLb4=w1261-h514-no)

### 3. 페이스북 픽셀 이벤트 태그 만들기 ###

이제 GTM 설정이 끝나간다. 마지막으로 페이스북 픽셀 이벤트 태그를 만들 차례다. 먼저 픽셀 기본 코드 태그를 만들자. [태그] → [새로만들기]를 선택하고 [태그 구성]에서 [맞춤 HTML]을 선택한다. 그리고 아래와 같은 형태의 픽셀 코드를 [페이스북 픽셀 페이지](https://www.facebook.com/ads/manager/pixel)로 가서 찾아 입력한다.

~~~ html
<!-- Facebook Pixel Code -->
<script>
!function(f,b,e,v,n,t,s){if(f.fbq)return;n=f.fbq=function(){n.callMethod?
n.callMethod.apply(n,arguments):n.queue.push(arguments)};if(!f._fbq)f._fbq=n;
n.push=n;n.loaded=!0;n.version='2.0';n.queue=[];t=b.createElement(e);t.async=!0;
t.src=v;s=b.getElementsByTagName(e)[0];s.parentNode.insertBefore(t,s)}(window,
document,'script','https://connect.facebook.net/en_US/fbevents.js');
fbq('init', '픽셀코드입력'); // Insert your pixel ID here.
fbq('track', 'PageView');
</script>
<noscript><img height="1" width="1" style="display:none"
src="https://www.facebook.com/tr?
  id=픽셀코드입력
  &ev=PageView&noscript=1"
/></noscript>
<!-- DO NOT MODIFY -->
<!-- End Facebook Pixel Code -->

<!-- 이 소스코드를 그대로 복사하여 붙여넣지 마세요! -->
~~~

그리고 아래의 [트리거]를 눌러 [All Pages]를 선택한 후 [저장]한다.

![태그-페이스북 픽셀 기본태그](https://lh3.googleusercontent.com/AICBHd60Hl6y6-27qHe4U3wVuIbzOurW27TSruD9zSxe4IF9FuK-MN41Lp6slc8NiSvAC1BTpQFqsS545PIFfL0j1XkXSmiw50Z_9y6_p6bif4Dxm3qlc0O3Sz-l_xglUVyrJprzB7MNAXQxcHAjkJABOID5xvhUEflXc2UV7OgKiKPWUfsEK_i4sfDXsEI79ZzqRVlsLmwaJe6BNwLNIGxXD5qMZ2NNK8662J2Pf9WnpZh5BoJ0oVc3vhla0z9bPa6zmG-kLto0SJqh8ivA5BLdraaOM_fMPLHLNYq21a4iKRO8DYZC83CbRwPDc8M9y_uNj8scW4U1_pLNRCav9u_LQB24HH92DT3UbtaWzpjM_-e3Ypo23JnCSZi-n85wiMnYehDQ0et78kc1QhvqDRQse91ciNwHrYQob0eikIC8Lrybv6LX_GGFDLwVboQvSHQVH1I56JNICuNgpHW7QntoZ-_9mARoYqqcT7tlhXrqnI-Kje_4FAxQJmC1JpgnoE0Ny-E-Blu54-5Rr3lXmMMRaAsbMpx0mq-gPEPZCRQqiuGCVD1_-V5dCpusGtraVLvl1sX3T4BdomwXleSowry_d769BuNg6vg5SwI_Cs7YMdcGvfDUDAkk_r-NkXmMYMWo23XDel7jGuK-tXNK_XzljfTJTE-2hOo=w1472-h1398-no)

이제 GTM 설정 마지막 단계인 페이스북 픽셀 스크롤 이벤트 태그를 만들어보자. [태그] → [새로만들기]를 선택하고 [태그 구성]에서 [맞춤 HTML]을 선택한다. 그리고 아래의 소스코드 링크에서 소스코드를 복사하여 붙여넣자.

{% highlight html %}
{% raw %}
<script>
	fbq('trackCustom', 'ScrollTracking', {
	content_title: document.title, // 페이지의 제목
	scroll_depth: '{{Scroll Depth Threshold}}%' // 스크롤 깊이
	});
</script>

{% endraw %}
{% endhighlight %}

이제 아래로 내려가서 [ > 고급 설정]을 누르고 아래로 내려가 [ > 태그 시퀀싱]을 눌러 'OOO태그가 실행되기 전에 태그를 실행합니다.' 에 체크하고 [설정 태그]를 위에서 만든 [페이스북 픽셀 기본 태그]로 선택한다.

그리고 좀 더 아래로 내려가 [트리거]를 눌러 처음에 만든 [ScrollDepth] 트리거를 선택하고 [저장]한다.

![태그-페이스북 픽셀 스크롤 이벤트](https://lh3.googleusercontent.com/mFlXCntDQPQqMCunOjNOPvlKz0hb_GbzEGfK7TSB7fn3Wl4kuoFB87Ms21tfy16WCCNm3fNk_di7o2zJnx8VYJSTh2uDNNlA_ghKhaKMBqgmrKK2BixpgmK9rTeZhlP6znjizV8Fw6EuU9w10oXYBsaCIvA4ODzutNrv8lZt3sk57UY4FhWJSMWH3Y5iyBVsIdgpUqCGTL1-Ws90M2oyJm7JlEK4a1kOyWdn5sSb70VqXIkyCxKVgQt4-5nx8vSenA8eAsq6hCRY1OC6ctWhK_2JGx8FlVfQu7nbOxBfyWQDGcq0LtGwAFuvBJ_kk4tXKIIrpMyA-lOYUvSSXpGiCz-r3gDfnQ11swO6TigBEIaUxfGUojz1CrCE5MFEZlAE90_73jk56-9uxw8hF1DXuEoIf4atnqgvMEz984CheWUE0I6qGWQSRJ14YRUR0n4lhzB2a6nMdIJ8aV4nTDwfzT4ZBpfE4Uu4Dr7WV6t1WBTuKKqgRUwSOrL0oOEEbLyKaR24RSld_Emwhe38EbSx6_JgrU_1T3NoIQNbd27HSTBPPJUxwIxXE8rkpivbXept5mrlVj24PzjRcBITJwxwyoFSTdXjNhfRrxbciJ8U_EBWTwf-1Rj70LM1K6CG3MkBfSwtx3JmCcuGub8L5wdHSetbkkuo_SXdAEQ=w888-h1518-no)

### 5. 작동 확인 ###

이제 페이스북 픽셀로 페이지 스크롤을 측정하기 위해 갖춰야 할 것들이 모두 갖춰졌다. [미리보기]를 통해 페이스북 픽셀이 잘 작동하는지 확인해보자. 픽셀의 작동을 확인하는 방법에는 크게 2가지가 있는데 첫 번째는 [[페이스북 애널리틱스]](https://www.facebook.com/analytics)의 [이벤트 디버깅]으로 확인하는 방법이 있고, 두 번째는 크롬 확장 프로그램인 [[Facebook Pixel Helper]](https://chrome.google.com/webstore/detail/facebook-pixel-helper/fdgfkebogiimcoedlicjlajpkdmockpc)를 이용하는 방법이 있다.

> 참고: [Facebook Pixel Helper 안내](https://developers.facebook.com/docs/facebook-pixel/pixel-helper)

우리는 가장 간편한 방법인 Facebook Pixel Helper를 이용해서 확인해보자. GTM에서 [미리보기]를 실행하고 적용할 사이트로 접속해보자. 그리고 크롬 브라우저에 있는 Facebook Pixel Helper 버튼을 눌러 ScrollTracking 이벤트가 실행되고 있는지 확인하면 된다.

![페이스북 픽셀 헬퍼로 확인](https://lh3.googleusercontent.com/Ez0Ch6bEw0l5ldrn28TqHFSAfTeTV3t9bTWK3jxHut1YJC4dXDSNTFABed-dEVlRzfkszI686sdf3u21axccv9VlyRXe6hC9i9yqX8c8rayqvR1Y8zrkq205SsBdxqeInMd96sBXX_jq_8Oo2QIsAxJXQtqzAY-mwwFRH36YIB0Fw8gpT22dDOLlXO2EXXZceR_LCFh9H0ZgrfIu8rjpJbRaWO4IbVP6vdMQkU4D5-5IKVX68pzWj4-vcRAAT3AC525yqEjXNt_pQH45Lz-PtX6_tIFxdwbpHNXRFc1c4u7Mzmj_zP3S4-7tpWtTw1GvGs46Ba8NFl2eHeZCyuvDyO0UHlbEDIzwNO35c694DbP7S_DVzT4cbDhCH7TfoZwuNuHhYXS9UuSlsSKK_-w2LWDMSIyFavnteVmCcUa7u_nfm7C_2BB9w2ZSCajboQOeSC1sxZjCKM56xv0x1Njgep9M5_BCB3pvGnYdLOh9nFpGwl49Rr4qN7RpXmifHFWCzeHdS6FP7N7boC1tK-WQIRnSHn6xfdhneTTt1NX8PKizoSg2FbOf4ntZz-2W4v1TIhoduNRHBA3CEqgz0EPSQgtNSkJOzgrABtX7fuI_sWUK5egYLqebPhhzjavF-QF1c7naHwBxZeAkrQdPLpQHhChGkYmx_M4zvgQ=w1412-h1310-no)

위 이미지와 같이 잘 작동하는게 확인됐으면 페이스북 픽셀로 스크롤 측정은 성공이다. 이제 GTM으로 돌아와 [미리보기]를 종료하고 [제출]하자.

이벤트에 대한 더 자세한 부분은 [Facebook Analytics](https://www.facebook.com/analytics)에 접속해 [이벤트] 메뉴로 들어가면 자세한 내용을 확인할 수 있다.

## 스크롤 데이터를 이용하여 페이스북 맞춤타겟 만들기 ##

이제 어느 정도 시간이 지나고 이벤트가 쌓였을 때, 위에서 설정한 이벤트를 가지고 맞춤타겟을 만들 수 있다. 예를 들어 '우리 콘텐츠에 관심이 많은 사람은 스크롤을 절반 이상 내린다.'라는 가설을 잡고 맞춤타겟을 만들어 볼 수 있다.

자, 그럼 실전으로 넘어가서 페이스북의 [타겟] 메뉴로 들어가 [타겟 만들기]를 눌러 [웹사이트 트래픽]을 선택한다.

![페이스북 타겟 생성](https://lh3.googleusercontent.com/yjipgIfpVxBGlDe78pziGCUPfNxrVvJ74G0z6iYU8Dd9XZZMcBiWJjdZIMpELLU-cojQ3Wmw9cMU7OJUhMTJPfpl-7f5oWyc7eDFipewZDqbqZLp0PsqO47R97m4DVhlD_F4gi0rn2HVp35ngVQZmkRXzL9MaTTMTfbKhKZFOQZjCRhZS2pYiQ8IBt8KNTsVU1nAavUD1bnDsIQwz9v6vXXkmFiWSXXgnXVIC5eSAccrWBOHSk-UFgIgcktUs2PG4BTvEfw7qlkDfjPU8qkQbxGUsDIsGAXhXPMGWMi8TaSCu3t6VL6OpBQcUwcdMYwfVAobvX7ddoUHksNLMnYbs35yjAMAZZ4wkZTlbbXzn85VkxgBHLdLebdmvWx9lAWurdi-nGgq1M0BGSEqaZq9QO-Pt_pDLw5X-5AhX-zOSfZbPIRhD4xX1Mjqqe8eU14SCzS2VHygd0YFgsGj1vYbzFFKPClDVD2J4-NMqMz-YVI7dBkCwMlP7LL2xVdc3X9OPXNFm2EXrdE6GxbINBy4oe_wKO8LpT_J6aWNw0eE8SMABsqpRq4a6VoE_6z9ZxpG-5bZXaZz9i9dVHuIrYP4XylmPUzuCpP6eXCa72k_O8Yqsy8ZIyKzPZi9Lc_Nv2NGzrGSSxfPcJXJsLpnp3nf0_qoLdOk2d3l1DY=w898-h900-no)

다음 화면으로 넘어가면 [모든 웹사이트 방문자 ▾]를 눌러 [ScrollTracking]을 선택하고, 아래 [세분화 기준]을 눌러 [URL/매개변수]를 선택한다. 그리고 [URL ▾]를 눌러 [scroll_depth]를 선택하고 아래에 '50%'를 입력하고 타겟을 만든다.

![페이스북 타겟 설정](https://lh3.googleusercontent.com/hbZ0f4td8Ypoj5v7HvAlfvekxkMfoAbKMwcjSJvUmQx64dPdl4YfTEXt59T2rmLVQ8wONqE8zNLAR9pfz3Haf1GvZdbG4-NSkUu9Ovrey9Y5ggg0J_PrJL2oNRkmHK0HGjs-kIy1tLcOwYIPYJ5hZObCnATMQt-V1cnvzTB8h1YU_WrxevZEwWdLiCM1zhUN93u8-vL9zC6pZ5eQE28G4KzwaVn9KeEHN4jW7F-rz-dEKjYgUB7IGzqes4MoUa7a6m7kUmXRf85839E5fQtxFrdvieUSKZQg6hf87uCjaBzbUGiBZctxvo1uCA8ErxLWWM77j7MJWlW2IXBZll5Y2-G63lvZuZxVNMg19E6r9SyAXDpy0VADc-uLgsojcTjIs-sY7QGXbBEFmvx1b0X8ROt37RhkOf4LzuMWnVUNLloy8-EkShimj4xNZf4cIQyKquvBeR9FOxkMVDw1cetzkabX2pSEaDxVkONb2J8ghuWyGN1Ix2rfz0_DvW0iDSw0cmnuXzFriFDcTbU2KEiLiJnjblGdkMh5JBXy9nd5v5CXQiNgqit98HU2YQLYiQ3KHbFf_KKmu9wjonRe3eM0Y1HUtTRNdVbbpJs4ldDQ11q4r6udPxtXYGPn8MUNybawGWf-SOiB83yvpwRESs8LH-qOrK5S6vJG8kk=w962-h592-no)

이제 잠시 기다리면, 50% 이상 스크롤을 내린 사용자의 맞춤타겟이 만들어진다.

## 정리 ##

초보자에게는 다소 어려운 부분이 있을 수 있지만, 이미 맞춤타겟/유사타겟을 생성해보거나 페이스북 광고를 운영해본 경험이 있다면 쉽게 따라올 수 있을거라고 생각한다. 작업 순서를 정리해보면 아래와 같다.

1. GTM으로 스크롤 트리거와 페이스북 픽셀 이벤트 설정
2. Facebook Pixel Helper로 작동 확인
3. ScrollTracking 이벤트와 매개변수로 맞춤타겟 생성

마지막으로 지금까지 따라한 내용에 구글 애널리틱스 이벤트 태그만 추가하면 구글 애널리틱스에서도 스크롤을 측정할 수 있으니 여유가 있을 때 시도해보길 추천한다.
