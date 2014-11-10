---
layout: post
status: publish
published: true
title: MS Excel(엑셀)에서 값 찾아 위치 값 갖고놀기
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 888
wordpress_url: http://blog.hannal.com/blog/?p=888
date: '2006-08-29 15:31:46 +0900'
date_gmt: '2006-08-29 06:31:46 +0900'
categories:
- "essay"
tags: []
permalink: "/2006/8/match_n_index_function_in_ms-excel/"
---
<p>A열에 중 '한날'이라는 글자가 있는 곳을 찾아 그 행 위치를 알아내고, 그 행 위치를 토대로 같은 행의 다른 열(B열) 값을 알아내는 함수가 필요하다면 MATCH 함수와 INDEX 함수를 쓰면 된다.</p>
<table border="1">
<tr>
<th>&nbsp;</th>
<th>A</th>
<th>B</th>
</tr>
<tr>
<td>1</td>
<td>한나</td>
<td>여자</td>
</tr>
<tr>
<td>2</td>
<td>한남</td>
<td>동네</td>
</tr>
<tr>
<td>3</td>
<td>한날</td>
<td>미청년</td>
</tr>
<tr>
<td>4</td>
<td>한낮</td>
<td>시간대</td>
</tr>
</table>
<p>MS Excel 함수 : <strong>=INDEX(A1:B4, MATCH("한날",A1:A4,0), 2)</strong></p>
<p>위와 같이 하면 '미청년'이라는 문자열을 반환한다.</p>
<p>MATCH 함수는 찾을 범위(두번째 인자 (A1:A4))에서 찾고자 하는 값(첫번째 인자 ("한날"))을 찾아서(세번째 인자는 찾는 방식이다. 0이면 완전 일치하는 값. -1과 1은 이하, 혹은 이상 값을 찾음) 그 값이 있는 행 값을 반환한다. 이때 행 값은 찾을 범위를 기준으로 한다. A1:A99로 했는데 반환 값이 5라면 A5나 B5 등을 뜻하지만, A3:A99로 했고 반환 값이 5라면 A8이나 B8 등을 뜻한다.</p>
<p>INDEX 함수는 지정한 범위(첫번째 인자(A1:B4))에서 이용자가 지정한 행 값(두번째 인자)과 열 값(세번째 인자)에 있는 값을 반환한다. 만일 <strong>=INDEX(A1:B4, 2, 2)</strong>라고 하면 "동네"라는 문자열이 반환되며 <strong>=INDEX(A1:B4, 3, 1)</strong>이라고 하면 "한날"이라는 문자열이 반환된다.</p>
<p>이를 조합해서 MATCH 함수로 찾고자 하는 값이 있는 행 값을  알아낸 뒤, INDEX 함수로 해당 행에 있는 다른 열의 값을 가져올 수 있다.</p>
