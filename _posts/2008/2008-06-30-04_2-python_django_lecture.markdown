---
layout: post
status: publish
published: true
title: "미) 파이썬 기초 문법과 데이터 모델링 (4편 2/2) - django 강좌"
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
excerpt: "이번엔 예전에 한 데이터 모델링을 django 모델에 적용하고 이와 관련된 django 사용법을 알아보려 한다. 드디어 이번 회부터
  django 를 본격 다루는 셈인데, 실제로 화면에 나타나게 하는 부분 보다는 기반 작업이라 할 수 있다."
wordpress_id: 2315
wordpress_url: http://blog.hannal.com/?p=72
date: '2008-06-30 02:23:32 +0900'
date_gmt: '2008-06-29 17:23:32 +0900'
categories:
- "start_with_django_lectures"
tags:
- django
- python
- "파이썬"
- "모델링"
- django입문강좌
permalink: "/2008/6/04_2-python_django_lecture/"
---
<p>이번엔 예전에 한 데이터 모델링을 django 모델에 적용하고 이와 관련된 django 사용법을 알아보려 한다. 드디어 이번 회부터 django 를 본격 다루는 셈인데, 실제로 화면에 나타나게 하는 부분 보다는 기반 작업이라 할 수 있다.</p>
<p>* 내용 고침 알림 : 2008년 7월 2일에 소스 파일을 새로 올렸으며, max_length 라는 내용을 maxlength 로 바꿨음. 하지만, <strong>django 0.97이상에서는 max_length 라고 써야 합니다</strong>.</p>
<h3>django 모델과 모델링</h3>
<p><a href="http://blog.hannal.com/04_1-python_django_lecture/">미) 편 중 1회</a>에서 우리는 django 에서 프로젝트와 어플리케이션 개념이 어떤 것인지 알아 보았다. 프로젝트는 서비스처럼 전체 포괄 개념이고 어플리케이션은 프로젝트를 구성하는 부품이나 부위라고 할 수 있다.</p>
<p>django 에서 모델은 어플리케이션 단위로 존재하는데, 이 모델을 실제 DBMS (DataBase Management System)에 연결하여 관리하는 체계 단위는 프로젝트이다. 지난 글에서 우리는 hannal 이라는 이름을 가진 프로젝트를 만들었는데 mysql 이든 postgresql 이든 DBMS에 접근을 하는 단위 혹은 물리 개념은 바로 프로젝트이다. 그래서 프로젝트를 설정하는 settings.py 안에 DBMS 접근 정보를 써넣는다. 그리고 프로젝트에 속하는 각 어플리케이션과 어플리케이션의 모델은 프로젝트가 접근하는 DBMS에서 정보를 다룬다.</p>
<p><img class="aligncenter size-full wp-image-73" title="django에서 프로젝트와 데이터베이스 연결 관계" src="http://blog.hannal.com/assets/uploads/2008/06/django_project_and_database.gif" alt="" width="500" height="276" /></p>
<p>이쪽 경험을 해보지 않은 이라면 앞선 설명이 다소 막연할 것이다. 쉽게 말해서 django 로 만든 프로젝트는 한 번에 DBMS 하나에만 접근할 수 있으며, 어플리케이션 역시 이 DB 안에서만 데이터를 다룬다고 할 수 있다. 프로젝트가 접근하는 DB 안에 어플리케이션 DB(테이블)이 여러 개 들어가게 되는 것이다. 매우 덩치 큰 서비스를 만들 거라서 각 어플리케이션(모델) 마다 접근할 DBMS를 따로 연결을 하고 싶은 경우가 있을텐데, 안타깝게도 django는 아직(2008년 6월 29일) 각 프로젝트가 한 번에 DBMS 하나에만 연결할 수 있다. <a href="http://code.djangoproject.com/ticket/1142">비공식 기능으로 프로젝트 하나가 한 번에 여러 DBMS에 연결</a>하도록 할 수도 있지만 정식으로 반영되진 않았다. django 모델 기능을 통하지 않아도 되긴 하지만, 그러면 django 에서 제공하는 편한 기능을 쓸 수 없으니 굳이 편한 django 를 쓸 이유가 없어 진다.</p>
<h3>models.py 안쪽 보기</h3>
<p>django 모델은 어플리케이션 단위로 존재한다고 했는데, 파일로는 <strong>models.py</strong> 이다. 지난 글을 통해 우리는 hannal 프로젝트를 만들었고 ( django-admin.py startproject hannal ), 그 안에 blog 어플리케이션을 만들었다 ( manage.py startapp blog ). blog 어플리케이션, 그러니까 blog 디렉토리(폴더) 안에 보면 __init__.py , models.py , views.py 가 있는데 여기서 models.py 가 바로 모델 파일이다. 이 파일을 편집기로 열어보자. 아마 이런 내용이 있을 것이다.</p>
<blockquote><p><code>from django.db import models</code></p>
<p><code> </code><code># Create your models here.</code></p></blockquote>
<p>참 썰렁하다. 맨 윗줄에 있는 from django.db import models 는 django 에 있는 db 모듈에서(from django.db) models 라는 모듈을 가져오라(import)는 뜻이다. 이건 실제로 존재하는 개체인데 django 를 설치한 곳에 가보면 django 라는 디렉토리가 있고 그 안에 db 라는 디렉토리가 있으며, 그 안에 보면 models 라는 디렉토리가 있다. 바로 이 디렉토리를(정확히 말하자면 디렉토리 안에 있는 모든 파이썬 파일) 가져오는 것이다.</p>
<p>참고로 다른 식으로 가져올 수도 있다.</p>
<blockquote><p>from django import db</p></blockquote>
<p>이렇게 하면 django 디렉토리에 있는 db 디렉토리를 가져오는 것인데, db 디렉토리에 models 가 들어가 있으므로 models 도 함께 가져온 것이다. 이렇게 한 경우 파이썬 코드 안에서는 db.models 로 접근하면 된다. from django.db import models 라고 가져왔다면 그냥 models 라고 바로 접근하면 된다. models 디렉토리(모듈) 전체가 아니라 그 안에 있는 query.py 파일만 가져오고 싶다면 예상하다시피</p>
<blockquote><p>from django.db.models import query</p></blockquote>
<p>라고 하면 된다. 이는 from django.db import models 라고 가져온 경우 파이썬 코드 안에서 models.query 라고 접근을 할 수 있다는 말이기도 하다. 무슨 말인지 이해가 안간다면 그냥 그러려니 하고 넘어가자. ^^ 필자 역시 처음부터 그런 것 하나 하나 신경쓰며 공부하지 않았지만 지금은 이렇게 알고 있다.</p>
<p>그 아래에 보면</p>
<blockquote><p># Create your models here.</p></blockquote>
<p>이런 줄이 있는데 이는 <strong>주석</strong>이다. 주석(comment)은 컴퓨터가 해석해서 실행하지 않는 부분이다. c/c++/php/javascript 같은 언어에서는 /* 과 */ 으로 주석 처리할 부분을 감싸거나 줄 단위로 주석 처리할 경우 그 문장 맨 앞에 // 를 붙이는데, 파이썬엔 <strong>한 줄 주석 처리를 #</strong> 으로 하고, /* 와 */ 처럼 <strong>여러 줄을 주석 처리는 <code>"""</code>과 <code>"""</code> (큰따옴표 세 개)</strong>로 한다.</p>
<blockquote><p><code>"""<br />
이렇게 하면 주석으로 처리 됩니다.<br />
이히힉!<br />
"""</code></p></blockquote>
<p>아무튼 저 문장은 주석이므로 실제 실행할 때 처리되지 않는다. 주석문을 보면 “여기에 당신의 모델을 만드시오 (Create your models here.)”라고 나와있는데 from django.db import models 라는 줄 아래에 모델을 만들면 된다는 말이다. 왜냐하면 모델을 만들 때 바로 저 models 라는 모듈을 쓰기 때문이다.</p>
<p>이제 실제로 모델을 하나씩 만들 차례가 됐다.</p>
<h3>django 모델 꼴, 그리고 클래스</h3>
<p>django 의 모델은 이렇게 생겼다.</p>
<blockquote>
<pre>class 모델이름(models.Model):
    주저리주저리</pre>
