---
layout: post
status: publish
published: true
title: mysql 서버 실행할 때 Starting MySQL.Manager of pid-file quit without updating 오류
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 2024
wordpress_url: http://blog.hannal.com/?p=2024
date: '2010-05-05 22:38:06 +0900'
date_gmt: '2010-05-05 13:38:06 +0900'
categories:
- "essay"
tags:
- mysql
- "권한"
- "서버"
permalink: "/2010/5/mysql_starting_error_about_permission_problem/"
---
<p>운영하던 mysql 서버의 root 비밀번호를 잃어버려서 root 비밀번호를 초기화 한 적이 있다. 구글링해서 찾은 초기화 방법은 다음과 같다</p>
<blockquote><p>safe_mysqld <strong>--user=root</strong> --skip-grant-tables &amp;</p></blockquote>
<p>문제는 굵게 표시한 “--user=root” 부분. 최근에는 mysql을 root 권한으로 구동하지 않고 mysql 이라는 계정으로 구동하는데, 저렇게 --user=root 옵션을 쓰면 안 된다. 이렇게 하면 mysql 의 데이터 디렉토리(data)에 생기는 로그 파일인 mysql-bin.000001 (숫자)파일이 평소에 mysql을 구동하는 계정이 아닌 root 계정 것으로 생긴다. 그래서 mysql 을 구동할 때</p>
<blockquote><p>Starting MySQL.Manager of pid-file quit without updat[실패]</p></blockquote>
<p>이런 오류가 나면서 가동이 안 된다. 이런 일을 예방하려면 --user=root 가 아니라 mysql 서버를 구동하는 계정 이름(가령, --user=mysql 등)으로 위 절차를 실행해야 한다. 이미 그런 일을 벌였다면 문제를 일으키고 있는 로그 파일의 소유자를 mysql 을 구동하는 계정으로 만들어 줘야 한다.</p>
<p>만약 mysql 의 데이터 디렉토리가 /usr/local/mysql/data 라면 다음과 같이 해보자.</p>
<blockquote><p>ls -al /usr/local/mysql/data</p></blockquote>
<p>만약 권한이 없다고 한다면</p>
<blockquote><p>sudo ls -al /usr/local/mysql/data</p></blockquote>
<p>라고 하면 된다(root 권한으로 ls -al 하는 것).</p>
<pre>-rw-rw----  1 mysql mysql    86537  3월 15 23:34 mysql-bin.000001
-rw-rw----  1 mysql mysql 67354804  5월  4 21:36 mysql-bin.000002
-rw-rw----  1 mysql mysql      254  5월  4 21:42 mysql-bin.000003
-rw-rw----  1 <strong>root root</strong> 16786292  5월  5 22:31 mysql-bin.000004
-rw-rw----  1 mysql mysql       76  5월  4 21:50 mysql-bin.index
drwx------  2 mysql mysql     4096  3월 15 16:26 test</pre>
<p>잘 보면 다른 파일과는 달리 혼자 다른 파일이 보인다. 저 녀석을 mysql 을 구동하는 계정(나는 mysql)의 것으로 만들어주면 된다.</p>
<blockquote><p>sudo <strong>chown mysql.mysql</strong> data/mysql-bin.000004</p></blockquote>
<p>이러면</p>
<pre>-rw-rw----  1 mysql mysql    86537  3월 15 23:34 mysql-bin.000001
-rw-rw----  1 mysql mysql 67354804  5월  4 21:36 mysql-bin.000002
-rw-rw----  1 mysql mysql      254  5월  4 21:42 mysql-bin.000003
-rw-rw----  1 <strong>mysql mysql</strong> 16786292  5월  5 22:31 mysql-bin.000004
-rw-rw----  1 mysql mysql       76  5월  4 21:50 mysql-bin.index
drwx------  2 mysql mysql     4096  3월 15 16:26 test</pre>
<p>이렇게 된다. 이젠 mysql 서버가 잘 가동될 것이다.</p>
