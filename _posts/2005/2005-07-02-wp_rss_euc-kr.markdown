---
layout: post
status: publish
published: true
title: Wordpress에서 euc-kr RSS 지원하기
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 653
wordpress_url: http://blog.hannal.com/blog/wp_rss_euc-kr/
date: '2005-07-02 13:45:56 +0900'
date_gmt: '2005-07-02 04:45:56 +0900'
categories:
- "essay"
tags: []
permalink: "/2005/7/wp_rss_euc-kr/"
---
<h3>개요</h3>
<p>워드프레스의 문자형을 UTF-8로 지정할 경우, euc-kr로 RSS를 지원할 수 없습니다. 때문에 RSS 구독자가 사용하는 RSS 구독기가 euc-kr을 지원하지 못하는 경우 워드프레스를 사용하는 이용자 블로그의 RSS를 볼 수 없습니다.<br />
이 글은 워드프레스에서 문자형을 UTF-8로 사용하는 이용자가 euc-kr형 RSS를 제공할 수 있는 방법을 제공합니다.<br />
이 방법은 그다지 좋은 방법은 아니고 약간의 꽁수로 처리합니다. 더 좋은 방법이 있다면 그 방법으로 하시기를 권합니다.<br />
마지막으로 본 확장 기능 도구(plugin)는 <strong>iconv</strong> 함수를 사용합니다. 만일, 서버가 iconv 함수를 지원하지 않을 경우, euc-kr을 지원하지 못할 겁니다.</p>
<h3>저작권</h3>
<p>본 확장 기능 도구는 저작권이 없으며, 마음껏 고치고 재배포 할 수 있으며, 상업 목적을 위해 사용할 수 있습니다. 또한, 재배포 할 시 본 파일의 제작자 이름만 고쳐서 자신이 만든 것이라고 주장하셔도 괜찮습니다.</p>
<h3>확장기 내려 받기</h3>
<p>내려 받기 : <a href="http://blog.hannal.com/wp-content/old_uploads/archive/wp_rss_euc-kr/wp_for_euc_kr_rss2_20050702.zip" alt="워드프레스용 euc-kr RSS 지원 확장 기능 파일 내려 받기">20050702판 확장 기능 도구 내려 받기</a></p>
<h3>사용 방법</h3>
<ol>
<li>압축 파일의 압축을 워드프레스가 설치된 곳에 풉니다.</li>
<li>압축 파일의 압축을 워드프레스가 설치된 곳에 풀면, hannal_merong.php 라는 파일이 index.php가 있는 곳에, createRewriteRules.php 파일은 ./wp-content/plugins 에 생성됩니다.</li>
<li>워드프레스 관리자 영역에 접근한 뒤, 플러그인 관리 영역으로 가십시오. Hannal's Rewrite Rules 라는 확장기(Plugin)이 추가되어 있는데, 이것을 <strong>사용할 수 있게</strong> 활성화하십시오.</li>
<li>'설정(option)' 관리 영역에 가신 뒤 '변하지 않는 링크 (Permalink)'에 방문하십시오. 방문하면 <strong>.htaccess</strong> 파일이 재생성됩니다.</li>
</ol>
<p>이제 '<strong>http://블로그위치</strong>/index.xml'로 접근하면 euc-kr로 된 RSS가 제공됩니다.</p>
<h3>활용하기</h3>
<p><strong>1. 파일 이름 바꾸기</strong><br />
본 확장기는 <strong>hannal_merong.php</strong> 라는 실제 RSS 접근 파일과 <strong>createRewriteRules.php</strong> 라는 Rewrite Rules에 문법 추가 기능을 하는 파일로 구성되어 있습니다. euc-kr로 처리되는 RSS 파일인 index.xml 은 실제로 존재하는 파일이 아닌 가상 파일입니다.<br />
이 중에서 hannal_merong.php 과 index.xml 은 파일 이름을 이용자가 원하는대로 변경할 수 있습니다.<br />
createRewriteRules.php 을 Ascii 문서 편집기에서 열면</p>
<blockquote><p>
$feed_file_name = '<font color="red">index.xml</font>'; // feed file name<br />
define('REAL_FILE_NAME', '<font color="red">hannal_merong.php</font>'); // feed processor file name
</p></blockquote>
<p>라는 부분이 있습니다. index.xml는 외부에서 접근하는 파일 이름입니다. euc-kr_rss.xml 로 이름을 원하신다면 index.xml를 euc-kr_rss.xml 로 바꾸면 됩니다. 가상 파일 이름이므로 실제 파일 이름을 찾아 변경하실 필요는 없습니다.<br />
hannal_merong.php 는 RSS 를 출력할 실제 파일입니다. 이 파일 이름이 마음에 들지 않는다면, 원하시는 파일 이름으로 변경하시면 됩니다. 단, hannal_merong.php는 실제 파일이므로 반드시 hannal_merong.php 이라는 파일 이름도 동일하게 변경하셔야만 합니다.<br />
이제 '관리자 영역'의 '설정' 영역에 가신 뒤 '변하지 않는 링크'에 방문하여, .htaccess 파일을 재생성하시면, euc-kr로 된 RSS 파일 이름이 변경됩니다.</p>
<p>만일, 사용하고 계신 서버가 Rewrite Rules(.htaccess) 기능을 제공하지 않는다면, euc-kr 로 된 RSS는 hannal_merong.php 가 됩니다. 이런 경우, '관리자 영역'의 '플러그인'에 있는 '<strong>Hannal's Rewrite Rules</strong>' 확장기는 사용하실 필요가 없습니다. RSS 파일인 hannal_merong.php의 파일 이름을 변경해도, 특별히 설정을 건드릴 필요가 없습니다.</p>
<p><strong>2. Atom 등 다른 Feed 방법을 euc-kr로 하기</strong><br />
이 확장기는 RSS 2를 기본으로 사용합니다. ATOM이나 RSS 0.92 등을 사용하시려 한다면 hannal_merong.php 파일 내용을 고치면 됩니다.<br />
파일을 문서 편집기에서 열면</p>
<blockquote><p>
ob_start();</p>
<p>require('<font color="red">wp-rss2.php</font>');</p>
<p>$str = ob_get_contents();
</p></blockquote>
<p>라는 부분이 있는데, 여기서 wp-rss2.php 라고 쓰여진 부분을 아래를 참고해서 고치면 됩니다.</p>
<ul>
<li>ATOM : wp-atom.php</li>
<li>RSS 0.92 : wp-rss.php</li>
<li>XML : wp-rdf.php</li>
</ul>
<p><strong>3. Wordpress 2.0에서도 작동되게 하기</strong><br />
<a href="http://prozac.pe.kr/bythelake/?p=194">http://prozac.pe.kr/bythelake/?p=194</a> 참조.</p>