</blockquote>
<p>그렇다. 모델이라고 해서 특별히 정해진 꼴이 있는 것이 아니라 클래스로 정의하면 되는데, 위에서 가져온(import models) models 를 넘겨 받아 이걸 이용해서 각 모델 구성 요소를 정의하는 것이다. 물론 이 모델 구성 요소는 클래스의 프로퍼티이다. 클래스니 프로퍼티니 하는 낱말이 익숙치 않다면 신경 쓰지 않아도 되지만 개념은 알아두면 좋으니 간단히 알아보려 한다.</p>
<p>클래스는 크게 메소드(method)와 프로퍼티(property)가 있다(깊게 들어가진 말자). 클래스를 자동차라고 하면, 메소드는 자동차의 행위라고 할 수 있다. 앞으로 나간다, 뒤로 간다, 왼쪽으로 꺾는다, 경고음을 울린다, 문을 연다 등. 프로퍼티는 엔진, 자동차 재질, 유리, 색깔 같은 요소를 뜻한다. 보통 프로그래밍 코드로는 이렇게 생겼다.</p>
<ul>
<li>메소드 : Car.move()</li>
<li>프로퍼티 : <code>Car.color = 'green'</code></li>
</ul>
<p>“블로그 글”이라는 객체(클래스)가 있다. 블로그 글은 제목이나 본문 등이 있는데 이들은 블로그 글이 어떤 행위를 하는 걸 정의하고 있지 않다. 그냥 블로그 글을 구성하는 “요소”이다. 그래서 django 에서는 글 제목이나 본문은 블로그 글이라는 클래스의 프로퍼티로 정의한다. 또, 블로그 글은 저장을 하거나 끄집어 내는 등 행위를 하거나 필요로 할 수 있다. 이런 행위들을 메소드로 정의하는 것이다.</p>
<p>django 모델은 models 라는(import models 로 가져온) 녀석을 통해 모델 요소들을 django 모델에 알맞게 정의한다. 즉 클래스 프로퍼티를 django 에 있는 models 기능으로 정의하는 것이다. 이해하기 어려운가? 그렇다면 신경 쓰지 말고 넘어가자. 몰라도 당장 문제가 되진 않는다.</p>
<h3>모델링 하기</h3>
<h4>글 모델링 하기</h4>
<p>지난 번에 우리는 블로그 글 구조를 다음과 같이 잡았다.</p>
<ul>
<li>일련번호(id) : 숫자형 (6만개)</li>
<li>글 제목 (Title) : 문자형 (80글자)</li>
<li>글 본문 (Content) : 문자형 (길게)</li>
<li>작성일시 (created) : 날짜/시간형</li>
<li>글 갈래 (Category) : 숫자형 (6만개)</li>
<li>꼬리표 (Tags) : 문자형 (20글자)</li>
<li>댓글 수 (Comments) : 숫자형 (6만개)</li>
</ul>
<p>기억나지 않는다면 “<a href="http://blog.hannal.com/03-python_django_lecture/">은) 블로그 기획</a>”편을 보면 된다. 위 요소들은 클래스 관점으로 보면(?) 프로퍼티들이다. 원시꼴로 모델을 만들어 보자.</p>
<blockquote>
<pre><code>class Entries(models.Model):
    id = 0
    Title = ''
    Content = ''
    created = ''
    Category = 0
    Tags = ''
    Comments = 0
