---
layout: post
title: "Jekyll 블로그 만들기(1) - 설치하기"
date: 2020-05-01 14:00:00 +09:00
author: "Ogaeng"
permalink: /jekyll-blog-install/
content_id: 20
image: /images/thumbnail/jekyll-thumbnail.jpg
categories: Jekyll
tags:
  - 블로그
  - Jekyll
description: 루비 기반의 정적인 웹사이트 생성기인 지킬을 이용한 블로그 만들기 첫 번째 단계를 시작합니다.
---

국내에는 네이버 블로그와 티스토리 등 다양한 블로그 서비스가 있다. 하지만 디지털 마케팅을 공부하면서 분석적인 마인드와 경험을 기르기 위해서는 네이버나 티스토리는 제약이 많다. 그래서 이런 제약을 벗어나기 위해 설치형 블로그인 워드프레스로 블로그를 제작하는 분들도 많다. 그러나 워드프레스는 호스팅이 필요하기 때문에 비용이 추가적으로 들고 로딩 속도도 다른 블로그 대비 느린 편이다.

그래서 이번에는 비개발자 입장에서는 난이도가 있지만 좀 더 자유롭고 빠르며 Github Page를 이용해 무료로 호스팅을 사용할 수 있는 Jekyll 블로그에 대해서 소개한다.

## Jekyll 이란

Jekyll은 정적인 웹사이트 생성기로 Ruby라는 언어를 기반으로 제작되었으며 마크다운 방식으로 글쓰기가 가능하다. 어려운 말은 넘어가고 그냥 웹사이트를 생성하는 도구 정도로 생각하면 편하다. 마크다운에 대해서는 다음에 다시 설명하겠다.
> Jekyll 웹사이트 방문하기 : <a onclick="window.open('https://jekyllrb.com', '_blank');">https://jekyllrb.com</a>

지금 보고 있는 이 블로그도 Jekyll을 이용해 제작되었고 최근 많은 스타트업이 개설하는 기술 블로그도 대부분 Jekyll을 이용해 제작되었다.(도메인이 OOO.github.io 로 구성되어 있는 블로그는 거의 Jekyll 블로그라 봐도 무방하다.)

## Ruby와 Jekyll 설치하기

Jekyll을 설치하기 위해선 먼저 컴퓨터에 Ruby가 설치되어 있어야 한다. 루비 설치 방법은 윈도우와 맥 두 가지 버전으로 나누어 설명하겠다.

### Mac 환경에서 Ruby와 Jekyll 설치

먼저 터미널(Terminal)을 실행하고 아래와 같이 타이핑해서 루비의 버전을 확인한다.

{% highlight plain %}
{% raw %}

ruby -v

{% endraw %}
{% endhighlight %}

이 글을 보고 있는 분은 Ruby를 만져본 적이 없을테니 구버전의 루비가 설치되어 있을 것이다. 이제 새로운 루비를 설치하기 위해 터미널에서 아래와 같이 타이핑하여 루비 버전 관리 도구인 RVM을 설치하자.

{% highlight plain %}
{% raw %}

\curl -sSL https://get.rvm.io | bash -s stable

{% endraw %}
{% endhighlight %}

텍스트가 주르르르 넘어가고 RVM이 설치됐으면 터미널의 아래와 같이 입력하여 새로운 버전의 Ruby를 설치한다.

{% highlight plain %}
{% raw %}

rvm install ruby-2.6.6

{% endraw %}
{% endhighlight %}

Ruby의 설치가 완료됐으면 아래 두 명령어를 하나씩 타이핑해 새로 설치한 버전을 적용시켜준다.

{% highlight plain %}
{% raw %}

rvm use ruby-2.6.6

{% endraw %}
{% endhighlight %}

{% highlight plain %}
{% raw %}

rvm --default use 2.6.6

{% endraw %}
{% endhighlight %}

