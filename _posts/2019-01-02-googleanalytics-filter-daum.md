---
content_id: 12
layout: post
title: 다음 검색 광고, 구글애널리틱스 필터로 쉽게 추적하기
date: 2019-01-02 01:35:00 +09:00
author: "Ogaeng"
permalink: /googleanalytics-filter-daum/
image: /images/thumbnail/googleanalytics-filter-daum_thumbnail.png
categories: GA
tags:
  - GA
  - 다음
  - 검색광고
  - 유입분석
  - 검색어
description: UTM 태그 노가다 없이 GA 필터를 이용해 쉽게 설정하는 다음 검색 광고 추적하기
---

2016년 브런치에 처음 올리고 이 블로그에도 올라와 있는 '네이버 키워드 광고, 구글 애널리틱스로 쉽게 추적하기'라는 글이 있다. 많은 분께서 그 내용을 참고하여 네이버 검색광고에 적용을 하고 업무 능률을 높일 수 있었다고 감사하다는 말을 많이 들었었다.

네이버 검색광고를 운영하는 곳은 대부분 다음 검색광고까지 함께 운영한다. 그러나 다음 검색광고는 네이버와 같이 치환자를 제공하지 않는다. 그래서 또다시 키워드 하나하나 URL을 설정하는 노가다를 해야만 했다. 이런 노가다를 줄이기 위해 이번 글에서는 구글애널리틱스의 필터 기능을 이용해서 쉽게 다음 검색광고를 분류하는 방법에 대해서 소개하려고 한다.

