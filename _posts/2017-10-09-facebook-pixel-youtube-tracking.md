---
content_id: 7
layout: post
title: 페이스북 픽셀로 웹사이트에 삽입된 유튜브 영상을 측정하고 맞춤타겟 만들기
date: 2017-10-09 14:30:00 +09:00
author: "Ogaeng"
permalink: /facebook-pixel-youtube-tracking/
image: /images/thumbnail/facebook-pixel-youtube-tracking_thumbnail.png
categories: Facebook
tags:
  - 페이스북픽셀
  - 유튜브
  - GTM
  - facebookanalytics
  - 맞춤타겟
description: 동영상은 우리의 서비스를 설명하기에 가장 좋은 수단이다. 특히 크라우드 펀딩이나 각종 랜딩 페이지에서 동영상은 페이지의 상단에서 가장 큰 부분을 차지하고 있을 만큼 그 중요성이 부각되고 있다. 하지만 사이트의 방문자가 동영상을 재생했는지, 어디까지 시청을 했는지에 대한 분석은 많은 사람들이 놓치고 있는 부분이다. 그래서 이번 글에선 Facebook Pixel을 이용하여 웹페이지에 삽입되어 있는 Youtube 영상의 행동을 측정하고 그것을 이용해 맞춤타겟을 만드는 것까지의 과정을 이야기해보려고 한다.
---

동영상은 우리의 서비스를 설명하기에 가장 좋은 수단이다. 특히 크라우드 펀딩이나 각종 랜딩 페이지에서 동영상은 페이지의 상단에서 가장 큰 부분을 차지하고 있을 만큼 그 중요성이 부각되고 있다. 하지만 사이트의 방문자가 동영상을 재생했는지, 어디까지 시청을 했는지에 대한 분석은 많은 사람들이 놓치고 있는 부분이다. 이번 글에선 앞서 작성한 스크롤 측정 방법에 이어 Facebook Pixel을 이용하여 웹페이지에 삽입되어 있는 Youtube 영상의 행동을 측정하고 그것을 이용해 맞춤타겟을 만드는 것까지의 과정을 이야기해보려고 한다.

## 이 글에서 다루는 도구 ##

* Google Tag Manager
* Facebook Pixel
* Facebook Analytics

## Google Tag Manager를 이용해 Facebook Pixel 이벤트 만들기 ##

이제 본격적으로 Google Tag Manager(앞으로는 GTM이라고 하겠다.)로 유튜브 재생을 측정하는 트리거를 만들고 Facebook Pixel 이벤트로 내보내는 태그를 만들어보겠다. 서두르지 말고 하나씩 따라 해보자.

### 1. 유튜브 동영상 재생 트리거 만들기 ###

평소 GTM을 유심히 살펴봤다면 알겠지만 GTM에는 기본적으로 유튜브 동영상의 재생, 일시정지, 완료, 진행 상황 등에 실행되는 트리거 유형이 제공되므로 이 유형을 이용하면 손쉽게 설정할 수 있다.

GTM에 접속한 후 [트리거] → [새로만들기]를 선택하고 [트리거 구성]에서 [YouTube 동영상]을 선택한다. 트리거 이름은 자유롭게 설정하자. 이 예제에서는 Youtube로 지정했다.

그리고 아래로 내려가 [캡처] 항목에 시작, 완료, 진행률에 체크하고 진행률의 비율에는 <code>25,50,75</code>를 입력한다. 진행률 비율은 동영상이 총 재생시간의 특정 %만큼 재생됐을 때 트리거를 실행한다. 조금 더 세밀하게 비율을 측정하고 싶다면 10,20,30과 같이 콤마(,)를 이용해 더 많은 비율을 입력하면 된다.

진행률 비율을 입력했으면 아래로 내려가 [자바스크립트 API 지원을 모든 YouTube 동영상에 추가합니다.]에 체크하고 [저장]한다.

