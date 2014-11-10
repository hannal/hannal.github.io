---
layout: post
status: publish
published: true
title: Firefox에서 누리집 주소 관리하기
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 754
wordpress_url: http://blog.hannal.com/blog/manage_bookmarks_on_firefox/
date: '2006-03-21 15:17:27 +0900'
date_gmt: '2006-03-21 06:17:27 +0900'
categories:
- "essay"
tags: []
permalink: "/2006/3/manage_bookmarks_on_firefox/"
---
<h3>Firefox 기본 기능 활용</h3>
<p>Firefox 확장 기능에 <a href="https://addons.mozilla.org/extensions/moreinfo.php?id=708&application=firefox">URL Alias</a> 라는 게 있다. 가고자 하는 주소를 원하는 글자로 연결시키는 도구인데, http://blog.hannal.com 를 hn 이라고 지정한 뒤 주소 입력란에 hn이라고 치면 http://blog.hannal.com 로 이동되는 편리한 도구이다. Internet Explorer 이용자는 <a href="http://www.ietoy.net">IE Toy</a>라는 풀그림을 통해 동일한 효과를 맛볼 수 있다.</p>
<p>나는 Bookmark(즐겨찾기)를 별로 이용하지 않고 자주 가는 누리집 주소를 직접 쳤었기 때문에 이 확장 기능은 대단히 편했다. 하지만 Firefox가 1.5 판으로 넘어가면서 URL Alias 확장 기능은 작동하지 않고 있다. 나같은 사람들에겐 참으로 불행한 상황이다.</p>
<p>하지만 다행스럽게도 Firefox에는 이미 URL Alias 확장 기능과 거의 같은 기능이 있다.</p>
<p class="centerphoto"><a href="http://blog.hannal.com/wp-content/old_uploads/ffbookmark_urlalias_01.png" rel="lightbox"><img src="http://blog.hannal.com/wp-content/old_uploads/ffbookmark_urlalias_01.png" alt="Firefox의 북마크 관리자 그림" /></a><br />
북마크 관리자</p>
<p>Firefox에서 북마크 관리자를 열면 저렇게 북마크한 주소들이 나온다. 여기서는 '스타크래프트 갤러리'를 예로 들어보자.</p>
<p class="centerphoto"><img src="http://blog.hannal.com/wp-content/old_uploads/ffbookmark_urlalias_02.png" alt="북마크 주소 속성 보기 그림" /><br />
북마크 주소 속성 보기</p>
<p>북마크한 주소의 속성을 보자. 상단에 있는 '속성'이라는 커다란 그림 단추를 누르거나 주소에 대고 마우스 오른쪽 단추를 누른 뒤 '속성 (R)'을 누르면 된다. 그럼 위와 같은 창이 뜨는데 여기서 <strong>키워드</strong>에다가 원하는 단축 이름을 쓰자. 나는 편의상 <strong>sg</strong>라고 했다.</p>
<p class="centerphoto"><a href="http://blog.hannal.com/wp-content/old_uploads/ffbookmark_urlalias_03.png" rel="lightbox"><img src="http://blog.hannal.com/wp-content/old_uploads/ffbookmark_urlalias_03.png" alt="주소창에 단축 이름을 쓴 그림" /></a></p>
<p>이제 간단하다. 위와 같이 주소창에 자신이 방금 지정한 단축 이름(여기서는 sg)을 넣고 이동하면 아래 그림과 같이</p>
<p class="centerphoto"><img src="http://blog.hannal.com/wp-content/old_uploads/ffbookmark_urlalias_04.png" alt="단축 이름에 연결된 실제 주소로 바뀐 그림" /></p>
<p>단축 이름에 연결된 실제 주소로 사삭 바뀐다.</p>
<h3>Foxylicious를 이용해 del.icio.us와 연동하기</h3>
<p>Firefox 확장 기능 도구 중에 <a href="https://addons.mozilla.org/extensions/moreinfo.php?id=342&application=firefox">Foxylicious</a> 라는 게 있다. <a href="http://del.icio.us">del.icio.us</a>에 관리하는 주소를 자신의 Firefox 북마크 꾸러미와 연동시켜준다.</p>
<p class="centerphoto"><a href="http://blog.hannal.com/wp-content/old_uploads/ffbookmark_foxy_01.png" rel="lightbox"><img src="http://blog.hannal.com/wp-content/old_uploads/ffbookmark_foxy_01.png" alt="Foxylicious 설정 화면" /></a><br />
Foxylicious 설정 화면</p>
<p>Foxylicious를 설치하고 설정 화면에 가면 위와 같이 del.icio.us 에서 사용 중인 자신의 ID와 비밀 번호를 입력할 수 있는데, 입력한 뒤 'Update bookmarks' 단추를 누르면 해당 계정에 있는 북마크 정보들을 가져온다. 가져온 정보들은 꼬리표(꼬릿말, Tag) 단위로 폴더를 생성하고 그곳에 담는다.</p>
<p>물론, Foxylicious를 설치하면 매번 주소 등록(Bookmark)을 하려고 del.icio.us에 방문할 필요 없이, 아주 쉽고 편하게 현재 이용 중인 누리집을 del.icio.us에 등록(Bookmark)할 수 있다.</p>
<p>무턱대고 Update bookmarks 단추를 누르면 갱신할 수 없는 폴더가 있어 일을 수행할 수 없다고 투덜대는데 나처럼 Foxylicious가 정보를 저장할 폴더를 따로 만들어 주고 그곳에다 담도록 하면 된다.</p>
<p class="centerphoto"><a href="http://blog.hannal.com/wp-content/old_uploads/ffbookmark_foxy_02.png" rel="lightbox"><img src="http://blog.hannal.com/wp-content/old_uploads/ffbookmark_foxy_02.png" alt="내 북마크 정보 관리 예제 그림" /></a><br />
내 북마크 정보 관리 예제</p>
<p>난 '북마크 도구 모음'에는 매우 자주 가는 곳들을 연결해둔다. 회사 웹 오피스라던가 개발팀 게시판이 해당된다. 그리고 del.icio.us라는 폴더는 Foxylicious가 del.icio.us에서 받아온 주소들을 받고 있다. 마지막으로 '잡다'라는 폴더는 저 위에서 설명한 '주소 단축 이름(keyword)'을 이용하기 위해 등록한 주소들이다.</p>
