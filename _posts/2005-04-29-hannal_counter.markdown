---
layout: post
status: publish
published: true
title: "워드프레스용 카운터/조회수 플러그인"
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 590
wordpress_url: http://blog.hannal.com/blog/hannal_counter/
date: '2005-04-29 01:17:11 +0900'
date_gmt: '2005-04-28 16:17:11 +0900'
categories:
- "essay"
tags: []
permalink: "/2005/4/hannal_counter"
---
<h3>알립니다</h3>
<p><ins datetime="2009-01-21T14:57:18+00:00">아직도 이 확장기능을 찾으시는 분들이 계셔서 알려드립니다. 이 확장도구는 워드프레스 1.5판을 기준으로 개발했으며, 최신판인 2.x 이상에서는 작동하지 않을 수 있습니다. 또한, 검색기 방문을 방문 수에 넣지 않는 기능이 없어서 실제 방문 수와 많은 차이가 있을 수 있습니다.</ins><br />
마지막 글 수정일 : 2009년 1월 21일</p>
<h3>개요</h3>
<p>본 도구는 워드프레스용 카운터/조회수 기능을 갖고 있는 플러그인입니다. 워드프레스 기본 기능에 없는 카운터(방문수) 기능과 글 조회수 기능을 이 도구를 이용하여 사용해보십시오.</p>
<ol>
<li>도구 이름 : 한날 카운터 (Hannal Counter)</li>
<li>도구 판 번호 : 1.0  (20050429)</li>
<li>공식 배포 주소 : <a href="http://blog.hannal.com/hannal_counter/">http://blog.hannal.com/hannal_counter/</a></li>
<li>만든 이 : 한날 ( <a href="http://www.hannal.net">http://www.hannal.net</a> )</li>
<li>저작권 : 이 도구로 <strong>나쁜 짓</strong>을 하지 않는 전제하에 저작권은 없습니다. 마음껏 제한없이 사용하고 재배포 하셔도 되며, 자신이 만들었다고 주장하셔도 됩니다. 나쁜 짓이란 공식 배포본처럼 꾸미고선 웹 개발을 모르는 이들에게 피해나 손해를 끼치는 일 등을 의미합니다.</li>
</ol>
<h3>설치 방법</h3>
<ol>
<li>파일 내려 받기</li>
<p>* <a href="/blog/wp-content/old_uploads/Hannal_Counter_1.0-20050429.zip">ZIP 파일</a><br />
* <a href="/blog/wp-content/old_uploads/Hannal_Counter_1.0-20050429.tar.gz">Tar 파일</a></p>
<li>파일 설치 하기</li>
<p>위 압축 파일을 워드프레스가 설치된 곳에 푸십시오. 정상적으로 설치하면 wp-admin 디렉토리에는 <strong>hannal_counter_site.php</strong> 파일과 <strong>hannal_counter_post.php</strong> 파일이 생기고, wp-content/plugins 디렉토리에는 <strong>Hannal_Counter.php</strong> 파일이 생깁니다.</p>
<li>플러그인 작동 시작하기</li>
<p>이제 워드프레스 관리자 영역에 간 뒤, '플러그인' 영역에 가시면 <strong>Hannal Counter</strong>라는 플러그인이 생겼음을 알 수 있습니다.<br />
<img src="http://blog.hannal.com/wp-content/old_uploads/plugin_in_admin.gif" alt="í??ë?¬ê·¸ì?¸ í??ì?±í??í??ê¸°" /><br />
이곳에서 해당 플러그인을 '사용할 수 있게'를 눌러서 활성화 하십시오.</p>
<li>데이터베이스 설정</li>
<p>본 플러그인은 필요한 데이터베이스 테이블이 없을 시 자동으로 생성하므로, 별도의 설정을 하실 필요가 없습니다.</ol>
<h3>사용 방법</h3>
<ol>
<li>방문수와 조회수 보기</li>
<p>본 플러그인을 활성화하면 관리자 영역에 <strong>Counter</strong>라는 메뉴가 추가됩니다. 해당 메뉴로 들어가면 <strong>Site Counter</strong>라는 메뉴와 <strong>Post Counter</strong>라는 보조 메뉴가 나오는데, Site Counter는 워드프레스로 운영 중인 블로그의 방문 수를 볼 수 있으며, Post Counter는 각 글의 조회수를 볼 수 있습니다.</p>
<li>조회수 증가 기능 작동시키기</li>
<p>방문수는 본 플러그인이 활성화되면서 자동으로 기록되기 시작하지만, 조회수 증가 기능은 저의 능력 부족으로 인해 이용자가 테마(스킨) 파일에 별도의 줄을 추가해야 합니다. 우선 사용하고 계시는 테마 중 <strong>single.php</strong> 파일을 여십시오. 그런 뒤<br />
<strong>&lt;?php if (have_posts()) : while (have_posts()) : the_post(); ?&gt;</strong><br />
라는 줄을 찾아 그 바로 아래에<br />
<strong>&lt;?php if ( function_exists('hannal_record_hits') == TRUE ) hannal_record_hits(); ?&gt;</strong><br />
를 적어 넣으시면 됩니다.<br />
<img src="http://blog.hannal.com/wp-content/old_uploads/edit_single.php.gif" alt="single.php í??ì?¼ì?? ì?´ì?? ê°?ì?´ ì??ì ?í??ì?­ì??ì?¤." /><br />
<strong><span style="color: red;">이렇게 하지 않으면 글을 읽어도 조회수는 증가하지 않습니다</span></strong>.</ol>
