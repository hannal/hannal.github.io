---
layout: post
status: publish
published: true
title: cakephp에서 테이블 필드를 tinyint 로 할 때 주의할 점.
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 1177
wordpress_url: http://blog.hannal.com/?p=1177
date: '2008-05-09 18:44:13 +0900'
date_gmt: '2008-05-09 09:44:13 +0900'
categories:
- "essay"
tags:
- cakephp
- php
permalink: "/2008/5/issue_about_tinyint_in_cakephp"
---
<h3>1. 문제 발생</h3>
<p>Cakephp 1.2 시험판(beta)에서 작업을 하다가 특정 필드에 값이 0 아니면 1로만 들어가는 현상이 발생했습니다. 문제가 된 테이블의 필드는 다음과 같습니다.</p>
<blockquote><p>field_1 tinyint(1) unsigned default 0,<br />
field_2 tinyint(1) unsigned default 0,</p></blockquote>
<p>위 두 필드엔 1자리 숫자(0~9)를 넣을 것이고, 기본값(default)은 0으로 한 것이지요.</p>
<p>그런데 cakephp에 있는 Model 클래스의 save 메소드로 데이터를 저장하면 field_1 과 field_2 에 넣을 값이 자동으로 1로 바뀌었습니다. 어떤 값을 넣어도 1로 바뀌는 기이한 현상이지요.</p>
<h3>2. 문제 원인</h3>
<p><strong>/cake/libs/model/datasource/dbo_source.php</strong> 에 보면 create 메소드 정의 부분 (function create)에 다음과 같은 부분이 있습니다.</p>
<blockquote><pre>for ($i = 0; $i < $count; $i++) {
	$valueInsert[] = $this->value($values[$i], $model->getColumnType($fields[$i]));
}
</pre>
</blockquote>
<p>이 부분은 데이터를 저장할 테이블의 스키마 정보를 가져 온 뒤, 각 스키마 자료형에 맞춰서 값을 검증하고 만드는 부분입니다. 바로 이 부분이 문제(?)입니다. 이 녀석은 필드 자료형이 tinyint이고 길이가 1 인 경우 (즉, <strong>tinyint(1)</strong>) 이 필드를 Boolean 쓰임새로(true/false) 판정합니다. -_-; 그래서 <strong>1 이상인 값이 들어오면 참으로 간주하여 테이블에 1을 넣은 겁</strong>니다.</p>
<h3>3. 문제 해결</h3>
<p>간단합니다. <strong>테이블 생성할 때 boolean 필드로 쓸 것이 아니라면 tinyint(1)을 쓰지 않으면 됩니다</strong>. tinyint(2) 나 tinyint 라고 쓰면 되지요.</p>
<p>이건 오류(bug)라고 하기도 애매한 것이, <a href="http://dev.mysql.com/doc/refman/5.0/en/numeric-type-overview.html">mysql 에서 테이블 필드를 bool 이나 boolean 으로 지정하면 tinyint(1) 로 바꿔서</a> 이렇게 처리한 듯 합니다. 역시 관련 문서를 정독하지 않고 "hello world"부터 찍고 보는 사파 개발 성향이 이번에도 제대로 드러났네요. 히히.</p>
