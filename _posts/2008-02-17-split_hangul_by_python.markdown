---
layout: post
status: publish
published: true
title: python에서 초성/중성/종성 뽑아내기, 음소로 나누기
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 1118
wordpress_url: http://blog.hannal.com/python%ec%97%90%ec%84%9c-%ec%b4%88%ec%84%b1%ec%a4%91%ec%84%b1%ec%a2%85%ec%84%b1-%eb%bd%91%ec%95%84%eb%82%b4%ea%b8%b0-%ec%9d%8c%ec%86%8c%eb%a1%9c-%eb%82%98%eb%88%84%ea%b8%b0/
date: '2008-02-17 19:32:31 +0900'
date_gmt: '2008-02-17 10:32:31 +0900'
categories:
- "essay"
tags:
- python
- "한글"
permalink: "/2008/2/split_hangul_by_python"
---
<p>기록 차원에서 간단하게 적어두기.<br />
아래 코드들은 CJKCodec 이 설치된 python 2.4부터 가능하며, 전부 unicode(utf-8)로 처리된다. 만약, CJKCodec이 안깔려있다면 <a href="http://cjkpython.berlios.de/index-ko.html">CJKPython</a>에서 받아서 깔면 된다.</p>
<h3>1. 글자 종류 알아내기</h3>
<blockquote><p>* 코드</p>
<pre>
# -*- coding:utf-8 -*-
from unicodedata import name

str = u'가'

print name(str)[:6]
</pre>
</blockquote>
<blockquote><p>* 실행 결과</p>
<pre>
>>> HANGUL
</pre>
</blockquote>
<h3>2. 한글을 음소로 나누기</h3>
<blockquote><p>* 코드</p>
<pre>
# -*- coding:utf-8 -*-
import hangul

str = u'안'

print hangul.split(str)
</pre>
</blockquote>
<blockquote><p>* 실행 결과</p>
<pre>
>>> (u'u3147', u'u314f', u'u3134')
</pre>
</blockquote>
<h3>3. 종성(끝소리) 있는지 확인</h3>
<blockquote><p>* 코드</p>
<pre>
# -*- coding:utf-8 -*-
import hangul

str = u'안'

if hangul._has_final(str):
    print 1
else:
    print 0
</pre>
</blockquote>
<blockquote><p>* 실행 결과</p>
<pre>
>>> 1
</pre>
</blockquote>
<h3>4. 초성만 뽑아내기</h3>
<blockquote><p>* 코드</p>
<pre>
# -*- coding:utf-8 -*-
import hangul

str = u'안'

a = []
for ch in str:
    if name(ch)[:6] == 'HANGUL':
        a.append(hangul.split(ch)[0])
</pre>
</blockquote>
<blockquote><p>* 실행 결과</p>
<pre>
>>> ['ㅇ','ㄴ','ㅎ','ㅅ','ㅇ']
</pre>
</blockquote>
<p>중성만 뽑아내려면 <strong>hangul.split(ch)[0]</strong>을 <strong>hangul.split(ch)[1]</strong>, 종성은 <strong>hangul.split(ch)[2]</strong>이라고 하면 된다.</p>