</code></pre>
</blockquote>
<p>0 이라고 한 요소는 숫자형, '' 는 숫자형이 아닌 자료형을 뜻한다. 이제 하나 하나 django 모델형으로 바꿔보자.</p>
<p>우선 <strong>일련번호(id)는 django 에서 자동으로 만들어서 관리</strong>한다. 새로운 자료가 들어오면 알아서 가장 마지막 id 값에 1을 더해서 id 를 만든다. 그래서 어차피 id 라는 이름을 가진 일련번호 요소를 쓸 것이라면 굳이 정의할 필요가 없다.</p>
<p>제목(Title)은 문자형이다. django 모델형에선 <strong>문자형을 CharField 와 TextField</strong> 라는 걸로 나타낼 수 있다. 차이점은 CharField 는 256 글자(혹은 byte) 이하에서만 쓸 수 있고, TextField 는 그보다 훨씬 많은(긴) 글자를 써넣을 수 있다는 점이다. 우리는 글 제목을 80글자로 하기로 했으니 CharField 를 쓰면 된다.</p>
<blockquote><p>Title = models.CharField()</p></blockquote>
<p>그런데 80글자 제한이 필요하다. 글 저장할 때마다 직접 글자 길이를 확인해도 되지만, 모델이 알아서 길이 검사도 하게끔 할 수 있다. 바로 <strong>maxlength</strong> 라는 옵션을 이용하는 것이다. SVN 으로 django 를 내려 받으면 django 가 0.97 이상인데 여기에선 <strong>max_length</strong>를 써야 한다.</p>
<blockquote><p>Title = models.CharField(maxlength=80)</p></blockquote>
<p>기왕이면 글 제목을 반드시 넣게 해보자.</p>
<blockquote><p>Title = models.CharField(maxlength=80, null=False)</p></blockquote>
<p><a href="http://technet.microsoft.com/ko-kr/library/ms191504.aspx">null</a> 은 설명하기 좀 모호하여 머리 속에 개념을 그려야 하는데, 여기서는 “값이 없다”라고 해두자. <strong>null = False 는 “값이 없음”을 허용하지 않겠다</strong>는 뜻이다. 반대로 <strong>값이 없어도 상관 없다면 null = True</strong> 라고 하면 된다.</p>
<p>글 본문(Content)도 문자형이다. 근데 본문은 제목에 비해 보다 많은 내용을 담는데, 방금 전에 Title 설명했을 때 이런 경우엔 TextField 로 정의하자고 언급했다. 그대로 해보자.</p>
<blockquote><p>Content = models.TextField(null=False)</p></blockquote>
<p>글 본문 역시 제목과 마찬가지로 반드시 내용이 있어야 하므로 null=False 옵션을 주었다.</p>
<p>이번엔 글 작성일시(created) 차례이다. 얘 역시 django 에서 알맞는 자료형을 제공하는데, 바로 <strong>DateTimeField</strong> 이다.</p>
<blockquote><p>created = models.DateTimeField()</p></blockquote>
<p>글이 작성될 때 글 작성일시를 써넣는 방법은 크게 두 가지이다. 별도로 현재 시각을 알아내서 다른 글 정보와 함께 써넣는 것이 첫 번째이고, 또 다른 방법은 따로 시각을 써넣지 않아도 글 작성이라는 “행위”가 일어날 때 자동으로 시각을 알아내서 써넣는 것이다. 당연히 두 번째 방법이 편하다. django 에서 이걸 하는 방법은 두 가지가 있는데 차이점은 아래와 같다.</p>
<ul>
<li>auto_now_add 옵션 : 데이터가 “처음” 만들어 질 때</li>
<li>auto_now 옵션 : 데이터가 저장 될 때</li>
</ul>
<p>간단히 말해서 <strong>auto_now_add 는 글이 처음 생성되어 저장할 때</strong> 작동하고, <strong>auto_now 는 글을 고칠 때(update)</strong> 작동한다. 우리는 글이 고쳐질 때에도 고쳐진 때를 저장해야 하니 auto_now 옵션도 필요하다.</p>
<blockquote><p>created = models.DateTimeField(auto_now_add=True, auto_now=True)</p></blockquote>
<p>정확한 의미로 created 는 “생성”이니 고쳐질 때 일시를 넣는 공간이 따로 필요하다. 이를테면 updated 같은 것 말이다.</p>
<blockquote><p>created = models.DateTimeField(auto_now_add=True)<br />
updated = models.DateTimeField(auto_now=True)</p></blockquote>
<p>이렇게 하면 처음 글을 쓴 일시는 created 와 updated 두 곳에 들어가지만, 글을 고치면 고친 일시는 updated 에만 들어간다. 다만 우리는 간단히 블로그를 만들 것이므로 created 하나에만 두 역할을 맡기려는 것이고, 그래서 두 옵션을 동시에 줬다.</p>
<p>이번엔 글 갈래(Category)이다. 이건 좀 더 복잡한 개념이 필요하다. 어려운 것도 아니고 아주 복잡한 건 아니지만 위에서 설명한 개념에서 새로운 개념이 들어간다.</p>
<p>지난 번에 글 갈래를 설명할 때, 각 글 하나는 글 갈래 하나를 갖는다고 했다. 이 말은 글 갈래 관점에서 보면 각 글 갈래는 여러 글을 가리키고 있다고 할 수 있다.</p>
<ul>
<li>낙서 갈래 : 1,2,3,6,7,10,23 글</li>
<li>소개 갈래 : 4,5,99,104 글</li>
<li>...</li>
</ul>
<h5>MANY TO ONE 관계형</h5>
<p><img class="alignleft size-full wp-image-74" style="float: left; margin-right: 10px;" title="Django에서 MANY-TO-ONE 관계도" src="http://blog.hannal.com/assets/uploads/2008/06/many_to_one_on_django.gif" alt="" width="250" height="261" /><strong>글 여러 개(many)가 글 갈래 하나(one)를 가리키는 것</strong>이다. 이것을 django 에서는 <a href="http://www.djangoproject.com/documentation/model-api/#many-to-one-relationships">MANY-TO-ONE 관계형</a>이라고 한다. 공식 누리집에 있는 문서에는 자동차와 생산 공장을 예로 들었는데, 블로그 글과 갈래에 비추어 보자면 생산 공장은 글 갈래, 자동차는 글이라고 할 수 있다.</p>
<p>이런 <strong>MANY-TO-ONE 관계형을 django 에서는 ForeignKey (외래키)로 정의</strong>한다.</p>
<blockquote><p>Category = models.ForeignKey(Categories)</p></blockquote>
<p>간단하다. 그런데 ForeignKey 안에 있는 Categories 는 어디서 튀어 나온 놈일까? 이놈 역시 글 모델(클래스)인 Entries 와 같은 글 갈래 모델(클래스)이다. 개념 흐름상 글 모델부터 정의를 했을 뿐, 실제로는 글 갈래 모델인 Categories 부터 정의해야 한다. 이건 글 모델 정의를 마친 후 하기로 하고, 일단 저렇게 놔두면 된다. 위 코드의 뜻은 “<strong>Category 라는 요소(프로퍼티)는 Categories 라는 클래스(모델)을 외래키로(MANY-TO-ONE) 가리킨다</strong>”이다.</p>
<h5 style="clear: left;">MANY TO MANY 관계형</h5>
<p><img class="alignright size-full wp-image-75" style="float: left; margin-right: 10px;" title="Django에서 MANY-TO-MANY 관계도" src="http://blog.hannal.com/assets/uploads/2008/06/many_to_many_on_django.gif" alt="" width="250" height="272" />이번엔 글 꼬리표(Tags)이다. 이것 역시 글 갈래와 비슷하다. 우리는 예전에 글 꼬리표를 구성할 때 <strong>글 여러 개(Many)가 꼬리표 여러 개(Many)와 연결</strong>된다고 했다. 이를 django 에서는 <a href="http://www.djangoproject.com/documentation/model-api/#many-to-many-relationships">MANY-TO-MANY</a> 라고 한다. 물론 MANY-TO-ONE 과 마찬가지로 정의하는 방법이 있는데 ManyToManyField 로 할 수 있다.</p>
<blockquote><p>Tags = models.ManyToManyField(TagModel)</p></blockquote>
<p>TagModel 은 글 갈래에서 봐서 예상할 수 있듯이 모델(클래스)이다. 물론 이것 역시 미리 정의되어야 하는데 일단은 이렇게 해두고 넘어가자.</p>
<p>django 공식 누리집에선 MANY-TO-MANY 관계형을 피자와 토핑으로 비유하고 있는데, 이 강좌에 비추어 보면 피자는 글이고 토핑은 꼬리표이다. 그 반대여도 상관 없다. 어차피 여러 개가 여러 개로(n 대 n) 연결되기 때문이다.</p>
<p>django 는 ManyToMany 관계형이 있을 경우, 이 <strong>ManyToMany를 관리하는 DB 테이블을 따로 만들어 관리</strong>한다. 글 모델은 글 테이블과 연결되고 꼬리표 모델은 꼬리표 테이블과 연결되는데, 각 테이블에는 서로가 연결되는 정보가 없다. 대신 각 글이 어떤 꼬리표들을 갖는지(혹은 그 반대로 각 꼬리표는 어떤 글을 갖는지) 알 수 있는 정보를 별도 테이블에서 다룬다. 1023번 글에 달린 꼬리표“들”을 가져오려 할 경우, 이 별도 테이블에서 1023글에 연결된 꼬리표 일련번호(id)들을 가져오고, 이 일련번호를 갖는 꼬리표 정보(꼬리표 이름)를 가져오는 방식이다. 물론 우리는 이 테이블을 신경 쓸 필요가 전혀 없다. django 가 알아서 다루기 때문이다. 우리는 그냥 사람에게 일을 시키듯이 1023번 글과 그에 달린 꼬리표도 가져오라고 시키면 된다.</p>
<p><img class="aligncenter size-full wp-image-76" title="Django의 MANY-TO-MANY 관계에서 연결 정보 테이블 위치" src="http://blog.hannal.com/assets/uploads/2008/06/many_to_many_table_on_djang.gif" alt="" width="400" height="358" /></p>
<p>물론 이런 모델 관계형을 쓰지 않아도 괜찮다. 모델 차원에서 이런 기능을 쓰지 않고 우리가 직접 해당 기능을 구현해도 된다. 다만 우리가 웹 프레임워크를 쓰는 이유와 목적이 보다 편해지기 위함임을 감안한다면, 그리고 실수 여지 없이 더 안전하게 처리하고 싶다면 이런 기능을 쓰는 것이 좋다.</p>
<p>이번엔 댓글 수(Comments)를 정의할 차례이다. 댓글 수는 숫자형인데, django 모델에서 쓰는 숫자형은</p>
<ul>
<li>큰 숫자형 (IntegerField)</li>
<li>작은 숫자형 (SmallIntegerField)</li>
</ul>
<p>이 있다. 각 각은 0과 양수만 표현할 수 있는 숫자형과 음수도 함께 표현할 수 있는 숫자형으로 나뉜다. 위 두 숫자형은 음수도 표현할 수 있는 숫자형이며, <strong>음수는 다루고 싶지 않다면 각 숫자형 이름 앞에 Positive 를 붙여서 PositiveIntegerField 나 PositiveSmallIntegerField</strong> 라고 쓰면 된다.</p>
<p>“어차피 사람 일이라는 것이 나중에 어떻게 될 지 모르니 만약을 대비해서 음수도 표현할 수 있는 숫자형으로만 하면 편하지 않나?” 생각을 할 수도 있는데, 이는 어디까지나 기획자 관점일 뿐이고(좋은 마음가짐이라고 본다^^) 개발자 관점에서는 표현 범위가 달라지므로 신경을 써야 한다.</p>
<p>이 역시 간단히 설명하자면, 컴퓨터 메모리 관리에 따라 양수든 음수든 각 숫자형은 다룰 수 있는 값 범위가 있다. 32비트 체제에서 가장 작은 숫자형(c 언어에선 int 형) 값 범위는 256인데, 이는 -128 ~ 127 까지 표현할 수 있다(0도 포함되므로 총 256개이다). 그런데 이런 음수 구분을 하는 데에도 자리(메모리)를 차지하는데 이 음수 구분을 없애버리면 이 자리만큼 숫자를 더 표현할 수 있으며, 이 범위가 0~255이다. 컴퓨터는 2진수로 자료를 다루는데 1바이트는 8비트이며, 음수를 가질 수 있는 숫자형은 음수 구분을 위한 자리 하나를 가지므로 7비트를 숫자 표현하는 데 쓰므로 2의 7승인 128(음수쪽으로 -1 ~ -128, 양수쪽으로 0 ~ 127)씩을 나타낼 수 있는 것이다. 근데 음수 구분하는 자리 역시 숫자 표현하는 데에 쓰면 2의 8승이 되므로 256이 되어 0~255까지 표현할 수 있는 것이다.</p>
<p>어쨌든 댓글 수는 음수가 될 일이 없다. 그리고 수 억개 댓글이 달릴 일도 사실상 없다. 그러니 <strong>양수만 수 만개를 표현할 수 있는 PositiveSmallInteger</strong> 를 쓰자. 이는 mysql 에서는 0 ~ 65535 까지 표현할 수 있다. (mysql 에는 이것보다 더 작은 숫자형인 tinyint 가 있지만 django 에선 제공하지 않는 숫자형이다)</p>
<blockquote><p>Comments = models.PositiveSmallInteger(default=0, null=True)</p></blockquote>
<p>null에 대한 설명은 위에서 이미 했는데, <strong>default</strong> 라는 놈이 대뜸 나왔다. 얘는 값이 저장될 때 따로 넣는 값이 없을 때 자동으로 넣을 기본 값을 뜻한다. default=0 은 따로 이 요소에 값을 지정하지 않을 경우 기본값(default)으로 0을 넣겠다는 뜻이다.</p>
<p>이제 블로그 글 모델 구조를 다 잡았다.</p>
<blockquote>
<pre>class Entries(models.Model):
    Title = models.CharField(maxlength=80, null=False)
    Content = models.TextField(null=False)
    created = models.DateTimeField(auto_now_add=True, auto_now=True)
    Category = models.ForeignKey(Categories)
    Tags = models.ManyToManyField(TagModel)
    Comments = models.PositiveSmallIntegerField(default=0, null=True)</pre>
