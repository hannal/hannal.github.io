---
layout: post
status: publish
published: true
title: Google App Engine의 모델에 있는 get_or_insert 메서드 사용할 때 주의점
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 1794
wordpress_url: http://blog.hannal.com/?p=1794
date: '2009-06-07 14:43:30 +0900'
date_gmt: '2009-06-07 05:43:30 +0900'
categories:
- "essay"
tags:
- gae
- get_or_insert
- google
- python
- "구글"
- "구글앱엔진"
- "모델"
- "파이썬"
permalink: "/2009/6/about_key_name_of_get_or_insert-method_of_gae/"
---
<p><a href="http://code.google.com/appengine/">Google App Engine</a> 에서 제공하는 파이썬(python) SDK로 개발을 할 때, Datastore API를 이용하여 DB를 모델링하고 자료를 넣을 겁니다. 이 중 <a href="http://code.google.com/intl/en/appengine/docs/python/datastore/modelclass.html#Model_get_or_insert">get_or_insert</a> 라는 유용한 클래스 메서드가 있는데, 지정한 값이 있으면 DB에서 그 자료를 가져오고, 없으면 DB에 그 자료를 집어넣습니다.</p>
<p>이렇게 쓰면 됩니다.</p>
<blockquote><p>모델.get_or_insert(키, 값들)</p></blockquote>
<p>Hannal 이라는 모델을 예로 들지요.</p>
<blockquote><pre>class Hannal(db.Model):
	name = db.StringProperty(required=True)</pre>
</blockquote>
<p>name이라는 프로퍼티만 있는 클래스이지요. 이 모델에 자료를 하나 넣어보지요.</p>
<blockquote><p>Hannal.get_or_insert('key_0000001', name=u'Hannal Cha')</p></blockquote>
<p>이러면 key_0000001 를 키(id)로 갖는 자료를 찾아보고 있으면 그걸 가져오고 없으면, key_0000001를 키로 하고 name 은 u'Hannal Cha'인 자료를 집어넣습니다.</p>
<p>주의할 점은 바로 이 키인데요. 이 키 문자열 맨 앞이 숫자로 시작하면 <strong>Names may not begin with a digit</strong> 라는 오류가 납니다. 키 자료형이 문자형이어도 마찬가지입니다. 자료형을 따지는 게 아니라 키 문자열의 맨 앞 문자가 숫자인지를 따집니다.</p>
<p>유레카! 를 외칠만큼 위대한 발견을 한 건 아닙니다. 오류 내용에서도 비록 영문이긴 하지만 숫자(digit)으로 시작하면 안 된다고 알려주니까요. 다만, 보통은 키라 하면 일련번호 id로 관리하기 때문에, 그리고 저처럼 초보들은 눈에 익지 않은 오류가 나면 겁부터 먹기 때문에 종종 당할 듯 해서 적어둡니다. 저는 친절하거든요.</p>
