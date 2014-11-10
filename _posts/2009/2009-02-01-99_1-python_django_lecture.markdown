---
layout: post
status: publish
published: true
title: Django 1.0에서 Admin 기능 쓰기 - 부록편
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
excerpt: |-
  이 연재글은 “날로 먹는 Django 웹 프로그래밍 강좌” 을 보완하여 현재 정식으로 배포되고 있는 Django 1.0판에서 제대로 작동시키는 내용을 담습니다. 또, 본 강좌에서 다루지 않은 내용 일부를 추가하려 합니다. 즉, 새 강좌 연재는 아니고, 부록에 해당하는 것이지요.

  이번 글에서는 Django 1.0판부터 바뀐 Admin 영역을 다루겠습니다.
wordpress_id: 2327
wordpress_url: http://blog.hannal.com/?p=288
date: '2009-02-01 22:59:26 +0900'
date_gmt: '2009-02-01 13:59:26 +0900'
categories:
- "start_with_django_lectures"
tags:
- django
- "장고"
- "파이썬"
- admin
- "부록"
- django입문강좌
permalink: "/2009/2/99_1-python_django_lecture/"
---
<p>안녕하세요,<span> </span>한날입니다.</p>
<p>오랜만에 Django 강좌 글로 뵙습니다.</p>
<p>지난 <span>2008</span>년 <span>8</span>월에 강좌 연재를 마친 뒤 얼마 후에 Django 1.0판이 새로 나왔습니다. 꽤 많은 점이 바뀌었더군요. 대체로 이전 정식판인 0.96를 기준으로 개발한 코드라도 잘 작동하는데, 일부 기능은 아예 작동하지 않기도 합니다.</p>
<p>이 연재글은 “<a href="http://blog.hannal.com/01-python_django_lecture/">날로 먹는 Django 웹 프로그래밍 강좌</a>” 을 보완하여 현재 정식으로 배포되고 있는 Django 1.0판에서 제대로 작동시키는 내용을 담습니다. 또, 본 강좌에서 다루지 않은 내용 일부를 추가하려 합니다. 즉, 새 강좌 연재는 아니고, 부록에 해당하는 것이지요.</p>
<p>그럼 바로 시작하겠습니다.</p>
<h3>Admin 영역 사용법 변화</h3>
<h4>admin.py 만들기</h4>
<p>“<a href="http://blog.hannal.com/05_1-python_django_lecture/">청) 컨트롤러(뷰) - 글 목록과 글 보기 기능 (5편 1/3)</a>”에서 Django의 Admin 영역/기능을 다룹니다. 그런데 1.0판부터 Admin 기능을 쓰는 방법이 크게 바뀌었습니다.</p>
<p>기존에는 각 모델마다 Admin 이라는 클래스를 만들어서 썼습니다.</p>
<blockquote>
<pre><code>class Entries(models.Model):
    Title = models.CharField(max_length=80, null=False)

    class Admin:
        pass</code></pre>