![트리거-Youtube](https://lh3.googleusercontent.com/mjqI0-wD-6KMHERvuiRr0Jb0wJTx82T1jtA1o4Aq8YfFF81qRB3qXdyIwwpTgyi_qEo4_YRrly-wpq0ERLGifNoWIjWGebaIviiwlZeM9HQOG5JrFzMrCky_xgJ4fyED_CjnQ4J-aMfnvQ_rjlHPVSs1L1Y0BrUPvY7e3NrZIeWWC7UxR9Mp0o_8TW0ZhP9_UntRLujl6zedr0Z-GgyoV5_79Ma9OzqNmatHvvLku51vg1gXfKakra7gwKAQgFWON79GsS6g_Bqccfpt5jgEiTO1W0tRPkwvLxaovGKrAO3XcqCIgvgAwmS2_538S4cX5pm-GhS8CHsU5a8HClXKqISJ6GKjfw9JBF42xxhVAHlSlnuXyBBA59CkYtcHAyAUFKB-aHUBWN5ODFXKpXovv_UZ6fYmzXH-CoGzfFF0hLqwgd4ve7lSazEykUxYU5hBkAbRnf-Jxvcq040OL2lu4HoWdjZJt0PuC3fYru4X9u8RSGJoLpwGGsK42jvSoEXyMQGBIEnOipPt3w3WcYg-NKB2l-0hOIKNxYwCvb_gzl0MP-UmWLFBV3zifSnZaJVIoF81L47rLKey2YwLLdeqlzO7d-twpDuF2p5i7AvgiwElzzm5do0x5xEjMaaTKjxSrsTxRGuMS6S6R2QdcWPxLz7FfzxDTsfs4vE=w1642-h1344-no)

### 2. 동영상 관련 변수 만들기 ###

이번에는 위에서 만든 유튜브 트리거가 실행될 때 우리에게 필요한 값을 GTM에서 저장하는 변수를 설정할 차례다.

[변수] → [구성]을 선택하고 스크롤을 내리다 보면 [동영상] 항목이 나온다. 여기에서 필요한 변수에 모두 체크하면 된다. 무엇이 필요할지 잘 모르겠다면 동영상 항목을 모두 체크하자.

![변수-video](https://lh3.googleusercontent.com/vgtW_DHKgZY9Ccjou8pOkyB3V5WzR6fYN3ZGoiOkA3-Rb1JpleNZFuivxMBP-DsiVraVIiOv-zsoDOKiXCViGHhKxmURbhtOTHsriZh_aiQMhZzirjv_l9y5XsZL35zO6xX0ncE143Dthb-ZxDMr725xF9YZNn_ADGzuHsvrpfQIlXvSeat4Vu_CLCmCQlRnP-sr6UPJTYqBJ3Kw8n3n43YjfCGnTlKWSzcjb4zYGfSAfHR-oYauQiaJs5RnQoC8j-wGqKobuORCo0HTI9utLMCwlxVbbrH6ZDHgN2Oz-rjc5FM1So8f2tgYwvkgKnJdM4-OIcs_5lAS05aRlrfvZI-t3Q3aPNqgEvxId3YO0OoymQia0W5zjTPelJht_M0JhkRG2Pw95rS_tv4vfwAxoLACSJn99N8NcOHGpwFwf1tfheageGTpVvpboTGbTMejzniwK9inuXuCBEpA94Lg2i1Yl5YAnHZMTRJZ5sxhr_863Nal_2yx9SOhAYH5yvNDiOs93Qo1gSa_R8q5B4LQye3Y-M1CRtAAxw3ZqxKQ4ISfBjr9_jX1mAhK0EkmOkzviW_8_ZMKBg_s0ZkdeU2X9Q3QSMIP5q_I1GZfIX8xJRSAAKMCrXIICoSmgviD3TM_TzDLAthAxTDuGdf94hep7BSQXhHqObx6MIM=w1270-h1016-no)

### 3. 페이스북 픽셀 이벤트 태그 만들기 ###

이제 마지막으로 픽셀 이벤트 태그를 추가해야 한다. 먼저 페이스북 픽셀 기본 코드를 만들어놓지 않았다면 추가해주자.

> 참고: [Google 태그 관리자에서 Facebook 픽셀 사용하기](https://www.facebook.com/business/help/1021909254506499)

위 링크를 통해 기본 코드를 설치했다면 이제 가장 중요한 유튜브 이벤트 태그를 만들어보자. [태그] → [새로만들기]를 선택하고 [태그 구성]에서 [맞춤 HTML]을 선택한다. 그리고 아래의 링크를 눌러 나온 소스 코드를 복사, 붙여넣기 하자.

{% highlight html %}
{% raw %}
<script>
	fbq('trackCustom', 'YoutubePlay', {
    	content_title: document.title,	// 페이지 제목
		youtube_title: '{{Video Title}}',	// 유튜브 동영상 제목
      	youtube_status: '{{Video Status}}',	// 유튜브 재생 상태
		youtube_percent: '{{Video Percent}}%'	// 재생 길이(%)
	});
</script>

{% endraw %}
{% endhighlight %}

소스 코드를 붙여 넣었다면 아래로 내려가 [> 고급 설정]을 누른 후 다시 아래의 [> 태그 시퀀싱]을 눌러 'OOO 태그가 실행되기 전에 태그를 실행합니다.'에 체크한 후, [설정 태그]를 위에서 추가한 [페이스북 픽셀 기본 태그]를 선택한다.

그리고 아래의 [트리거]를 눌러 우리가 처음 추가한 [Youtube 트리거]를 선택하고 [저장]한다.

![태그-페이스북 픽셀 유튜브 이벤트](https://lh3.googleusercontent.com/l0iDoxSo84MxPoBYQCUS7qaT5K5rHH9vk9Oe5A2_jxlS4hiVwW0kZiCZZqMwa824iMt04IMZ3xBqP43r-gCINIqVqed-AO3cnNdZ6ByvqiRgnSLF7mMWhufB6cP8eTPQnxGH_3rFOrKyyXW2j1_spmGvgjfrHIsuw5D7PccwYvT1oJh40F0ReHslgXMUgFIBZAMNtoBtguWidVnNcHGrEbPE2oRgqB2oMSYAkhDTI-LdWq9_RPNYiD1nMWMYDtf0KEPpPgYl-ygagDjgowRExV2wTztJRdJqV0q6uu--59v8azoPX42PoVOQdgFdc03UIONjgwQ2aA_rQgJZYtziUtmCwF-E7A5hOvNeG1i3SjLM4fCCGtvpc6xy857xjrHYiQFhK43U1wevgJ1BDJ1ZpAwS4biWqo_f88Qbq-mt9cXTqH_jAOnZj-uPzjwTVTJH_rQhJsAoLhtbGExkRtYxtGGH2MZllGTZAJxYsBufTSWDm_8PjCXH3bzvaKk4fpNEXPYsg8l2Kh49w4vVUVFAYi-pqt5bQThpHpvIfzCgcavpZbE9PUxC5U1wOMglfzeNl2MNQ28t1_O2aEIKVG-FGwcrna1WlYwR3kRrdwAvfX3hgf8AzGxs17f9HT36krljbTlLPow0oWz9ELaHyy98lDQfa63pcaLhnEg=w964-h1606-no)

이 이벤트는 유튜브 동영상이 실행될 때 아래의 정보를 수집한다.

* content_title: 유튜브 동영상이 삽입된 웹페이지의 제목
* youtube_title: 유튜브 동영상의 제목
* youtube_status: 유튜브 동영상의 상태(start, progress, complete 등)
* youtube_percent: 유튜브 동영상의 재생길이(0%, 50%, 100%)

Facebook Analytics를 이용한다면 위 정보가 담겨있는 매개변수를 기억해두자.

### 4. 작동 확인 ###

GTM에서의 설정은 모두 마쳤으니 이제 실제로 작동하는지 확인할 단계가 남았다. 먼저 GTM의 [미리 보기]를 통해 설정한 태그가 잘 실행되고 있는지 확인하고 잘 작동한다면 [[Facebook Pixel Helper]](https://chrome.google.com/webstore/detail/facebook-pixel-helper/fdgfkebogiimcoedlicjlajpkdmockpc) 혹은 [[Facebook Analytics]](https://www.facebook.com/analytics)의 [이벤트 디버깅] 메뉴로 확인해보자.

> 참고: [Facebook Pixel Helper 안내](https://developers.facebook.com/docs/facebook-pixel/pixel-helper)

여기에서는 Facebook Pixel Helper를 이용해 작동 여부를 확인해보겠다. 먼저 아래와 같이 유튜브 영상이 삽입된 예제 페이지를 만들어보았다.

![유튜브 예제 웹페이지](https://lh3.googleusercontent.com/f4hDG-I02Zjz0LTPiEp7LxDnrD4ukix1MwZL7aCy6uvLu0gC5BhDwRZbFedwMey56Ko3nxsTt4zHEAbcx-aKxqmtNIJ-OZCAhSiIdJ9z3D6SrQKZWDNM9zusvHrcnMR3ktOMA84Z7jH4e_yC8Y17YuSQANmA62lRM-Q_JiktoaiTLsLTIIXuyb1QEtJrsjLRm8uzhtElVGHwJus9mZjYqE341rjyVu0nR6HggQ2nVPVzjkl2HdYPVUJPIP8qNL74BcjMwBAJGMpFKRO6_I0Fn4j0CVemjSPjrD9owKs3dF9ObLGxIqP2PcMlWkGmBam_2yrjdKpBN6eDiMgqrC-wMOhetnV2nLImLprZoQnJn5dWvvNporiPiJ3ZTD2-TTHmV4XqqJmY5rYwwwJ_BZ8QsRPwAelr5bMh2R-QZPF1BFaue1Vo5DL8Uc0935msYVu0j-zydOAVhMBeRoY0WOTiMWrIs2WH3sWoPG9KIdPvb_3OTc0Zk4y1r67voUTBHdy3mnmFnmhQUCo0Fr8G3xUTSvulL1UZKUcY-t084HyxdmIUvCvHtzPGTbcPJztY6cQYzUnugI5I6VN11ATTAMJxrA5uDtJ9l6cUE_vKjGReCP2lk9nq_PSrzHL_ziDro7DOz_H6rQ0V67Pg7lp3GrsujD-N_BPSFfdaOhI=w1154-h882-no)

이제 위 유튜브 영상을 실행하고 Facebook Pixel Helper로 확인해보자.

![페이스북 픽셀 헬퍼로 확인](https://lh3.googleusercontent.com/ubKeGpVMEqA8Nk51HmSX1pUKtU3KBg4QQTN02fEbe1yui6bDshePxxz62dhBG7tCQGJ4S1fCz1kqaf-z3AM39n0m6JV23PVU2bhP5m8fDarwGfSpg7jLrao6mGBV-iM-RWBh-22t7KF6gaMbDycgQTpz2H-9ckhAjqGvMELeeVoO76hmPAhWjSa5dnJRb76c9nd8yTQxdLF0npnLWWS2KF86pWFO-BhbRu-D94m-PTgzycnC0h99YugM7lAl5wWLwbT59nWi5maijeD4JhU6GFpuI2QPf-gWRS-HWm3VSOzG6cKEGogw_tlUlxYL1t-_eknA-vQ5dNpc1vAUPa_g_20lrw2DMjTbP9cvk3BNxH4ngbu8lpZKJlQeLl7mBhj4XDFp_nur4cvCmuVU52N2_zBT2kJFaA0BeQaAKGiE7RVKE6xay2tKGliXGtUVamgQqI304pVVbuazFVNpyD_DMKT9ttlijM1deWZURqw7n4e_HpvQDr1cuoC18dvSFc99mx152pH5C1-VZgaamGrWLtefPDMS4Sxp2HqZSMAdIY2HDuvliS3DY3VBdjzj2686t9KkLtw7VWDR-6kPXNI-Zgh_azAo5qgJN9dFRloPP4M_bdQiCONferXJiKjeYhyg5Ij3gjbgHoqWsy8XXzu8B_X6OINNFzNtXaY=w1600-h1083-no)

영상을 실행하자 위 이미지와 같이 지정한 정보가 수집되는 것을 확인할 수 있다. 영상의 노래가 좋으니 조금 더 재생해보자.

![페이스북 픽셀 헬퍼로 확인 2](https://lh3.googleusercontent.com/Df-4MIPog3sLXU9X9tlYL4slaa1KqNjpuNQnOkJzKSun82aw1qV3csIvMCCqkBmLXA2E4YDrTrFtoB9FdDsOrbOSJeju5Y4ARE09eRyaMmGWvLTCeHhoOp-FSTJUlgo3ZzkwO7jF1cgAgdTXOQJhVHRljYAKGsc06-4HSD68kzWoiu9DAK9FlvvQATsOPleMQhAMg9apVHECHhr63wGPC6uGVnl3gCMNKrVpV_iC5nOTUjNtxGScpFxEhK2lkeXF5Fnc4SE1EgPB3mclCd9Ssm6AmdXnIgrDLon4_z6VIpwZXYDWjrqLUHF99I5vQ3DE73qkH2hIDCZF5973QtrzXb-gwCNUmdYxC8_taywkb2p66ul6NLTDNBL8CHtUn3PP-JpEsY4IHb5_j4QkT-rm-6EhpCI6hkuBoFRPoL1xviXuPxF-R0tfzdH2PlDqHE9YjjPBHZXBwVVHnntbL1plE_Kb5MiJizBug4eXCezHAuWWq4_whqTc33NQyBn3wggdE791Sac4pAwfN6CN3xqZ30WT_q0NDjeJ6BqA96PTCLm8brlZtaktrO7A4DSYcnvqPLuvZLb-Y7rGa3oazVXZMDqfV11snpW9tmBYepKtnGbZq3bzNCcprkMjUEFD8ee-lwxvrVZViPm94PKhvgR0J210CcFTShde3rk=w1600-h1153-no)

영상이 25% 정도 진행되자 YoutubePlay 이벤트가 하나 더 생겼다. 내용을 확인하니 youtube_percent가 25%로 youtube_status가 progress로 바뀐 걸 볼 수 있다. 이제 잘 작동되는 걸 확인했으니 GTM으로 돌아와 변경사항을 [제출]하자.

수집된 이벤트에 대해 더 자세히 보고 싶다면 [Facebook Analytics](https://www.facebook.com/analytics)에 접속해 자신의 픽셀을 선택하고 [이벤트] 메뉴에 들어가면 더 많은 내용을 확인할 수 있다.

## 동영상 측정 데이터를 이용하여 페이스북 맞춤타겟 만들기 ##

자 이제 우리가 만든 페이스북 픽셀 이벤트가 쌓이면 유튜브 이벤트를 가지고 맞춤타겟을 만들 수 있다. 예를 들어 '우리 웹사이트에 방문해 동영상을 재생하고 일정 부분까지 시청한 잠재 고객'을 기준으로 맞춤타겟을 만들 수 있다.

이제 맞춤타겟을 만들어보자. 페이스북의 [타겟] 메뉴로 들어가 [타겟 만들기]를 눌러 [웹사이트 트래픽]을 선택한다.

![페이스북 타겟 생성](https://lh3.googleusercontent.com/yjipgIfpVxBGlDe78pziGCUPfNxrVvJ74G0z6iYU8Dd9XZZMcBiWJjdZIMpELLU-cojQ3Wmw9cMU7OJUhMTJPfpl-7f5oWyc7eDFipewZDqbqZLp0PsqO47R97m4DVhlD_F4gi0rn2HVp35ngVQZmkRXzL9MaTTMTfbKhKZFOQZjCRhZS2pYiQ8IBt8KNTsVU1nAavUD1bnDsIQwz9v6vXXkmFiWSXXgnXVIC5eSAccrWBOHSk-UFgIgcktUs2PG4BTvEfw7qlkDfjPU8qkQbxGUsDIsGAXhXPMGWMi8TaSCu3t6VL6OpBQcUwcdMYwfVAobvX7ddoUHksNLMnYbs35yjAMAZZ4wkZTlbbXzn85VkxgBHLdLebdmvWx9lAWurdi-nGgq1M0BGSEqaZq9QO-Pt_pDLw5X-5AhX-zOSfZbPIRhD4xX1Mjqqe8eU14SCzS2VHygd0YFgsGj1vYbzFFKPClDVD2J4-NMqMz-YVI7dBkCwMlP7LL2xVdc3X9OPXNFm2EXrdE6GxbINBy4oe_wKO8LpT_J6aWNw0eE8SMABsqpRq4a6VoE_6z9ZxpG-5bZXaZz9i9dVHuIrYP4XylmPUzuCpP6eXCa72k_O8Yqsy8ZIyKzPZi9Lc_Nv2NGzrGSSxfPcJXJsLpnp3nf0_qoLdOk2d3l1DY=w898-h900-no)

다음 화면에서 [모든 웹사이트 방문자 ▾]을 누르고 [YoutubePlay]를 선택한다. 그리고 아래의 [세분화 기준]을 눌러 [URL/매개변수]를 선택한다. 그리고 [URL ▾]을 눌러 [youtube_percent]를 선택하고 아래의 <code>50%</code>를 입력하고 타겟을 만든다.

![페이스북 맞춤타겟 설정 2](https://lh3.googleusercontent.com/Nbq7ZzkXhn0IwR0fEXvH9XlMCGJJQojzLJqIb0dYY1Kk9FKEWutqm6_W-57zAO38j7-1GgLz_zHxeaE61f-CEOSB2VnwR0-NU-jbPJ2GAFo99rh5S8GTIUT16xTgfiMCxut6WL5m6mVshyAtxm91R5Fj01mHCkEaRO-hj2qGN811UxfzPNagkO19_Ipobdchqa1coNWhUp6_yfeI0xWmS9XGm-HyQ3np85jRlPkR4P806rcFa3FvRLNNNLhF3av4H_lxnNi24YYfPX_V0X9o8cWj190Mvs0EBX6UDhxqGwkTuLyIWUsQYfINrQ6fL8UW0GmTGAoCFGrvT463Zv9TD8e8QAkuTWEXOrKgdndTdYQ1XL3Pzhva-ekZ0jNq2-3r6pw00XHO7J4WhyIRw3RaAhDqEdHuWG86EKQdFn2_SjS7MuGGO7KLJdb3cktNaD9ScDwEVHTANlW4SPYDopLYKFWhkDgVu19BFfGKkcqyejwj_LWJboSkpnsmomg-szpyUl6zSI0NPyZ61Pg8CHzZHDb9np8r3rK_YRZ31ddEL5H3hWnRdCW1yYlooh2vhSeCa53j2i1r8h78au7zOeglnFl0rRizLpoJxeUyqq3NO-yX5ee5GhTKFKCp2j5XHw34JbegOpHTRLLLWcfe4muuWvrhObPW-qhSbRk=w1153-h648-no)

이 이벤트에는 동영상 제목, 페이지 제목 등의 다양한 매개변수를 포함하고 있기 때문에 단순히 퍼센트만을 가지고 타겟을 만들기보다는 '특정 페이지의 동영상 완료 수' 등과 같이 매개변수를 다양하게 조합하여 만드는 것도 좋다.

## 정리 ##

이번 글은 지난번에 작성한 스크롤 측정보다는 약간 난이도가 낮아 더 수월하게 따라 할 수 있을 거라고 생각한다. 스크롤 측정과 유튜브 측정 등을 보고 응용하여 자신에게 필요한 이벤트를 직접 만들어보는 것도 실력을 향상시키는데 굉장히 도움이 되니 꼭 시도해보는 걸 추천한다.

마지막으로 유튜브를 측정하는 이벤트의 작업 순서를 다시 한번 정리하면 아래와 같다.

1. GTM으로 유튜브 트리거와 페이스북 픽셀 이벤트 생성
2. Facebook Pixel Helper로 작동 확인
3. YoutubePlay 이벤트와 매개변수로 맞춤타겟 생성

이번 글에선 페이스북 픽셀의 이벤트를 만들어봤지만 위와 같은 방법으로 Google Analytics에서도 유튜브 동영상을 측정할 수 있다. GA에서의 설정 방법이 궁금한 분이 있다면 댓글을 남겨주길 바란다.
