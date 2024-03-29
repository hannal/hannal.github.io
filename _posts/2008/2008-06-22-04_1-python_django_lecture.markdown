---
layout: post
status: publish
published: true
title: "미) 파이썬 기초 문법과 데이터 모델링 (4편 1/2) - django 강좌"
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
excerpt: "이번 글(“미”)은 2회로 나누어 진행한다. 1회는 파이썬 기초 문법이고 2회는 지난 글에서 기획한 자료(data)를 모델로 만드는
  모델링 작업을 한다. 이 글은 그 중 1회로 파이썬 기초 문법 몇 가지와 django의 모델링에 대한 기본 내용을 다룬다. 파이썬 언어 강좌나
  문법 강좌가 아니므로 자세히 다루진 않고, 코딩 할 때 주의 할 점 위주로 알아보도록 하자."
wordpress_id: 2314
wordpress_url: http://blog.hannal.com/?p=70
date: '2008-06-22 17:36:44 +0900'
date_gmt: '2008-06-22 08:36:44 +0900'
categories:
- "start_with_django_lectures"
tags:
- django
- python
- "파이썬"
- django입문강좌
permalink: "/2008/6/04_1-python_django_lecture/"
---
<p>이번 글(“미”)은 2회로 나누어 진행한다. 1회는 파이썬 기초 문법이고 2회는 지난 글에서 기획한 자료(data)를 모델로 만드는 모델링 작업을 한다. 이 글은 그 중 1회로 파이썬 기초 문법 몇 가지와 django의 모델링에 대한 기본 내용을 다룬다. 파이썬 언어 강좌나 문법 강좌가 아니므로 자세히 다루진 않고, 코딩 할 때 주의 할 점 위주로 알아보도록 하자.</p>
<h3>들여쓰기 문법</h3>
<p>우리가 읽고 쓰는 글에는 글 단락이라는 개념이 있다. 긴 글을 “내용에 따라” 나눌 때, 나뉜 이 짧은 이야기 토막을 단락이라고 한다. 프로그래밍에도 이런 단락이 존재한다.</p>
<p>저녁 밥을 차려 먹은 뒤 한날에게 기부를 할 것인가 결정한 뒤 책을 읽는 행동을 프로그래밍 코드처럼 보이게 짜보자.</p>
<blockquote>
<pre>식사준비(저녁);
먹기(밥, 반찬);
마시기(물);
<strong>if 현재_내_돈 &gt; 지출비 - 10000원
	주기(한날, 10000원);
else
	말하기(한날, “강좌 잘 보고 있어요”);</strong>
읽기(책);</pre>
</blockquote>
<p>위 코드를 보면 각 행동들이 차례대로 나열되어 있는데, 기부를 할 것인지 말 것인지 고민하는 부분은 <strong>조건문</strong>으로 되어 있다. 조건에 따라서 행동이 달라지는 것이다. <strong>이런 단락을 프로그래밍 쪽에선 블럭(block)이라고 부른다</strong>.</p>
<p>사람이야 줄바꾸기로 단락을 한 눈에 알아 볼 수 있지만 컴퓨터(프로그래밍 언어)는 그렇지 않으므로 별도 표식을 해줘야 한다. 이런 표식을 프로그래밍 언어 마다 다르게 하는데, c/c++ 나 php 등 언어는 { 와 } 로 한다.</p>
<blockquote>
<pre>if 현재_내_돈 &gt; 지출비 - 10000원 {
	주기(한날, 10000원);
} else {
	말하기(한날, '강좌 잘 보고 있어요');
}</pre>
</blockquote>
<p>위 코드는 c 나 php 등에서 흔히 볼 수 있는 모습인데, 파이썬은 조금 다르게 표현한다. { 나 } 같은 기호를 쓰지 않고 <strong>공백과 콜론( : ) 으로 블럭을 표현</strong>한다. 위 코드를 실제 파이썬 코드로 나타내면</p>
<blockquote>
<pre>if ME.money &gt; ME.outlay - 10000:
	Action.give(Hannal, 10000)
