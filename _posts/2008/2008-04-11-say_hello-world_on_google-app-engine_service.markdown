---
layout: post
status: publish
published: true
title: Google App Engine으로 “안녕하세요, 여러분”  출력하기
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 1144
wordpress_url: http://blog.hannal.com/?p=1144
date: '2008-04-11 17:53:04 +0900'
date_gmt: '2008-04-11 08:53:04 +0900'
categories:
- "essay"
tags:
- app engine
- django
- google
- python
- "구글"
permalink: "/2008/4/say_hello-world_on_google-app-engine_service/"
---
<h3>1. 들어가며</h3>
<p>며칠 전에 구글에서 <a href="http://appengine.google.com">Google App Engine</a>이라는 플랫폼을 공개했다. 소식을 접하자 마자 계정 신청을 했는데 오늘 계정이 활성화 됐다는 전자우편이 왔다. Google App Engine에 대해 보다 자세한 우리말 글을 보고 싶다면 likejazz님께서 쓰신 <a href="http://www.likejazz.com/archives/280">구글 웹 어플리케이션 플랫폼: App Engine</a> 글을 참조 바란다.</p>
<h3>2. 가볍게 둘러 본 느낌</h3>
<p>난 방금 전에 Google App Engine (<strong>이하 GAE</strong>)에 예제를 따라 <a href="http://hello-hannal.appspot.com/">Hello world성 문장</a>을 만들어 뿌려봤다. 마침 <a href="http://www.djangoproject.com">Django</a>도 지원하길래 <a href="http://uw.appspot.com/">django를 이용해서 비슷한 일을 해봤다</a>.</p>
<p>이리 저리 둘러 보고</p>
<ul>
<li>이거 정말 짱이다</li>
<li>GAE를 기반으로 하는 서비스가 많아지면 <a href="http://www.passport.com">MS passport</a> 서비스가 실패했던 것과는 달리 Google account(gmail)은 성공할 수 밖에 없다. Open ID보다 더 Open ID스러운 놈이 될 수도 있을 것이다</li>
<li>기획자나 작은 서비스 기업, 혹은 개발자(엔지니어 말고)들은 정말 신나겠다</li>
<li>역시 구글은 무섭다. GAE는 서비스 개발계의 Google Adsense가 될 것이다</li>
</ul>
<p>이와 같은 느낌을 아주 강렬하게 받았다.</p>
<h3>3. 좋은 점</h3>
<p>요즘 개발 인트라넷으로 각광 받고 있는 도구는 <a href="http://trac.edgewall.org/">Trac</a>과 <a href="http://subversion.tigris.org/">Subversion</a>이라고 생각한다. 자료 판숫자(version)나 Wiki를 통한 문서 관리가 편하기 때문이다. 무엇보다 <strong>협업</strong>을 편리하고 안전하게 할 수 있다는 점이 아주 좋다. GAE 역시 이런 점이 잘 되어 있는 걸로 보인다. 잘 되어 있다고 확실하게 말을 하지 못하는 이유는 그런 기능이 보이기는 하지만 아직 제대로 실험을 하지 않았기 때문이고, svn 같은 것도 여전히 따로 필요해 보이기 때문이다.<br />
하지만 소스를 갱신할 때마다 소스 판숫자를 자동으로 올릴 수 있다.</p>
<p>어렵고 골치 아픈 부분은 신경 쓰지 않고 서비스 개발 자체에 집중할 수 있는 환경도 좋은 점이다. 서비스가 잘 되어 확장을 해야 할 때, 개발 초기에 확장성을 고려하지 않으면 큰 고생을 한다. 그래서 개발 초기에 설계에 많은 시간을 들인다. GAE를 쓰면 이런 부분을 신경 쓸 일은 없을 듯 싶다. 서비스 확장을 할 때 Database Server와 Application Server를 주로 확장할텐데 이 두 부분을 GAE가 맡기 때문이다. 서버 뒷단에서 어떻게 분산을 하는지 신경 쓸 것 없이 나는 API로 자료를 꺼내 쓰거나 넣으면 된다.</p>
<p>아직 이용자가 많지 않아서 그런지는 몰라도 GAE 서비스 자체가 빠른 점도 좋다. 값싼 어지간한 웹호스팅 보다 훨씬 낫다. -_-;</p>
<p>SDK도 잘 만들어져 있다. Rails 나 Django가 그러하듯이 GAE SDK도 내장 웹서버가 있어서 GAE에 계정이 없더라도 개발 자체는 진행해 나갈 수 있다. 그리고 이 SDK 기반에서 잘 작동하면 GAE 위에서도 잘 돌아갈 확률이 <strong>높은 편</strong>이다. 왜 “높은 편”이라는 다소 불확실한 말을 썼냐하면, 아직 SDK 완성도가 완전치 않아서 개발 환경(내장 웹서버)에서 잘 작동하는 것이 GAE 안에서는 오류가 나는 경우가 있고, GAE에 소스 코드를 올릴 때 GAE가 소스 파일 일부 내용을 고치는지 개발 환경에서 뜨는 화면과 GAE 위에서 뜨는 화면이 달라지는 경우가 있는 듯 싶다. 대체로 python 경로(path)와 관련된 부분에서 그러한데, GAE에서 제공하는 문서를 보면 어렵지 않게 문제를 피할 수 있다.</p>
<p>이용자 인증 체계도 아주 쉽게 처리할 수 있다. <a href="http://code.google.com/appengine/docs/users/">The Users API 문서</a>를 보면 알겠지만, Google account(Gmail)과 이용자 정보 기능을 연결해놨다. <strong>Google App Engine 을  기반으로 하는 서비스가 많아지면 많아질수록 gmail 계정 하나로 편리하게 서비스에 접근하여 쓸 수 있게 될 것</strong>이다. 이거 어디서 보던 개념 아닌가? 그렇다. 바로 Open ID이다. Open ID는 인증 체계만 덜렁 제공한다면, GAE는 인증 체계와 더불어 Application을 편하게 만드는 환경까지 제공하는 것이다. 정말 기가 막히고 무섭다.</p>
<p>마지막으로 나한테 좋은 점은 (현재) 쓸 수 있는 개발 언어가 python인 점이다. 난 python을 좋아하기 때문이다. :)</p>
<h3>4. GAE 안쪽 살펴보기</h3>
<p><img src="http://blog.hannal.com/assets/uploads/2008/04/google_app_engine_011.jpg" alt="" title="Google App Engine 첫 화면" width="500" height="264" class="alignnone size-full wp-image-1145" /><br />
우선 처음 가면 Application을 만들라고 나온다. 현재는 이용자당 3개를 만들 수 있으며 한 번 만든 Application은 지우거나 ID를 고칠 수 없다.</p>
<p><a href='http://blog.hannal.com/assets/uploads/2008/04/google_app_engine_021.jpg'><img src="http://blog.hannal.com/assets/uploads/2008/04/google_app_engine_02-300x157.jpg" alt="" title="서비스 로그 영역" width="300" height="157" class="alignnone size-medium wp-image-1146" /></a><br />
Dashboard에는 각 Application(Service) 관련 로그들이 나온다. 서비스 감시 및 현황을 파악할 때 좋을 것이다.</p>
<p>내가 마음에 들어했던 부분은 바로 로그 보기 기능이다. 총 7가지에 로그를 볼 수 있다.</p>
<ul>
<li>debug log line</li>
<li>An info log line</li>
<li>A warning log line</li>
<li>An error log line</li>
<li>A critical log line (most severe)</li>
</ul>
<p><img src="http://blog.hannal.com/assets/uploads/2008/04/google_app_engine_031.jpg" alt="" title="각종 로그 내용들" width="500" height="396" class="alignnone size-full wp-image-1147" /></p>
<p>크흑. 정말 기가 막히지 않은가!</p>
<p><img src="http://blog.hannal.com/assets/uploads/2008/04/google_app_engine_041.jpg" alt="" title="협업 관리 기능" width="499" height="208" class="alignnone size-full wp-image-1148" /></p>
<p>Google Account를 가진 이용자에 한해서 개발에 참여시킬 수 있다. <a href="http://www.google.com/a">Google apps</a>를 통해 계정을 만든 이용자도 참여가 가능하긴 한데 몇 가지 문제가 있어서 깔끔하게 되지 않는다. gmail.com 계정을 초대하고 참여하는 것이 명랑한 삶을 사는 데 도움이 될 것 같다.</p>
<p>당연히 있을 법한 기능 중 하나는 GAE에 올린 서비스에 도메인을 따로 붙일 수 있는 것이다. 이 기능은 Google apps를 통해야 한다.</p>
<p><a href='http://blog.hannal.com/assets/uploads/2008/04/google_apps_setting_021.jpg'><img src="http://blog.hannal.com/assets/uploads/2008/04/google_apps_setting_021.jpg" alt="" title="관리자 화면에 새 기능이 나오게 하는 설정" width="500" height="111" class="alignnone size-full wp-image-1151" /></a></p>
<p>Google Apps 관리자 화면에 가면 아마 GAE 관련 서비스 추가 항목이 없을 것이다. 그럴 경우 위 그림과 같이 도메인 설정 영역 중 제어판에서 “다음 세대”라는 항목을 적용해야 한다. 그러면</p>
<p><img src="http://blog.hannal.com/assets/uploads/2008/04/google_apps_setting_011.jpg" alt="" title="Google App Engine에 도메인 연결하기" width="392" height="161" class="alignnone size-full wp-image-1150" /></p>
<p>이와 같은 도메인 연결란이 생긴다. 예를 들면 <a href="http://hello-hannal.appspot.com">http://hello-hannal.appspot.com</a>에 <a href="http://helloworld.hannal.com">http://helloworld.hannal.com</a>을 연결한 것이다.</p>
<h3>5. 실제로 Hello world 찍어보기</h3>
<p>여기서는 GAE에 hello world를 출력하는 과정을 보이는 것이 목적이지 python이나 django 강의를 하는 건 아니다.</p>
<h4>5-1. 어플리케이션 생성</h4>
<p>우선 GAE SDK 디렉토리 안에 GAE에 추가한 Application 디렉토리(폴더)를 하나 만든다. GAE에 만든 Application ID와 꼭 같을 필요는 없다. 근데 여기서는 Django를 이용할 것이므로 django-admin.py 을 이용해 만들기로 하자.</p>
<blockquote><p>django-admin.py startproject uw</p></blockquote>
<p>그러면 <strong>uw</strong>라는 디렉토리가 생기고 django 관련 기본 파일들이 만들어져 있다. 왜 helloworld가 아니라 uw인가하면 별 뜻 없다. hello world가 지겨워서 unlimited world라고 하고 싶었기 때문이다.</p>
<p>이제 <strong>app.yaml</strong> 파일을 uw 디렉토리 안에 만들고 다음 내용을 써넣는다.</p>
<blockquote><pre><code style="font-size: 0.8em; font-family: Arial;">application: uw
version: 1
runtime: python
api_version: 1

