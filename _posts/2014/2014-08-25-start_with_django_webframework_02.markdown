---
layout: post
status: publish
published: true
title: 2. 개발 환경 꾸리기
author:
  display_name: Kay
  login: Kay
  email: kay@hannal.net
  url: ''
author_login: Kay
author_email: kay@hannal.net
wordpress_id: 3125
wordpress_url: http://blog.hannal.com/?p=3125
date: '2014-08-25 00:05:44 +0900'
date_gmt: '2014-08-24 15:05:44 +0900'
categories:
- "start-with-django-webframework"
tags:
- django
- python
- "파이썬"
- virtualenv
- pip
- virtualenvwrapper
- "설치"
- postman
permalink: "/2014/8/start_with_django_webframework_02/"
---

* [날로 먹는 Django 웹프레임워크 강좌 목차](http://blog.hannal.com/category/start-with-django-webframework/)
* 마지막 갱신일시 : 2017년 1월 29일 18시 00분

이번 2편에서는 Pystagram을 만드는 데 필요한 개발 도구를 설치하겠습니다. 저는 Mac OS를 쓰기 때문에 Mac OS 환경을 기준으로 설명하며, 윈도우나 리눅스 환경용 설명은 웹에 있는 관련 자료로 대신하겠습니다.

### 1. Python 설치

#### (1) Mac OS, Linux

##### Python 설치

Python은 현재 두 개 큰 버전이 배포되고 사용됩니다. 2 버전과 3 버전입니다. 우리가 사용할 Python 버전은 3.4 이상입니다. 터미널을 연 뒤에 다음 명령어를 입력해보세요. $는 입력하지 마시고요[^1].

```
$ python --version
```

아마도 `Python 버전` 형식인 문장이 출력될 겁니다. 대개는 버전이 2.7대입니다. 2버전대의 최신 버전은 [Python 공식 웹사이트에서 내려받아 설치](https://www.python.org/download)하거나 패키지 관리 도구로 설치하면 됩니다. 패키지 관리 도구로 프로그램과 같은 패키지를 관리하면 편하니 패키지 관리 도구를 권합니다. Mac OS라면 [Homebrew](http://brew.sh/)를 많이 씁니다. 저도 Homebrew로 설치하겠습니다.

```
$ brew install python3
```

Homebrew가 설치되어 있지 않다면 [brew부터 설치](http://brew.sh/#install)해야 하며, brew 설치할 때 xcode 관련 도구인 Command Line Tools가 요구되기도 합니다. 대개는 관련 과정이 함께 안내되는데, 실수로 그냥 넘어갔거나 안내되지 않았다면 다음 명령어로 Command Line Tools를 설치하면 됩니다.

```
$ xcode-select --install
```

Debian 계열 Linux라면 운영체제에 내장된 apt-get을 씁니다. Redhat 계열 Linux는 yum을 사용합니다.

```
$ apt-get install python3
```

그런데 아마 접근 권한 문제로 설치되지 않을 겁니다. 오류 내용을 잘 보면 `Permission denied` 문구가 포함되어 있지요. 이런 경우 `sudo` 명령어도 써야 합니다. sudo는 다른 이용자(대개는 최고권한자(superuser 또는 root))의 권한으로 sudo 명령어 뒤에 나오는 내용을 수행하는 프로그램입니다.

```
$ sudo apt-get install python3
```

그러면 비밀번호를 묻는데, 시스템 관리자 권한이 있는 계정의 비밀번호를 입력합니다. Mac OS라면 대개는 현재 로그인한 자신의 계정이 이미 시스템 관리자 권한을 갖고 있으므로 자신의 계정 비밀번호를 입력하면 됩니다.



#### (2) Windows

제가 Mac OS를 사용하다 보니 Windows 환경에서 Python을 설치하는 방법에 관해서는 설명하기 어렵습니다. 그래서 설치 관련 글을 소개하는 걸로 대신하겠습니다.

* [파이썬 윈도우 설치](http://goo.gl/v1ikhD)
* [Installing Python on Windows](http://docs.python-guide.org/en/latest/starting/install/win/)


### 2. 개발 환경 구축

여기서부터는 Mac OS, Linux 구분을 하지 않습니다. 다만, Windows는 제가 실제 설치와 작동을 확인하기 어려우니 설치 관련 자료를 별첨하겠습니다.

#### (1) pip 설치

pip는 Python에 사용되는 각종 패키지[^2]를 설치하거나 업그레이드, 삭제 등을 하는 관리 도구입니다. 이 도구를 설치하는 이유는 Django를 비롯하여 Python에 유용한 패키지를 쉽고 편하게 관리하기 위해서입니다.

설치는 간단합니다. pip를 설치해주는 스크립트인 [get-pip.py](https://bootstrap.pypa.io/get-pip.py) 파일을 받아서[^3] 실행하면 됩니다.

```
$ python3 get-pip.py
```

별문제 없이 설치가 끝날 겁니다. `OSError: [Errno 13] Permission denied:` 이런 식으로 오류가 발생하며 설치가 중단된다면 `sudo`를 이용하여 시스템 관리자 권한으로 설치하면 됩니다. 

이외 다른 방법으로 pip를 설치하실 거라면 [pip installation](https://pip.pypa.io/en/latest/installing.html) 문서를 참조하세요. pip는 Mac OS, Linux, 그리고 윈도우에서도 설치되고 작동합니다.

#### (2) virtualenv 설치

virtualenv는 가상으로 Python 환경을 만드는 도구입니다. Virtual Environment를 줄인 이름이겠지요?!

실제 환경인 주 시스템(운영체제)에 패키지를 설치하면 패키지가 바뀔 때마다 그 패키지를 사용하는 프로젝트 모두가 영향을 받습니다. 예를 들어, Django 1.6 버전을 기반으로 Pystagram을 개발하였는데, 얼마 후 1.6버전과 호환성이 떨어지는 Django 1.7버전이 출시됐다고 가정하겠습니다. 만약 Django 1.7 버전을 설치한다면 Django 1.6 버전에서 잘 작동하던 Pystagram에 문제가 발생할 지도 모릅니다.

Pystagram은 Django 1.6 버전을 기반으로, Hannal 프로젝트는 Django 1.5 버전 기반으로, Kay 프로젝트는 Django 1.7 버전 기반으로 환경을 분리하면 되는데, 한 시스템에서 이런 환경을 가상으로 분리하여 편하게 관리하도록 도와주는 도구가 바로 virtualenv입니다. Python 패키지 뿐만 아니라 사용할 Python 버전도 가상 환경으로 분리할 수 있습니다. 한 시스템에 여러 Python 버전을 설치하고, 버전에 따라 사용할 Python을 프로젝트 마다 지정할 수 있는 것이죠.

가상 Python 환경을 만드는 도구는 몇 가지가 있는데, 우리는 Python 3에 내장된 virtualenv(venv)를 사용합니다. `python3 -m venv` 명령어를 실행해서 다음과 같은 사용법 안냇말이 나오면 Python 3에 내장된 venv를 사용할 수 있습니다.

```
$ python3 -m venv                                               │~
usage: venv [-h] [--system-site-packages] [--symlinks | --copies] [--clear]                                    │~
            [--upgrade] [--without-pip] [--prompt PROMPT]                                                      │~
            ENV_DIR [ENV_DIR ...]                                                                              │~
venv: error: the following arguments are required: ENV_DIR
```

만약 `No module named venv` 문구가 있다면 따로 설치해야 합니다. Mac OS에선 `brew install pyenv`를 실행하면 되고, Debian 계열 Linux에선 `sudo apt-get install python3-venv`를 실행하면 됩니다.


#### (3) SQLite 설치

데이터를 저장하는 데 필요한 데이터베이스는 [SQLite](http://www.sqlite.org/)를 사용할 겁니다. 실제 서비스를 운영하기엔 부족하지만, 공부하는 데엔 참 좋습니다. 

Mac OS나 Linux 계열 운영체제엔 SQLite 3가 보통은 이미 설치되어 있습니다. 

```sh
$ sqlite3
SQLite version 3.8.5 2014-06-04 14:06:34
Enter ".help" for usage hints.
Connected to a transient in-memory database.
Use ".open FILENAME" to reopen on a persistent database.
sqlite>
```

그래서 SQLite 3 설치를 신경써야 하는 경우는 흔하지 않으며, 여러분이 SQLite 3이 설치되어 있지 않아서 문제를 겪는 경우도 드뭅니다. 하지만 그 드문 일을 대비해 설치하는 과정을 다루겠습니다.

Mac OS에서는 Homebrew로 설치하거나 버전 업그레이드를 하면 편합니다.

```
$ brew update
$ brew upgrade
$ brew install sqlite3
$ brew link --force sqlite
```
하지만 이미 `sqlite3`이 설치되어 있다는 경고가 표시되며 설치되지 않을 겁니다. 이미 설치되어 있을 테니까요.

Debian 계열 Linux라면 apt-get으로 설치하면 됩니다.

```
$ apt-get install sqlite3 libsqlite3-dev
```

`libsqlite3-dev`는 SQLite 3 개발용 라이브러리입니다.

그런데 패키지 관리 도구로 설치가 안 되거나 직접 소스를 직접 컴파일하여 설치하고 싶다면 [sqlite.org의 Download](http://sqlite.org/download.html) 페이지에 가서  “Source Code” 영역에 있는 `sqlite-autoconf`로 이름이 시작하는 파일을 받습니다. 파일 확장자는 `tar.gz`입니다. 이 소스 파일을 다음과 같이 컴파일하여 설치하면 됩니다.

```
$ ./configure --prefix=/usr/local
$ make
$ sudo make install
```

SQLite 3가 `/usr/local/` 에 설치되고, `/usr/local/bin/`에 `sqlite3` 파일이 생깁니다. 하지만 패키지 관리 도구로 설치되지 않을 정도로 오래된 운영체제라면 **직접** 소스 컴파일해서 프로그램을 설치하느니 운영체제를 더 최신 버전으로 업그레이드 하는 게 나을 것 같습니다. :)

Python으로 SQLite 3를 사용하는 데 필요한 인터페이스는 대개 Mac OS나 Linux에 기본 내장되어 있습니다. Python 쉘에서 간단히 확인할 수 있습니다.

```
import sqlite3
```

##### Windows에 SQLite 3 설치

* [윈도우 sqlite 설치](http://goo.gl/buqvp3)


### 3. Django 설치

#### 가상 환경 생성과 진입

이제 드디어 Django를 설치할 차례입니다. virtualenv로 가상 환경을 만들고 그곳에 Django를 설치하겠습니다.

```
$ python3 -m venv env_pystagram
```

python3에 내장된 `venv`를 이용하여 `env_pystagram`라 이름 붙인 가상 환경을 만드는 명령입니다. 잠깐 멈칫하다가 `env_pystagram` 이름으로 된 디렉터리가 만들어집니다. 이 디렉터리가 바로 Python 가상 환경입니다. 

Python 가상 환경을 활성화하려면 Python 가상 환경 디렉터리 안에 있는 파일을 이용해야 합니다. Mac OS나 Linux 계열에선 다음 명령어를 실행합니다.

```
$ source env_pystagram/bin/activate
```

`env_pystagram` 디렉터리 안에 있는 `bin` 디렉터리에 `activate` 파일을 이용한 겁니다. Windows에선 다음 명령어를 실행합니다.

```
$ env_pystagram\Scripts\activate.bat
```

`env_pystagram` 디렉터리 안에 있는 `Scripts` 디렉터리의 `activate.bat` 파일을 실행한 겁니다.

가상 환경에 잘 진입하면 쉘 프롬프트에 가상 환경 이름이 덧붙어 표시됩니다.


가상 환경에서 빠져 나오려면 `deactivate` 명령어를 실행하세요. 그러면 쉘 프롬프트에 `(pystagram)` 부분이 표시되지 않는데, `pystagram`이라 이름 붙인 가상 환경에서 빠져 나와서 그렇습니다.

#### Django 설치

Django를 pystagram 가상 환경에 설치하기 위해 다시 가상 환경을 활성화하고 `pip` 명령으로 django 최신 버전을 설치합니다.

```
(env_pystagram)$ pip install django
```

주 시스템에 설치하지 않고 가상 환경에 설치하기 때문에 관리자 권한은 필요 없어서 `sudo`를 쓰지 않았습니다.

설치가 잘 됐는지 확인해 보겠습니다. 

```    
(pystagram)$ python -c "import django; print(django.__file__)" 
```

화면에 Django가 설치된 경로가 표시됩니다. 경로를 잘 보면 Python 가상 환경의 하위 경로에 Django가 설치됐다는 걸 알 수 있습니다.

### 4. 편리한 도구 설치

Pystagram을 만드는 데 필요한 Python 패키지는 그때그때 설치하겠습니다. 대신 Pystagram을 편하게 만드는 데 좋은 도구는 먼저 소개하겠습니다. 사용 여부는 여러분 마음입니다. :)

#### (1) Postman - REST Client

[Postman - REST Client](http://www.getpostman.com/)는 HTTP 기반으로 동작하는 API를 편리하게 호출하는 클라이언트(client)입니다. 서버에 기능을 구현한 후 동작 여부를 확인하려면 클라이언트에서 접근할 수 있는 인터페이스(interface)를 만들어야 합니다. 이 클라이언트쪽 인터페이스를 만드는 것 자체가 귀찮기도 하지만, 오류를 확인하고 추적하는 디버깅(debuging) 환경이 미비하여 문제를 파악하기도 불편합니다. Postman은 개발에 용이한 클라이언트 인터페이스를 제공하는 도구입니다.

#### (2) 편집기

프로그래밍은 해본 적이 없고 정말 Python 등으로 Hello world만 출력해본 분이라면 코딩에 필요한 편집기를 아직 결정하지 못하셨을 겁니다. Django로 프로그래밍을 하는 데 필요한 전용 편집기는 따로 없습니다. 편집기는 다양하며 취향에 맞는 걸 쓰면 됩니다. 

* Atom (무료)
* Notepad (일명 메모장. 유료 운영체제에 기본 내장)
* PyCharm (무료, 유료)
* Sublime Text (유료)
* Vim (무료)

저는 Sublime Text 2/3를 구입하여 사용하고 있으며, Vim도 많이 씁니다. 무료인데 꽤 잘 만들어졌고 빠르게 개선되고 있는 Atom도 좋습니다. 

Apple Pages나 Microsoft Word 같은 도구는 코딩에 적합하지 않습니다. :)


### 5. Python 

이 강좌는 Python 입문자를 대상으로 하지 않으므로 Python 문법 등에 대해서는 다루지 않습니다. 대신 Python 관련하여 몇 가지 규칙을 정하고, 여러분은 이 규칙을 따르시길 권합니다. 

#### (1) 들여쓰기

Python의 언어 문법은 코드를 들여 쓰는 규칙(indentation)을 엄격히 따릅니다. 같은 맥락에 있는 코드는 들여 쓰는 깊이가 같아야 합니다. 들여 쓰는 깊이는 탭(tab)으로 만드는데, 이 탭은 한 자리 공백(space) 문자로 표현하는 소프트 탭(soft tab) 방식과 자판에 있는 탭 키로 표현하는 하드 탭(hard tab) 방식이 있습니다.

저는 [스페이스 네 칸을 탭 한 칸으로 표현하는 소프트탭 방식](http://legacy.python.org/dev/peps/pep-0008/#indentation)을 쓰겠습니다.

--------

이것으로 강좌 2편을 마칩니다. 분량이 적을 거라 예상했는데, 왜 설치하고 왜 그렇게 설치하는지 설명하려다 보니 예상보다 길게 썼네요.

3편에서는 Django로 프로젝트를 만들고 데이터베이스에 데이터도 넣어볼 겁니다. 그럼 3편에서 만나요.

아참. 9월 초까지 블로그를 분리하고 이전하느라 접속이 잘 안 될 수 있습니다. :)

-----

[^1]: $ 기호는 보통은 쉘 프롬프트(shell prompt)를 뜻합니다. 즉, $가 있다면 터미널을 열어서 쉘에서 실행한다는 의미입니다.

[^2]: Python Package : 모듈은 함수나 변수, 클래스를 모아 놓은 파일이며, 패키지는 모듈을 묶어놓은 것이다. [Python Modules - Packages](https://docs.python.org/2/tutorial/modules.html#packages) 참조.

[^3]: wget을 사용하면 터미널에서 파일을 간편하게 받습니다. `wget 주소(URL)` 라고 입력하면 끝이지요. wget은 brew나 apt-get으로 설치하면 되며, [윈도우용 wget](http://gnuwin32.sourceforge.net/packages/wget.htm)도 있습니다.

[^4]: [bashrc와 bash_profile 차이](https://kldp.org/node/38265)

[^5]: 항상 맨 위는 아닙니다. 리눅스나 유닉스에서 Python 소스 코드 파일 자체를 실행하려는 경우 소스 파일 맨 위에는 이 파일을 실행하는 실행기를 명시하는데, 이런 경우 인코딩을 지정하는 내용을 두 번째 줄에 표기합니다.

