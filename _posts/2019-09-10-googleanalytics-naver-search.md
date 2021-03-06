---
layout: post
title: 구글 애널리틱스로 방문자의 네이버 검색 위치 자세히 알아보기
date: 2019-09-10 02:00:00 +09:00
author: "Ogaeng"
permalink: /googleanalytics-naver-search/
content_id: 14
image: /images/thumbnail/googleanalytics-naver-search_thumbnail.png
categories: [Naver,GTM,GA]
tags:
  - GA
  - 네이버
  - 실시간검색어
  - 검색어
description: GA와 GTM을 이용해 네이버 실시간 검색어, 연관 검색어, 토스 행운퀴즈 등의 마케팅 성과 알아보기.
---

최근 네이버 검색 순위가 심상치 않다. 심심찮게 매일 상위권에는 광고 문구가 쏟아지고 있다. 바로 토스 행운퀴즈와 캐시슬라이드의 초성퀴즈, OK캐쉬백의 O퀴즈와 같은 다양한 광고 상품에서 사용자에게 리워드의 대가로 네이버 검색을 유도하기 때문이다.

물론 좋은 현상은 아니다. 하지만 디지털 마케터라면 이런 4천만원 이상을 호가하는 고가의 광고 상품을 집행했을 때 그 효과가 어떤지 정확히 측정을 해야한다.

이번 글에서는 네이버에서 검색하려고 검색어를 입력할 때 나오는 자동완성, 연관 검색어, 실시간 급상승 검색어 등 네이버에서 사용자가 할 수 있는 다양한 검색 방법 중 우리 웹사이트 방문자가 어떤 방식으로 검색을 하여 접속했는지에 대한 정보를 구글 태그 매니저와 구글 애널리틱스를 이용해 수집하는 방법을 설명하려고 한다.

## 이 글에서 다루는 도구

- Google Analytics
- Google Tag Manager

## 작동 원리 이해하기

네이버의 검색 결과의 주소는 <code>https://search.naver.com/search.naver</code>로 시작해 뒤에 <code>sm=top_hty&fbm=1&ie=utf8</code>와 같이 다양한 매개변수가 붙어있는 형태다. 이 매개변수에는 현재 검색 결과가 어떤 방식으로 검색을 했는지, 어떤 탭인지 등에 대한 정보가 담겨 있다.

우리 웹사이트에 접속하기 이전에 머물던 웹사이트를 리퍼러(Referrer)라고 부른다. A 사이트 페이지에 있는 B 사이트의 링크를 눌러 B 사이트에 접속했다면 A는 B의 리퍼러가 되는 것이다. 예를 들어 네이버에서 검색하고 검색 결과에 등장한 우리 웹사이트를 눌러 접속했다면 리퍼러는 네이버가 되는 것이다. 더 정확히 말하자면 네이버 검색 결과 페이지다.

이 때 리퍼러의 주소를 수집하고 리퍼러인 네이버 검색결과 페이지 주소의 담겨있는 매개변수가 어떤 의미가 있는지를 이해한다면 우리는 방문자가 네이버에서 어떤 방식으로 검색을 했는지에 대한 정보를 알 수 있게 되는 것이다.

많은 정보가 있지만 이번에서는 방문자의 검색 방식에 대한 정보만을 GTM(Google Tag Manager)으로 수집하고 GA의 맞춤측정기준 기능을 이용해 GA(Google Analytics) 보고서에서 확인 할 수 있도록 해보자.

## 구글 애널리틱스 맞춤 측정기준 만들기

가장 먼저, 맞춤 측정기준을 만들기 위해서 GA의 [관리] → [속성] → [맞춤정의] → [맞춤 측정기준] 순서로 접속하자.

