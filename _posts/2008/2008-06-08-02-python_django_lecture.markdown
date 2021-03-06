---
layout: post
status: publish
published: true
title: "날) Django 설치 (2편) - django 강좌"
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
excerpt: "이번 글에서는 여러분의 컴퓨터에 Django 를 설치하는 방법을 다룬다. 크게 Windows XP와 Mac OS X 환경만 다룰
  것인데, Linux나 FreeBSD 같은 다른 Unix 환경은 Mac OS X 와 거의 비슷하고, 평소 쓰는 운영체제가 Linux나 Unix 계열인
  사람은 알아서 잘 처리하는 경우가 흔해서 이쪽 운영체제에 설치하는 법은 생략한다."
wordpress_id: 2312
wordpress_url: http://blog.hannal.com/?p=59
date: '2008-06-08 20:17:10 +0900'
date_gmt: '2008-06-08 11:17:10 +0900'
categories:
- "start_with_django_lectures"
tags:
- django
- python
- "파이썬"
- django입문강좌
permalink: "/2008/6/02-python_django_lecture/"
---
<p>이번 글에서는 여러분의 컴퓨터에 Django 를 설치하는 방법을 다룬다. 크게 Windows XP와 Mac OS X 환경만 다룰 것인데, Linux나 FreeBSD 같은 다른 Unix 환경은 Mac OS X 와 거의 비슷하고, 평소 쓰는 운영체제가 Linux나 Unix 계열인 사람은 알아서 잘 처리하는 경우가 흔해서 이쪽 운영체제에 설치하는 법은 생략한다.</p>
<p>뿐만 아니라 이 강좌에선 설치나 설정 방법에 대해 자세히 다루진 않는다. 관련 정보가 이미 인터넷에 많으며 이 글에서 다루기엔 내용이 많아져 글이 길어지기 때문이다.</p>
<h3>MySQL 설치</h3>
<p>우리가 이 강좌를 통해 만들 블로그 도구는 MySQL이라는 데이터베이스 무른모(software)를 쓴다. 그러므로 MySQL을 설치해야 한다.</p>
<h4>Windows XP 에 MySQL 설치</h4>
<p>설치는 간단하다. <a href="http://dev.mysql.com/get/Downloads/MySQL-5.0/mysql-essential-5.0.51b-win32.msi/from/pick#mirrors">MySQL 공식 누리집</a>에 가서 받으면 된다. 여러 나라 국기 그림이 있는데 태극기가 있는 곳에 보면 HTTP나 FTP 중 하나를 누르면 파일을 설치 파일을 받는다. 만약 Windows가 64bit 라면 <a href="http://dev.mysql.com/get/Downloads/MySQL-5.0/mysql-essential-5.0.51b-winx64.msi/from/pick#mirrors">64bit 용 MySQL</a>을 받아야 한다.</p>
<p>자세한 설치법과 설정법은 다른 이의 글을 참조하자.<br />
	•	<a href="http://idodream.springnote.com/pages/407176">MySQL 5.0.45 + Windows XP sp2 설치 - 짧게</a><br />
	•	<a href="http://www.google.co.kr/search?complete=1&hl=ko&newwindow=1&q=xp+mysql+설치+utf&btnG=검색&lr=&aq=f">구글에서 다른 글 찾아보기</a></p>
<h4>Mac OS X 에 MySQL 설치</h4>
<p>Windows XP와 마찬가지로 <a href="http://dev.mysql.com/downloads/mysql/5.0.html#macosx-dmg">MySQL 공식 누리집에서 Mac OS X용</a> 설치 파일을 받아다 설치하면 된다. 근데 나중에 판올림을 할 때는 이 방법이 나중에 귀찮음을 일으킬 수 있다. 내 경우는 mac port 라는 도구를 써서 판올림 관리를 하는 무른모를 설치하거나 판올림을 한다. 참 좋은 도구이니 이걸로 설치해보자.</p>
<p>우선 <a href="http://www.macports.org/install.php">mac port 설치 파일</a>을 받자. 레오파드용, 타이거용 등 여러 가지 설치 파일이 있는데 자신의 운영체제에 맞는 걸 받아다 깔자. 자신의 운영체제가 무엇인지 모르겠다면, 화면 맨 왼쪽 위에 있는 사과 아이콘을 누른 뒤 “이 매킨토시에 관하여”를 누르면 버전이 나오는데 10.5.x 라면 레오파드, 10.4.x 라면 타이거이다.</p>
<p>처음 mac port 로 뭔가를 설치하면 “이거 뭔가 잘못 된 것 아닌가?” 싶을 정도로 오랜 시간 화면에 별 반응이 없다. 별 말이 없다면 제대로 되고 있는 것이니 차 한 잔 마시며 기다리자.</p>
<p>설치가 끝나면 터미날을 실행한다 (난 내장된 터미날 대신 따로 <a href="http://iterm.sourceforge.net/">iTerm</a>을 쓴다. 물론 공짜다). 그런 뒤 다음과 같이 명령어를 실행한다.</p>
<blockquote><p>sudo port install mysql5 +server</p></blockquote>
<p>보다 자세한 설치 설명과 설정은 다른 이의 글을 참조하자.</p>
<p>	•	<a href="http://rukikuki.tistory.com/87">Mac OS X 에서 MySQL 설치</a><br />
	•	<a href="http://www.google.co.kr/search?complete=1&hl=ko&newwindow=1&q=macport+mysql+설치+utf&btnG=검색&lr=&aq=f">구글에서 다른 글 찾아보기</a></p>