</blockquote>
<p>이런 식이죠. 하지만, 1.0판부터는 admin.py파일을 따로 만들어야 합니다.<sup><a href="http://blog.hannal.com/99_1-python_django_lecture/comment-page-1/#comment-226">참고1)</a></sup></p>
<p>이 파일은 각 어플리케이션마다 씁니다. 본 강좌에서 우리는 blog 라는 어플리케이션을 만들었는데(manage.py startapp blog), 이 어플리케이션 안에, 그러니까 어플리케이션 디렉토리(폴더) 안에 admin.py 파일을 만들면 됩니다.</p>
<p>admin.py 파일을 만든 뒤 관련 모듈을 가져오겠습니다.</p>
<blockquote>
<pre><code>from django.contrib import admin</code></pre>
</blockquote>
<p>이 admin 모듈을 써서 Django admin 영역에서 다룰 모델을 등록할 수 있습니다. 바로 admin.site 에 있는 register 메서드, 즉 admin.site.register 로 하는 것이지요.</p>
<p>먼저 Admin 영역에서 다룰 모델을 가져옵니다. 편의상 Entries 를 해보겠습니다.</p>
<blockquote>
<pre><code>from hannal.blog.models import Entries</code></pre>
</blockquote>
<p>from 경로는 여러분이 어떻게 프로젝트를 만들고 어플리케이션 이름을 지었는지에 따라 다릅니다.</p>
<p>이렇게 가져온 모델을 admin.site.register 로 등록합니다.</p>
<blockquote>
<pre><code>admin.site.register(Entries)</code></pre>
</blockquote>
<p>이렇게 하면 Django admin 영역에서 Entries 모델을 다룰 수 있습니다.</p>
<h4>Admin 영역에서 좀 더 예쁘게 하기</h4>
<p>Django admin 영역에서 다룰 모델을 등록하는 것 말고도 화면에 나타나는 영역을 바꿀 수 있습니다. 본 강좌에서는 “admin 영역에서 자료 좀 더 예쁘게 나타내기”라는 단락에 나와있지요. 이 부분도 Django 1.0판에서 조금 바뀌었습니다. Entries 모델을 기준으로 예를 들어서, 우리는 글 번호와 제목, 작성일시만 뜨게 해보겠습니다.</p>
<p>admin.site.register 위에 아래와 같이 클래스를 하나 만듭니다.</p>
<blockquote>
<pre><code>class EntriesAdmin(admin.ModelAdmin):
    pass</code></pre>
</blockquote>
<p>이 클래스는 Admin 영역에서 Entries 모델을 어떻게 출력하고 다루게 할 것인지에 대한 정보를 담는 역할을 합니다. 자세한 내용은 잠시 뒤에 하기로 하고 우선 pass 로 빈 클래스를 만듭니다.</p>
<p>이 클래스를 방금 admin.site.register 로 Entries 모델을 등록하는 부분에 추가합니다.</p>
<blockquote>
<pre><code>admin.site.register(Entries, EntriesAdmin)</code></pre>
</blockquote>
<p>이렇게 register 메서드에 두 번째 인자로 넣는 겁니다.</p>
<p>이제 글 번호, 제목, 작성일시가 뜨게 해보겠습니다. 간단합니다. EntriesAdmin 클래스에 아래 내용을 써넣습니다.</p>
<p>list_display = ('id', 'Title','created',)</p>
<p>list_display 라는 클래스 프로퍼티에 튜플 자료형으로 출력할 모델 요소를 문자열로 담는 겁니다. 기존 방법과 같습니다.</p>
<blockquote>
<pre><code>class EntriesAdmin(admin.ModelAdmin):
    list_display = ('id', 'Title','created',)</code></pre>
</blockquote>
<p>코드는 이렇게 생겼습니다.</p>
<p>이외에도 좀 더 다양한 변화를 줄 수 있습니다. 자세한 내용은 공식 문서(<a href="http://docs.djangoproject.com/en/dev/ref/contrib/admin/#ref-contrib-admin">The Django admin site</a>)를 참조하시길 바랍니다.</p>
<h4>urls.py 내용 바꾸기</h4>
<p>이걸로 끝이면 좋겠는데, urls.py 에서 Admin 영역에 접근하는 방법도 바뀌었습니다.</p>
<p>먼저 urls.py 맨 위에다가</p>
<blockquote>
<pre><code>from django.contrib import admin
admin.autodiscover()</code></pre>
</blockquote>
<p>를 넣습니다. 그런 뒤 주소 체계는 아래와 같이 넣으면 됩니다.</p>
<blockquote>
<pre><code>    (r'^admin/(.*)', admin.site.root),</code></pre>
</blockquote>
<p>Django 1.0판에서 프로젝트를 만들면(django-admin.py startproject hannal) 이러한 내용은 이미 반영되어 있으니 크게 어렵진 않을 겁니다.</p>