</blockquote>
<h4>글 갈래 모델링 하기</h4>
<p>이번엔 글 갈래 모델을 만들 차례이다. 우리는 글 모델에서 글 갈래(Category) 정보를 글 갈래 모델에 연결해서 쓰고 있다.</p>
<blockquote><p>Category = models.ForeignKey(Categories)</p></blockquote>
<p>이렇게 말이다. 바로 이 Categories 를 만들텐데, 이 <strong>Categories 는 글 모델인 Entries 보다 먼저 선언이 되어야</strong> 한다. 파이썬은 소스 코드를 차례대로 주욱 읽기 때문에 Entries 모델(클래스)를 주욱 읽다가 models.ForeignKey(Categories) 줄을 만나게 된다. 그런데 Categories 가 Entries 보다 밑에 있으면 당연히 이 줄에서 Categories 라는 개체가 없다며 오류가 나게 된다. 그래서 Categories 모델 정의를 Entries 위에 해야 한다. 그럼 어떻게 위에(앞) 둘까? Categories = be_in_front_of(Entries) 라는 명령어라도 있을까? 그럴 필요 없이 Categories 정의하는 소스 코드를 Entries 윗 부분에 두면 그만이다. ^^</p>
<blockquote><p>class Categories(models.Model):</p></blockquote>
<p>눈에 익은 틀이다. Categories 라는 클래스를 선언하는 것이다. 글 갈래는 일련번호와 갈래 이름만 있으면 되는데(이 역시 우리는 이미 “은)” 단계에서 블로그 기획하며 그리 하기로 했다), 일련번호는 django 에서 알아서 다루니 갈래 이름만 정의하면 된다. 문자형에 40글자로 하기로 했으니</p>
<blockquote><p>Title = models.CharField(maxlength=40, null=False)</p></blockquote>
<p>라고 하면 된다. 물론 글 갈래 이름은 반드시 필요하니 null 은 False 라고 했다. 이걸로 끝이다.</p>
<blockquote>
<pre>class Categories(models.Model):
    Title = models.CharField(maxlength=40, null=False)</pre>