> [네이버 키워드 광고, 구글 애널리틱스로 쉽게 추적하기](https://ogaeng.com/mkt-ga01/)

## 다음 검색 광고의 추적URL 기능 사용하기 ##

우리 눈에 자주 보이지만 많은 마케터가 놓치고 있는 부분이 있다. 바로 URL인데, 검색엔진 혹은 광고캠페인의 URL을 살펴보면 많은 정보를 가지고 있는 걸 볼 수 있다. 이번 글에서 다룰 다음 검색 광고만 하더라도 광고를 클릭해 접속할 때 아래와 같은 URL의 형태를 가지고 있는 걸 볼 수 있다.

> http://www.example.com/?DMKW=광고키워드&DMSKW=검색어&DMCOL=PM

위와 같은 URL 형태를 갖기 위해 우리가 가장 먼저 해야 할 일은 다음 검색 광고의 추적URL 기능을 켜는 것이다. kakao 키워드 광고에 로그인한다. 그리고 [계정] → [광고대상 관리]에 접속한 후 등록된 웹사이트의 추적URL을 [ON]으로 선택하면 된다.

![카카오 키워드 광고 추적URL](https://lh3.googleusercontent.com/wrKsc3QMiFoCE9A-dVkjT8pj0dTKVotmYuIrsDNlb30JdZJqQLAShFrzKRtAWMXuR4l2vjtJSTu4i4IruFbMkf25_qPR2HJtuCw7VwbbcJAQWYGOJ43J2WWGRb9ejWFZlORVH-oHd1_TEwX9gnC6H93BRwhsTFHb8y2J7Scp_f6S0eK1UyBFrDNoOkY1sLfzC1YUaGchQP7tM_JhZ9cMXraIIeQTizm8tAbWzCIsz44VPPSwgrQQxbCmyeOnkxqYPPiUFEYH3o1NMEvXE_fxpMHMoWMl9oqWvMZvHxPsVpv8Pb740573HKL-he07hmsLUGmfUXyVJtqOprckpnJ2I6D4OrtomEDdXJlbhFDg4F8GxsClyD4eiHVHq9CGzF4z3Dbi2Q_06Zzp66tVRC806dOrXxAItD-YZRRF6ITOfogD1uNTjAYRTARfwE3XhMEe6fodXX9ufCrendII9qkV2waeLs-OVHxWHwwzCNwGGygT58El8YvopCy2XsqJpoqHk0QR2APk6L7gFwgEULocRJB4kkffuXIv7ZG9sftp9Xl7ZwRRr6QZD_IM3YyUMklA6s-OfT_rCtpV5i3sRNli_DHT-ihycPOJzdI99iWx2SYL1F3eHgQw8iixI-ASRhLrZFoGwrQYEglsHjxa79aCiRenYTIwj7-SmTvEerbnlFiyUK8gLLONr9ZbKw8cyEJGSimmW-GLYVWNSNAXUA=w1744-h520-no)

추적URL 기능을 켜면 다음 검색 광고를 통해 접속했을 때 URL 뒤에 다음과 같은 매개변수가 붙는다.

* DMKW : 광고주가 구매한 입찰 키워드
* DMSKW : 검색 사용자가 검색창에 입력한 키워드 값
* DMCOL : 클릭이 발생한 광고서비스명

이번 글에서는 방문자가 접속한 URL에 위 매개변수 중 DMKW가 있을 때, 구글 애널리틱스에서 다음 검색 광고로 인식하도록 필터를 설정하고자 한다.

## 구글 애널리틱스 보기 필터 ##

구글애널리틱스의 데이터는 필터링을 통해 필요 없는 데이터를 보지 않을 수도 있고, 우리가 원하는 대로 정리할 수도 있다. 필터는 '보기' 단위로 설정이 되는데 필터를 사용하면 설정한 필터에 따라 데이터가 변하고 한 번 필터링 된 데이터는 다시 되돌릴 수가 없으므로 필터를 사용할 때에는 항상 신중해야 한다.

먼저 구글 애널리틱스에 접속하여 [관리] 메뉴로 들어가 [보기] 란을 살펴보자. 그럼 [필터]가 보일 것이다.

![구글애널리틱스 보기 필터](https://lh3.googleusercontent.com/1TbJFicWsICtNellsuBTNZL_jkMUwcz24ZZKugF6IcBjND8mTMRsqpOC5JFOG_YeRjdwe-V5zFvrJCAFhLAn0ZnN9CliQcbZ3BGx1RRaNpyToBre9JpsSdgyvCh6q-fQQghaSvKT5fyigKrn0NoHkWFemckFafTWjsY4DCZp0HxdHK5cfeppMy8t5rfzXErjJpNx4mhQHcWhPsS8vmKbjAHxUJmIbgMtf1_7pWw29Sx4wEnaAtkkDRU3JNv4OlQvow6dXNhkJK-W3XMJPQ3p6zrLmzvHuREkItcNc3-YAIdL0fg1JGVrjQ_A9Edt3owbR-0S5O1Sws9tafM1v7rswSKvKxAKzLj43tFT63D7Ha_87sNVFaUZ5kk0015mhRkp1Lf2jxTv4ARZxowaL5T9YF-JiruV11AlTWOz0L3RAnZke8jXikc5I5rjwhqUK2M70ibVsDogGj-8AonS8CJOxqn0gW02F3SFnfqgKg4eqhB-vc8IDdQdwDTZtKXXDshRFHSbbg3Xqg1IijAiECP3aTTFHGofEM_J9E6FbsO2xOjbWMGmJkKW1HHNF7QTYNguFpuszqLjMgfiA8FjD3BDe2CpZmmssHP3b7l337QwDHRtCU_I0OJbL1HOf2UTLbUj85HjfR_jnOQM28VlOuHkLtdVNVP8cbtFSrBYyjRo_FPu0CB5r4uzA0-m4kqxSzOudrgvi2AyFOJ5t9RYyA=w796-h1244-no)

[필터]를 눌러 필터 메뉴에 들어가면 아래와 같은 화면을 볼 수 있다.

![구글애널리틱스 필터 메뉴](https://lh3.googleusercontent.com/rKqVEOvnOgFyxUvToOvXkxB9W3bTT-4AzgTyMTOf8ckd7Fn_909vfb7l2KD8m-TVfG47517rtGokSjRynVdutyHmAdfDJnqlSBU7rt0-WI7u3l0WCyNSr2c5PoIDzFC0CJ8m1AILxw0vY6j1IED0gdshh1MQoyo6MnxeJy2XF-As7JnZvplvsFsGASbmgbBc6ehFKCbwJVlOs5bqLkYjthmMCxQuI1TNpxVjvw0egdaeqTGWwZ1kNj-EoF0oeoYXNccY9rvX4EKsRGDH3WsoR70uqIHeI-APLN_2huJnrpwRQ_mxSN-XeKZhCNChHzyLFH7kpdb_AO8vAJbUOZMKwCMKkteaqNzhprQ_9jxJ6_Wdt46hkoEEQ4dUTe-KSpM0ZVfdtRHREg_lPEIjoP0WG9drlwhwfssQlhbomY_2UhKsuerffffYI2Qe8lY91H0Mxs2LOjQ3xtOKBiMuDUAmFYX8F9aqHUWKxBffKNxHiOF2lNVstkRr4DwH6a3DtzJX0zmkgEljUzuN2HvF82aQZfpoDaeM0KE81ZTZxx6cHfkExLPTYm-fR4vrISKtrMxdXAzTBkshWhZGXbHzM9Iv5v2kfHBcchGxMFr0C4EfthnzcCv51sp0bsgA4vEldNbNJtwuDnzec-_8iqKs7wY3D-fxLGHEUiNV8EBHKIdFAHQOB08Ecg_HV1vTCJyB_FZdOmL9yJMfgfbCpMGXfw=w1723-h500-no)

일반적으로 구글 애널리틱스를 처음 설치할 때, 내부 IP를 제외하는 필터를 많이 생성한다. 여기서 말하는 내부 IP는 서비스를 운영하는 회사의 사무실의 IP 주소가 될 수도 있고, 담당자의 집의 IP 주소가 될 수도 있다.

자, 이제 본격적으로 다음 검색광고 유입 데이터를 필터링해보도록 하자. 먼저 새로운 필터를 만들기 위해 [+ 필터 추가]를 눌러 새 필터를 만들자.

필터 이름을 자유롭게 적어주고 아래와 같은 순서로 입력 혹은 선택을 하고 [저장]을 누른다.

1. 필터 유형: [맞춤] 선택, [고급] 체크
2. 필드 A -> 추출 A: [요청 URI] 선택, 입력란에  <code>(\?|&)DMKW=</code> 입력
3. 출력 대상 -> 생성자: [캠페인 매체] 선택, 입력란에 <code>cpc</code> 입력

![구글애널리틱스 필터 입력](https://lh3.googleusercontent.com/YAd_DQTAIVriqUS5_LZKz96KxFfK75KH0nllEefuTFQOTJLZwt8mLSOEoUbrXWou0yYK8hvOWc7O4eHHGiGUdmNkPXWHnBg01zKEHz-b1JDgZeSTWKWrufk1sJQu9oSubZ8FE-wKduD1JCjaZGdHRVvrLRoq49ba4Jdbhmgpt4as4GPP99FIKE5o80avF7qWUiEvxSQqMWSIa_aP2vjRvCwR-ZEyiwUUx6_m4R7opNQ0ZZGbFtgB2Ux9NJyPngKM7subWqcHgHcKrDHN_wFXjMY9_NVo1yfXiMyxydxDzTpBt-Q78kIUVhZiRp1di84ar_MZ6om6MCw92MpZ1N3LOP30AQsjT5L9H11TG_wgpGepBgdRQZvAHQj3WiLHG24QnRJRFMH28fRID3fxW7MxdcaeGxr2EU5N6kiu10rrb5MDrFcomXaOA1ZyMcpN4PYclkPHJNkCpqXtlusBT8yyQB_X3wdZvzC0sdnX7Z8OLz-dkk5bc7QcmE2ccBw72Run4Bg6iWYmLHycqQlgVQTsD0gvX9ujHI-BWYbLiOBiuZeH-KNN96mF6aUCZmbJcYOPpjfwj9x8SP_cikFSGBDd4zeY6kxCvcAvZqwjQIc1zUacHeB_Ki-QO_hfcM1fbsQdRbsawx2y20cj-9SNRhGHSqtnhyMh04CnrMnDtCvSaKRZSlt6OntEj7s43vP4sGBE3oPWCsJVK1NvfHek7g=w1202-h1536-no)

위 필터에 대해 설명을 더한다면 방문자가 접속한 URL에 '?DMKW=' 혹은 '&DMKW='가 있을 경우 원래라면 'organic'으로 찍힐 매체명을 'cpc'로 변경하라고 설정하는 것이다. 매체명을 'cpc'로 설정하게 되면 자연스레 해당 캠페인은 Paid Search로 인식한다.

여기까지 완료했다면 모든 과정이 끝났다. 참 쉽죠?

![참 쉽죠?](https://lh3.googleusercontent.com/1tBcPQbnvQNug9juLYFTe7jww7v0AgP9Tqlwm2kUH4c2bN63ZuVqfOWq-WpDnscGyL0gAjYEHpaeKLop4yMIU-GQFm4oj8m_pGUJXLQ3IcPNsMYf2YD7E6YgjzuSAGk-oh43vQysQMlIrwLIhbTn_wSIvYzzRk05E-h-tjd4CuhXrzmYSAvMplur3YHIzVzRQvfcxCoLqe8hfQMEm3sJa8S29m1zUX6FDIjR0_LJn1KvSWwrZdaEikwudKHA4zWczB7t9dhkU2htkXDv020ENaXnpZ83z60Hhd7M9vbm3AmptzBNDkyP47CiRwtrDdsh4pLbGPWEC6XmhmhH_A--42mdycEjQGHWHLuQrV_1sta8wMrGcoVNi7mqLJXqTTB5tJTXkUV7xc9wDOKxVflxQvFoILum4taD7hL9TXQIUB_eNMcW731rPTq9aHW5b395v6JJXx6FVPKk1wATVC2NR0GCcKsimF1KBUT2qXdxme6NIOw3stXM3rMYmXTkBZxPR6PWyqVpYDPUylpm1IxfsuemoWYVtx5HRznH53HxtU7xMfyL3wR3jnjfij3OQ6LlgTEHLuTyLGVWYosc2q20KCiLzTR4dUrlrKDuKlw7VcWfqR_BSBCgn9MF0S2bfu9O9UjX2s8LizWdF-mgwbd44V20Xg-RcNi5J-PHdlEOS49omMxJb3S4hpterB25d2XGZPJaCNYC01pZheCyNw=w1200-h675-no)

이제 하루 정도의 시간을 두고 다음 검색 광고를 통해 유입된 세션의 소스/매체가 daum/cpc 로 나타나는 걸 확인하면 된다.(필터를 적용한 시점부터 적용되고 과거의 데이터는 적용이 되지 않는다.)

여기에 더해 구글 애널리틱스의 획득 보고서에서 유료 키워드에 다음 검색 광고 키워드가 들어가 있는 것도 확인할 수 있다.

> [맞춤 필터 입력란 - 구글 애널리틱스 도움말](https://support.google.com/analytics/answer/1034380?hl=ko&ref_topic=1034830)

## 정리 ##

구글 애널리틱스의 필터를 활용하면 의외로 간단하게 처리할 수 있는 것이 많다. 다른 필터 활용 방법과 위에서 이야기한 URL에 대한 자세한 내용도 기회가 되면 블로그에 소개하겠다.

마지막으로 GA로 다음 검색 광고 유입을 추적하는 방법을 정리하면 다음과 같다.

1. kakao 키워드 광고, 광고 대상의 추적URL 설정
2. GA 보기에서 필터 설정

이번 내용을 통해 많은 마케터들이 흔히 'UTM 노가다'라고 불리우는 업무에 들일 시간을 절약할 수 있기를 바란다.