handlers:
- url: /.*
  script: main.py</code></pre>
</blockquote>
<p>YAML 설정에 대한 자세한 내용은 <a href="http://code.google.com/appengine/docs/configuringanapp.html">Configuring an App 문서</a>를 참조 바란다.</p>
<p>보면 / 로 들어오는 모든 요청을 main.py 이 받게 했다. 이 main.py는 <a href="http://code.google.com/appengine/articles/django.html">Running Django on Google App Engine</a> 문서에 설명이 잘 나와있다. 다만 이 문서대로 하면 settings.py 에서 오류가 발생하는데 settings.py 맨 위에 <strong>import os</strong> 를 써넣으면 된다. 템플릿 경로 지정 문제 때문에 그런 것이다.</p>
<h4>5-2. 화면 만들기</h4>
<p>이번엔 / 로 들어왔을 때 보일 화면을 만들자. 간단하다. 우선 urls.py 에 접근 요청 경로와 이 경로에 연결할 함수를 정의한다.</p>
<blockquote><pre><code style="font-size: 0.8em; font-family: Arial;">urlpatterns = patterns('',
    (r'^$', 'views.root_page'),
    (r'^hannal/$', 'views.goto_hannal'),
)</code></pre>
</blockquote>
<p>난 아무 경로 없이 최상위 경로(/)로 왔을 때 views.root_page 를, /hannal로 왔을 때 views.goto_hannal을 실행하겠다고 했다.</p>
<p>이번엔 views.root_page 를 만들 차례이다. 취향에 따라 views.py 안에 각 함수를 만들어도 되고 views 디렉토리 안에 root_page.py 나 goto_hannal.py 파일을 만들어도 되지만, 난 views.py 안에 각 함수를 만들기로 했다.</p>
<p>urls.py 나 settings.py 파일이 있는 곳에 views.py 를 만든 뒤 다음과 같이 내용을 만든다.</p>
<blockquote><pre><code style="font-size: 0.8em; font-family: Arial;"># -*- coding:utf-8 -*-
from django.http import HttpResponse, HttpResponseRedirect
import django

