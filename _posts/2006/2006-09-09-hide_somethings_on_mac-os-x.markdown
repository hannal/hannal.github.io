---
layout: post
status: publish
published: true
title: Mac OS X에서 파일 및 폴더 숨기기
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 895
wordpress_url: http://blog.hannal.com/blog/?p=895
date: '2006-09-09 00:15:49 +0900'
date_gmt: '2006-09-08 15:15:49 +0900'
categories:
- "essay"
tags: []
permalink: "/2006/9/hide_somethings_on_mac-os-x/"
---
<p>Windows를 쓰는 사람이건 Mac OS X를 쓰는 사람이건 잘 안쓰는 기능 중 하나가 파일이나 폴더를 숨기는 기능인 것 같다. Windows에선 파일 속성에 숨김(Hidden) 속성을 추가하면 숨길 수 있다. 이렇게 숨긴 파일이나 폴더를 다시 드러내려면 숨김 속성을 빼거나 탐색기 등에서 보기 부가 기능에 숨김 파일도 볼 수 있게 하면 된다.</p>
<p>Mac OS X는 Windows와는 달리 파일 이름을 바꿔주는 걸로 간단히 파일을 숨길 수 있다. Mac OS X 이전 판(Mac OS 9라던가)은 어찌하는지 모르겠지만, Mac OS X는 Unix 운영 체계이기 때문에 Unix나 Linux에서 파일을 숨기는 방법을 그대로 하면 된다. <strong>파일 이름 앞에 점을 하나 찍는 것</strong>이다. 그런데 '파인더'에서는 파일 이름 앞에 점을 찍을 수 없다.</p>
<p class="centerphoto"><img src="http://blog.hannal.com/wp-content/old_uploads/cannot_input_dot.png" alt="파인더에서 파일 이름 앞에 점을 찍자 거부하는 경고창을 띄운다" style="border: 1px solid #000;" /><br />
이런 경고창을 띄우며 거절한다</p>
<p>그럼 어떻게 파일이나 폴더 이름 앞에 점 하나를 찍어 비밀스러움을 만끽할 수 있을까? 터미널을 사용하면 된다. Mac OS X의 '응용 프로그램' 폴더에 보면 '유틸리티'라는 폴더가 있는데 그 안에 보면 '터미널'이라는 아이콘이 있다. 그걸 실행하면 Linux나 Unix 환경에서 흔히 볼 수 있는 터미널 화면이 뜬다. 나는 Mac OS X에 내장된 것을 쓰지 않고 ITerm이라는 걸 쓴다.</p>
<p class="centerphoto"><img src="http://blog.hannal.com/wp-content/old_uploads/hide_01.png" alt="hide_me라는 폴더를 숨길 준비한 그림" style="border: 1px solid #000;" /></p>
<p>자, 보다시피 hide_me 라는 폴더가 있다. 편의상 이용자의 최상위 위치에 만들었다. 터미널에선 <strong>cd ~/</strong> 이라고 치면 된다. 이제 터미널에서 hide_me 폴더 이름 앞에 점 하나를 찍어 숨겨보자.</p>
<p class="centerphoto"><img src="http://blog.hannal.com/wp-content/old_uploads/hide_02.png" alt="hide_me라는 폴더를 숨겨서 파인더에 안보이는 그림" style="border: 1px solid #000;" /></p>
<p>짠! 터미널에서 <strong>mv hide_me .hide_me</strong>라고 치자 폴더가 숨겨져 파인더에서 사라졌다. 단지 보이지 않을 뿐 분명 존재한다. <strong>mv .hide_me hide_me</strong> 이렇게 점을 없애주면 다시 나타난다.</p>
<p>위 명령에서 mv 라는 놈은 파일이나 폴더를 다른 곳으로 옮겨주는 일을 하는 파일이다. Mac OS X 위에선 찾을 수 없는데 터미널에서 <strong>ls -al /bin/mv</strong> 라고 쳐보면 이 mv 파일이 /bin 라는 폴더(Directory)에 있는 걸 볼 수 있다.</p>
<p>사용법은 아주 간단하다. <strong>mv 옮길대상체 옮길곳</strong>이다. <strong>mv hannal.pages /tmp</strong>라고 하면 hannal.pages라는 파일을 /tmp 라는 곳에 옮기겠다는 뜻이다. 옮길대상체는 꼭 파일이 아니어도 돼서 폴더도 옮길 수 있다. 옮길곳 이름을 쓸 때 옮길대상체의 이름을 바꾸고 싶다면 그 이름까지 같이 쓰면 된다. <strong>mv hannal.pages /tmp/hannal2.pages</strong> 이렇게 하면 hannal.pages 파일이 /tmp 에 옮겨질 때 hannal2.pages 라는 이름으로 간다. 당연히 옮길 곳을 지정하지 않고 그 이름만 쓴다면 제자리에서 대상체를 옮기며 이름을 바꾸게 된다. 즉, 파일이나 폴더 이름을 바꾸는 것이다. 그래서 <strong>mv hannal hannal2</strong>라고 하면 hannal이라는 것이 hannal2라는 이름으로 바뀌는 것이고, 이 방법을 이용해서 파일 이름 앞에 점을 덧붙이는 것이다. mv에 대한 자세한 설명을 보고 싶다면 터미날 안에서 <strong>man mv</strong> 라고 치면 설명서가 나온다(아마 영문일 것이다). 참고로 man은 manual의 줄임말이다.</p>
<p>3줄 요약을 하자면,</p>
<ul>
<li>Mac OS X에서 파일이나 폴더를 숨기려면 그것의 이름 앞에 점을 써주면 된다.</li>
<li>이름 앞에 점을 쓰려면 터미널 안에서 해야 한다.</li>
<li>터미널 안에서 파일이나 폴더 이름을 바꾸려면 mv 라는 실행 파일을 이용해야 하며, 사용법은 'mv 옮길대상체 옮길곳'이다.</li>
</ul>
<p>이다.</p>
<p>만일, 숨긴 파일이나 폴더가 어딨는지 까먹어서 못찾겠다면 파인더에서 쉽게 찾아도 되고 터미널에서 찾을 수도 있다. 터미널에서 파일이나 폴더 목록을 보는 실행 파일이 <strong>ls</strong>인데 이 것을 실행할 때 <strong>-a</strong>라는 꼬리표를 붙이면 <strong>모든</strong> 파일이나 폴더를 보여준다. 나는 습관처럼 <strong>ls -al</strong>이라고 친다. (ls -al 은 ls -a -l 과 같은 뜻이다. -a 꼬리표와 -l 꼬리표를 쓴 것이다)</p>
<p>터미널의 무뚝뚝하고 불친절한 화면이 싫다면 파인더에서 파일 찾기 기능을 이용해도 된다. 이것에 대한 설명은 Mac OS X 안에 이미 있으니 그것을 참조하자. 바탕화면에 마우스를 누른 뒤 <strong>Command 글쇠(사과그림 있는 글쇠) + Shift 글쇠 + / 글쇠</strong>를 누르면 도움말이 뜰 것이다. 안뜨면 파인더를 열고 '도움말'을 직접 열자. 도움말 창이 뜨면 검색란에 <strong>'가려진 파일 찾기'</strong>라고 치면 파인더에서 숨긴 파일이나 폴더를 찾는 방법이 나와있다.</p>
