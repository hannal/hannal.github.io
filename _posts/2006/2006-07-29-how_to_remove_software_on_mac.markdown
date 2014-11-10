---
layout: post
status: publish
published: true
title: Mac OS X에서 무른모 삭제하기
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 843
wordpress_url: http://blog.hannal.com/blog/?p=843
date: '2006-07-29 15:37:24 +0900'
date_gmt: '2006-07-29 06:37:24 +0900'
categories:
- "essay"
tags: []
permalink: "/2006/7/how_to_remove_software_on_mac/"
---
<p>Mac OS X에서 무른모(프로그램, Software)를 삭제하는 방법은 아주 간단하다. <strong>지울 무른모 대표그림(icon)을 휴지통에 끌어다 놓으면 그만</strong>이다. 환경 설정 정보를 Registry라는 '정보 관리 영역'에 담는 MS Windows 계열과는 달리 Mac OS X는 각 무른모가 사용하는 환경 설정 정보이나 필요한 보조 파일들을 함께 담아둔다. 정확하고 자세하게 설명하자면, MS Windows는 운영 체제가 무른모가 사용하는 일부 파일을 가져가 system이나 system32 같은 폴더에 담아둔다. 하지만, Mac OS X는 거의 모든 경우 운영 체제가 파일을 가져가지 않는다. 전부 사용자 전용 공간에 배치한다.</p>
<p>이러다보니 MS Windows 계열은 무른모 제거 기능이 필수이다. MS Windows XP에 와서는 '프로그램 추가/제거'에서 관리할 수 있었지만, 그 이전 판들(95, 98, 2000 등)에선 각 무른모가 갖고 있는 전용 제거기(uninstall.exe 등)를 찾아가 지워야 하곤 했다. 그런데, 안타깝게도 이런 제거기로도 무른모는 깨끗하게 지워지지 않는다. 무른모가 사용하던 dll 파일이 남아있거나 환경 설정 정보가 Registry에 그대로 남아있어 운영 체제에 부담을 준다. 그래서, MS Windows에 대해 어느 정도 능숙한 사람들은 직접 이런 찌꺼기를 찾아 지운다.</p>
<p>Mac OS X는 저런 과정으로 무른모를 지울 일이 <strong>거의</strong> 없다. Mac OS X는 훨씬 간편하고 안전하며 깔끔하다. 앞서 얘기했듯이, 지울 무른모 대표 그림을 휴지통에 끌어다 놓으면 끝이다. 그러면 Mac OS X가 알아서 무른모가 사용하던 보조 파일(library)이나 환경 설정 정보를 지워준다. 너무 쉽고 간단해서 MS Windows만 써오던 사람들은 똥 누고 닦지 않은 것처럼 께름칙하다고 생각하곤 한다.</p>
<p>물론, 드물지만 가끔 깨끗하게 지워지지 않는 경우가 있다. 내가 겪은 경우인데, <u>iLife 06 무른모들을(iPhoto, iDvd, iMovie, GarageBand) 휴지통에 버리는 걸로 지우려 했는데 iPhoto, iDvd, iMovie의 일부 파일이 지워지지 않고 남아있었다. 나중에 iLife 06을 다시 깔려는데 iPhoto, iDvd, iMovie를 설치하지 못해 설치 과정이 계속 취소</u>되었다.</p>
<p>이런 경우엔, Finder에서 해당 무른모의 관련 파일을 찾아 지워주면 된다. Finder에서 iPhoto 관련 찌꺼기를 지운다고 가정하고, 이 과정을 단계별로 보자.</p>
<p class="centerphoto"><a href="http://blog.hannal.com/wp-content/old_uploads/finder00.png" rel="lightbox"><br />
<img src="http://blog.hannal.com/wp-content/old_uploads/finder00.png" width="450" alt="Mac OS X의 Finder 화면" /></a><br />
그림을 누르면 원래 크기로 볼 수 있음</p>
<div style="width: 600px; margin-left: 40px;">
1. Finder를 열기 : 잘 모르겠다면 바탕화면에 마우스 지시자를 대고 단추를 누른 뒤, Command 글쇠 + f 글쇠를 누르면 된다. Command 글쇠는 사과 그림이 그려져 있는 글쇠이다. 바탕화면에 있는 Macintosh HD 을 열어도 된다. Finder는 <del datetime="2006-07-29T06:41:35+00:00">우리 마음 속에</del> <ins datetime="2006-07-29T06:41:35+00:00">어디에나</ins> 있다.</p>
<p>2. iphoto 관련 파일 찾기 : 돋보기 그림이 있는 검색 입련란에 iphoto 라고 써넣으면 iphoto 관련 파일들이 나온다. 정확히 따지자면, 파일 이름에 iphoto 라는 말이 들어가있거나 파일 내용에 iphoto 라는 말이 들어가있는 파일들이 나온다.</p>
<p>3. 찌꺼기 지우기 : 이곳에 나온 파일들을 지운다.
</p></div>
<p>이렇게 하면 어지간해서는 찌꺼기를 완전히 지울 수 있다.</p>
<p>얼핏보면 복잡해보이지만 위와 같은 일은 거의 일어나지 않는다. 대게 지울 무른모 대표 그림을 휴지통에 버리는 걸로 무른모 삭제는 깔끔하게 끝난다.</p>
<p>※ 보다 자세하게 알아보기 : <a href="http://myhome.naver.com/mr_x_man/">Mr X</a>님의 <a href="http://myhome.naver.com/mr_x_man/page_1/install_uinstall/install_uinstall.html">응용프로그램의 설치와 삭제</a> 글 참고.</p>