else:
	Action.speak(Hannal, '강좌 잘 보고 있어요')</pre>
</blockquote>
<p>이런 비슷한 모양새가 될 것이다(세미콜론( ; )이 없는 것에 주의하자).</p>
<p>이미 다른 프로그래밍을 접했거나 어깨 너머로 본 사람이라면 함수 선언문을 본 사람도 있을 것이다.</p>
<blockquote>
<pre>function say_hello() {
	speak('안녕, 여러분');
}</pre>
</blockquote>
<p>이것을 파이썬에서 나타낸다면</p>
<blockquote>
<pre>def say_hello():
	speak('안녕, 여러분')</pre>
</blockquote>
<p>이렇게 된다. 위에 코드는 function say_hello() { speak(“안녕, 여러분”); } 이렇게 한 줄로 표현해도 되지만 파이썬은 그렇게 하면 언어 문법 오류가 발생한다. 사람 마다 취향이 다르겠지만 난 기호를 최소한으로 쓰고 우리 눈에 익숙한 들여쓰기로 블럭을 표현하는 이런 파이썬 문법을 좋아한다.</p>
<p><strong>들여쓰기 방법</strong></p>
<ul>
<li>블럭을 알리는 줄 끝에는 콜론( : )을 붙이고</li>
<li>블럭 안 내용은 들여쓴다</li>
</ul>
<p>들여쓰는 공백은 크게 두 가지 방법으로 넣을 수 있다. <strong>자판에 있는 스페이스 바(Space bar) 글쇠로 공백 문자를 하나씩 넣거나, 탭(tab) 글쇠로 탭 공백 문자를 넣는 것</strong>이다. 무엇으로 하든 자기 마음이다. <strong>주의할 점은 한 소스 파일 안에서 각 각을 섞어 쓰면 안된다는 것</strong>이다. 예를 들면,</p>
<blockquote>
<pre>def say_hello():
    speak('이 줄은 공백 문자 네 개로')
    speak('이 줄은 탭 공백 문자 하나로')</pre>
</blockquote>
<p>이렇게 <strong>섞어 쓰면 들여쓰기 오류(indent error)</strong>가 뜬다. hannal1.py 에선 모든 들여쓰기를 공백 문자로, hannal2.py 에선 모든 들여쓰기를 탭 공백 문자로 하고 이 둘을 hannal3.py 에서 읽어 들여와서 쓰는 건 상관 없다. 하지만 각 파일 안에서 스페이스와 탭으로 들여쓰기를 섞어 쓰면 안된다. 이게 상당히 귀찮고 짜증스럽다. 일부 문서 편집기(editor)는 탭 입력하면 스페이스 공백 문자를 지정한 개수만큼 자동으로 바꿔 넣어주는 기능을 쓸 수 있지만 그렇지 않는 문서 편집기도 있다. 앞으로 우리가 들여쓰기 오류를 만난다면 십중팔구는 이 문제일 것이다.</p>
<p>두 번째 주의점은 들여쓰는 깊이이다. <strong>의미상 같은 깊이(?)에 있는 블럭은 반드시 들여쓰는 깊이도 같아야 한다</strong>.</p>
<blockquote>
<pre>def say_hello():
    speak('hi')
    speak('I am a boy.')
    speak('You must be a girl.')
    speak('How are you?')</pre>