<h3>Python 설치</h3>
<p>Django 는 Python이라는 프로그래밍 언어를 기반으로 하므로 Django 설치에 앞서 Python 이 설치 되어 있어야 한다. Mac OS X 나 Linux/Unix 엔 이미 Python 이 깔려 있지만, Windows 에는 따로 깔아야 한다.</p>
<h4>Windows XP 에 Python 설치</h4>
<p><a href="http://www.python.org/download">공식 누리집</a>에 가서 Python 을 받아 설치하자. 이 강좌를 쓰는 현재 2.5판이 최신판이니 2.5를 받으면 되는데, 보면 몇 가지 종류로 나눠 놨다. 간단하게 설치본(setup)을 쓰자. 이 글이 쓰여진 2008년 6월 8일 기준으로 Python 2.5.2 Windows installer 를 받으면 된다.</p>
<p>설치는 간단하다. 받은 파일을 실행하면 그만인데, 따로 설정을 하지 않으면 c:python25 에 설치된다.</p>
<p>이번엔 python 에서 mysql 에 접속하는 데 필요한 py-mysql 을 깔아야 한다. 이건 <a href="http://sourceforge.net/project/showfiles.php?group_id=22307&package_id=15775&release_id=491012">MySQL for Python</a> Windows 판을 받아다 설치하면 된다.</p>
<h4>Mac OS X 에 Python 설치/판올림</h4>
<p>Mac OS X에 깔려 있는 Python이 예전 판이면 판올림을 하고, 깔려 있지 않은 경우 받아다 깔면 된다. 마찬가지로 mac port 를 이용해 설치/관리하자.</p>
<p>방법은 MySQL 과 마찬가지로</p>
<blockquote><p>sudo port install python25</p></blockquote>
<p>라고 치면 된다.</p>
<p>설치하면 역시 /opt 에 설치 되는데, 터미널에서 python 이라고 쳐서 실행을 해보자. 만약 2.5라고 나오지 않는다면 파일 경로 우선 순위에서 예전 python이 새로 설치한 python 보다 앞서서 그렇다.</p>
<p>이 경우 두 가지 방법으로 해결할 수 있다. which python 이라고 하면 python 이 있는 위치가 나오는데, 이곳에 가서 ls -al 하면 이 python 에 연결된 실제 python 파일 경로를 알 수 있다. 왜냐하면 symbolic link 로 연결된 파일이기 때문이다. 이 경로를 방금 설치한 python 으로 연결하는 것이다.</p>
<blockquote><p>sudo rm /usr/bin/python<br />
ln -s /opt/local/bin/python2.5 /usr/bin/python<br />
rm /usr/lib/python<br />
ln -s /opt/local/lib/python2.5 /usr/lib/python<br />
rm /usr/include/python2.5<br />
ln -s /opt/local/include/python2.5 /usr/include/python</p></blockquote>
<p>두 번째 방법은 경로 우선 순위를 바꾸는 것이다. 이건  ~/.profile 파일(home 디렉토리에 있는 .profile 파일)에 경로 설정(PATH)에서 방금 설치한 python 경로를 앞에 두면 된다.</p>
<blockquote><p>vi ~/.profile</p></blockquote>
<p>(export PATH 라고 써있는 줄에 간 뒤에)</p>
<blockquote><p>/opt/local/bin:/opt/local/lib:/opt/local/include</p></blockquote>
<p>을 맨 앞에 넣어 다음과 같이 만든다.</p>
<blockquote><p>export PATH=/opt/local/bin:/opt/local/lib:/opt/local/include:/usr/local/bin:$PATH</p></blockquote>
<p>여기서는 vi 를 다룰 줄 안다고 가정한다. vi 를 다룰 줄 모른다면 다룰 줄 아는 다른 걸 쓰거나 vi 를 간단히 익히자.</p>
<p>이번엔 python 에서 mysql 에 접속하는 데 필요한 py-mysql 을 깔자. 이것 역시 mac port 를 이용하자.</p>
<blockquote><p>sudo port install py25-mysql</p></blockquote>
<h3>Django 설치</h3>
<p>두 가지 방법이 있다. 따로 꾸러미로 묶어내 배포하는 안정판을 받는 것과, 현재 개발 중인 소스를 svn 으로 그때 그때 받아올 수 있는 개발판을 받는 것이 그것인데, 이 강좌에선 배포판인 0.96.1 을 쓸 것이다. 근데 몇 몇 괜찮은 기능들이 svn 으로 받을 수 있는 0.97 pre 판에 있다. 아쉽지만 이런 기능들은 포기하고, 나중에 0.97 로 판올림 했을 때 충돌이 나지 않게 하기로 하자(강좌 중간 중간 그런 조치를 취할 것이다).</p>
<h4>Windows XP 에 설치</h4>
<p><a href="http://www.djangoproject.com">django 공식 누리집</a>에 가면 현재(2008년 6월 8일) 0.96.1판을 받을 수 있다. 받은 뒤 압축을 풀자.</p>
<p>그 안에 보면 여러 디렉토리들이 있는데 그 중 django 폴더를 찾자. 그것을 python 이 설치된 곳에 있는 lib/site-packages 안에 넣자. 경로가 c:python25libsite-packages 에 넣어서 <strong>c:python25libsite-packagesdjango</strong> 이런 형태가 되면 된다.</p>
<h4>Mac OS X 에 설치</h4>
<p>MySQL이나 python 설치와 마찬가지로 mac port 를 이용하자.</p>
<blockquote><p>sudo port install py25-django-devel</p></blockquote>
<p>정말 편하지 않은가. ^^ 설치하면 자동으로 /opt/local/lib/python2.5/site-packages/django 에 깔린다.</p>
<hr />
다음 글에서는 블로그를 기획하고 만들 준비를 할 예정이다. 물론 다음 글에서도 실제로 코딩을 하진 않는다. :)</p>
