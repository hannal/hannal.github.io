---
layout: post
status: publish
published: true
title: 'Django 0.97에서 unknown encoding: X-MAC-KOREAN 문제'
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 1143
wordpress_url: http://blog.hannal.com/?p=1143
date: '2008-04-09 19:10:37 +0900'
date_gmt: '2008-04-09 10:10:37 +0900'
categories:
- "essay"
tags:
- django
- encoding
- python
- "오류"
- "인코딩"
- "파이썬"
permalink: "/2008/4/django-097-encoding_error_on_mac"
---
<p>맥(Mac OS X)에서 Django 0.97 pre1판을 기반으로 개발을 할 때 디버깅 화면이 나오지 않고</p>
<blockquote><p>unknown encoding: X-MAC-KOREAN</p></blockquote>
<p>위와 같은 문자열이 들어간 오류 화면이 뜹니다. 저는 맥 언어 환경을 우리말로 했기 때문에 X-MAC-KOREAN 이라고 뜨는 것이죠.</p>
<p>이것은 Django에 있는 tzinfo.py (timezone 정보 관리하는 역할)가 기본 locale 을 가져와서 그럴싸한 문자열을 만들려 하기 때문에 맥 환경에서 발생하는 문제입니다. 이게 0.97 언제쯤 고쳐질지는 모르겠으니 일단 아쉬운대로 고쳐서 써봅시다. (음냐. 이러면 svn update 할 때 conflict 뜨는데 그 해결은 알아서 하세요)</p>
<p>대상 소스 파일 : <strong>django/utils/tzinfo.py</strong></p>
<p>위 파일을 열면</p>
<pre>try:
    DEFAULT_ENCODING = locale.getdefaultlocale()[1] or 'ascii'
except:
    # Any problems at all determining the locale and we fallback. See #5846.
    DEFAULT_ENCODING = 'ascii'</pre>
<p>이런 부분이 나옵니다. 소스 파일 안에서 거의 맨 위에 있지요. 여기서 문제가 되는 부분이 바로 try 안에 있는 녀석입니다. 맥에서는 저걸 실행하면 <strong>X-MAC-뭐시기</strong>라고 가져옵니다. 그런 게 없으니 오류가 나는 것이고요. 그 아래에 <strong>DEFAULT_ENCODING = 'utf-8'</strong>을 넣어주면 오류 안납니다.</p>
<pre>try:
    DEFAULT_ENCODING = locale.getdefaultlocale()[1] or 'ascii'
    <strong>DEFAULT_ENCODING = 'utf-8'</strong>
except:
    # Any problems at all determining the locale and we fallback. See #5846.
    DEFAULT_ENCODING = 'ascii'</pre>
<p>이러면 django의 디버깅 화면이 잘 나옵니다. 찝찝하면 DEFAULT_ENCODING = locale.getdefaultlocale()[1] or 'ascii' 를 주석으로 하세요.</p>
<p>여기서 교훈 한 마디. 역시 시험판, 특히 미리맛보기판(pre version)은 안쓰는게 정신 건강에 좋다.</p>