이제 마지막으로 아래와 같이 입력해서 업데이트된 Ruby의 버전이 2.6.6으로 나오는지 확인한다.

{% highlight plain %}
{% raw %}

ruby -v

{% endraw %}
{% endhighlight %}

Ruby 설치가 완료되었다면 이어서 아래의 명령어를 타이핑해 Jekyll을 설치하자.

{% highlight plain %}
{% raw %}

gem install jekyll bundler

{% endraw %}
{% endhighlight %}

정상적으로 프로세스가 지나갔다면 Jekyll 설치가 완료되었다.

### Windows 환경에서 Ruby와 Jekyll 설치

윈도우 환경에서는 먼저 <a onclick="window.open('https://rubyinstaller.org/downloads/', '_blank');">Ruby 다운로드 사이트</a>에 방문하고 '⇒ Ruby+Devkit 2.6.6-1 (x64)'를 다운로드한다.

다운로드한 파일을 실행해 설치를 진행하면 된다. 단, 주의할 점은 설치 시 아래와 같은 화면이 나온다면 <code>Use UTF-8 as default external encoding.</code>에 꼭 체크를 하고 넘어간다.

![윈도우 루비 설치 화면](/images/post/20/01-ruby-install-win.png)

이제 모두 설치가 완료되었으면 시작 메뉴에서 명령 프롬프트 혹은 Start Command Prompt with Ruby를 실행한다.

일반적으로 처음 접속된 경로는 <code>C:\Users\사용자이름</code> 일 것이다. 여기서 아래와 같이 타이핑하여 Jekyll을 설치한다.

{% highlight plain %}
{% raw %}

gem install jekyll bundler

{% endraw %}
{% endhighlight %}

정상적으로 프로세스가 흘러간다면 Jeykyll 설치까지 완료되었다.

## Jekyll 블로그 생성하기

Ruby와 Jekyll 설치가 완료되었다면 이제 Jekyll 블로그를 생성할 차례다. 다음 글에서 테마를 이용해 시작하는 방법을 소개할 예정인데. 바로 테마를 적용해 블로그를 함께 만들어 볼 분은 이 블로그 생성하기 챕터를 건너 뛰고 다음 글로 넘어가는 것이 좋다.

그래도 첫 단계에서 블로그를 생성하는 방법을 알고 싶다면 아래 내용을 참고하자.

Mac에서는 터미널, Windows에서는 명령 프롬프트를 실행한다. 이제 여기서 새로운 블로그를 생성하려면 아래와 같이 명령어를 입력하면 된다.

{% highlight plain %}
{% raw %}

jekyll new 블로그폴더이름

{% endraw %}
{% endhighlight %}

<code>블로그폴더이름</code> 에 입력한 이름으로 새로운 폴더가 생성되고 그 안에 블로그 파일이 들어가게 된다. 이제 새로운 블로그를 생성했으니 cd 명령어를 이용해 블로그 폴더로 접속해보자.

{% highlight plain %}
{% raw %}

cd 블로그폴더이름

{% endraw %}
{% endhighlight %}

이제 이 블로그를 실행시켜서 테스트하려면 아래의 명령어를 입력하면 된다.

{% highlight plain %}
{% raw %}

bundle exec jekyll serve

{% endraw %}
{% endhighlight %}

 <code>bundle exec jekyll serve</code> 를 입력하면 Jekyll 서버가 내 컴퓨터에서 실행된다.

정상적으로 서버가 실행되었으면 터미널 혹은 명령 프롬프트를 종료하지 않은 상태에서 웹 브라우저를 열어 <code>localhost:4000</code> 을 입력하여 접속하면 내 블로그가 실행되고 아래와 같은 웹사이트가 열린다.

![localhost 실행 예시](/images/post/20/04-localhost.png)

확인 후 Jekyll 웹 서버를 종료하려면 터미널 혹은 명령 프롬프트에서 Ctrl+C 를 누르면 된다.
