---
layout: post
status: publish
published: true
title: "워드프레스용 목차 기능 확장기"
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 659
wordpress_url: http://blog.hannal.com/blog/wp_index_table/
date: '2005-07-20 15:50:57 +0900'
date_gmt: '2005-07-20 06:50:57 +0900'
categories:
- "essay"
tags: []
permalink: "/2005/7/wp_index_table/"
---
<h3>개요</h3>
<p>이 확장 기능 도구(Plugin)은 워드프레스용으로, 글의 목차를 만들거나 목차 번호를 매기는데 사용됩니다.<br />
HTML tag인 &lt;h>(예 : &lt;h1> ~ &lt;h5>) tag는 문단의 제목을 지정하는 용도로 본 확장기는 이 HTML tag를 본문에서 찾아내어 목차를 만들거나 목차 번호를 매깁니다.<br />
실제로 이 글은 본 확장기를 사용한 예제입니다.</p>
<p>* <strong>사용 예제</strong></p>
<div style="border: 1px dashed #000 ">&lt;h3>명바기 나빠요?&lt;/h3><br />
이번 대중 교통 체계 개편은 크게 4가지로 분류할 수 있다.<br />
&lt;ol><br />
&lt;li /> 버스 노선 개편<br />
&lt;li /> 버스 번호 개편<br />
&lt;li /> 버스 전용 중앙 차로 도입<br />
&lt;li /> 서울시<b>내</b> 대중 교통 요금 체계 개편<br />
&lt;/ol><br />
하나같이 좋은 내용이다. 대단히 필요한 내용이다. 그런데 욕을 먹고 있다. 이 중에서 욕을 먹는 부분은 4가지이다. 즉 전부 욕을 먹고 있다는 것인데 이에 대해 개인적 경험과 판단에 의한 이야기를 해보려 한다.</p>
<p>&lt;h3>버스 노선 개편&lt;/h3><br />
이번 버스 노선의 개편 중 가장 눈에 띄이는 점은 중복 노선을 줄였다는 점이다. 예를 들어 송파구에서 강남역으로 가는 버스가 10대였다면 이를 5~7대 정도로 줄였다는 것이다. 강남역으로 가는 버스 양이 줄어들어 차량의 수가 줄어드는 것이다. 구체적 예를 들면 송파구 끝지역인 거여동에서 출발하여 강남역까지 오던 555-2번은 571번의 종점인 강변역으로 향하게 되었다. (번호가 몇 번인지는 모르겠다)
</p></div>
<p>* 확장 기능 사용 전<br />
<img src="http://blog.hannal.com/wp-content/old_uploads/archive/wp_index_table/no_use_htt_plugin.gif" alt="" /></p>
<p>* 확장 기능 사용 후<br />
<img src="http://blog.hannal.com/wp-content/old_uploads/archive/wp_index_table/use_htt_plugin.gif" alt="" /></p>
<p>* 목차표 나타내기<br />
<img src="http://blog.hannal.com/wp-content/old_uploads/archive/wp_index_table/make_htt_table.gif" alt="" /></p>
<h3>저작권</h3>
<p>본 확장 기능 도구는 저작권이 없으며, 마음껏 고치고 재배포 할 수 있으며, 상업 목적을 위해 사용할 수 있습니다. 또한, 재배포 할 시 본 파일의 제작자 이름만 고쳐서 자신이 만든 것이라고 주장하셔도 괜찮습니다.</p>
<h3>확장기 내려 받기</h3>
<p>내려 받기 : <a href="http://blog.hannal.com/wp-content/old_uploads/archive/wp_index_table/htt_20050720_03.zip">20050720판 확장 기능 도구 내려 받기</a> (2005년 7월 20일 16시 05분판이 가장 최신판입니다)</p>
<h3>사용 방법</h3>
<ol>
<li>압축 파일의 압축을 푼 뒤, Hannal_Title_Table.php 파일은 Plugin 디렉토리에, Hannal_Title_Table.css 파일은 현재 사용하고 있는 Theme 디렉토리에 넣으십시오.</li>
<li>#<br />
# 워드프레스 관리자 영역에 접근한 뒤, 플러그인 관리 영역으로 가십시오. Hannal’s Rewrite Rules 라는 확장기(Plugin)이 추가되어 있는데, 이것을 <strong>사용할 수 있게</strong> 활성화하십시오.</li>
<li>이제 본문에 &lt;h> HTML tag가 있을 경우 자동으로 목차 기능이 작동됩니다.</li>
<li>목차표는 사용하고 계신 Theme의 single.php이나 index.php 파일에 <font color="red">&lt;?php if ( function_exists('print_index_table_htt')==TRUE) echo print_index_table_htt('Index Table', 1); ?></font> 라는 문자열을 본문이나 글 제목이 출력되는 부근에 넣으시면 됩니다. '<strong>Index Table</strong>'은 목차표 이름으로 취향에 따라 변경할 수 있습니다. </li>
<li>본문에 있는 문단 제목에 목차 번호를 매기고 싶으시다면, 글 쓰기 영역에서 <strong>Numbering</strong>을 <strong>Use</strong>로 하십시오.<br />
<img src="http://blog.hannal.com/wp-content/old_uploads/archive/wp_index_table/numbering_use.gif" alt="" /></li>
</ol>
<h3>활용하기</h3>
<p><strong>1. 목차 출력 제어하기</strong><br />
첨부된 CSS 파일을 수정하면, 보다 다양한 표현을 할 수 있습니다.</p>
<p><strong>2. 세밀한 조정</strong><br />
Hannal_Title_Table.php 파일을 문서 편집기로 열면 다음과 같은 내용이 있습니다.</p>
<pre lang="php">
define('NUMBERING_TABLE_CODE_1', ' : ');
define('NUMBERING_TABLE_CODE_2', '. ');
define('KEYWORDS_META_NAME', 'numbering_htt');
define('STYLESHEET_NAME', 'Hannal_Title_Table.css');
</pre>
<p>이 내용들은 취향에 맞게 바꾸어서 사용하실 수 있습니다.</p>
<ol>
<li>NUMBERING_TABLE_CODE_1 : <strong>목차표</strong>에서 목차 번호와 목차 이름을 구분시켜줄 용도의 문자열 지정</li>
<li>NUMBERING_TABLE_CODE_2 : <strong>글 본문</strong>에서 목차 번호와 문단 이름을 구분시켜줄 용도의 문자열 지정</li>
<li>KEYWORDS_META_NAME : 글 본문에 목차 번호를 지정하는지 여부를 DB에 저장하기 위한 접근(key) 이름. (Custom Field)</li>
<li>STYLESHEET_NAME : 본 확장기가 사용할 StyleSheet(CSS) 파일 이름.</li>
</ol>
<h3>이외</h3>
<p>본 확장기는 성능보다는 기능 구현에 중점을 두었습니다. 성능이 떨어져도 저를 너무 구박마세요. :)</p>
