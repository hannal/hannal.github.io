---
layout: post
status: publish
published: true
title: "유튜브에서 고화질 동영상 버퍼링이 심할 때."
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 2013
wordpress_url: http://blog.hannal.com/?p=2013
date: '2010-03-20 21:22:36 +0900'
date_gmt: '2010-03-20 12:22:36 +0900'
categories:
- "essay"
tags:
- adobe
- "유튜브"
- "플래시"
- "하드웨어가속"
permalink: "/2010/3/hw_acceleration_of_video_for_adobe_flash"
---
<h3>유튜브에서 고화질 동영상 보기 너무 힘들다</h3>
<p>유튜브에서 고화질 동영상, 가령 480p 이상인 영상을 볼 때 재생 막대에 빨간색이 아주 느리게 차오르는 버퍼링 지연 현상이 종종 생긴다. 처음엔 워낙 고화질 영상이라 동영상 용량이 크고, 유튜브 서버가 국외에 있어서 네트워크에서 지연이 걸리나보다 생각했다. 그런데 가만히 지켜보니 네트워크 문제라고 하기엔 지나치게 버퍼링이 오래 걸렸다.</p>
<p>그래서 네트워크 송수신 상황을 따로 들여다보니(monitoring) 네트워크 문제가 아니었다. 왜냐하면 동영상 파일(flv)은 몇 초 안에 다 내려 받았는데, 여전히 동영상 재생기에서 빨간색 막대는 느릿느릿 차올랐기 때문이다.</p>
<h3>버퍼링이 심한 원인 분석</h3>
<p>동영상을 볼 때 지연이 일어나는 요소는 대체로 두 가지를 들 수 있다. 하나는 전송 속도이고, 다른 하나는 화면 출력 속도이다. 가령 HD 1080p 급 동영상을 보려는데 하드디스크 자체가 지나치게 느리거나 파일 복사를 하고 있어서 하드디스크에 과부하가 걸릴 경우 영상이 뚝뚝 끊기게 된다. 온라인 동영상이라면 네트워크 전송 속도가 더 큰 영향을 미칠 것이다.</p>
<p>두 번째는 화면 출력 속도인데, CPU나 그래픽(비디오) 카드가 고화질 동영상을 빠르게 재생하지 못하는 것이다. 요즘엔 CPU 성능이 워낙 좋아서 어지간한 고화질 영상이야 CPU 성능으로도 감당이 가능하긴 하다. 하지만, 대체로 그래픽 처리를 전문으로 하는 그래픽 카드, 정확히는 GPU가 CPU보다는 더 효율 있게 처리할 수 있다.</p>
<p>원인에 대해 결론부터 말하자면, 내가 유튜브 동영상 볼 때 버퍼링 심한 것은 화면 출력쪽이 원인이었다. 하지만, 내가 쓰는 그래픽 카드는 HD 1080p도 별 무리없이 재생하는 성능을 갖고 있는데, 고작(?) 480p 급을 재생하는 데 이렇게 버벅대다니? 그렇다면 그래픽 카드 문제라기 보다는 동영상을 재생하는 재생기를 의심해야 한다. 바로 Adobe Flash Player이다.</p>
<h3>해결 방법 : 하드웨어 가속 기능은 Adobe Flash Player 10.1부터...</h3>
<p>현재(2010년 3월 기준) 사용되는 Adobe Flash Player는 10.0이 정식판이다. 그런데 이 판은 동영상을 재생할 때 하드웨어 가속을 이용하는 기능이 신통치 않다. 제대로 된 하드웨어 가속 기능은 아직 시험판인 10.1에 들어갔다.</p>
<p>자신의 운영체제에 깔린 Adobe Flash Player의 판번호는 <a href="http://www.adobe.com/software/flash/about/">http://www.adobe.com/software/flash/about/</a> 에서 확인할 수 있다. 화면 가운데쯤에 Version information이라는 상자에 판번호가 나오는데 아마도 10.0.42 나 10.0.45 일 것이다. 이 판이라면 아마도 고화질 동영상이 버벅댈 것이다. 버벅대는 걸 확인하려면 유튜브에 가서 <a href="http://www.youtube.com/results?search_type=videos&amp;search_query=kara&amp;high_definition=1&amp;suggested_categories=10,24,22&amp;uni=3">Kara</a> 나 <a href="http://www.youtube.com/results?search_type=videos&amp;search_query=snsd&amp;high_definition=1&amp;suggested_categories=10,24,22&amp;uni=3">SNSD</a> 로 검색해보면 카라나 소녀시대의 고화질 동영상을 보면 된다. 480p 정도인데도 버퍼링이 느리다면 아래 단계를 진행해본다.</p>
<p>먼저 <strong>10.0판을 지워야</strong> 한다. 자신의 운영체제에서 기존에 설치된 Adobe Flash Player 를 지워야 한다. 윈도우 운영체제 사용자라면 제어판에서 “프로그램 및 기능”이나 그 비슷한 이름을 가진 아이콘을 실행하면 된다. 거기에서 Adobe Flash Player 10(혹은 9) Plugin 과 Active-X 를 지운다.</p>
<p>이번엔 <strong>10.1판을 설치</strong>한다. 정식판이 아닌 시험판이므로 별도 웹페이지에서 받아야 하는데, <a href="http://labs.adobe.com/technologies/flashplayer10/">http://labs.adobe.com/technologies/flashplayer10/</a> 여기에서 받으면 된다. 열려있는 모든 웹 브라우저를 종료한 뒤에 설치하면 끝난다. 2010년 3월 중순 기준으로 10.1.51.95판이 가장 최신판이다.</p>
<p>설치가 끝났다면 앞서 버벅댔던 동영상을 다시 봐보자. 아마도 빨간색 재생 막대가 이전보다 더 긴박하게 움직일 것이다. ^^</p>
