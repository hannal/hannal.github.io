---
layout: post
status: publish
published: true
title: FCKeditor에서 lightbox 연계하기
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 900
wordpress_url: http://blog.hannal.com/blog/?p=900
date: '2006-09-19 16:20:15 +0900'
date_gmt: '2006-09-19 07:20:15 +0900'
categories:
- "essay"
tags: []
permalink: "/2006/9/lightbox_rel_tag_in_fckeditor"
---
<p>HTML 편집기로 <a href="www.fckeditor.net/">FCKeditor</a>는 훌륭한 도구이다. 여러 웹 브라우저(Web browser)를 지원하며, 쉽고 편하기 때문이다.</p>
<p><a href="http://www.huddletogether.com/projects/lightbox2/">Lightbox</a>는 그림 출력 도구로 연출이 예쁘다. 현재 화면에 작은 그림을 출력한 뒤 큰 그림 주소를 연결하여 작은 그림을 누르면 큰 그림이 뜨도록 할 때, Lightbox를 이용하면 별도의 새 창(window)를 띄우지 않고도 예쁘게 그림을 띄울 수 있어 곳곳에서 애용되고 있다.</p>
<p>이 두 가지를 함께 쓰고자 하는 경우, 그러니까 FCKeditor에 그림을 넣은 뒤 이 그림을 누르면 Lightbox로 화면을 연출하며 그림을 띄우려 하는 경우 조금 손이 간다.</p>
<blockquote><p>
&lt;a href="hannal.jpg" <span style="color: red;">rel="lightbox"</span>&gt;여기 누르면 한날 사진이 대뜸 뜹니다&lt;/a&gt;</p></blockquote>
<p>위와 같이 rel 속성을 넣어야 lightbox가 이를 인식하여 화면 연출을 해주는데, FCKeditor에는 이를 지정해주는 곳이 없다.</p>
<p>이를 지정하게 해줄 방법은 여럿 있지만 나는 FCKeditor를 직접 손 봐서 직접 적용했다.</p>
<p><strong>1. fck_image.html</strong><br />
fck_image.html를 찾아서 열어보면</p>
<blockquote><p>
&lt;div id="divLink" style="display: none"&gt;
</p></blockquote>
<p>이런 부분이 있다. 그 아래에 보면</p>
<blockquote><p>
&lt;div&gt;<br />
&lt;span fcklang="DlgLnkTarget"&gt;Target&lt;/span&gt;&lt;br /&gt;
</p></blockquote>
<p>이렇게 시작하는 부분이 있는데 그 div 구역 아래에 아래를 추가하자.</p>
<blockquote><p>
&lt;div><br />
	&lt;span fcklang="DlgLnkRel">Rel&lt;/span&gt;&lt;br /&gt;<br />
	&lt;input id="txtLnkRel" style="width: 100%" type="text" /&gt;<br />
&lt;/div>
</p></blockquote>
<p>저 위에 있는 Target div 영역 아래(&lt;/div&gt; 아래)에 추가하면 된다.</p>
<p><strong>2. fck_image.js</strong><br />
fck_image.js 파일을 찾아 열면</p>
<blockquote><p>SetAttribute( oLink, '_fcksavedurl', sLnkUrl ) ;<br />
SetAttribute( oLink, 'target', GetE('cmbLnkTarget').value ) ;</p></blockquote>
<p>저 두 줄 뿐을 찾아가자. 혹시라도 찝찝하다면 Ok 함수(function Ok()) 안에서 찾자. 위 두 줄 아래에</p>
<blockquote><p>SetAttribute( oLink, 'rel', GetE('txtLnkRel').value ) ;</p></blockquote>
<p>이 줄을 추가하자.</p>
<p><strong>3. 원리</strong><br />
FCKeditor에서 그림 관리 영역은 fck_image.html을 열어서 출력한다. Rel 관련 부분을 이 파일에 껴넣어 그림 관리에 Rel 값 입력란을 추가한 것이다.</p>
<p class="centerphoto"><img src="http://blog.hannal.com/wp-content/old_uploads/imagebox_in_lightbox.GIF" alt="예제 화면" /><br />
위와 같이 Rel이라는 항목이 추가된다.</p>
<p>그런 뒤 OK 단추를 누를 때 이 입력란의 값을 가져와(GetE('txtLnkRel').value) rel이라는 속성을 만든다(SetAttribute). 그래서 &lt;a href="그림주소" rel="써넣은내용" ...&gt; 식으로 HTML tag를 만들어서 Lightbox가 이것을 찾아서 연출을 쓸 수 있도록 해준다.</p>
