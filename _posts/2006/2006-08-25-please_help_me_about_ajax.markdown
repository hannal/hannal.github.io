---
layout: post
status: publish
published: true
title: ajax (prototype) 좀 도와주세요.
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 883
wordpress_url: http://blog.hannal.com/blog/?p=883
date: '2006-08-25 22:11:57 +0900'
date_gmt: '2006-08-25 13:11:57 +0900'
categories: []
tags:
- "희망"
permalink: "/2006/8/please_help_me_about_ajax/"
---
<p><!-- 나의 추천 글 --><br />
안녕하세요.</p>
<p>prototype을 이용해서 ajax(xmlhttprequest)로 서버와 통신을 하려 합니다. form 값을 서버에 있는 cgi 파일로 넘기는 것을 ajax로 넘기는 것이지요. form method 방식은 post 입니다.</p>
<p>제가 문제에 부딪힌 건 HTTP 인증을 걸어놓은 경우입니다. Apache의 경우 .htaccess와 .htpasswd 파일을 이용해서 HTTP 인증을 걸지요. 이것이 왜 제게 문제가 된 것이냐면, HTTP 인증이 걸린 곳에서 ajax를 이용해서 post 방식으로 서버에 접근하면 HTTP 인증이 풀려버립니다. 말이 애매하니 순서대로 설명하겠습니다.</p>
<ol>
<li>http://domain/test 는 HTTP 인증 걸려있음</li>
<li>http://domain/test 에 접근해서 HTTP 인증을 받음</li>
<li>http://domain/test 에서 어떤 기능을 작동시켜 ajax를 이용해 post 방식으로 서버의 cgi 파일에 접근</li>
<li><strong>http://domain/test 에 대한 HTTP 인증이 풀려서 다시 인증하라는 입력창이 뜸</strong></li>
</ol>
<p>바로 굵게 표시한 저 부분입니다. ajax래도 get method로 날리면 HTTP 인증이 풀리지 않아 아무 문제가 없습니다. post method로 접근만 하면 HTTP 인증이 풀려서 인증 창이 뜹니다.</p>
<p>무엇이 문제일까요? HTTP 인증이 걸린 곳에서 어찌하면 post method로 서버에 접근해도 인증이 풀리지 않을까요? 도와주세요 T_T</p>