def root_page(request):
    str = '&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "DTD/xhtml1-transitional.dtd">'
    str += '&lt;html>'
    str += '&lt;head>&lt;title>&lt;/title>&lt;meta http-equiv="content-type" content="text/html; charset=utf-8"/>&lt;/head>'
    str += '&lt;body>'
    str += '&lt;p>안녕하세요, 여러분.  이 화면은 '
    str += '&lt;a href="http://www.djangoproject.com"&gt;Django %s.%s&lt;/a&gt;로 만들었고, '  % (django.VERSION[0],django.VERSION[1])
    str += '&lt;a href="http://appengine.google.com"&gt;Google App Engine&lt;/a&gt;에서 '
    str += '돌리고 있답니다.&lt;/p>'
    str += '&lt;/body>&lt;/html>'

    return HttpResponse(str, mimetype='text/html')

def goto_hannal(request):
    return HttpResponseRedirect('http://www.hannal.net')</code></pre>
</blockquote>
<h4>5-3. 소스 반영 및 확인</h4>
<p><a href='http://blog.hannal.com/assets/uploads/2008/04/google_app_engine_051.jpg'><img src="http://blog.hannal.com/assets/uploads/2008/04/google_app_engine_051.jpg" alt="" title="Google app Engine에 소스 갱신하기" width="500" height="327" class="alignnone size-full wp-image-1149" /></a></p>
<p>이제 appcfg.py 를 이용하여 소스를 GAE 서버에 올린다. 처음 실행하면 gmail 계정 인증을 요청하고, 이후엔 .appcfg_cookies 파일을 만들어 인증을 생략한다.</p>
<p>다 올린 뒤 <a href="http://uw.appspot.com">http://uw.appspot.com</a>에 접속하면 화면이 잘 나온다. <a href="http://uw.appspot.com/hannal">http://uw.appspot.com/hannal</a>로 접속하면 http://www.hannal.net 로 이동한다.</p>
<h3>6. 글을 마치며</h3>
<p>기대보다 훨씬 재밌고 끝내주는 서비스, 아니 플랫폼이 등장했다. 시장에 미칠 파급력이 엄청날 것이라고 본다. 기획자도, 소프트웨어 엔지니어가 없거나 경험이 없어 서비스 확장 구조 설계에 어려움을 겪는 작은 팀이나 회사도 모두 매력을 느낄 개발 플랫폼이다.</p>
<p>그나저나 이런 것 나왔으니 <a href="http://blog.hannal.com/some_news_for_apr_first_day_2008/">가벼운 말로 꺼낸 django 개발 책</a>을 정말 써야 하는 걸까? ^^</p>
