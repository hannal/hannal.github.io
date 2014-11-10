---
layout: post
status: publish
published: true
title: "비잉~~ 돌아서 오기"
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 984
wordpress_url: http://blog.hannal.com/?p=984
date: '2007-02-11 00:55:00 +0900'
date_gmt: '2007-02-10 15:55:00 +0900'
categories: []
tags:
- "희망"
permalink: "/2007/2/my_long_way_round-_/"
---
<p>요 근래  정신을 어디다 두고 사는 건지 하지 않아도 될 실수를 무척 자주 많이 한다. 엉뚱한 곳에서 문제 원인을 찾느라 몇 시간이나 허비하고는 멍하니 있다가 찾아내기 일쑤다. 하루에 기능 하나 만드느라 바쁘다. 헐.</p>
<p>오늘은 livejournal.com에 있는 계정 하나를 wordpress로 옮기는 일을 했다. 내 블로그는 아니고, 친구 블로그인데 오늘 하루를 헛되게 보내서 화가 날만큼 실수를 했다. 할 일은 간단했다. livejournal에 있는 글을 xml로 내려 받은 뒤, 각 xml을 xml 해부 파일로 연 뒤 wordpress에 집어 넣는다. 정말이지 아주 쉽고 간결하다.</p>
<p>그런데 잘 안됐다. 자꾸 몇 개만 옮겨지고 특정 xml에서 뻗더라. 이것 때문에 2시간을 헤맸다. 도저히 안되겠다 싶어서 인터넷에서 cgi 형태로 실행하지 않고, 계정에서 php로 직접 실행했다. <strong>Segmentation fault</strong>가 떴다. 이 오류 원인은 다양하지만, 대체로 메모리 과부하를 일으나려 할 때 발생하며 스스로 죽을 때 나온다. 인터넷에서는 이 오류 내용이 뜨지 않고 웹서버가 죽은 것처럼 보여서 당연스럽게도 이런 내용을 알 수 없다. 2시간 만에 원인을 깨닫고는 메모리 낭비 부분을 찾아 손을 보기 시작했다...만, 효과는 없었다. 아무래도 xml 해부기(parser)가 쓰는 꾸러미가 문제인 것 같다. 고작 xml 하나 가져와 요소 단위로 나누는데 그 덩치 큰 PEAR를 열고, DB에 내용을 넣으려고 ADODB까지 연다. 뻗을만 했다. 어쩐다. 크기가 큰 xml 파일은 여럿 있고, 이 놈들 읽을 때마다 Segmentation fault로 뻗을 건 뻔하고. 답이 나오지 않는 상황이었다. Perl로 짤까? 했지만 그러고 싶지 않았다. -_- python도 귀찮았다. (치환문 때문에)</p>
<p>진도도 안나가고 해서 wordpress 관리자 영역에 들어가 괜히 이것 저것 눌러 봤다. 그런데 '가져오기(import)' 기능이 있고, 그곳에 Livejournal용 xml 파일을 wordpress로 가져오는 기능이 있었다. 내가 쓰는 wordpress 두 개 모두 판올림 하기 귀찮아서 예전 판 그대로 둬서 몰랐는데(하나는 1.5.2, 또 하나는 2.0.3), 최신 판(2.1)에는 있던 모양이다. 오오, 하는 마음에 말썽을 일으키는 뚱뚱한 xml 파일을 wordpress로 가져와 봤는데(import) 놀랍게도 아주 간단히 들어왔다. 뭐지, 뭐지, 하면서 소스(source)를 들여다 봤는데... 아뿔싸... 나처럼 <strong>무식</strong>하게 xml을 요소 별로 나누느라 덩치 큰 도우미들 불러오지 않고, 아주 간단하게 치환문(replace)으로 해결했더라. 이를테면</p>
<blockquote><p>preg_match('|&lt;subject&gt;(.*?)&lt;/subject&gt;|is', $post, $post_title);</p></blockquote>
<p>이런 식이다. 컥... 아찔했다. 굳이 wordpress에 있는 db table 구조를 파악할 필요도 없었다. wordpress에 집어 넣을 xml 파일을 지정하고 내 목적에 맞게 내용을 가져와 가공한 뒤 wordpress에 집어 넣는 함수로 내용물을 던져주면 끝이었다. 그래서 wp-admin/import/livejournal.php 을 열어서 xml 파일을 가져오는 부분을 손 본 뒤 718개나 되는 xml 파일을 wordpress에 다 넣었다. 소스 고치고 집어 넣는데 20분이 채 안걸렸다.</p>
<p>애초 wordpress 새 판을 좀 더 꼼꼼히 살펴보고 이렇게 했다면 총 30분 만에 끝났을 일이 3시간 넘게 걸렸다. 원래 예정대로라면 토요일 하루는 livejournal에 있던 글을 모두 wordpress로 옮기고 글 갈래(category)를 지정하고 미리 매겨 놓은 꼬리표(tag)까지 집어 넣었어야 했다.</p>
<p>할 일은 많고 시간은 없는데, 적은 일을 많은 시간을 들여 처리하는 요즘같은 비효율에 아주 진저리 난다.</p>
<p>음... 갑자기 네이버 지도 API 때문에 2시간 정도 뻘짓한 기억이 나는군. 문서엔 분명히 utf-8로 정보를 주고 받는다고 했는데, geocode.php (주소를 치면 좌표 정보를 xml로 뱉어주는 파일)은 euc-kr로 정보를 받고 xml 파일도 euc-kr이었다. utf-8로 정보를 아무리 넘겨줘도 마치 접근을 하지 못한 것처럼 결과를 뱉어내서 php으로 proxy server를 만들었다. 그런데 아무래도 curl 함수들이 필요해서 웹호스팅 업체에 요청을 했더니 안해준다고 했다. 싫다는데 어쩔 수 없지. 그래서 python으로 proxy server를 만들었다. 그래도 안되서 하늘 멍하니 쳐다봤다가 새로운 마음으로 다시 시작하려는데 혹시나 해서 euc-kr로 정보를 날려주니 반응하더라. -_-+ utf-8로 정보를 주고 받는다는 naver openapi에 있는 문서에다가 “뻥치지마”라고 글을 걸고(trackback) 싶었다.</p>
