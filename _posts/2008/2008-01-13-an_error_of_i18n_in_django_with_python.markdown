---
layout: post
status: publish
published: true
title: "맥 OS X 환경에서 django로 다국어 기능 쓸 때 주의점."
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 1108
wordpress_url: http://blog.hannal.com/an_error_of_i18n_in_django_with_python/
date: '2008-01-13 17:38:28 +0900'
date_gmt: '2008-01-13 08:38:28 +0900'
categories:
- "essay"
tags:
- django
- i18n
- msgfmt
- python
- "다국어"
permalink: "/2008/1/an_error_of_i18n_in_django_with_python/"
---
<p>맥 OS X (Mac OS X) Tiger (10.4.x)에서 python 과 django 를 이용하여 다국어 기능(i18n, gettext)을 쓰려고 할 때 오류가 발생합니다. po 파일을 생성하는 make-messages.py 에서는 환경 오류가, po 파일을 mo 파일로 변환하는 compile-messages.py 에서는 msgfmt 매개변수 오류가 납니다. ( django 에서 i18n 을 지원하는 것은 <a href="http://www.djangoproject.com/documentation/0_90/i18n/">Django 국제화</a> 문서를 참조하세요. )</p>
<p>해결 방법은 두 가지입니다. 원래 깔려 있는 gettext를 최신판으로 판올림 하거나 혹은 django 에서 문제가 되는 부분을 피해가는 겁니다.</p>
<h3>1. django에서 문제가 되는 부분을 피해가는 방법</h3>
<p>gettext 판올림이 귀찮다면 django 에 있는 문제만 피해가면 됩니다. 우선 gettext 0.10 에서는 확실히 오류가 발생합니다.</p>
<p>po 파일을 생성하는 make-messages.py 을 실행하면</p>
<blockquote><p>`Python' unknown'</p></blockquote>
<p>이런 오류가 납니다. 이건 직접 po 파일을 만들면 그만입니다. (make-messages.py 가 편하긴 합니다)</p>
<p>두 번째는 po 파일을 mo 파일로 변환하는 compile-messages.py 를 실행하면</p>
<blockquote><p>$ /usr/lib/python2.5/site-packages/django/bin/compile-messages.py<br />
processing file django.po in /생략/locale/ko/LC_MESSAGES<br />
<strong>msgfmt: unrecognized option `--check-format'</strong><br />
Try `msgfmt --help' for more information.</p></blockquote>
<p>굵게 표시한 내용으로 오류가 발생합니다.</p>
<p>msgfmt 을 이용하여 파일을 변환하려는데 <strong>--check-format</strong> 이라는 알 수 없는 매개변수를 썼다며 실행이 취소된 겁니다. msgfmt --help 이라고 쳐보면 실제로 존재하지 않고 <strong>-c</strong> 라는 놈이 존재합니다. 그러므로 compile-messages.py 안에서 저 부분을 고치면 됩니다.</p>
<p>이 파일은 <strong>django설치된경로/bin/compile-messages.py</strong> 에 있으며, 파일을 열어서 <strong>cmd = 'msgfmt --check-format -o "$djangocompilemo" "$djangocompilepo"'</strong> 이렇게 된 부분에서 --check-format 을 -c 로 바꾸십시오. 그러면 잘 변환되어 작동합니다.</p>
<h3>2. gettext 판올림</h3>
<p>이건 gettext 를 0.12 이상으로 판올림하면 위 두 문제 모두 해결됩니다. :) 전 <a href="http://darwinports.com/">DarwinPorts</a> 라는 놈으로 처리했습니다. DarwinPorts를 설치한 뒤에 터미널에서</p>
<p><strong>sudo port install gettext</strong></p>
<p>이라고 치면 gettext 최신판을 받아다가 설치합니다.</p>