</blockquote>
<p>정말 간단하다. 이 줄을 class Entries(models.Model): 이라고 써있는 줄 위에 두면 된다. 아마 소스 코드는 이런 모양이 될 것이다.</p>
<blockquote>
<pre>from django.db import models

class Categories(models.Model):
    Title = models.CharField(maxlength=40, null=False)

class Entries(models.Model):
    뭐시라 뭐시라</pre>
</blockquote>
<h4>글 꼬리표 모델링 하기</h4>
<p>이번엔 꼬리표(TagModel)를 만들 차례이다. 이것 역시 글 갈래(Categories)와 같은 개념이므로 <strong>Entries 위에 있어야</strong> 하며, 꼬리표 이름만 있으면 된다. 그러므로</p>
<blockquote>
<pre>class TagModel(models.Model):
    Title = models.CharField(maxlength=20, null=False)</pre>
</blockquote>
<p>이렇게 될 것이다.</p>
<h4>댓글 모델링 하기</h4>
<p>마지막으로 댓글 모델(Comments)을 만들자. 댓글 구조는</p>
<ul>
<li>일련번호(id)</li>
<li>글쓴이(Name) : 문자형 (20글자)</li>
<li>암호(Password) : 문자형 (32글자)</li>
<li>본문(Content) : 문자형 (2000글자)</li>
<li>작성일시(created) : 날짜/시간형</li>
<li>가리키는 글 정보(Entry) : 숫자형 (그 글의 일련번호)</li>
</ul>
<p>이렇게 생겼었다. 일련번호는 생략하면 되고, 글쓴이, 암호, 본문, 작성일시는 이미 위에서 만들어 봤으니 한 번에 만들어 보자.</p>
<blockquote>
<pre>class Comments(models.Model):
    Name = models.CharField(maxlength=20, null=False)
    Password = models.CharField(maxlength=32, null=False)
    Content = models.TextField(maxlength=2000, null=False)
    created = models.DateTimeField(auto_now_add=True, auto_now=True)</pre>
