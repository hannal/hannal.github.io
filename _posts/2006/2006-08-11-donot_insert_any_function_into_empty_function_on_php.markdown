---
layout: post
status: publish
published: true
title: php에서 이유 없이 오류 날 때
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 864
wordpress_url: http://blog.hannal.com/blog/?p=864
date: '2006-08-11 10:58:41 +0900'
date_gmt: '2006-08-11 01:58:41 +0900'
categories:
- "essay"
tags: []
permalink: "/2006/8/donot_insert_any_function_into_empty_function_on_php/"
---
<p>php 에서 empty 함수 안에 문자열이나 변수를 넣지 않고 함수를 넣으면 다음과 같은 Parse error가 난다.</p>
<blockquote><p>Parse error: parse error, unexpected T_STRING, expecting T_VARIABLE or '$' in ~~~~</p></blockquote>
<p>예제 :<br />
<textarea name="code" language="php">< ?php<br />
$vars = ' 안녕하세요, 한날님 ';</p>
<p>echo empty(trim($vars));<br />
?></textarea></p>
<p><del datetime="2006-08-11T02:47:48+00:00">이유는 모르겠다. 문법상 맞는데 틀리다고 하는데 왜 틀리다고 하는지 그 이유를 찾지 못했다. </del><ins datetime="2006-08-11T02:47:48+00:00">empty는 변수 자체를 확인할 뿐 문자열을 확인하지 않는다. 그렇기 때문에 문자열을 반환하는 함수를 쓰면 문제가 발생한다.</ins>(<a href="http://blog.hannal.com/donot_insert_any_function_into_empty_function_on_php/#comment-8406">dnzin님 댓글 참조</a>) 문제가 발생하니 이를 피하려면 empty 함수 안에 다른 함수를 넣지 않아야 한다.</p>
<p>예제 :<br />
<textarea name="code" language="php">< ?php<br />
$vars = ' 안녕하세요, 한날님 ';</p>
<p>$vars = trim($vars);<br />
echo empty($vars);<br />
?></textarea></p>
<p>이렇게 하면 잘 작동한다. 예제에선 trim 함수를 썼는데, trim 이건 무엇이건 empty 함수 안에 값을 반환하건(return) 하지 않건 함수를 넣으면 오류가 발생하며 작동하지 않는다.</p>
