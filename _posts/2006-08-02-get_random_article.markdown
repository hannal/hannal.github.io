---
layout: post
status: publish
published: true
title: "워드프레스에서 아무 글이나 읽기 기능"
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 850
wordpress_url: http://blog.hannal.com/blog/?p=850
date: '2006-08-02 00:57:16 +0900'
date_gmt: '2006-08-01 15:57:16 +0900'
categories:
- "essay"
tags: []
permalink: "/2006/8/get_random_article"
---
<h3>개요</h3>
<p>안녕하세요.</p>
<p>혼자서 갖고 놀던 워드프레스용 Script 하나를 공개합니다. 심장에 바람이라도 들었나봅니다.</p>
<p>이 도구는 워드프레스에 써놓고 공개한 글 중 아무거나 하나를 골라 보여줍니다.</p>
<p>전 가끔 제가 쓴 지난 글을 읽으며 키득거리곤 합니다. 아~ 나도 참 가열차게 참견하고 다녔구나! 하는 생각이 들기도 합니다. 하지만, 블로그의 화면 구성 특성상 최근 글은 봐도 지나간 글은 잘 보지 않지요. 불편하달까요. 제 블로그가 지난 글 찾아보기에 불편한 구조라서 그런 걸지도 모르겠습니다. HTML이나 CSS 고치기 귀찮아서 좀 더 귀찮지 않은 방법을 생각하다가 워드프레스에 있는 기능을 이용했습니다. 이 도구 덕분에 지난 글을 읽기 편해졌습니다.</p>
<ul>
<li>도구 이름 : Random article. ...이라고 해두죠.</li>
<li>저작권 : 없습니다.</li>
<li>실험해본 곳 : Wordpress 2.0.3. 실은 1.5.2에서도 확인해볼 수 있었지만, 운영 정책상 1.5.2에선 실험해보기 귀찮아서 안했습니다. 아마 잘 될 겁니다.</li>
<li>내려받기 : <a href="http://blog.hannal.com/wp-content/old_uploads/archive/get_random_article/random_article-20060804.txt">random article 20060804판</a>. 쓰는 법은 저 아래를 보세요.</li>
</ul>
<h3>쓰는 법</h3>
<p><strong>1.</strong> 아주 간단합니다. 이 파일을 여러분이 쓰고 있는 Wordpress의 template 디렉토리에 넣으세요. 저를 예로 들면, 제가 사용하는 껍데기는 almost-spring 이기 때문에 이 파일을 <strong>wp-content/themes/almost-spring</strong> 에 넣었습니다.<br />
이것은 확장도구(plug-in)가 아니니 wp-content/plugins 에 넣으면 아무 일도 일어나지 않습니다. 그만큼 이것은 위험도가 낮습니다. (?????)</p>
<p><strong>2.</strong> 이 파일의 확장자를 php로 바꾸십시오. 내려받을 때 확장자는 txt 입니다. 파일이름에 이상한 숫자도 붙고 했으니 아예 파일이름 전체를 바꿔봅시다. <strong>random_article.php</strong>로요. 원하는 다른 파일이름이 있다면 그걸로 해도 상관없습니다. 파일이름은 여러분의 마음 속에 있으면 그만입니다. 단, 파일 확장자는 반드시 <strong>php</strong>로 해야 합니다.</p>
<p><strong>3.</strong> 이 파일은 정확히 말해서 Template 파일입니다. 때문에 '쪽 (page)'에다가 붙여야 합니다. 그러려면 '쪽'부터 써야 겠군요.<br />
관리자 화면에 가셔서 '쪽 쓰기'로 가십시오. 아마 주소는 http://여러분의주소/워드프레스/wp-admin/page-new.php 일 겁니다. 남의 워드프레스 주소를 입력한게 아니라면 아마 쪽쓰기 화면이 잘 나올 겁니다. 거기에 잘 찾아보시면 '<strong>쪽 템플릿</strong>'이라고 있을 겁니다. 거기에서 '펼침선택상자'(Select box)를 누르면 <strong>Random article</strong>이라고 보일 겁니다. 위에 있는 1~2번 과정을 정확히 했다면 분명히 Random article이라는 것이 나옵니다. 그걸 고르십시오. '쪽이름'은 적당히 짓고 '주소'도 예쁘고 지으십시오. 편의상 쪽이름은 '아무거나 읽기'로 하고 주소는 'random_article'이라고 해보죠.</p>
<p><strong>4.</strong> 이제 여러분이 방금 만든 쪽을 열어보십시오. 다른 글로 이동할 겁니다. 제 블로그인 '<a href="http://blog.hannal.com">한날은 생각한다</a>'에 와서 보시면    친절한 길라잡이에 '아무 글이나 읽기'라는 것이 있는데, 이것이 지금 제가 이렇게 배포하고 있는 걸로 작동하는 겁니다.</p>
<p>어때요. 참 쉽죠?<br />
<img src="http://blog.hannal.com/wp-content/old_uploads/bob_rossvery_easy.jpg" alt="밥 로스 아저씨" /></p>
