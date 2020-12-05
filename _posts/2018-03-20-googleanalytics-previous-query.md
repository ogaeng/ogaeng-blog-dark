---
content_id: 10
layout: post
title: 구글애널리틱스로 방문자의 이전 검색어 보기
date: 2018-03-21 00:53:00 +09:00
author: "Ogaeng"
permalink: /googleanalytics-previous-query/
image: /images/thumbnail/googleanalytics-previous-query_thumbnail.png
categories: [GA,GTM]
tags:
  - GA
  - 네이버키워드
  - 유입분석
  - 검색어
  - 이전검색어
description: 우리나라에서 가장 많이 사용되는 검색엔진인 네이버에서 검색 후 웹사이트에 접속할 때, 유입 키워드가 아닌 한 단계 이전의 검색 키워드를 구글애널리틱스를 통해 볼 수 있는 작업을 따라해보자.
---

우리는 어떤 정보를 얻기 위해, 특정 웹사이트를 들어가기 위해 검색엔진을 이용한다. 특히 우리나라에서는 네이버를 이용하는 비율이 압도적으로 높다. 그래서 많은 기업이 네이버 검색결과 상위에 노출되기 위해 많은 노력을 기울이고 많은 비용을 지출하면서 검색광고를 이용하고 있다.

구글애널리틱스와 같은 분석 도구가 많이 이용되면서 많은 기업이 유입분석을 하기 시작했고 어떤 검색어를 통해 우리 웹사이트에 접속했는지도 큰 관심사 중에 하나로 자리 잡았다. 하지만 구글애널리틱스에서는 직접적으로 유입에 영향을 준 검색어만을 알 수 있고, 그 이전 단계에 대해서는 알 방법이 없다. 그래서 이번 글에선 우리나라에서 가장 많이 사용되는 검색엔진인 '네이버'에서 검색 후 웹사이트에 접속할 때, 유입 키워드가 아닌 한 단계 이전의 검색 키워드를 구글애널리틱스를 통해 볼 수 있는 작업을 해보려고 한다.

예를 들어 네이버에서 '새우버거' 검색 → '치즈버거' 검색 → 웹사이트 접속의 순서로 접속할 때 '새우버거'라는 이전 검색어 정보를 구글애널리틱스로 볼 수 있도록 하는 작업이다.

## 이 글에서 다루는 도구 ##

* Google Analytics
* Google Tag Manager

## Google Analytics 맞춤측정기준 만들기 ##

구글애널리틱스의 보고서는 측정기준과 측정항목으로 구성되어 있다. 그리고 기본적으로 제공하는 것 이외의 우리가 직접 커스터마이징해서 사용할 수 있는 방법이 있는데, 그것이 바로 맞춤 측정기준과 맞춤 측정항목이다. 측정기준과 측정항목에 대한 더 자세한 내용은 아래 링크를 참고하자.