</blockquote>
<p>각 각이 뭘 뜻하는 지는 알 것이라 생각한다.</p>
<p>가리키는 글 정보(Entry)는 글 갈래와 글 관계를 생각하면 된다. <strong>댓글 여러 개는(Many) 각 글 하나씩(One)에</strong> 달려 있다. 그러므로</p>
<blockquote><p>Entry = models.ForeignKey(Entries)</p></blockquote>
<p>라고 하면 된다. 당연한 말이지만 위 줄은 Entries 라는 개체(클래스)가 선언된 뒤에 쓰여야 하므로 class Comments(models.Model): 이하 줄은 Entries 모델 아래에 있어야 한다.</p>
<h3>DB 테이블로 만들기</h3>
<p>django 는 모델을 DB 테이블로 만들어 주는 편리한 기능을 제공한다. 일일이 DB에 쿼리(query)문을 날려서 DB 작업을 하지 않고 모델에게 다소 추상화된 요청하여 DB 작업을 처리 하듯이, 테이블 생성 역시 하나 하나 테이블 스키마(schema) 디자인을 할 필요 없이 모델을 만든 뒤 django 에게 테이블 생성을 시킬 수 있다. 이 작업은 django 프로젝트에 어플리케이션을 추가하거나 내장 웹서버를 실행하는 데 도움을 준 manage.py 로 할 수 있다.</p>
<h4>프로젝트에 어플리케이션 등록</h4>
<p>위에서 모델은 어플리케이션 단위로 존재한다고 했다. 그래서 어떤 어플리케이션에 있는 모델을 django 프로젝트 안에서 활성화하려면 어플리케이션을 프로젝트에 등록해야 한다. 이렇게 활성화하면</p>
<ul>
<li>활성화한 어플리케이션의 DB 테이블 스키마문(schema statement)을 만들어 주는 기능을 쓸 수 있고,</li>
<li>각 모델 개체에서 DB 기능들(API)을 쓸 수 있게</li>
</ul>
<p>된다.</p>
<p>manage.py startapp 명령어로 어플리케이션을 생성했다. 그런데 이는 어디까지나 생성이며 프로젝트에 등록하는 일은 따로 있다. <strong>등록은 프로젝트 설정 파일인 settings.py 에 써넣으면</strong> 된다.</p>
<p><strong>settings.py</strong> 파일을 편집기로 연 뒤 맨 아래로 내려가보자. 그러면</p>
<blockquote>
<pre><code>INSTALLED_APPS = (
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.sites',
)</code></pre>
</blockquote>
<p>이렇게 생긴 부분이 보인다. 바로 여기에 등록을 하는 것이다. 우리는 hannal 이라는 프로젝트에 있는 blog 라는 어플리케이션을 등록할(activate) 것이므로</p>
<blockquote><p><code>'hannal.blog',</code></p></blockquote>
<p>이 줄을 추가하면 된다. 그러면 아래와 같이 된다.</p>
<blockquote>
<pre><code>INSTALLED_APPS = (
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.sites',
    'hannal.blog',
)</code></pre>
</blockquote>
<p><strong>줄 마지막에 쉼표가 빠지지 않고 있는 점에 주의하자</strong>(이유는 이 글 끝에 나온다). 이렇게 하면 hannal 프로젝트에 있는 blog 어플리케이션을 등록하여 활성화한 것이다.</p>
<p>이번엔 DB 설정을 해야 한다. settings.py 맨 위로 올라가면</p>
<blockquote><p><code>DATABASE_ENGINE = ''<br />
DATABASE_NAME = ''<br />
DATABASE_USER = ''<br />
DATABASE_PASSWORD = ''<br />
DATABASE_HOST = ''<br />
DATABASE_PORT = ''</code></p></blockquote>
<p>이런 부분이 있다. 여기에 DB 연결 정보를 써넣는다.</p>
<p>우선 DATABASE_ENGINE 은 연결할 DBMS 종류이다. django 는 mysql, sqlite, postgresql, oracle 등 다양한 DBMS를 지원하는데 우리는 mysql을 쓸 것이므로 <strong>mysql</strong> 이라고 쓰면 된다.</p>
<p>DATABASE_NAME과 USER, PASSWORD, HOST 는 DB에 연결하는 계정 정보인데 여러분들이 생성하거나 가지고 있는 연결 정보를 쓰면 된다. DATABASE_HOST 와 PORT는 따로 값을 넣지 않고 빈값으로 설정하면 이미 설정된 기본값(HOST 는 localhost, PORT는 3306)으로 자동 설정된다.</p>
<p>아래는 필자가 쓰는 DB 연결 정보이다.</p>
<blockquote><p><code>DATABASE_ENGINE = '<strong>mysql</strong>'<br />
DATABASE_NAME = '<strong>testing</strong>'<br />
DATABASE_USER = '<strong>hannal</strong>'<br />
DATABASE_PASSWORD = '<strong>1023</strong>'<br />
DATABASE_HOST = '<strong>localhost</strong>'<br />
DATABASE_PORT = ''</code></p></blockquote>
<p>NAME, USER, PASSWORD 는 사람마다 다를테니 자신의 환경에 맞게 바꾸면 된다.</p>
<h4>테이블 SQL문 확인해보기</h4>
<p>설정을 마치고 나면 manage.py 를 이용해서 모델을 DB 테이블로 만들 수 있게 된다. 바로 만들기 전에 모델을 제대로 만들었는지 여부를 확인할 수 있다. 여러 방법이 있겠지만 나는 다음과 같은 방법으로 확인하려 한다.</p>
<blockquote><p>manage.py sql blog</p></blockquote>
<p>manage.py 에는 여러 기능이 있는데 그 중 sql 명령어를 쓴 것이다. 이는 지정한 어플리케이션(위 명령어 줄에선 blog 라고 썼다)에 있는 모델을 테이블 sql 문으로 만드는 것이다. 문제가 없으면 BEGIN; 으로 시작해서 테이블 스키마 sql문에 주욱 나오고 COMMIT; 으로 끝난다. 문제가 있으면 오류가 발생한다.</p>
<p>이 글에 나온대로 잘 해왔다면 오류 없이 sql문이 주루룩 나온다.</p>
<h4>DB에 테이블 생성해넣기</h4>
<p>오류가 없이 sql문이 잘 나온다면 이걸 DB에 반영하여 모델을 테이블로 만들 수 있다. 방법은 아주 간단하다. manage.py 에 있는 <strong>syncdb</strong> 라는 명령어를 쓰면 된다.</p>
<blockquote><p>manage.py syncdb</p></blockquote>
<p>이러면 Creating table 테이블이름  이런 줄이 주루룩 화면에 흘러 지나간다. 아마 처음 실행했다면</p>
<blockquote><p>You just installed Django's auth system, which means you don't have any superusers defined.<br />
Would you like to create one now? (yes/no):</p></blockquote>
<p>이렇게 django 가 정중하게 뭔가를 물어본다. 이는 <strong>django에서 제공하는 인증 체계를 설치했는데 아직 최고 권한 이용자(superuser)가 없으니 지금 하나 만들어 보라고 권하는 것</strong>이다. yes 라고 쳐서 하나 만들어두자. 어느 서비스처럼 개인 계정 정보가 누출되거나 외부에 팔아먹지 않으니 안심해도 된다. 어차피 내 컴퓨터에 깔리는 건데 뭐. 계정 생성 과정에서 나오는 영어는 단순하니 설명은 생략하고, 필자가 계정을 만든 과정만 다루겠다.</p>
<blockquote><p>You just installed Django's auth system, which means you don't have any superusers defined.<br />
Would you like to create one now? (yes/no): yes<br />
Username (Leave blank to use 'hannal'): hannal<br />
E-mail address: iam@hannal.net<br />
Password:<br />
Password (again):<br />
Superuser created successfully.</p></blockquote>
<p>이 계정은 나중에 쓸 일이 있다. 미리 알아봐야 머리만 아프니 언제 어떻게 쓰일지는 비밀로 하련다. :)</p>
<hr />
<h3>파이썬 상식</h3>
<h4>소스 파일 인코딩</h4>
<p>이번 글부터는 글을 마치기 전에 파이썬 문법이나 상식들을 한 두 개씩 다룰 예정이다. 이번 글에선 파이썬 소스 파일 안에서 한글 쓰는 방법을 알아보려 한다. 잉? 소스 파일에 한글 쓰는 방법이 따로 있다고?  그렇다. 만일 별도 처리를 해주지 않으면 주석이든 뭐든 소스 코드에 한글이 들어가면 오류가 발생할 것이다.</p>
<p>해결 방법은 간단하다. 파이썬 소스 파일 맨 윗 줄에</p>
<blockquote><p># -*- coding: utf-8 -*-</p></blockquote>
<p>이 줄을 써넣으면 된다. 그런 뒤 <strong>소스 파일을 저장할 때 UTF-8 로</strong> 해야 한다. 우리는 문자들을 utf-8로 다룰 것이므로 소스 파일도 utf-8 로 저장하고, 한글(정확히 말하면 한글이나 일본어 같은 2bytes 문자)가 들어간 파이썬 소스 파일 맨 윗 줄에 저 줄을 써넣으면 된다.</p>
<p>왜 그런지는 <a href="http://python.kr/viewtopic.php?t=22395">장혜식님께서 잘 설명</a>하셨다.</p>
<h4>튜플 자료형 주의점</h4>
<p>위 INSTALLED_APPS 라는 변수는 <strong>튜플(tuple)이라는 자료형</strong>으로 값을 담고 있는 것인데, <strong>튜플 자료형 구성자는 쉼표</strong>이다. 단지 어디서부터 어디까지를 묶어서 튜플로 표시하는지 <strong>명확하기 구분하기 위해 괄호를 쓴 것 뿐</strong>이다. 그렇기 때문에</p>
<ul>
<li>(1,) 은 튜플 자료형이지만</li>
<li>(1) 은 그냥 숫자(int) 자료형</li>
</ul>
<p>인 차이가 있다. 위 INSTALLED_APPS 도 hannal.blog 위에 있는 네 개를 지우고 hannal.blog 만 남긴답시고</p>
<blockquote><p>INSTALLED_APPS = ('hannal.blog')</p></blockquote>
<p>이렇게 하면 <strong>INSTALLED_APPS 변수는 튜플이 아니라 일반 문자형이 돼버린다</strong>.</p>
<blockquote><p>INSTALLED_APPS = ('hannal.blog'<strong>,</strong>)</p></blockquote>
<p>이렇게 쉼표를 찍어줘야 튜플이 된다. 이런 <strong>쉼표 실수를 막기 위해 튜플 자료형을 쓸 때는 언제나 구분자인 쉼표를 뒤에 넣는 습관을 들이는 것이 좋다</strong>.</p>
<hr />드디어 데이터 모델링까지 마쳤다. 실은 이 과정에서 몇 가지 더 알아볼 것도 있고 추가할 모델도(이용자 정보 같은 것) 있다. django 에서 제공하는 쓸만한 모델 관련 기능들은 차츰 알아갈 것이며, 이용자 모델은 나중에 숙제로 나갈 예정이다. 아직은 스스로 숙제를 풀지 못할 수 있어도 숙제로 나갈 때쯤이면 스스로 풀 수 있을 것이다.</p>
<p>다음 글에선 실제로 눈에 보이는 작업들을 할 예정인데 바로 컨트롤러(django 에선 view) 제작이다. 컨트롤러 과정은 총 3회에 걸쳐 다룰만큼 양이 많지만, 차근 차근 따라하다 보면 큰 어려움 없이 따라올 수 있을 것이다.</p>
<hr />이번 <a href="http://blog.hannal.com/assets/uploads/2008/07/python-django_mi_4-2_source.zip">“미) 파이썬 기초 문법과 데이터 모델링 (4편 2/2)” 글에서 작성한 소스 파일</a>을 내려 받고 싶다면 연결한 주소를 클릭하세요. python-django_mi_4-2_source.zip 파일을 받을 수 있습니다.</p>
