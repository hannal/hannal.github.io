---
layout: post
status: publish
published: true
title: 내 개발 환경.
author:
  display_name: Kay
  login: Kay
  email: kay@hannal.net
  url: ''
author_login: Kay
author_email: kay@hannal.net
date: '2015-12-18 14:45:00 +0900'
categories: [devlife]
tags:
- python
- golang
- 개발환경
- os X
permalink: "/2015/12/my-dev-envs/"
---

프로그래밍 입문자, 또는 새로 프로그래밍이나 도구에 입문하는 사람과 얘기를 나누다 보면 다른 사람, 기왕이면 그 언어나 도구에 익숙한 사람이 사용하는 개발 환경을 무척 궁금해 한다는 걸 느꼈다. 그냥 공식 홈페이지에 있는 걸 내려 받아서 설치하면 되는 거 아닌가? 생각하며 관련 자료를 찾아보니 사람들은 공식 홈페이지에 소개되지 않은 방법으로 개발 환경을 꾸린다는 걸 발견하면 더 혼란이 빠져서 아예 입문 자체를 부담스러워 하는 사람도 많다.

그래서 내가 쓰는 개발 환경을 정리해 본다.

### 공통 환경

#### PC

- Macbook Pro 13인치 (2015년 early)
- Macbook Pro 15인치 (2014년 early)
- iMac 20인치 (2011년 mid)

#### 운영체제

- 주 환경 : OS X. 내가 주로 활동하는 분야는 윈도우 보다는 리눅스나 OS X에서 개발하기 더 편하다.
- 보조 환경 : Ubuntu. 주로 실 서버에 올리기 전에 시험 동작하려고 사용하거나 라즈베리 파이용 뭔가를 만들 때 쓰는 환경이지만, 집에 있는 리눅스 박스가 저사양이라서 평소엔 잘 안 쓴다.
- 쉘(shell) : bash를 주로 써왔지만, 2015년 11월부터 zsh을 쓰고 있다. [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)을 쓰고 설정은 기본값으로 쓰고 있으며, plugin만 git, virtualenv, virtualenvwrapper를 설정했다.
- 터미널은 OS X에 기본 내장된 것을 사용한다.
- 맥 패키지는 [Homebrew](http://brew.sh)로 관리한다.
- 파일, 디렉터리 구조는 [tree](http://mama.indstate.edu/users/ice/tree/)을 쓴다. OS X는 `brew install tree`.
- 원격에 있는 파일은 [wget](https://www.gnu.org/software/wget/)로 받는다. OS X는 `brew install wget`.

#### 글꼴

[Hack](http://sourcefoundry.org/hack/)을 사용한다.

#### VCS client

- [git](https://git-scm.com/) : 기본 클라이언트를 터미널에서 쓴다.
- [sourcetree](https://www.sourcetreeapp.com/) : 커밋이 복잡하게 꼬였을 때 쓰지만, 느려서 가끔 쓴다.
- [joe](https://github.com/karan/joe) : `.gitignore` 파일을 다룰 때 쓴다.

### Python

#### 에디터

- [PyCharm](https://www.jetbrains.com/pycharm/) : 2015년 11월부터 쓰고 있다. 아직 익숙하지 않다.
- [Sublime text 3](http://www.sublimetext.com/) : 평소에 주로 써왔는데, 최근엔 조금씩 빈도를 줄이고 있다.
    - SublimeLinter + Python Flake8 lint : 코드 검사기는 [Flake8](https://flake8.readthedocs.org)을 SublimeLinter에 연동해 쓴다.
- VIM : 급히 간단히 편집할 때 쓴다.

#### Python 관련

- Python 3, 2.7 : 최근엔 3 버전으로 시작하는 프로젝트가 늘고 있지만, 아직은 2.7로 동작하는 게 더 많다.
- PyPy : 실 사용 환경에서 사용하고 있긴 한데, 여전히 제한되게 쓰고 있다.
- virtualenv/virtualenvwrapper : 주로 사용하는 Python 환경 격리 도구.


### Golang

#### 에디터

- [IntelliJ IDEA](https://www.jetbrains.com/idea/) : 2015년 11월부터 쓰고 있다. 느려서 답답한데, 편하긴 하다. golang 정식 plugin이 출시되었다.
- [Sublime text 3](http://www.sublimetext.com/)
    - plugin : Goimports, GoSublime, SublimeLinter-contrib-golint

### 문서와 자료

#### 편집

- markdown : 로컬에서 문서를 작성하는 경우엔 대부분 markdown으로 작성한다. 편집은 [Atom](https://atom.io/)으로 하는데, 한글이 많으면 어느 에디터든 무척 느려지기 때문에 코딩 할 땐 사용하지 않는 Atom을 markdown 문서 편집용으로 쓴다.
- google drive : 다른 사람과 협업하거나 공유해야 하는 경우에 사용한다. 주로 google docs, spreadsheet.
- dropbox paper : 베타판부터 쓰고 있긴 한데, dropbox의 최근 선택과 집중 행보를 보자니 오래 유지 안 하고 종료할 것 같아서 이젠 별로 사용하지 않는다.

#### 자료 관리

- 웹 스크래핑 : 모바일 환경에선 [pocket](https://getpocket.com/), PC 환경에선 pocket과 [devonthink](http://www.devontechnologies.com/products/devonthink/overview.html)로 스크랩한다. evernote + clearly를 썼는데, 갈수록 구려져서 안 쓴다.
- PDF : devonthink에 담아서 관리하며, dropbox에도 올려서 모바일 환경에서 접근한다.
- bookmark : 구글 크롬에 북마크한다. 구글 계정 동기화를 해놔서 내가 사용하는 장비 모두와 북마크 동기화가 늘 되어 있다.


### 이외

- Google chrome : 느리고 뚱뚱하지만, 구글 계정 연동이 편해서 여전히 쓴다.
- 이외 도구는 가장 기본 설정대로 사용한다.
    - R Studio, Apache spark, React, Jupyter, ...
