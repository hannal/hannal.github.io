---
layout: post
status: publish
published: true
title: KT... 무슨 짓을 하고 있는거지?
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 741
wordpress_url: http://blog.hannal.com/blog/kt-%ec%9d%b4%eb%86%88%eb%93%a4-%eb%ac%b4%ec%8a%a8-%ec%a7%93%ec%9d%84-%ed%95%98%ea%b3%a0-%ec%9e%88%eb%8a%94%ea%b1%b0%ec%a7%80/
date: '2006-02-10 13:39:53 +0900'
date_gmt: '2006-02-10 04:39:53 +0900'
categories: []
tags:
- "희망"
permalink: "/2006/2/kt_what_are_you_doing_now/"
---
<p>우리 집은 KT의 메가패스를 이용한다. 공유기를 이용해 셈틀(PC) 2대로 인터넷을 사용하고 있다.</p>
<p>예전엔 그러지 않은 것 같은데 언젠가부터 누리집 이동할 때마다 이동한 곳을 거쳐간다. 이상해서 화면이 바뀌기 직전에 ESC 단추를 누른 뒤 '소스보기'를 봤다. 그랬더니</p>
<p>&lt;html><br />
&lt;frameset rows='0,*' border='0'><br />
&lt;frame src='http://59.9.131.12/NoticeManage/top1.asp?중략&x=www.hannal.net' >후략&lt;/frame>&lt;/frameset><br />
&lt;/html></p>
<p>이런 소스가 나왔다. 오호. 이것봐라? Internet Explorer라면 내가 모르는 사이 동생이나 어머니께서 이상한 Active-X를 설치하는 실수를 할 수도 있지만 Firefox를 사용하는데도 저런게 나온다면 좀 더 거대한(?) 음모가 있을 거라는 생각이 들었다. 그래서 저 수상한 59.9.131.12 라는 ip를 조사해보기로 했다. 그랬더니</p>
<p># KOREAN</p>
<p>조회하신 IPv4주소는 ISP가 아직 할당하지 않은 주소이거나 고객(End-User)에게 IPv4주소를<br />
할당한 후 할당내역을 한국인터넷진원에 등록하지 않은 주소공간입니다.</p>
<p>따라서, 조회하신 IPv4주소에 대한 문의는 아래의 ISP 담당자에게 문의하시기 바랍니다.</p>
<blockquote><p>[ ISP의 IPv4주소 관리기관 정보 ]<br />
기 관 명      : (주)케이티<br />
서비스명      : KORNET<br />
기관 주소     : 성남시 분당구 정자동<br />
기관 상세 주소: 206 한국통신 e-Biz본부 기획팀</p>
<p>[ ISP의 IPv4주소 책임자 정보 ]<br />
이    름      : IP주소관리자<br />
전화번호      : +82-2-3674-5708<br />
전자우편      : ip@ns.kornet.net</p>
<p>[ ISP의 IPv4주소 관리자 정보 ]<br />
이    름      : IP주소담당자<br />
전화번호      : +82-2-3674-5708<br />
전자우편      : ip@ns.kornet.net</p>
<p>[ ISP의 Network Abuse 담당자 정보 ]<br />
이    름      : 스팸/해킹담당<br />
전화번호      : 080-223-5577</p></blockquote>
<p>라고 한다. 어? KT라고라? 그래서 이번엔 저 ip 있는 곳으로 가봤다. <a href="http://59.9.131.12/NoticeManage/top1.asp">http://59.9.131.12/NoticeManage/top1.asp</a> 여기로 갔는데 역시 아무 화면도 안나온다. 그래서 소스보기로 가봤더니...</p>
<blockquote><p>function popup()<br />
{<br />
	g1 = frm.g1.value;<br />
	n_type = frm.n_type.value;<br />
	userID = frm.userID.value;<br />
	cnt_g = frm.cnt_g.value;<br />
	cnt_i = frm.cnt_i.value;<br />
	msgUrl = frm.msgUrl.value;<br />
	width = frm.width.value;<br />
	height = frm.height.value;<br />
	msgUrl2 = frm.msgUrl2.value;<br />
	width2 = frm.width2.value;<br />
	height2 = frm.height2.value;<br />
	total = frm.total.value;<br />
	<strong>mainsvIP = frm.mainsvIP.value; //임시 (공유기 서비스가입)</strong></p></blockquote>
<p>이렇게 더 수상한 javascript 부분이 보였다. 저기 굵게 표시된 부분을 보면 왜 수상하다고 하는 지 이해갈 것이다.</p>
<p>기분이 팍 상하면서 대단히 찝찝했다. 그래서 이번엔 공유기를 사용하지 않고 셈틀에서 바로 메가패스 단말기(modem)로 연결해서 확인해보기로...<br />
.<br />
.<br />
.<br />
.<br />
마음만 먹었다. 접속 풀그림(program) 설치하고 랜선 다시 붙이는 일이 너무 너무 <strong>귀찮았기</strong> 때문이다. orz</p>
<p>그래서 그런데 누가 좀 알아봐주시라. 이미 알고 있으면 살짝 알려주고.</p>
