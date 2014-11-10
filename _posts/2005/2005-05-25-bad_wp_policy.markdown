---
layout: post
status: publish
published: true
title: "마음에 안드는 워드프레스의 댓글 정책"
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 616
wordpress_url: http://blog.hannal.com/blog/%ec%9b%8c%eb%93%9c%ed%94%84%eb%a0%88%ec%8a%a4%ec%97%90%ec%84%9c-%eb%a7%88%ec%9d%8c%ec%97%90-%ec%95%88%eb%93%9c%eb%8a%94-%ec%a0%95%ec%b1%85/
date: '2005-05-25 11:42:47 +0900'
date_gmt: '2005-05-25 02:42:47 +0900'
categories:
- "essay"
tags: []
permalink: "/2005/5/bad_wp_policy/"
---
<p>나는 요즘 워드프레스를 대단히 예뻐하고 있다. 아주 훌륭한 도구이기 때문이다. 블로그밈 개발자인 <a href="http://blog.blogmeme.com/stardust">꺼칠이</a>님께도 툭하면 워드프레스를 자랑하여 세심한(소심한?) 그의 빈정을 상하게 할 정도다.</p>
<p>하지만 얼마 전 발견한 마음에 들지 않는 워드프레스 정책을 발견했다. 관리자 영역에서 관련 설정을 아직 찾지 못한 걸 보면 이용자 차원에서 어찌 저찌 할 수 없는 워드프레스의 정책이라 생각한다.</p>
<p class="centerphoto"><img src="http://blog.hannal.com/wp-content/old_uploads/wp_bad_tag.gif" alt="" /><br />
단지 출력에서 &lt; 이 글자가 사라진 것이 아니라 Database에 아예 없다.<br />
? ? 쌡 이렇게 깨져보이는 것은 내가 사용하는 텔넷 도구가 UTF-8 출력을 지원 안해서 그러함.</p>
<p>위에서 별도로 표시한 부분을 보면 그들이 '<strong>>_< </strong>' 이 글자표정(emoticon)을 쓰려 했음을 알 수 있다. 그러나 안타깝게도 &lt; 는 어디로 사라지고 '>_' 만 덜렁 있다. 이는 워드프레스가 < 으로 시작하는 부분을 HTML 태그(tag)로 인식하고 삭제한 현상이다. 이용자가 댓글을 쓸 때 악의를 가지고 &lt;xmp> 태그나 자바 스크립트를 사용하는 걸 막기 위한 CGI의 기본 보안 정책이다.</p>
<p>문제는 워드프레스의 댓글에 대한 HTML 태그 정책은 허용하지 않은 HTML 태그는 아예 </strong><strong>삭제</strong>하는 데 있다. 출력을 하지 않는 것이 아니라 데이터베이스(Database)에 댓글을 입력할 때 불안전하거나 불완전한 HTML 태그는 아예 삭제를 해서 입력을 하는 것이다. 워드프레스에서 '<strong>>_< 꺄하하하하 > 메롱롱</strong>'이라고 댓글을 입력하면 실제로는 '<strong>>_ 메롱롱</strong>'이라고 입력이 된다. '<strong>&lt; 꺄하하하하 ></strong>'가 삭제되었다.</p>
<p>불안전하건 불완전하건 이용자가 만든 자료(글)는 최대한 원본을 유지해야 한다. 설령 정책 때문에 출력을 할 때 최소한의 가공을 가할지라도 입력된 자료는 최대한 원본을 유지해야 한다. 그러나 <u>워드프레스의 정책은 원본을 변경하여 데이터베이스에 저장하기 때문에 워드프레스 이용자가 어찌 손을 쓸 방법이 없다</u>.</p>