![관리-속성-맞춤측정기준](https://lh3.googleusercontent.com/MY43S0uOpFps_4PVqelMrYet-r83IX2gz7uhsJAJNXWsganp18TmrdinDE81W05ewehgIgMX9lmXhgKVA1n2HWvW3LTJUREM-DmX5vjlG746qRMXmVal-T_QDYH4bpX45qy_fXcwJzJJPZjZtZi9OgYJrzbIFLqFpjledTYLN_L6ERtvNwqcnGHCoVmqI8hHWNcHQzB3-YmFJ0nLPkbgLnq0XhBoPSDi8cYtz7_FZQSzNLUCpBtk0uy8djAdSqPz9cQDbz-nn-mIF4oEofme3q8n5kyCqQMAzUeLx59RNKeK7oESn1CLaHvngCDmStL2LMzy4KpyUoaZe2NgW2Ira3kY39l-xbhJR5KTaKkxsF7i5h8J2zU9aYYrU5eBTiMnaTrJqzsl98aE-Uy7KvfZmHfT4SWRQJWRzRaJRDyjxrNW_Fg1sdwGOhx1kbu9rD6Yb_m3tnLQnmfUqEXwPA9NJU_DaoUAvKf2MaLEx7jVPwAiIc3Oc81JDjzm53aUs7HQ2HfVmw8ROeakaUxj4f4Wqt3Xft86CkamP8NDC0LcLyk19q7jzAtCqgetNmd-0KLZILSVkxXlTquaoWRJpnPTUyWrxSZ6xyQJwgDqxA4=w1346-h1238-no)

그리고 [+ 새 맞춤 측정기준]을 눌러 이름을 <code>네이버 검색 위치</code>, 범위는 '세션', 사용중에 체크하고 [만들기]를 누르자.

![맞춤 측정 기준 생성](https://lh3.googleusercontent.com/LJk4V2YITdWvMA-PTVLVZ_OROQLnZJIM3D-Xmq0Vz0M-rx7NjaSVNyAv2lZv-_rk7RYmoDFlzTM0BdKqaD9REzVgcIYOKiFk5WOsKUBt7P2-2QeqIBJm3SP0_37nsic-91JSBVhUKVOpgEtWUx83Jo4XPK0nSiIbMBWVw-I7OeWbEp91lelt7ybeK6zjQtek-nlyjM6c2Z4e-BYp8jgzFpkCMfqRFRpblEOjkRfBPo7gfxzfU-Xf3ACgxpZb00h3N1nZ_tE5RFV0Z1yIiFSCdwFzeLx0PWMAaATdSSo1mZcQfbTakhCGUwbWdh8poarIaMmJ9LLZ12Vp96D5o_-giBMEHC44uwryyGtBtPlQruFLbYmeF-pzO6lah5kk1O2hjutWNVWaIgQCNiqi-t2-hEFmPVRCMRdmb6WTeyVv4B3wD-hRdnwedkkHDZzgh2ChYH_IBxLNr_k1I_Q_DpAo-qh0iuswI-q5R8oPXEqKwbqu5jmMjG9sZeljUXx8Wksh0vIi4fApfYz9fP71wqwGz-EO-LpvS7g14vZZoOeHN-J0kcgOkcFUK3CIlAlda2NTESa_2GiEcyOb9TpRK0vaEH0AYYjIEFSp0hKoFEtZrpEtb7IU1JxFXM1DZhiB5L8iqqcpB3aci9mtaEXsBODknRNRYhz2Xyfc7qAOW0Y-K4plvE-Xwwb9JgKy_So3dNaJeKGhrp-TePfvbNf_fzlSnWgZxtFciVJfkxOVPNGzPCZPMBc=w848-h585-no)

이제 필요한 맞춤 측정기준이 생성되었다. 이 맞춤 측정기준의 지수를 확인하고 기억해두고 GTM에서 이 맞춤측정기준을 사용하도록 하자.

## 구글 태그 매니저로 검색 정보를 담는 변수 만들기

### 1. 리퍼러 호스트 변수 만들기

네이버를 통한 유입을 구분하기 위해 GTM에서 호스트 이름을 저장하는 변수를 만들어보자.

GTM에 접속한 후 [변수] → [새로만들기]를 선택하고 [변수 구성]에서 [변수 유형]을 [HTTP 리퍼러]로 선택한다. 그리고 아래의 [구성요소 유형]을 눌러 [호스트 이름]을 선택하고 'www.' 생략에 체크한 후 이름을 <code>referrerHost</code>로 하여 [저장]한다.

![변수-referrerHost](https://lh3.googleusercontent.com/gbfs1IEk5Ab6HTMH9PYGiId0PnzWz4wZ2-ygQ78OxH706wyLcaKt80BrXnZeX32-Q-EV37liLKLip3IQdX60QJ_Cjom5-M_4Bob65okX5LwRpmWdXQpO3kRhiL9bpTCcKsyHC8aoq9n4mKZ80dx-qARlYfAd8dA6yR8RDGxPr0hDwagiNe_I6GZkCpoFezsJLx7uwCX0WztPtg9BWfqiXA0-HC6miM65Vh-iWUFVDcCAznPnV796jK-YN3TxPqqzCMSCaKWZbWlFcJpZEKyL3WamXfNVv5jH7eybtle05o_VHw7SHT0q8FObDItOP01RalY6_B8jnrNSz8mzm0EyaISFvd4QmHECdsZJg5LTXQzE6ANO8A3XLUbYIJbjjg1FX_5TREWQzdTzQmh-GkJFhvrEULcSYrFPimXi23doUoUpDBTfdLZQzArSs48WX6t8On_wL_3vGXiHOYtNflM2pFQJP7JBzQSfv7-DwICWxgeNCIEusv-zV7kYeBhdD7_PtXdgEur0JE10M8U1O7pGsoCal5jStnJwUn7KDKgETo1ejLaAT3EzWDYvaGaP5iyRENuZW-_i-Wb0kK2CSZeAxcbgp7SX8nScgJKW7o7ZkCT8HYYx4wsN6P9_2YqR42FAwBcOffJqOoSo8YfgpSSdA3_ZM8pRU4ZRzs6Mullqlp-9ooGFtFLfm-IrMlYMbKWVducLmYGMJxeNSz-Gie8vPNVQbTND49uPGXD2tfWH03xQ9xc=w1608-h634-no)

### 2. 네이버 검색 위치를 담는 변수 만들기

가장 중요한 방문자의 네이버 검색 위치를 담는 변수를 만들어야 한다. GTM의 미리보기를 이용해 네이버에서 우리 웹사이트로 접속하는 리퍼러를 확인하면 아래와 같은 형태로 이루어져 있다.

> https://search.naver.com/search.naver?sm=top_hty&fbm=1&ie=utf8&query=Ogaeng

이 주소에서 우리에게 필요한 부분은 검색 위치를 나타내는 매개변수인 <code>sm=top.hty</code> 부분이다. 급상승 검색어를 눌렀는지, 연관 검색어를 눌렀는지, 자동완성 키워드를 눌렀는지에 따라 <code>sm=</code> 뒤에 있는 값이 달라진다. 이 값을 저장하고 값에 따라 설명을 입력해주는 변수를 만들 차례다.

[변수] → [새로만들기]를 눌러 [변수 유형]을 [맞춤 자바스크립트]로 선택한다. 그리고 입력창에 아래의 소스 코드를 복사하여 붙여넣고 변수의 이름을 <code>네이버 검색 위치</code>으로 하여 [저장]한다.

{% highlight javascript %}
{% raw %}
function() {
  // Naver Search Parameter
  var sm = {
    "top_hty": "메인 검색",
    "tab_hty.top": "검색 결과 상단 검색창",
    "tab_hty.btm": "검색 결과 하단 검색창",
    "mtp_hty.top": "M 메인 검색",
    "mtb_hty.top": "M 메인 검색",
    "tab_she": "연관 검색어",
    "mtb_she": "M 연관 검색어",
    "top_sug.pre": "검색어 자동완성",
    "tab_sug.top": "검색어 자동완성",
    "mtp_sug.psn": "M 검색어 자동완성",
    "mtp_sug.top": "M 검색어 자동완성",
    "mtb_sug.psn": "M 검색어 자동완성",
    "mtb_sug.top": "M 검색어 자동완성",
    "top_lve": "급상승 검색어 - 메인",
    "mob_grd.lveallnow": "M 급상승 검색어 - 메인",
    "tab_lve": "급상승 검색어 - 검색 결과",
    "tab_lve.allnow": "급상승 검색어 - 검색 결과",
    "mtb_lve.allnow": "M 급상승 검색어 - 검색 결과 하단",
    "mtb_crk.allnow": "M 급상승 검색어 - 검색 결과 상단",
    "mtb_lve.allnow": "M 급상승 검색어 - 검색 결과 하단"
  };
  // © 2019 ogaeng.com All rights reserved.
  var refHostname = {{referrerHost}};
  var fullReferrer = {{Referrer}};
  var url = new URL(fullReferrer);
  // Active
  if (refHostname == "search.naver.com" || refHostname == "m.search.naver.com") {
    var naverParam = url.searchParams.get("sm");
    var result = sm[naverParam];
    return result;
  }
}
{% endraw %}
{% endhighlight %}

`인트렌치 컨설팅에서 위 소스에 사용된 매개변수 정보를 수집하는데 도움을 주셨습니다.`

![네이버 검색 위치 변수](https://lh3.googleusercontent.com/eYpB_FBU1IMD3u0HJ0YJUdqDaLJyVP9dWS340D4IVVPuRRF-JxsaPFpomyT60F5KJ_bb_YrGEq3J3XVVZDRhBN8zysMf0HFhP-bzs8MMjNgjhgRRZYCYMIImSbcV0V93a4iGznLlBoYiOOs7uJBB7fuU0Rfb4GFnU5YRdI5X17RWb5vLZjDPYuy5-DZyhrZ1dsfSHcyrnCL9xy0dUmwXHtvBqPDXQw5C7sZ_GQR3r1N3XNNmRCUWuo-pnaxehx0s-RCcrhrUj_AYgCzmcQjCfLTIBm-liouI4aIRHyJxbxF3VtIvULs4nqOc2ZRFYSBU5uw6hq5LW1AyTdhSW1a_PehXvTaPpHZatPP2RCGwaB60mTxbK1cDsEqLbde1KukmDLs9GFm8K81ZLLOc-05Wxy9VI4TTNq-MqITVlr39wuwd4SANVjVGfxUitok77KcBQgPl_JgtCwP8cSFmyKMV1Xd44uWlrYkFM7IDyMdKWa9IoDtbQZQAfIoBl65vxDhJaWz9c3l3YfI4InX0Mtjp74X-r2iAz07_tr-iztKbpRelmzyidufAtM9iyIZQsTisPq_rJ-c383XMCban8lSr3J_R06Mn8HxcozzRsTgnGuQI-iBOr0g2E4ORRRPr4AAW-ZmPUwcxP4fl3maI58IYQ2C4cDxvD4dMdXYzWuDZBwav8SSMPuClL7YHiEIewk78KHp9GN12midwwFmaIv251HhIvT6GxTDWGK9Wdz3vXNrJwqE=w1604-h1076-no)

이 코드를 통해 수집하는 검색 위치는 아래와 같다.

- 메인 검색(모바일은 M 메인 검색)
- 검색 결과 상단 검색창
- 검색 결과 하단 검색창
- 검색어 자동완성(모바일은 M 검색어 자동완성)
- 연관 검색어(모바일은 M 연관 검색어)
- 급상승 검색어(메인과 검색 결과 우측, 모바일은 M 급상승 검색어)

## 구글 애널리틱스 태그에 맞춤 측정기준 추가하기

이제 네이버 검색 위치 변수를 GA 맞춤측정기준과 연결할 차례다. 이 글을 보고 있다면 이미 GTM을 이용해 GA를 설치해 사용하고 있을 테니 이미 만들어둔 GA 태그를 열어 수정하면 된다.(절대로 새로운 GA 태그를 만들지 말 것!)

[태그] 메뉴로 가서 이미 생성되어 있는 [GA 태그]로 들어간다. 그리고 조금 내려가서 [이 태그의 설정 재정의 사용]에 체크를 한다. 그러면 아래에 [> 기타 설정]과 [> 고급 설정]이 새로 생기는데 [> 기타 설정]을 눌러 나온 하위 메뉴에서 [> 맞춤 측정기준]을 누른 후 [+ 맞춤 측정기준]을 누른다. 이제 [지수]와 [측정기준 값]을 입력할 차례다. [지수]에 입력할 값을 알기 위해서 다시 GA로 돌아가서 [관리] → [속성] → [맞춤정의] → [맞춤 측정기준]으로 들어가 먼저 만들었던 맞춤 측정기준의 지수를 확인한다.

![맞춤측정기준 지수 확인](https://lh3.googleusercontent.com/cWcM5YkvEAEWvVsMW2-qjf94431hk0VXgKnp2mrxGLJkSG6lW1yfLlpDQmKo77JR_RaisnfERGTlEHCKDNt4S24QQT4D462q1ka8ZGNww2HNAGqjCEnJq42X7Kd6NSlhr9HVNDygm5Bf5x6_idvTw2ekb1GRqRg3ORZFZ4BLjXJWF0YwyYK07QX0UUXMBdsMTsZ7HJbkqEWu_F_-gyaNuSXuSTJJSyiAS7eajU1Qf6oEyaO4_TTrAO6t2vhnacgOyuasmXXmcBuNhN3cs4d9i2Jyx5wwvt7wn2SBxvTUlnQY9FQcAA_7cKMfBYRKykV5EKRilFNwolpbLMFd5rZx_iRtJH8EaE_0eAx7jM8d-iqYgYypCHusjUNCC5BUUVOgLDufvAaMM_--Nw1FwP8n3Cz8jg4yPVi6V8knaRO5Y0lpjRojPdw-wD4kOlrlaCrqzeY58qqAFa4mxB-XtLWLAhJ8VowOEhHra0UgKJ60LRvEIbzVzB_z1ImsG0bZSGZynugtjL4yF9HeATJPRe_tvTrxZd_otwX_hhHul-4E4_v19-fJjqwtU35TEywDmW-Djhr_JC0RgO0CGaxEL5Oa9cN-Torrb24Iw181qQp9aMvTQ7QWHACk5aKZ9ZpBrOfG0_L_WSj68sTJlQDlmKmrogqI66nIOB2sU28GOdAxwmyEGbd3VcerLwMqkFJxfYxTn-UGRblo9La-W7JqT6CuM-pwOvUTlN7svioSisG3VFEAYaE=w1474-h398-no)

위 이미지에는 '네이버 검색 위치'의 지수가 1로 나와있는데 이미 생성해둔 맞춤 측정기준이 있다면 지수가 다를 수 있으니 본인의 GA에 나온 지수가 몇인지 꼭 다시 확인한다.

지수를 확인했으면 다시 GTM으로 돌아와 [지수]란에 해당 번호를 입력한다. 이제 [측정기준 값]을 입력할 차례인데 측정기준 값 입력란 오른쪽에 있는 +블록 모양을 눌러 '네이버 검색 위치' 변수를 선택한다. 그리고 [저장]을 눌러 수정을 완료한다.

![GA 태그 수정](https://lh3.googleusercontent.com/iWVXcNQYZiS4X77ay7zxMlQAa8jtZqZuHoN0M4gT4FQNTdO_oGjqFsNIB87eqfRrSj_6bj4QPtCqCuUYxFM-pJDSXtZejTchQTIpglDHbtxEQrH-FoHU5tp413g10-CaO6HH40gV1lfhsDwPvXzuVCgoeo69o5dMUYdNaFyHtmEIDZHtQfKbhNTPABenahZilDRtBa5V36E3NR3C8a1dHqRB3G4ZxTSvDjxccnyg0ncevpeFiLrn8pKxRsQz6ElTlrPz8vjd6tXDyyHnJN0CeQzVCh3aGu5V_qbRfwqTvQrfSLCcRA9N3LqxA9WqS2zJsrVSO3jThigPPxJfRuLgn-yFFZotv2J5xwku5WQ-KRNizLgfci53AnI8JHjGDWQCL_yHN2oDVBvrlsUJTpS3CGQufAgKQBpPjm-rCSAPLaXrBAKbPqMyJ1pQ9dwxForanBhN22D9gwJXqKiLoxaUGefH0nuDnrRorgfMfSA6GRxHjaTwBUGYyt4qo72Yra0GXqtb_wyGeEf0gbTfXqCLIyHZI3vcUtl-CROCn-kqWS4lPcCA1gFISqw2EY68wckM-msPi-O0OrEyobRSDOL07i1f4K5Ratn8wAVR7LyE22a3F-z84ls9hRsEceyOEySQiRMEWS6y73v4wKqXVlp6OnizOKgpX4ufUaQDJfBrmHaF5uvAHTan0PIRZEc7K_ZFmHEOFJJcrggLhXxKJnEk6jbqAvsafAp3rX_RlhGl4FYuxu8=w1866-h1506-no)

## 작동 확인

이제 우리가 설정한 맞춤측정기준이 잘 작동되는지 확인할 단계다. 먼저 GTM 상단에 있는 [미리보기]를 이용해서 확인해보자. [미리보기] 버튼을 누르고 아래와 같은 메시지가 나온다면 미리보기 상태가 된다.

![GTM미리보기 메시지](https://lh3.googleusercontent.com/4f8SU9uppPTIdqfvQhUqZw9WFBiEqk2ZHQRaN-wS-poiMMbIL18RjCifM58nfjTLlyjc0P3aoud-L7gjx91WaHCL7VHPhEnvWTFiVrG0aKRBWzu16vnTFGRRYkk43EyurrUrLLvN4KlvJ1sTRZlNcoGsK8UAt4q7ToDaWi61NKMZhhwSuG1MznHiHgWXC9DYD9ppEeQE7o8FAQ53w4rcQwsiRbf0Lp7YnmuPcq-zn_r2pKvflXClxIre2-9UDSjNZjJQuu5UbjSfe1zLWR6UhZScNfprbUzmy5nb0bXRwuWAyCZ0wpnG63ZUu67iCEMhr35Zedg46rKGVlcd5mS72SBI_RjEy6rTKp0Xrpejtdkizs2DHF5F2-1D-n46rBwRsYFGSXk1P9z_-ARs2BFmn9D7G_xUQFFe6yA5vdJ9hJzhJ-3gOVhGxzbWV11QAsekyhysiQq2ud6pZ56tCrDmzRJCwKQtOE4vzG8FEvPWABy7BJuKJ4yuFa2L_eIDs1vHIsZ1xfBIKhzhK1dliibl09mHWx2LoQ48xShDeA2dHplnYYlY3F0QjxL-w08tcjZFRsOPcTaMvnIBr_blR_omWqT-hMJYOW9PzJkMn-0=w1574-h278-no)

이 창이 뜨면 이제 확인을 할 차례다. 네이버에서 우리 웹사이트가 나올만한 검색어를 입력해 검색해보고 그 검색 결과를 통해 작업한 웹사이트에 접속해보자.

![GTM 미리보기](https://lh3.googleusercontent.com/njr6LyFhLLGJfcHyUqYGk_v7W_3OhIiGi2AhKlpCaThR80pbJNmd3t2jRGsQDN7imeWI_GVn_txmPrTTzpCQLgAEXaDV-G0ed74WEiM2_gsYr0WZLQZ4798borYCjKUJH9Hwgq3S0anvVWXaK2MOV5gaeNhhwfwwgGJbqNc-snn0j43eDwW7byWNGC8J8ppTPm62W2FbNlaZbO_eL5Gk0Dpb5uVAYbL91p2Uub4Mnu6l4S10uV3QLX17GivvIgl7zNIlm_wJ6Yw3GaS_boahQTNeI6-dzJ0oS6NC4B3PfESF67KofV3da7nW8nkC37SHf4s9XovN6-1D4a4gnTGMxICxWN7kbzmGQrHFt966D2TkiIdP8rAQwGgez65lJyC0RHJYu1np6rnjM8yP8FP4A2OvWP3DXD8L5u6lksr6yWnlhjz3U8Ww0hd6G0YPrrRfMxIr2sTZey--l0dyIbxRNcawH6co6rQm4mI9TNmVeXLkB_irTWJ9s43TwUi0DK8GqtSwwbCzQj-72aTEPalJLQ0dc-IwGRwSemBzYbjN2xLlkLOv4nQRJrpmyfjiMcgwDt8J8lLX6BumrqlG7a9E3p5j85tRKBp65qmYyiZm03k2Nfv5PT44_ZB4Nd-xt7_sSQkfrBOiMwBQYbQo_mV6FgggpmgFAA0puyVMDpZKOz6_IAMyOtYINQGthld7qw-D-P5-OOfpEvaqQ-GAkYTejBUqezXgztKkacWL_2yiUZuazp4=w1794-h486-no)

미리보기 모드를 켜고 웹사이트에 접속하면 하단에 GTM 디버그 창이 등장한다. 여기에서  'Tags Fired On This Page' 부분에 현재 작동 중인 GA 태그를 선택한다.

![미리보기 확인](https://lh3.googleusercontent.com/Mn7Uh5iuOvulXylvHXwbpHJyJxvdvzCtHcros739_YZ-wgvi8FAGX8KjLSoxF6UPxuvlnTGA8X53SVZ0iutfgxtuj_DwfPxqV50FIK7v4Dz6z0mz6J2c9r-i_-TDm131VBT9_zJFPhXkSwT2tBI3mLXgWOk9yjalsQDpty6xYwfELP2G-QWuS23W79gCUBEuZXIVrW67xmzotEmzZdXZYr8zbLlK-xZun-dKvE9IQOBYpVerm7LJ0jYv6DmZJnrpQP-4Bt_o9lkj7dUTm-CxYDZsd3an0dh2cjtwJezzhNp8ub5MG-TBjyFO2PjY6HAK2me_JnCrZZvjH-KFCTgltK83dhfTCYzEXN46JS9o9hqixHI1IOiT3igNPHDMEQfDZ5fHnJVzpjeGO8hAhMiby1Wz0VzjiUR88w7mTFQ92nz-010U_ULKAgt3QxvafS2K-BMkPyD9A6H1WHeLRtSGypl3V6sP33OhLPqcWm-oKupx_vPA_0zfUS2RfMh3xOyquK7Bn3DbOMzlinpyNUUDctLym3GgxwWqwzcHxLNYmgzkS7BAIAE3dxAA1T0TznLwQ8676aOiuzi8P4s8mCzV2ewS4AXYlGpP2VvK3C5k_IpTpAD0k-tUn4wLVkt3XdISyWIaKr2fEcUfqS3b8X_YXDlYfomaiw3xokMwe3J5ZmbiXc6jYbWUoOxLGajzgxlsmrCbjRCgUB-AH-fa6ucLC92vTxujQaSZ2Nlp0Qs5kEkcuLU=w1368-h1188-no)

태그를 선택하면 해당 태그의 자세한 내용을 볼 수 있는데 여기에서 '맞춤 측정기준'을 확인할 수 있다. 방금 설정한 지수 1번에 메인에서 검색해서 들어가니 '메인 검색'이라는 문구가 나오는 걸 볼 수 있다. 이제 잘 작동하는걸 확인했으니 GTM으로 돌아가 [제출]을 눌러 작업을 게시하자.

이제 GA에서도 우리가 만든 맞춤 측정기준이 잘 들어오는지 확인할 차례다. 실제로 처음 맞춤 측정기준을 만들면 보고서에 실시간으로 반영되진 않고 일정 시간이 지난 후부터 보이기 시작하기 때문에 GA에서의 확인은 다음날 하는 것을 추천한다.

GA에서 [획득] → [전체 트래픽] → [소스/매체] 혹은 [자연 키워드] 보고서에 들어가자. 그리고 보고서 표 상단에 있는 '보조 측정기준'을 눌러 '네이버 검색 위치'를 검색하고 선택하자. 그러면 아래와 같은 보고서를 볼 수 있다.

![GA 보고서 보기](https://lh3.googleusercontent.com/VMwd7wLMunxoq0f2wjDmR7hfsBDx6NUFnvgdkric9RDP_dnkN0AD3mV-5dvLgBhXbDTPmzqCL6QG3CGIy6qTVXsxBB2X5JopiiQIvETyqIUXsJvzaCPhsZ100RvFA98acFblt-vudpJM2fnDlQUOQQ_u8oJeYcrAYirzZbtuKvRJKOIYh2swThx0DTcbe59-7NkU3iRjMD3iIBjx5OznPD63h_6vANMn7gkLWpSPnSsMKaQhlyV63FbEuwSypmWcZmiXaEcqsmXRz9ulpX9VW2t5X-SywacRP2m_lAMI-q5sA79qrkGbZI8829LHphib1Dbbule25xUAiJ1NZOh6RpS1VpdwOJM2TJ-NwkkCxbIRHYsTRw3jCqAKMMiJe35nGaWojX0QIN6IKrSpd8MfK-PTBudIQbD5AdokVrm1xdIKplu_VgYhsO6NVqdLO8NgO0HbqHy1-NgHntqXucolLwLdB05aZ12tyULE-XWW4Yc03W2oU1L5NQADzT2dPmYPmSeZJeQDho3Pr0qc1_2530oO46D9ard3Hhj6kFhz2SKvDn59xa08qTFKolRJBx2u6aZkUf56Vs1_ELel7Db8JvOYPI8BQpCLQMOdiCBZ2HLuNFVzp8iicjec26tiasy_7B5z6v8aP-VelHAx_xvjMsFOlnm-4cQozUkS4HJ2Zoi8Wy3mxlGR5Tk4OZqtSmfV46Q2Gz4v1ya5dQunYPK_gcCR7GOz1eWO7-BXYv8OLAfLkUg=w1204-h556-no)

이제 이 맞춤 측정기준을 통해 방문자가 어떤 방식으로 검색을 했는지를 더 자세히 알 수 있게 되어 최근 유행하는 행운퀴즈나 초성퀴즈의 실시간 검색어를 통한 유입이나 연관 검색어 등의 성과를 정교하게 측정할 수 있다.

## 정리

GA 맞춤 측정기준을 이용하면 GA에서 제공하지 않는 정보도 직접 만들어 사용할 수가 있다. 항상 이야기하지만, GA와 같은 도구들은 우리가 의사결정을 하는데 보조 역할만을 할 뿐이다.

이번 글을 통해 최근 많은 기업에서 시도하고 있는 실검마케팅이나 연관검색어 등을 통한 유입이 얼마나 되는지, 어떤 효과를 가져오는지 더 세밀하게 측정할 수 있다는 것에 의미를 두려고 한다.

마지막으로 GA를 이용해 네이버 검색 위치를 추적하는 방법을 정리하면 아래와 같다.

1. GA 맞춤 측정기준 만들기
2. GTM으로 네이버 검색 위치를 수집하는 변수 생성
3. GTM의 GA 태그에 맞춤 측정기준 추가
4. GTM 미리보기로 동작 확인

이 글을 보고 따라해보고 잘 작동하는 것까지 확인했다면 댓글로 인증샷을 한 번 남겨보면 어떨까?