> [측정기준과 측정항목 - 구글 애널리틱스 도움말](https://support.google.com/analytics/answer/1033861)

> [맞춤 측정기준 및 측정항목 - 구글 애널리틱스 도움말](https://support.google.com/analytics/answer/2709828?hl=ko&ref_topic=2709827)

구글애널리틱스 보고서를 살펴보면 다양한 측정기준이 있다. 우리가 흔히 보는 측정기준으로는 소스, 매체, 캠페인 등이 있고 유입 키워드를 보기 위해 '키워드' 측정기준을 많이 활용해봤을 것이다. 하지만 이번에 측정하려고 하는 '이전 검색어'에 대한 측정기준은 없기 때문에 맞춤 측정기준을 직접 만들어 사용해야 한다.

먼저, 맞춤 측정기준을 만들기 위해서 구글애널리틱스의 [관리] → [속성] → [맞춤정의] → [맞춤 측정기준] 순서로 들어가자.

![관리-속성-맞춤측정기준](https://lh3.googleusercontent.com/MY43S0uOpFps_4PVqelMrYet-r83IX2gz7uhsJAJNXWsganp18TmrdinDE81W05ewehgIgMX9lmXhgKVA1n2HWvW3LTJUREM-DmX5vjlG746qRMXmVal-T_QDYH4bpX45qy_fXcwJzJJPZjZtZi9OgYJrzbIFLqFpjledTYLN_L6ERtvNwqcnGHCoVmqI8hHWNcHQzB3-YmFJ0nLPkbgLnq0XhBoPSDi8cYtz7_FZQSzNLUCpBtk0uy8djAdSqPz9cQDbz-nn-mIF4oEofme3q8n5kyCqQMAzUeLx59RNKeK7oESn1CLaHvngCDmStL2LMzy4KpyUoaZe2NgW2Ira3kY39l-xbhJR5KTaKkxsF7i5h8J2zU9aYYrU5eBTiMnaTrJqzsl98aE-Uy7KvfZmHfT4SWRQJWRzRaJRDyjxrNW_Fg1sdwGOhx1kbu9rD6Yb_m3tnLQnmfUqEXwPA9NJU_DaoUAvKf2MaLEx7jVPwAiIc3Oc81JDjzm53aUs7HQ2HfVmw8ROeakaUxj4f4Wqt3Xft86CkamP8NDC0LcLyk19q7jzAtCqgetNmd-0KLZILSVkxXlTquaoWRJpnPTUyWrxSZ6xyQJwgDqxA4=w1346-h1238-no)

그리고 [+ 새 맞춤 측정기준]을 눌러 이름을 <code>이전 키워드</code>, 범위는 '세션', 사용중에 체크하고 [만들기]를 누르자.

![맞춤 측정기준 설정](https://lh3.googleusercontent.com/tNGVQH2-JcyEEcEiYr-5FPE0YehEJU004CdzrUGIM0KpVOJQwSCZ7BPP18-DYZxWprM0VUHIMSuaGs-kBTE1bu0yWSOhpTYaoIuZOez5qgzy3jpJ3YfEgQD4gkTWbU3Srts44G3fqeFNYVoqvWB0CoViILEv4ZHbMNtsuWQ_92C_n6LXoWHeL3xHkBSimpf0qaofpjktvnKb6U0F81WFZMiWa8GVjHFnY1yOC-a_-IiN4q5q2V9GcTFmo83iGgSH5ew62_0dtNGqLUZUZS-43Fq5mRl06Mf7QGeVxast460c43X6M2oKklZQLXk8GY6sFFDKkwCRnzB-tCKbACOuoucyOplXE8Vxxo1yFEMBnrcdoXKc9W_-dzgxRPDlqijSDZ6khWQb7ctBnFLaXiTdpShE67eikyIqtF_lTs8OdOXmd7A298YoV-V3fCcuEUPJ2jK_0fuyJtvXrb7YrEssOCyuUoPAF5wo9YiNM5xf760HofIvapwyVQ0BPc12dNrATAROi9UIenTNc_z04o8plF8V8DKNoOBKRTduccKnaEXsDAL2TwKaSnljwPHrJ8y9N9eU_lv9P4kDixWFIUaHVi5wBzZ6gYaqNfERhd4=w1094-h692-no)

이제 이전 키워드를 보기 위한 맞춤 측정기준이 생성되었으니 구글 태그 매니저에서 이 맞춤 측정기준을 이용하는 방법에 대해서 알아보자.

## Google Tag Manager로 이전 검색어 수집하기 ##

구글애널리틱스의 환상의 짝꿍인 구글 태그 매니저를 이용해 이전 검색어를 담는 그릇을 만들어보자.

### 1. 리퍼러 호스트 변수 만들기 ###

먼저 네이버 검색을 통한 유입을 구분하기 위해 유입 경로를 저장하는 변수를 만들어보자.

구글 태그 매니저에 접속한 후 [변수] → [새로만들기]를 선택하고 [변수 구성]에서 [변수 유형]을 [HTTP 리퍼러]로 선택한다. 그리고 아래의 [구성요소 유형]을 눌러 [호스트 이름]을 선택하고 'www.' 생략에 체크한 후 이름을 <code>referrerHost</code>로 하여 [저장]한다.

![변수-referrerHost](https://lh3.googleusercontent.com/-aSHAVzwY9pgwfBZNWsalP9aHfuzqcUvWGSbmHvzgwtmxAEvXetx5TIU49OcAYoMQ5psw1DrQRjkwhWxMt9KdpPIz7f94nCRaiEMXkEXZDA-Udlw0QC8T2hcmabvstKoB9OrdiDGbmE_YfcZLx8hhAjMUgqxOTyZ-rrdK3wtreS7-b6xTBUiVYGIHeh-umBMbNqgcekZLgIf4RYd_i72zSh9ZptShhRO3IAu7BPPAlZMt0ti-F6eirf7yWUgSh4Nz0P-1jjPUOJXR9lzXJcm7o69D4q3tTDC-Im-0oi3GPdl27sywJbLCbowDuWBejZN-CKcanAq6Id5xDpiktTxyOh9erHm6dLgkzABsAcDH3z4ONADcLxBBa0Sx7ZGlk_Px3rHE0TMUWEWelySdx0rNGijybEl_dswQaHB0G0ZIL6YfN3hzpYoy2Sgob0IQJ1cQUQZE7hXXW5xcRWdVDVer-zTQ80b2b_o7EcAFjTKCFC5qkZJsqWS9q_CtvGAlg32Okjw_sk0-iJiBW--NqUzfGqbU2re2wsQMQBtLzNMYoxsxnpyZeh4Nmj8moNsTFc8TjXl16uVTibyu-o2iovFqAP32anWZEn_p-NK1L5aww=w1666-h712-no)

### 2. 이전 검색어 변수 만들기 ###

이제 가장 중요한 단계인 이전 검색어를 수집하는 변수를 만들어 볼 차례다. 먼저 네이버에 접속해서 검색을 해보고 그 결과값을 살펴볼텐데 이전 검색어를 얻기 위해선 당연히 2번의 검색을 해야 한다. 필자는 버거를 좋아하니 햄버거와 관련된 두 가지 키워드로 검색을 해보겠다.

먼저 '새우버거'를 검색한 결과 URL이다.

![검색어URL](https://lh3.googleusercontent.com/QdUOTuvRZxbAeZmG4M3SdNpljnwaGCEp4TSKZRGwsfhFkNWm-OxrOfLKeZPMWXRv-TgpEccv7ZcgAZNgwGs_KV8yWT_v_DQsBsE1v7vNWVpwp_sWrG8Dq0Ik4C0IIzyUBPurqtBHCXzwEM5zaquykwq7u719_mRtzwz4ZmBeKRbHPXOFxyUJb5dqjWin0V7DyJhCiahZtm3jU-AmaKVcpsY7BpwqVNZR-a3nGQUeO7g29m46bxxhYEVxED5ze-zlPu1AaH-1RdVkdViXnA6tikB-VODKAgiQ65HRWBx5AyZrFz4oR2pr2qZiCNaJjxMKNZCaphzXmiE2yZFydUCcIy0A4vdIFywQPuzaRAfcpBT7uv-BLcH-X6Vje8GONUdcpIpPh4uPhhRvkyC6hDkd9GrylWmLVQIso8m4-KM0osfH3cX40ajx2lQXxQqbJjpcpvClt6PhTfXdMt9z7kFeGxoQZ7JmzCSSs7JohjMX2klfmgaZJddms0PVVIwzdRHuZc4ufYJG_usDya4m-4jIIdLf1v1k-5veb6NMxoyFSpXbewRYYWgsit-hra2txcAxbWD3tNa4LKw80IWsiRQD2Wkn7alDFPDMIClvnnDvRscXkhzki24st5iCS7L5La_MlEYdUzlyD-ebwsNu0hVJ9vaPU7aUGKbV=w1884-h72-no)

네이버 검색결과 주소를 살펴보면 다양한 정보를 담고 있는걸 알 수 있다. 여기서 우리가 주목해야 할 것은 검색어 정보를 담고 있는 'query=새우버거'이다.

이제 새우버거에 이어서 '치즈버거'를 검색한 결과 URL을 살펴보자.

![이전검색어URL](https://lh3.googleusercontent.com/VzVsSqSS_3oC-TUwTNmUNQY-h1IkfUBdYVvdKIf4VOnG4YuVfSkjp5a4qEtyKMvYRk84_SxbJpmxyQpCASvAUQpvfMS4UfFS-byyjaYyWUdI4naoWuENssNUUV7G1snSGBkE7k8Q92dXZA4jZPuciY93DCybyRKsIfUHfG2LPwoO8svsoPaJa2Z8DKGu_WXQ56TBof4K5ULdQ2PSKq5MMk15MYS8NAINg5kxoX2apwAHjaNn8hKD2C9P4_Y-jjr3eTMrVq_seyZn6A0gCBSrOYzhglw0TKgA6A_hOd4NQVUw23_kT50r2tzReSAx88hdXtz09sPDA4yqltGCa21lkANp8rVyWhghANIyn8PUF5UDBn2nGLcj_wHKUJL0I2Y0pYNl0oMxSSB1eWqIe534l28NOKk_xRPCZy9raZ3mlenf95VWfJFEtQfgLmHEAAOI9mZTNNlNK9npZEtyEJVmNVPis9xlXwTTpjr7NS_GJfDj4RhhSWi-3QFaMwwxYvH9ZsAolg4rD0VtE_jKCnGjbmumcIW54azeKFWMZ0WzjpSfhK5gAYP234rcitKsZHzUXbfWWoU09gQg6IfKU7KpBq0ecP9a2B3XYkK7qOBOktZGrw_Ve3N0eg-eQNduZO0cnpzK_fq5vQ92_JdIegX-1kQzjvcqM_pr=w1884-h72-no)

아까와는 비슷한듯하지만 주소가 조금 더 긴 것을 알 수 있다. 방금 검색한 '치즈버거'의 정보가 담겨있는 'query=치즈버거' 뒤에 'oquery=새우버거' 가 추가된 것을 볼 수 있다. 이 정보를 토대로 query는 현재 검색어를, oquery는 이전 검색어를 담고 있다는 것을 알 수 있다.

이제 무엇을 해야 할지 답이 나왔다. oquery에 있는 값을 가져오면 된다. 이제 구글 태그 매니저의 변수로 이전 검색어 값을 담아보자.

[변수] → [새로만들기]를 눌러 [변수 유형]을 [맞춤 자바스크립트]로 선택한다. 그리고 입력창에 아래의 소스코드 링크의 소스를 복사하여 붙여넣고 변수의 이름을 <code>previousKeyword</code>로 하여 [저장]한다.

{% highlight javascript %}
{% raw %}
function() {
  var referrer_hostname = {{referrerHost}};
  var url_string = {{Referrer}};
  // previousKeyword by ogaeng.com, CC BY - SA
  if (referrer_hostname == "search.naver.com" || referrer_hostname =="m.search.naver.com"){
    var url = new URL(url_string);
    var result = url.searchParams.get('oquery');
    return result;
  }
}
{% endraw %}
{% endhighlight %}

![변수-previousKeyword](https://lh3.googleusercontent.com/d1YEXKKNVqTEL0rJkwQjXhPMMyg_gA7NZaNmcCkSv4_YtiRhoRvedPqt8EHjoqACsJdeu8415J2y-aRsfi2PLur6VwWqbfgGg4_rsmGp9N2x1izNKkAItngKJrCqp0Q8ieQbkXehVGnnXxFwue8lRWr-mdB6t0_gulY_yiKWcShiAGju7nkBlcqtuG9ni7pZcP5KNS45GcQisrDQZreQenW6iFIfKOHBOQp0097VjW73YzOqdQ3CdAzA5O-gQKMrSisxli_LX9iJDz_bLsIdKPU0w5LNziSnoUr_UV0zJpUFOEEyA15tnj-mQZGaVA82odZ0fExBeHb5IV7YyTTZvxasBkX2Qo5-DwnzGRSN1TCF8dLuRYcB-hr00PhjGqJPIasOL3xOC2Cc0gKHv8vnPbhEBDfnw3PY5jKbWvoh9kYyMKt7K2bCjmpqhTzGKUzJ8vc1JGUxlpXPp9lcql-KtnfNzNJVtCF6Y6aQW6pA3JCd8TibhE2y2fmu-5k6yqjJIH_mSbRmnZ1MFxlAYTjJptH9yknRMCdsrdC_6CPEB6ozwO5IxMctXaxlhclRaN2r344rkSOFQF93EdeciVVoFWV0tYzSwz8gPoDyygCGjbE0gt4llCerJ_Yt018eAj64hwDPe1_t-jBSupJQbDwSO9UcOtJJixTe3ciPbNAUui7BnopVGALbAlqFR4UAFSP9fjuOqUqsMcbajYi9NC_tLNAvcnGFV2CsOP2pR9ov4xkugcM=w1986-h1074-no)

### 3. 구글애널리틱스 태그 만들기 ###

이제 우리가 만든 변수를 구글애널리틱스에 적용할 차례다. 이미 구글태그매니저에 구글애널리틱스 태그가 있다면 그것을 열어서 수정하고, 없다면 새로 만들자.

[태그] 메뉴로 가서 [새로만들기]를 선택하고 [태그 구성]에서 [유니버설 애널리틱스]를 선택한다. 그리고 [추적유형]은 [페이지뷰]로 지정하고 [Google 애널리틱스 설정]을 눌러 기존에 만든 GA 변수가 있다면 그것을 선택하고 없다면 [새 변수]를 눌러 구글애널리틱스 추적 ID(UA-XXXXX-XX의 형태)를 넣고 새로 만들고 그것을 선택하자.

![GA ID 새변수](https://lh3.googleusercontent.com/St0QSFJJuoFucX9nHsQcxJPuly3iVfnpGrHlJBfl1u1ImWCbC75nSAKKAi0xiHF8eLl3uNIJ6zPxP3A2opGuu6z1uw49sV2i53k2Bbnu8eHBzPEZ-bYlC--eSuBYeIVNu3lhFwVhXDYNR_4PArdT7E_bYJjCp4rtTV_KvTBBViLojCqD3It4VbjxpIOEnhNJNAYWE7elVx5egFgra4Wcy4JjbQ5xxXxyasAlcWH6fFLPFjuT8HudHAsO4pRSx9PvGUZFFrm6N6IDeYtDwy3ZWmZQMbz_WN-1U--SYb_jRd7keW9K5bc8fcjavczzUZ1kKMgff3MewTQKwZ13G3jfBXtJsD0AAz0yRV5uVyQ7-iN7ZcpDEEzoMCDjALwmy0YESIODG33q2McuAgOgH6cWrsEf6JnD4x4EmJ_qm9zkP73lhHDIUormehcYZNU4S1T5ryHmrV1E1qBRqd_7DKK28DbGuqbOjVovlvgSdW6X_NmckIQ8mGKv4D-1EC3wJFM3hr16Hk2wmSfUaW0EkXdRhOJjrRkAEayp0gSTl0dB8o1SHNVUGhA2_m5pB81cuakKgXr3zxFAqenvmz1U4hPjjDUnUPZyN7uHWJKw47k=w1646-h962-no)

그리고 [이 태그의 설정 재정의 사용]에 체크를 한다. 그러면 아래에 [> 기타 설정]과 [> 고급 설정]이 새로 생기는데 [> 기타 설정]을 눌러 나온 하위 메뉴에서 [> 맞춤 측정기준]을 누른 후 [+ 맞춤 측정기준]을 누른다. 이제 [지수]와 [측정기준 값]을 입력할 차례다. [지수]에 입력할 값을 알기 위해서 다시 구글애널리틱스로 돌아가서 [관리] → [속성] → [맞춤정의] → [맞춤 측정기준]으로 들어가 먼저 만들었던 맞춤 측정기준의 지수를 확인한다.

![맞춤측정기준-지수](https://lh3.googleusercontent.com/Yh3L7dYXPzeALZLGv6gxN0XjZW5B9Q-PXLZCE3PAGeDAx3Hf7NjSFKfw8xZRsWPJWgDjMLJyfTLQBMwg6klnafYkQlEJVV5a9qWpJ_UkF1LMIx0WrKhWvI-ulR53SIWeRpbmgN1Jywdpn0K-NN1rj9dFmJ4N7_PSH0VYxyw-oYkcdJYgK1_Mpy60NNfKAA7PsN8HpKKE6fC3yC2XZUb3_6Hlu5j1DL3iwxmdRUpe4Ia6vcQbssLwU7Ptf8p7xvBXBFeME0uDdaT85IKTzmgzFMtpcPFYZXITxFySlhT8rmF-iJfwR9ZrX8VBBAIzt0mIoh51wXw8j871CBriTgp9gy2EG9-KefERS61lT9IUCM8lBpf-c4TEH4CnMdrflEY9T-A2XiXykKr4u9tp04aryQsi-dNZFNBufm4oAa49ExOhIx7vKbTb-X7MxW7bEicFC9fWfPLekUusjKxZaUIltQkYiIeznDkGCGv6n06juM_iLqdq4ntHc3cEN_FA2veHr6laSlKFpZ3xGNgNV2A4cGy70ldikNXaZjRBy8pfKxijxKqmdI4omdE03V3qXYsdNcM8PEFPxIL4w4nHtCbmleOxeegqO86y944ak-U=w1380-h346-no)

위의 이미지에서는 지수가 1로 나와 있지만 사용자에 따라서 지수가 다를 수 있으니 본인의 구글애널리틱스에 나온 지수를 다시 구글 태그 매니저로 돌아와 [지수]란에 입력한다. 이제 [측정기준 값]을 입력할 차례인데 측정기준 값 입력란 오른쪽에 있는 +블록 모양을 눌러 위에서 만든 previousKeyword 변수를 선택한다.

그리고 아래의 [트리거]를 눌러 [All Pages]를 선택하고 태그의 이름을 지정한 후 [저장]한다.

![태그-GA](https://lh3.googleusercontent.com/Nu05EItz8N5SMfPtWA3jAnjHR8WxnZTRxveb-4rzOkf1Nm4js5ABXFLAU642cUBGC76O2n-dxdh9lP1MMlPb8IaDBrZK5VGp1D9mHsyncRRFnDh7EBuYyuxyePs5jaXKGszJ83Uoc7ca3kE5D1vEoOkanewW0K2JkB0TQ2ihQYSqQPVll_bCa-1qPpNo4XReCzFSf9PWoJmPaqzq-xyXP9fbgcggy7QjVvrIxh8zXDxiFjmEEo1CvJusmUjAz6eK9KBKTKBhektnOQNY2klgWzUek3CDmerzs_RDx3uYVDHOA805SETKUrdjRl0DGmeBZssrCgQB_XODmWfKvNqQvUeqH3f_Lq8pGQkP5UD1cTaQfBcNgKiIwmdItsFeddyRDdPesmvsf-3XxDhY0sYIG1Xo-Gh8sF81TyqiQxkaZn-d7v40OfHLRS-jOVwkYUgJE9ZofbxS0rg5ipgt_BD7LIEacgep7_SjBdrG3gJhEW1U3-XJUMQUR0ei6ofiusB1wy6wMNAMvi2zKGLJuZyZqRMBqfsft5AAj2eEsfi54ctXoF_fzVHsCIBW1xxGWNNBflvQFsCmVbH1Irlqiwy5OWbtcYEbmsXF2Kf22t8=w1192-h1606-no)

### 4. 작동 확인 ###

이제 우리가 설정한 맞춤 측정기준이 잘 작동되는지 확인할 단계다. 먼저 구글 태그 매니저 상단에 있는 [미리보기]를 이용해서 확인해보자. [미리보기] 버튼을 누르고 아래와 같은 메시지가 나온다면 미리보기 상태가 된다.

![GTM미리보기 메시지](https://lh3.googleusercontent.com/4f8SU9uppPTIdqfvQhUqZw9WFBiEqk2ZHQRaN-wS-poiMMbIL18RjCifM58nfjTLlyjc0P3aoud-L7gjx91WaHCL7VHPhEnvWTFiVrG0aKRBWzu16vnTFGRRYkk43EyurrUrLLvN4KlvJ1sTRZlNcoGsK8UAt4q7ToDaWi61NKMZhhwSuG1MznHiHgWXC9DYD9ppEeQE7o8FAQ53w4rcQwsiRbf0Lp7YnmuPcq-zn_r2pKvflXClxIre2-9UDSjNZjJQuu5UbjSfe1zLWR6UhZScNfprbUzmy5nb0bXRwuWAyCZ0wpnG63ZUu67iCEMhr35Zedg46rKGVlcd5mS72SBI_RjEy6rTKp0Xrpejtdkizs2DHF5F2-1D-n46rBwRsYFGSXk1P9z_-ARs2BFmn9D7G_xUQFFe6yA5vdJ9hJzhJ-3gOVhGxzbWV11QAsekyhysiQq2ud6pZ56tCrDmzRJCwKQtOE4vzG8FEvPWABy7BJuKJ4yuFa2L_eIDs1vHIsZ1xfBIKhzhK1dliibl09mHWx2LoQ48xShDeA2dHplnYYlY3F0QjxL-w08tcjZFRsOPcTaMvnIBr_blR_omWqT-hMJYOW9PzJkMn-0=w1574-h278-no)

이 상태에서 적용된 사이트에 접속하면 되는데 우리는 이전 검색어가 잘 작동하는지 알기 위해 네이버에서 검색을 두 번 해봐야 한다. 필자는 '페이스북 마케팅'과 'ogaeng' 이렇게 두 검색어로 검색을 한 후 이 블로그로 접속을 해보았다.

![검색-사이트접속](https://lh3.googleusercontent.com/ZWkQK_YMHoUF_9EFZo7PRIIZfiqvUrjES1x5sqswg7k7RwNuY0zdmYo1k_48g_M3WxL0MrSdgP9VILKN8GMR9JbSofv_1ungCtWW5iD5WzeKKBxpfjKi9qsMLgBB6xBENArV8TBWdxjbG5iucpnDC2doW5dnIOhTbm1KINnRvW9UbPRT4Im6c8pOJbul4Bo_3e9Hk1SRSWdO1399kzJYfBn9h3Udmi4eQIKR_8bX1hhRrkmxI9I1BHswJglOlhK3ptNXLnEUda6zqssEnZRPjam4RfjXq8z4-duLsVUQoy8OctfLvKqmhuswLeect6lGDrFRU4EFZknRauWpBgZCUxqWp3UsrXCLXtzwi-UyRPN6cNrkHbqgSfNihPBiojAG5uNukgxyghAVwS-oLUKNxRgGhUzuk-okA0K7llSYEFNXxin5-xjfVeOVSg5ci0tlKNqpuLf4xms50QNLOvOxiwR1V2M31eu4K_8DB88MJIhMImoT7hOF7LPx--b6sqQBOwewVWUbjN6MJwrmGRSWf-BugASVALDVsl2DisVjQwM0vxPG43XkIKFrYRn-qCPWsnE1AFn31MQlhMJbB6Tb4OpOk_7-_HBJL4UTHUk=w2303-h1290-no)

접속하면 하단에 구글 태그 매니저 창이 하나 뜨게 된다. 여기에서 'Tags Fired On This Page' 아래에 현재 작동 중인 태그 목록이 나오는데 살펴보니 우리가 방금 만든 태그가 작동되고 있는걸 알 수 있다. 일단 태그를 한번 눌러보자.

![사이트접속-GTM미리보기](https://lh3.googleusercontent.com/1CRL3XHrh8wuX_58ir8RAVf-830CgfMurqOZu3Ti1fOQPcnHxT0GpzRsv2SehS7-dVaIzaKkIxrWO79UND2kHYAQ1CZ5hQuv6zjF605t8x-gML4Pqgw0t_8_dY0KcB7wqkth9HOwLhdP04KDIqUaa5-G2Z6gBaTQ8k5rKQKT7Zb_jZIEtbNQSBM1buzPFedoVJuO7r9KleQFZtef2jDQg4h1rweqAnP2731YW6lWMsg8Q7O6ITJhxsEgiyrfzZx-eBBa54gGjmFBdzXX47Bu24K98bWgIH9tRcb3j1UQ4h6J8MTA4CfN2gfFlmF32TqdfJLr29YB_Cx_-HRG_9AG-1TG0t38zpTAQAp_1xQZuNLGt4MdTPZw7y4tnf3NnRC9pzKI4lgtyubdHqotE0foUzejtwVTb4UjK_-a7VXpYV-iVUlo3zsFChH6kIdSd5ReDe07OXYhSoZ3uNZ24sdwvxph7DJc3vV6_K2CukGzMGqkDNK4t136G9BOHXyskFLocGTIM2kfMjudpAgFqSVICfgZhShPk8d19dqOXF0apHSjEOEs9m-bdLR_Bmg4Fvxz2Nlri50kvG6DqLaKBESoYt0y_wht5imfO-0IBM8=w2302-h1604-no)

태그를 눌러보면 태그에 대한 정보가 나오는데, 이곳에 우리가 설정한 [맞춤 측정기준]이 보인다. 정보를 보니 index에 '1', dimension에 '페이스북 마케팅'이 있는걸 보니 우리가 만든 맞춤 측정기준이 잘 작동하고 있는걸 확인할 수 있다. 잘 작동이 되는걸 확인했으니 구글 태그 매니저로 돌아가 [제출]을 눌러 작업을 게시하자.

이제 구글애널리틱스에서 이전 검색어 정도가 잘 들어오는지 확인을 해 볼 차례다. 실제로 맞춤 측정기준을 적용하고 바로 보고서에 보이지는 않고 조금 시간이 지난 후부터 보이기 시작하기 때문에 다음날 확인하는 걸 추천한다. 우선 여기에서는 실제 적용한 기업의 키워드 보고서로 살펴보자.

구글애널리틱스에서 [획득] → [캠페인] → [자연 키워드] 혹은 [유료 키워드] 보고서에 들어가자. 그리고 데이터가 나오는 표 윗부분을 살펴보면 [두 번째 측정기준] 이라는 것이 보인다. 이 부분을 눌러보자.

![두 번째 측정기준](https://lh3.googleusercontent.com/6Vn6Jbg2MmK1KofonN-KIlA8tePqdea1ZOqWb7SKUvOZ2F1ErpA2_4x2f_IFCKW38stBroGLyVFPzwMUR9pmKkEN48-aZawx61n8m8WQtSvBJ_8wifCFIcSCkxjFMKlaBLNrrpAngtW2-elj1xCKsN5jei40QDvLm7iwsPNtL_cGiETwpdjD6VyUx0PkMUu85FgGC4g0CdNgThMNspmsyj1reNcpYt4PoqGpN_uOaLZ8ZtebW3Naryp_CXZdLkNvU6ESXwbO97cnyP77N8VHFE_3iapEVSgiY6b4JOpeUQedqo4drDXPt2OpuONUMhFWIigVTXanyzK_8YWP_HDW7r6YCKYJkZ9gdHO-0yCWrCmSeKziLJPeHH5e9-zAYxxpMRL0hPJGA7D_6e5SQqLpKviqCqweF77b4ApuiH33Bdy35Jury-6Uxkv_OpqutEbiXgs3bCea1UW8TfnH-rv8_Bv89mnySWqdrBk-lVATS21QxSXZ-oOJBUglwEgjk6cqWQqIzbh94IBxFuRIoi669sVY9t1CFtmQaviahpAEU2NCWpWW8dQ5kV-mwC8uqN8POoNIy_GZ8JtyhK-PWRXDcIXPxPRdTXOOLe6NLtA=w1376-h346-no)

[두 번째 측정기준]을 누르면 [맞춤 측정기준]이라는 항목이 있다. 그곳을 누르면 우리가 설정한 [이전 키워드]가 보이는데 이것을 눌러보자.

![이전 키워드 목록](https://lh3.googleusercontent.com/FMIMXquFyrdOxsZtGh-12eGf3e2fL7s-NsyvgW4LITjW-g_aK2qG29NGQ0puU2ExZ2jMABI3Q382s7koVtANLV8jWlCG0rrd9JP1pokOhCnJzzisKnjliMsvJDZ4p149L8oLNrLHnHzL2-yqSwyPkZtTkvU3bwI7_brRfowCCEFxt005qgFeTJYzqnc9eH2UzJxizqBQ9Jvblfua05qaUtAoSRqVxARlPd82oZYMhau4kXTpTeXEqXLlT1GjnowwZlGcFR_7623fFMRxrFag-wN4CJdzHXtAtbSMYAEh65jcSHbwEF5kR7ww4VjCDulpatWMbLtgheCFayLaOeu2fq1uE1hjU_mSmxc0WTHV-FaapeElt8xJfgH3UC2WBj_6JngJxi0GSwIkbVMeGY4C_CYnpgxXM_LPpXeQmcOGrJjsucn2J3WT2M6ROhJe7ZgsTT3JGikYa_qMUQJiX3qM4w1t3z0T_DBzje6hxCpdIAV5N9SvIn833a9ZL58baDA5CmbTKZaWe3IeBnBwskvMWpSpCX8c9cAYgBXw2Dta_C2gLh5IM0fhzWp8C9m3ppwEoGqoqiZ7Hz1gMr3DhYwtuBuEVmS7ybdYbv8VHBM=w926-h604-no)

건물 리모델링을 전문으로 하는 회사의 사이트를 예제로 가져왔기 때문에 '키워드' 항목에는  '건물리모델링'이라는 키워드가 있는것이 보이고 그 오른쪽에 '이전 키워드' 항목이 있는걸 볼 수 있다. 이 표를 보면 한 방문자는 '건물리모델링'을 검색하기 전에 '도시재생'이라는 키워드를 먼저 검색했음을 알 수 있다. 이렇게 유입 키워드와 이전 키워드를 함께 본다면 우리의 고객이 우리를 찾기 전에 어떤 생각을 하고 있었는지가 파악이 가능하기 때문에 단순히 유입 키워드만을 가지고 분석을 했을 때보다 더 나은 분석을 할 수 있다.

## 정리 ##

이전 검색어를 수집하는 것 이외에도 구글애널리틱스의 맞춤 측정기준을 이용하면 구글 애널리틱스의 단점을 보완하고 많은 일을 할 수 있기 때문에 이번 기회를 빌려 맞춤 측정기준을 활용한 다른 방법도 연구해본다면 분석과 마케팅에 큰 도움이 될 거라 생각한다.

마지막으로 구글애널리틱스로 네이버의 이전 검색어를 맞춤 측정기준으로 수집하는 작업 순수를 정리하면 아래와 같다.

1. 이전 키워드 맞춤 측정기준 생성
2. 리퍼러 호스트와 이전 검색어 변수 생성
3. GTM의 GA태그에 맞춤 측정기준 설정
4. GTM 미리보기와 GA로 동작 확인

이전 검색어를 이용해 더 효율적인 마케팅을 실행할 수 있기를 바라며 따라 하다가 막히는 부분이 있다면 언제든 댓글을 남겨주기 바란다.