</blockquote>
<p>이런 함수가 있다고 가정하자. say_hello 라는 함수 안에 있는 저 speak 문들은 분명 의미상 같은 깊이에 있으므로 모두 동일한 깊이만큼 들여쓰기가 되어 있다. 그런데</p>
<blockquote>
<pre>def say_hello():
    speak(hi')
    speak('I am a boy.')
    	<strong>speak('You must be a girl.')</strong>
    speak('How are you?')</pre>
</blockquote>
<p>이런 식으로 들여쓰기를 하면 들여쓰기 오류가 난다. 물론</p>
<blockquote>
<pre>def say_hello():
    speak('hi')
    speak('I am a boy.)
    if the_person is girl:
        <strong>speak('You must be a girl.')</strong>
    else:
        <strong>speak("I don’t like a boy. however...")</strong>
    speak('How do you do?')</pre>
</blockquote>
<p>위와 같이 if 절이 들어가는 경우는 그에 따른 들여쓰기를 해야 한다. 이를</p>
<blockquote>
<pre>def say_hello():
    speak('hi')
    speak('I am a boy.')
    if the_person is girl:
    <strong>speak('You must be a girl.')</strong>
    else:
    <strong>speak("I don’t like a boy. however...")</strong>
    speak('How do you do?')</pre>
</blockquote>
<p>이래도 들여쓰기 오류가 난다. 또한,</p>
<blockquote>
<pre>def say_hello():
    speak('hi')
    speak('I am a boy.')
    if the_person is girl:
        speak('You must be a girl.')
    else:
          <strong>speak("I don’t like a boy. however...")</strong>
    speak('How do you do?')</pre>
</blockquote>
<p>I don’t like a boy. however... 문에 강조를 한답시고 스페이스 공백 문자를 한 두 개 더 넣어도 오류가 난다. <strong>You must be a girl. 문과 들여쓰기 깊이가 다르기 때문</strong>이다.</p>
<p>정리하면 대부분은 한 줄에 행동 하나씩 쓰고, 블럭은 : 과 들여쓰기로 표현할 수 있다(꼭 그런 건 아니지만 그렇게 생각해도 무방하다). 이 정도면 파이썬 기초 문법은 됐다. 어딘지 많이 부실해 보이지만 그건 기분 탓이다.</p>
<p>초보자들이 파이썬을 다룰 때, 문법상 분명 맞는데 도대체 오류가 왜 나는지 이유를 알 수 없는 경우는 대체로 저 문제가 많다. 글자로 보면 틀린 게 없는데 오류가 나니 환장 할 노릇이다. 게시판에 소스 코드를 붙여 넣으며 질문을 해도 원인을 찾기는 어렵다. 물론 <strong>IndentationError</strong> 예외 상황이 발생하는 오류가 떠서 들여쓰기 오류라는 걸 알 수 있지만, 우리글이 아니면 겁부터 덜컥 먹고 혼돈에 빠지는 사람은 저런 오류는 눈에 안보일 것이다.</p>
<p>이후 파이썬 문법은 중간 중간 간단히 알아보기로 하고, 이제 django 의 모델링을 들여보기에 앞서 django 웹 프로젝트를 개설해보자.</p>
<h3>djang-admin.py 과 manage.py</h3>
<p>django 는 우리가 웹 개발을 할 때 자주 되풀이 하는 행동들을 몇 가지 도구로 만들어 제공하고 있다. <strong>django-admin.py</strong> 과 <strong>manage.py</strong> 이 그런 도구들에 속한다.</p>
<p>django-admin.py 도구(파일)은 쓸 일이 별로 없다. djang-admin.py 에 있는 기능 대부분이 manage.py 에 있기 때문이다. 어쨌든 한 번 쯤은 쓸 일이 있는데 바로 프로젝트를 개설할 때이다.</p>
<p>django 는 개념상 다음과 같이 구성되어 있다.</p>
<ul>
<li>프로젝트
<ul>
<li>프로젝트 설정</li>
<li>어플리케이션
<ul>
<li>모델</li>
<li>뷰 (컨트롤러)</li>
</ul>
</li>
<li>템플릿 (뷰)</li>
<li>미디어</li>
</ul>
</li>
</ul>
<p>프로젝트를 개설한다는 것은 특별한 뜻이 있는 건 아니고, 디렉토리(폴더) 하나 만들고 django 실행에 필요한 설정 파일들을 구성하는 것이다. 즉 django 로 웹 프로젝트를 돌리는 데 필요한 초기 구성을 뜻한다. 방법은 간단하다.</p>
<blockquote><p>django-admin.py startproject hannal</p></blockquote>
<p>별 다른 안내가 없다면 잘 작동한 것이다. django-admin.py 이 없다고 할 수도 있다. 그럴 경우 django 를 설치한 곳에 보면 bin 이라는 디렉토리가 있는데 그 안에 있는 djang-admin.py 을 다음과 같이 하자.</p>
<ul>
<li>Windows 환경 : c:windowssystem32 에 복사</li>
<li>Linux/Mac OS X : 실행 경로가 잡힌 곳에 복사. 잘 모르겠다면 sudo ln -s django-admin.py /usr/bin 이라고 하자. 실제로 파일을 복사하진 않고 Symbolic Link 를 걸어둔 것이다(윈도우에 있는 “바로가기”와 아주 조금 비슷한 개념이다). 혹 실행이 안된다면 실행 권한을 주면 된다. chmod 700 django-admin.py 이라고 하자.</li>
</ul>
<p>프로젝트가 개설되었다면 저 명령어를 내린 곳에 보면 hannal 이라는 디렉토리가 생겼을 것이고, 그 안에는</p>
<ul>
<li>__init__.py</li>
<li>manage.py</li>
<li>settings.py</li>
<li>urls.py</li>
</ul>
<p>이렇게 파일 네 개가 있다. <strong>__init__.py 는 django 용이라기 보다는 python 에서 필요한 파일</strong>이다. 파이썬은 모듈을 읽어 올 때 초기화 작업을 필요로 하는데, 이 초기화 작업을 __init__.py 로 한다(모듈 뿐 아니라 클래스도 마찬가지). 만약 aaa 라는 디렉토리에 speak.py 을 두고 이 speak 을 모듈로 가져오고 싶다면 <strong>aaa 디렉토리에 반드시 __init__.py 가 있어야 한다</strong>. 초기화 할 것이 없다면 __init__.py 내용은 아무것도 써넣지 않아도 상관 없다.</p>
<p>settings.py 와 urls.py 는 설정 파일이다. 자세한 설명은 다음 글에서 다루기로 하고, 이름에서 알 수 있듯이 <strong>settings.py 은 프로젝트 설정 정보를 담고 있는 파일</strong>, <strong>urls.py 는 주소 체계 정보를 담고 있는 파일</strong>이라고만 알아두자.</p>
<p><strong>manage.py 는 프로젝트 관리에 필요한 각종 기능을 담고 있는 도구</strong>이다. django-admin.py 에 있는 기능 중 프로젝트 관리에 필요한 것만 따로 빼낸 것이라고 봐도 된다. 각 기능은 그때 그때 알아가기로 하자.</p>
<h4>어플리케이션 생성</h4>
<p>django-admin.py 으로 프로젝트를 생성했으니 각 프로젝트에 필요한 어플리케이션을 생성해야 한다. 예를 들면, hannal 이라는 웹 서비스에는 블로그와 방명록, 그리고 회원 관리로 구성되어 있다고 했을 때, 이 각 각을 어플리케이션이라 할 수 있다. 사실 이런 분류는 다분히 개개인의 주관을 따르므로 저것들을 하나의 어플리케이션에 넣어도 상관은 없다. 단지 나중에 코드나 개발 관리가 곤란해질 뿐이다.</p>
<ul>
<li>hannal 웹 서비스(프로젝트)
<ul>
<li>블로그 서비스(어플리케이션)</li>
<li>방명록 서비스(어플리케이션)</li>
</ul>
</li>
</ul>
<p>우리가 처음 만들 어플리케이션은 블로그이다(마지막으로 만들 어플리케이션도 블로그이다 ^^). 만들어보자. 우선 방금 만든 프로젝트 디렉토리 안(hannal)으로 들어가야 한다.</p>
<blockquote><p>manage.py startapp blog</p></blockquote>
<p>Linux 나 Mac OS X 같은 Unix 계열 운영체제에서 현재 디렉토리에 있는 파일을 실행하려면 파일 앞에 ./ 를 붙여줘야 한다. ./manage.py startapp blog 라고 말이다. 그래도 안된다면 역시 실행 권한 문제이다. chmod 700 manage.py 라고 하자.</p>
<p>이러면 프로젝트를 개설했을 때처럼 아무 말도 없을 것이다. 잘 된 것이다. 참 불친절한 장고씨다.</p>
<p><strong>잘 생성되고 나면 현재 디렉토리(hannal) 안에 blog 라는 디렉토리가 생겼을 것</strong>이다. 그 안에 보면 별 거 없다. 역시나 __init__.py 가 있고 views.py 와 models.py 가 있다. views.py 는 django식 뷰(mvc로는 컨트롤러) 역할을 맡고 있는 파일이고, models.py 는 모델 파일이다.</p>
<ul>
<li>/hannal
<ul>
<li>__init__.py</li>
<li>settings.py</li>
<li>urls.py</li>
<li>manage.py</li>
<li>/blog (정확한 구조는 /hannal/blog)
<ul>
<li>__init__.py</li>
<li>views.py</li>
<li>models.py</li>
</ul>
</li>
</ul>
</li>
</ul>
<p>참 간단한 구성이다. 왜 저리 파일이 많아야 하나 생각이 들 수도 있지만, 익숙해지면 정말 몇 개 안되는 파일로 참 깔끔하게 역할을 나눴다는 생각을 하게 된다.</p>
<h4>내장 웹 서버로 프로젝트가 잘 생성됐나 확인</h4>
<p>어플리케이션까지 안만들더라도 프로젝트만 잘 개설되면 django 에서 제공하는 내장 웹 서버로 개설 여부를 확인할 수 있다.</p>
<blockquote><p>manage.py runserver<br />
(Mac OS X 나 Linux 라면 ./manage.py runserver)</p></blockquote>
<p>이러면</p>
<blockquote><p>Validating models...<br />
0 errors found</p>
<p>Django version 0.97-pre-SVN-7719, using settings '<strong>hannal</strong>.settings'<br />
Development server is running at <strong>http://127.0.0.1:8000/</strong><br />
Quit the server with CONTROL-C.</p></blockquote>
<p>이와 같이 안내말이 나오면서 웹 서버가 작동한다. hannal 이라고 한 부분은 우리가 아까 개설한 프로젝트가 hannal 이기 때문이다. (hannal 디렉토리에 있는 settings.py 를 가져오는 것이다)</p>
<p>http://127.0.0.1:8000 은 내장 웹 서버가 사용하는 주소이다. 앞에 127.0.0.1 은 IP나 도메인이겠고 : 뒤에 있는 숫자는 포트(네트워크 포트) 번호이다. 이 주소를 바꿀 수도 있는데, runserver 뒤에 사용할 주소를 쓰면 된다.</p>
<blockquote><p>manage.py runserver <strong>localhost:8000</strong></p></blockquote>
<p>이제 웹 브라우저를 연 뒤 주소창에  <strong>http://localhost:8000/ </strong> 이라고 쳐보자. 잘 작동하고 있다며 django 로 만든 첫 화면을 띄운 걸 축하하고 있다.</p>
<p><img class="aligncenter size-full wp-image-71" src="http://blog.hannal.com/assets/uploads/2008/06/django_is_working.png" alt="" width="493" height="305" /></p>
<p><strong>이 내장 웹 서버는 굉장히 편하고 쓸모가 많다</strong>. 개발 단계에서 작동하는 걸 눈으로 확인을 해야 하는데, 웹 서버 환경을 따로 갖출 필요 없이 이 내장 웹 서버를 쓰면 된다. 여러 기능 필요 없이 django 를 위한 환경이므로 가볍고 빠르다.</p>
<p>웹 서버에 python 으로 만든 cgi 를 돌릴 때 보통 <a href="http://www.terms.co.kr/FastCGI.htm">fast-cgi</a> 나 wsgi 방식을 쓴다. 자세한 건 생략하고, 이들은 cgi 를 실행할 때 그 cgi 를 웹 서버 메모리에 올려 놓는다.</p>
<p>근데 중간에 cgi 내용을 바꿀 경우가 있는데 이미 cgi 가 메모리에 올라가 있으면 하드 디스크에 있는 cgi 파일을 실행하지 않는다. 이런 경우 웹 서버를 재실행하거나 cgi 를 메모리에 올려놔 들고 있는 녀석(processor)을 종료해야 하는데, 개발을 하고 소스 파일을 저장할 때 마다 이 짓을 할 순 없다. 꼭 cgi 뿐만 아니라 다국어 작업을 할 때 언어 번역 파일을 갱신할 때도 마찬가지이다. 아주 귀찮지 않겠는가.</p>
<p><strong>django 내장 웹 서버는 이용자가 소스 파일을 고칠 때 마다 자동으로 스스로를 재실행해서 바뀐 소스 파일을 다시 읽어 들인다</strong>. 내장 웹 서버를 띄워 놓은 상태에서 settings.py 를 문서 편집기로 연 뒤 맨 아래에 빈 줄 하나 넣고 저장을 해보자. 그러면 내장 웹 서버가 재실행 되는 모습을 볼 수 있다. (언제나 자동으로 재실행 되는 건 아니고, django 가 감시하는 확장자가 py 인 파일들이 바뀔 때만 재실행 된다고 보면 된다)</p>
<p>django 내장 웹 서버는 이런 점에서 아주 편하다. 의욕을 가득 품고 부지런하게 미리 자신의 컴퓨터에 웹 서버(Apache Web server 같은 것)를 설치 해놨다면 좀 김이 새겠지만 우린 당분간 내장 웹 서버만 쓸 것이다.</p>
<hr />이번 1회 글은 여기서 마치려 한다. 이제야 말로 웹 브라우저에 글자 몇 개 뿌려보나 기대를 했다면 좀 미안하다. 되도록 강좌 첫 글에서 “안녕, 여러분”을 출력해서 여러 분들 입에서 “오오올~” 소리 나오게 하고 싶었지만, 그러면 나중에 고생하게 되더라. 역시나 기초가 튼튼해야 한다.</p>
<p>하지만 지루한 것도 사실인지라 최대한 간단하게 다루려 애썼다. 그래도 이번 글에서는 자판 몇 번 두드리며 눈에 보이는 행동도 하지 않았나. ^^ 다음 주에 올라 올 강좌부터는 정말 django 를 제대로 쓰기 시작할 것이다. 그동안 정 심심하다면 파이썬 강좌를 보며 파이썬 공부를 간단히 하면 좋겠다. 그래도 정 심심하다면 django-admin.py startproject 를 1초 안에 치기, manage.py startapp 를 0.7초 안에 치는 연습이라도 하거나. :)</p>
<p>파이썬 강좌</p>
<ul>
<li><a href="http://turing.cafe24.com/">왕초보를 위한 파이썬 프로그래밍 강좌</a></li>
<li><a href="http://www.google.co.kr/search?aq=f&amp;complete=1&amp;hl=ko&amp;newwindow=1&amp;q=python+%EA%B0%95%EC%A2%8C&amp;btnG=%EA%B2%80%EC%83%89&amp;lr=lang_ko">구글에서 찾아보기</a></li>
</ul>
