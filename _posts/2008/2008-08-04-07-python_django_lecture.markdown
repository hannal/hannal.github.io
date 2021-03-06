---
layout: post
status: publish
published: true
title: "이) RSS 기능, 로그인 기능 (7편) - django 강좌"
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
excerpt: "어느 덧 “<strong>날로 먹는 Django 웹 프로그래밍 강좌</strong>”에서 기능 구현을 하는 마지막 글이다. 이번
  글에서는 RSS 기능과 로그인을 위한 세션 처리 기능을 만들 것이다. 어려울 것 하나 없으니 부담갖지 말고, 깔끔하게 기능 구현을 마무리 지어보자."
wordpress_id: 2321
wordpress_url: http://blog.hannal.com/?p=161
date: '2008-08-04 13:51:41 +0900'
date_gmt: '2008-08-04 04:51:41 +0900'
categories:
- "start_with_django_lectures"
tags:
- django
- python
- rss
- "파이썬"
- django입문강좌
permalink: "/2008/8/07-python_django_lecture/"
---
<p>어느 덧 “<strong>날로 먹는 Django 웹 프로그래밍 강좌</strong>”에서 기능 구현을 하는 마지막 글이다. 이번 글에서는 RSS 기능과 로그인을 위한 세션 처리 기능을 만들 것이다. 어려울 것 하나 없으니 부담갖지 말고, 깔끔하게 기능 구현을 마무리 지어보자.</p>
<h3>RSS 기능</h3>
<p>django에서 RSS 기능을 구현하는 방법은 크게 두 가지이다. 저차원(low-level)으로 직접 RSS 기능을 구현하는 방법이 첫 번째이고, 두 번째는 보다 다양한 RSS 기능을 구현할 때 거치게 되는 귀찮은 작업 몇 개를 간편하게 해주는 고차원(high-level) 방식이다. 수준이나 능력 차이가 아니라 좀 더 사람에게 편의를 제공하는지에 따라 나눈 것이다. 고차원 방법은 저차원 방법을 기반으로 하므로 저차원 방법부터 알아보자.</p>
<h4>저차원 방법으로 RSS 구현하기</h4>
<p>RSS 를 제공하려면 RSS 주소가 필요하다. /blog/feed/ 라는 주소를 쓰기로 하고, 여기로 접속하면 views.py 에서 rssfeed 라는 함수가 적절한 처리를 하도록 하자.</p>
<p>우선 urls.py 에는</p>
<blockquote><p><code>(r'^blog/feed/$', 'hannal.blog.views.rssfeed'),</code></p></blockquote>
<p>이렇게 한다. 그런 뒤 views.py 에는 rssfeed 라는 함수를 만든다.</p>
<blockquote><pre><code>def rssfeed(request):
    pass
</code></pre>
</blockquote>
<p>이제 RSS에 필요한 xml 자료형을 쉽게 만드는데 필요한 모듈을 가져와야 한다. django에는 있는 utils 엔 <strong>feedgenerator</strong> 라는 모듈이 있다(/django/utils/feedgenerator.py). 이것이 RSS 자료 제공(Feeding)에 필요한 xml 자료를 만들어준다. 이 모듈은 rssfeed 함수에서만 쓸테니 이 함수 안에서 가져오자(import).</p>
<blockquote><pre><code>def rssfeed(request):
    from django.utils import feedgenerator
    pass
</code></pre>
</blockquote>
<p>RSS 같은 자료형에는 크게 RSS 0.91, RSS 2.01, Atom  등이 있다. 거의 비슷한데 RSS 2 가 많이 쓰이니 RSS 2 형식으로 xml 을 만들자(django 에서 atom 으로 하는 방법도 사실상 동일하다).</p>
<blockquote><pre><code>fd = feedgenerator.Rss201rev2Feed(
     title = u'my blog rss',
     link = u'/blog/feed/',
     description = u'this is a rss of my blog'
)</code></pre>
</blockquote>
<p>feedgenerator 클래스에 있는 Rss201rev2Feed 라는 메소드를 이용해서 객체를 만든 뒤 fd 에 넣는다. RSS 2.01 형태로 자료형을 만드는 메소드이다. <strong>title 은 RSS 제목, link 는 RSS 주소, description 은 RSS 설명</strong>이다. 적당히 원하는 내용으로 쓰면 된다.</p>
<div style="border: 1px dahsed #666; padding: 3px; background-color: #dfdfff;">* 2010년 11월 28일 추가 참고사항 : <a href="http://blog.hannal.com/07-python_django_lecture/comment-page-1/#comment-866">django 판이 올라감에 따라 패치 적용</a>해야 합니다.</div>
<p>이제 최근 글 5개를 가져와서 xml 에 넣을 차례이다.</p>
<blockquote><pre><code>for entry in Entries.objects.all().order_by('-created')[:5]:
    fd.add_item(
        title = entry.Title,
        link = u'/blog/entry/%d/' % entry.id,
        description = entry.Content,
        pubdate = entry.created,
        categories = (entry.Category.Title,)
    )</code></pre>
</blockquote>
<p>for 반복문 내용은 굳이 설명하지 않아도 알 것이다. 그럼 fdd.add_item 부분을 보자.</p>
<p>위에서 Rss201rev2Feed 메소드를 통해 만든 fd 객체엔 <strong>add_item 메소드가 상속되어 있다</strong>. 이 메소드는 이름에서 알 수 있듯이 RSS의 item 요소(element)를 추가해준다. RSS의 xml 내용을 보면 글 내용이 item 요소로 반복되는데, 바로 그 item 요소이다.</p>
<p><a href="http://www.djangoproject.com/documentation/syndication_feeds/#the-low-level-framework">add_item 메소드로는 여러 인자를 넘겨 받는다</a>.</p>
<p>주요 인자만 살펴보자면</p>
<ul>
<li>title : 글 제목</li>
<li>link : 글 낱장 주소</li>
<li>description : 글 본문</li>
<li>pubdate : 글 작성일시</li>
<li>comments : 댓글 볼 수 있는 주소</li>
<li>categories : 글 갈래들 (낱개가 아니라 여러 개이다)</li>
</ul>
<p>들을 들 수 있다. 이 중 <strong>categories 는 글 하나에 글 갈래가 여러 개 달릴 수 있기 때문에 tuple 자료형으로</strong> 글 갈래를 보내줘야 한다.</p>
<p>위 for 반복문에 보면 title 에는 글 제목인 entry.Title 을, link 엔 글 낱장주소인 /blog/entry/숫자 를 u'/blog/entry/%d/' % entry.id 이렇게 만들어서, description 엔 글 본문인 entry.Content 을, pubdata 엔 글 작성일시인 entry.created 를, 마지막으로 categories 엔 글 갈래 이름인 entry.Category.Title 를 넣는데 중요한 건 categories 는 글 갈래를 여러 개 넣을 수 있게 되어 있어서 tuple 자료형이나 list 자료형으로 넣어줘야 해서 (entry.Category.Title,) 이렇게 넣어줬다. 만약 (entry.Category.Title) 이렇게 쉼표를 빼먹으면 tuple 자료형이 아닌 일반 문자열로 인식을 해서 categories 엔 글 갈래 이름이 글자 하나 하나로 쪼개져서 들어갈 것이다. 앞서 말을 하고선 또 말을 반복하는 이유는 실수하기 쉬운 부분이기 때문이다.</p>
<p>이걸로 준비는 끝났다. 정말 간단하다. 이제 이걸 화면에 뿌려주면 그만이다.</p>
<blockquote><p><code>return HttpResponse(fd.writeString('utf-8'), mimetype='application/rss+xml')</code></p></blockquote>
<p>fd 객체에 있는 <strong>writeString 메소드</strong>로 fd 에 있는 자료를 문자열로 출력하여 HttpResponse 로 보내준다. 만일 <strong>파일로 저장하고 싶다면 파일 객체 지시자(file pointer)를 인자로 필요로 하는 write 메소드를 이용</strong>하면 된다.</p>
<p>fd 객체에 있는 자료를 문자열로 출력할 때엔 자료가 utf-8 이므로 utf-8 라고 인자를 넣어줬다. 그리고 이 RSS의 HTTP 자료 형식은 <strong>application/rss+xml</strong> 이므로 이 내용을 HTTP 헤더에 넣도록 하기 위해 HttpResponse 에 <strong>mimetype 이라는 인자</strong>를 넣었다. 지난 글에서 ajax 접근 요청에 대해서 json 이라는 javascript 자료형으로 값을 반환했는데 이때에도 엄밀히 말해서 mimetype 을 지정해줘야 했다(text/javascript 같은 걸로).</p>
<p>이제 웹브라우저에서 /blog/feed/ 에 접근을 해보자. RSS 내용이 잘 뜬다.</p>
<h4>고차원 방법으로 RSS 구현하기</h4>
<h5>주소 설정</h5>
<p>고차원이라 아주 간편하게 구현할 수 있을 것 같지만, 실은 좀 더 설정을 해야 한다. 우선 주소 규칙을 추가하자. 주소는 아까 만든 /feed 를 좀 더 쓸 것인데, /blog/feed/rss/ 로 접근하면 RSS 방식으로 자료를 가져와서 출력하고, /blog/feed/atom/ 으로 접근하면 ATOM 방식으로 자료를 가져와서 출력할 것이다. 물론, 고차원 방법으로. 흐흐.</p>
<blockquote><p><code>(r'^blog/feed/(?P&lt;url&gt;.*)/$', 'django.contrib.syndication.views.feed', {'feed_dict': feeds}),</code></p></blockquote>
<p>앞에 주소 체계는 익숙한데, 뒤에 붙은 django.contrib.syndication.views.feed 과 {'feed_dict': feeds} 이건 익숙하지 않다.</p>
<p><strong>django.contrib.syndication.views.feed 는 RSS 같은 자료 생성을 고차원으로 하는 모듈</strong>이다. 저 주소 체계로 접근하면 관련 내용을 django.contrib.syndication.views.feed 여기로 던져주는 것이고, 인자값으로 url 을<strong><code>(?P&lt;url&gt;.*)</code></strong> 받는 것이다. 넘겨 받을 인자값이 하나 더 있는데 바로 <strong>feed_dict</strong> 이다. 이건 인자로 넘겨 받는 url과 관련된 것이다. 우리는 /blog/feed/rss/ 나 /blog/feed/atom/ 으로 접근한다고 가정했으므로 rss 와 atom 을 url 인자값으로 넘겨준다. 이때, <strong>url 인자값으로 넘겨받은 값(rss 나 atom)에 대응하는 클래스를 이어주는 것</strong>이다. 이 이어주는 자료를 dictionary(dict) 자료형으로 넘겨줘야 하는데, 나는 feeds 라는 변수에 담은 것이다. 그럼 feeds 변수를 만들어야 한다는 말인가? 맞다.</p>
<blockquote><p><code>feeds = {'rss': RssFeed, 'atom': AtomFeed}</code></p></blockquote>
<p>위 feeds 변수를 urlpatterns = patterns('', 이 위에 넣자. 적어도</p>
<blockquote><p><code>(r'^blog/feed/(?P&lt;url&gt;.*)/$', 'django.contrib.syndication.views.feed', {'feed_dict': feeds}),</code></p></blockquote>
<p>이 부분 보다는 위에 있어야 한다. :)</p>
<p>이번엔 feeds 변수를 보자. feeds 변수는 dictionary 자료형으로써, 키로는 rss (feeds['rss'])와 atom (feeds['atom'])이 있다. 이 각 키에는 RssFeed 와 AtomFeed 라는 개체가 연결되어 있는데, 얘가 참 뜬금없이 등장했다. feeds 처럼 위에서 만든 것도 아니고, 그렇다고 어디에서 가져온(import) 것도 아니고. 즉, 이 두 녀석을 urls.py 안에 만들든 따로 파일로 만들든 만들어야 한다. 따로 feeds.py 파일로 만들어서 두 개체를 클래스 객체로 만들어보자.</p>
<h5>RSS 클래스 만들기</h5>
<p>우선 views.py 와 models.py 파일이 있는 blog 디렉토리 안에 feeds.py 파일을 만든다. 그리고 RssFeed 클래스와 AtomFeed 클래스를 만들면 되는데, 이 두 클래스는 <strong>django.contrib.syndication.feeds 에 있는 Feed 라는 객체를 상속 받아야 한다</strong>. 우리가 models.py 에서 모델을 만들 때 models.Model 객체를 상속시켰듯이 Feed 객체를 상속시켜야 고차원(쓸 수록 느끼지만 참 마음에 안드는 표현이다) 구현이 가능하다. 그러므로</p>
<blockquote><p><code>from django.contrib.syndication.feeds import Feed</code></p></blockquote>
<p>이렇게 이 객체를 먼저 가져온 뒤</p>
<blockquote><pre><code>class RssFeed(Feed):
    pass

class AtomFeed(Feed):
    pass</code></pre>
</blockquote>
<p>이렇게 두 객체를 초기화 한다. 그런 뒤 잠시 urls.py 로 되돌아가자. 왜냐하면 만든 두 객체가 urls.py 가 아닌 feeds.py 에 있으므로, urls.py 에서 feeds 라는 변수에 지정한 두 객체를 urls.py로 가져와야(import) 하기 때문이다.</p>
<blockquote><p><code>from hannal.blog.feeds import RssFeed, AtomFeed</code></p></blockquote>
<p>위 코드는 당연히</p>
<blockquote><p><code>feeds = {'rss': RssFeed, 'atom': AtomFeed}</code></p></blockquote>
<p>이 코드 위에 있어야 한다.</p>
<p>이제 준비는 끝났다. 고차원스럽게(-_-; ) RSS 를 구현하러 feeds.py 로 돌아가자.</p>
<p>먼저 RssFeed 클래스. 우리가 저차원으로 RSS 구현했을 때 RSS 이름이나 주소, 설명을 넣었다. 거의 같은 방법으로 이들을 정하면 되는데, RssFeed 클래스 안에 프로퍼티로 title, link, description 을 정의하면 된다.</p>
<blockquote><pre><code>class RssFeed(Feed):
    title = 'my blog rss'
    link = '/blog/feed/'
    description ='this is a rss of my blog'</code></pre>
</blockquote>
<p>그런 뒤, RssFeed 클래스가 글을 가져오는 기능을 할 메소드를 만들어야 하는데, 그 메소드는 <strong>items</strong> 라는 이름으로 만들면 django 가 자동으로 해당 메소드를 실행한다.</p>
<blockquote><pre><code>def items(self):
    return Entries.objects.order_by('-created')[:5]</code></pre>
</blockquote>
<p>잠깐. Entries 라는 모델 객체를 쓰고 있다. 그러므로 이 객체를 가져와야(import) 한다.</p>
<blockquote><p><code>from hannal.blog.models import Entries</code></p></blockquote>
<p>이 코드를 feeds.py 위에 추가한다. 이로써 기능 구현은 끝났다. 아니, 거의 끝났다.</p>
<h5>link 요소(element) 만들기</h5>
<p><strong>django는 각 글(item)의 xml 요소(element) 중 title, description, link 는 기본 요소로 쓴다</strong>. 그리고, 이 셋은 다른 RSS xml 요소와는 조금 다르게 다룬다.</p>
<p>먼저 각 글의 낱장주소 값을 담는 link. django 는 link 정보를 가져올 때 두 가지 방법을 취한다.</p>
<ul>
<li>모델에 있는 get_absolute_url 메소드로 자료(글) 낱장주소를 얻거나</li>
<li>RSS 클래스(RssFeed)에 있는 item_link 메소드로 얻는다.</li>
</ul>
<p>먼저 item_link 메소드를 만들어 나타내보자.</p>
<blockquote><pre><code>def item_link(self, item):
    return '/blog/entry/%d/' % item.id</code></pre>
</blockquote>
<p>item_link 는 인자를 두 개 받는다. 우선 클래스에 속한 메소드이므로 객체 자기 자신을 가리키는 self 를 받는다. 그리고 item 을 받는데, 이는 items 메소드로 자료들을 넘겨받은 뒤, for 반복문으로 빙빙돌며 자료를 하나씩 꺼내어 넘기는 것과 같다. 예를 들자면</p>
<blockquote><pre><code>for item in items():
    item_link(self, item)</code></pre>
</blockquote>
<p>이런 흐름이라고 볼 수 있다. 이렇게 넘겨받은 item(글 객체)를 이용해 글 낱장주소를 문자열로 반환하면 된다. 왜냐하면 우리는 글 낱장 주소를 “/blog/entry/글일련번호” 꼴이기 때문이다.</p>
<p>사실 위와 같이 주소를 반환하면 안된다. URL 에서 도메인 같은 접속할 서버 주소가 없기 때문이다. 그러므로 제대로 한다면</p>
<blockquote><pre><code>def item_link(self, item):
    return 'http://localhost:8000/blog/entry/%d/' % item.id</code></pre>
</blockquote>
<p>이런 식으로 서버 주소(도메인이든 ip주소든)를 써야 한다. 다만 이 강좌는 내부(localhost)에서만 돌리며 소스 간결함과 설명을 위해 /blog/entry 식으로 한 것 뿐이다.</p>
<p>어쨌든 item 으로 글 객체를 넘겨 받았으니, 글 낱장주소에 쓸 글 일련번호인 id, 즉 item.id 를 이용해 주소를 완성해서 반환(return)하면 된다.</p>
<p>두 번째 방법은 <strong>모델에 get_absolute_url 메소드를 이용하는 것</strong>이다. 이 방법이 두고 두고 편하므로 권할 만하다.</p>
<p>먼저 models.py 파일에서 글 정보 모델인 Entries 모델을 보자. 거기에 보면 아마도</p>
<blockquote><pre><code>class Admin:
    pass</code></pre>
</blockquote>
<p>이와 같이 django admin 영역을 위한 코드가 있을텐데 그 위든 아래든 다음과 같이 get_absolute_url 메소드를 넣자.</p>
<blockquote><pre><code>def get_absolute_url(self):
    return '/blog/entry/%d/' % self.id</code></pre>
</blockquote>
<p>item_link 메소드와 별 다를 바 없는 구조이다. get_absolute_url 메소드가 Entries 클래스의 메소드이므로 자기 자신을 가리키는 self 를 인자로 넘겨받았고, 낱장주소로 쓸 문자열을 반환받는 것이다. 이렇게 하면 글 낱장 읽기, 그러니까 views.py 에서 read 함수가 가져다 출력하는 read.html 에서 글 제목에 연결한 글 낱장주소를 일일이</p>
<blockquote><p><code>&lt;a href="/blog/entry/{{current_entry.id}}"&gt;</code></p></blockquote>
<p>라고 적을 필요 없이</p>
<blockquote><p><code>&lt;a href="{{current_entry.get_absolute_url}}"&gt;</code></p></blockquote>
<p>라고 써주면 된다. 글 낱장 주소를 바꿀 때 마다 낱장주소를 쓴 파일을 하나 하나 찾아서 고치지 않아도 되니 참으로 편하다. 이런 get_absolute_url 메소드도 직접 주소를 문자열로 만들 필요 없이 urls.py 에 있는 주소를 알아내어 urls.py 에서 주소 체계를 바꾸면 자동으로 해당 내용이 반영되게 할 수도 있다. 다만, 여기서는 이 방법을 다루진 않겠으니 이 방법을 알고 싶은 이는 <a href="http://www.djangoproject.com/documentation/url_dispatch/#reverse">공식 문서에 있는 reverse 유틸리티</a>를 참조하길 바란다.</p>
<p>모델에 get_absolute_url 메소드를 만들어주면 feeds.py 에서 item_link 메소드를 쓰지 않아도 된다. 다만 <strong>우선순위가 item_link 메소드 호출이 모델에 있는 get_absolute_url 메소드 호출 보다 높으므로 혼란을 피하기 위해 두 방법을 동시에 쓰지 않는 것이 좋다</strong>.</p>
<h5>title과 description 요소(element) 만들기</h5>
<p>link 요소에서 진을 다 뺀 기분이다. 다행히 title 과 description 요소는 훨씬 간단하다. django 가 알아서 이런 저런 처리를 해주기 때문이다. 우리는 단지 파일 두 개만 만들면 그만이다. title 과 description 요소는 템플릿 디렉토리(폴더)에 feeds 라는 디렉토리를 만든 뒤 그 안에 각 요소에 대응하는 템플릿 파일을 만들기만 하면 된다.</p>
<p>지금 우리는 /blog/feed/rss/ 로 접근했을 때 작동할 RssFeed 클래스를 만들고 있다. /blog/feed/ 뒤에 붙는 주소가 rss 이면 RssFeed 클래스, atom 이면 AtomFeed 클래스를 쓰는 것인데, <strong>바로 이 rss 와 atom 을 slug 라고 한다</strong>(여기서만 쓰는 용어는 아니다). 템플릿 디렉토리에 파일을 만들 때 바로 이 slug 를 파일 이름에 쓴다.</p>
<p>현재 rss 슬러그에 대한 처리를 하고 있으니 templates 디렉토리에 feeds 라는 디렉토리를 만들고, 그 안에 <strong>rss_title.html</strong> 을 만들자 ( templates/feeds/rss_title.html ). 그리고 그 안에</p>
<blockquote><p><code>{{ obj.Title }}</code></p></blockquote>
<p>라고 써넣는다. 같은 디렉토리에 <strong>rss_description.html</strong> 파일을 만들고 그 안에는</p>
<blockquote><p><code>{{ obj.Content }}</code></p></blockquote>
<p>라고 써넣는다. read.html 템플릿 파일을 떠올린다면 눈에 익은 치환자와 구조이다. read.html 에선 current_entry 라는 치환자를 쓰므로</p>
<blockquote><p><code>{{ current_entry.Title }}</code></p></blockquote>
<p>이런 식으로 썼을 뿐이다.</p>
<p>이제 정말 끝났다. 소스를 저장한 뒤에 /blog/feed/rss/ 로 접근하면 잘 뜬다. 어떤 식으로 저 템플릿 파일이 쓰이는지 모르겠다면 rss_title.html 파일을 연 뒤</p>
<blockquote><p><code>{{ obj.Title }} [[까르르르르륵]]</code></p></blockquote>
<p>이렇게 바꾼 뒤 다시 저 주소로 접근해보자. 글 제목에 방금 덧붙인 문자열이 붙어있다.</p>
<p>저차원 방법에서 구현한 것처럼 그냥 글 제목이나 본문을 덧붙이면 되지 왜 이렇게 귀찮게 템플릿 파일을 거치게 했을까? 여러 이유가 있겠지만, 한 가지 상황을 가정하는 걸로 이 방식이 편하다는 걸 알 수 있다. RSS 주소가 바뀌어서 관련 공지를 덧붙이거나 RSS 글 본문(description)란에 자신의 얼굴 사진을 덧붙이고 싶다고 가정해보자. 이럴 때 소스 코드를 건드릴 필요 없이 템플릿 파일만 고치면 쉽지 않을까?</p>
<h5>나머지 RSS 요소 넣기</h5>
<p>이제 글 작성일시와 글 갈래도 RSS 에 넣어보자. 위에서 item_link 메소드로 글 낱장주소를 넣은 것처럼 관련 메소드를 만들면 된다.</p>
<p>우선 <strong>글 작성일시는 item_pubdate 메소드</strong>가 맡고 있다.</p>
<blockquote><pre><code>def item_pubdate(self, item):
    return item.created</code></pre>
</blockquote>
<p>그리고 글 갈래는 item_categories 메소드가 맡고 있다.</p>
<blockquote><pre><code>def item_categories(self, item):
    return (item.Category.Title,)</code></pre>
</blockquote>
<p>위에서 이미 설명한 바와 같이, 글 갈래는 여러 개가 들어갈 수 있으므로 tuple 자료형으로 값을 넘겨줘야 한다.</p>
<p>자, 이제 RssFeed 클래스는 이렇게 생겼을 것이다.</p>
<blockquote><pre><code>class RssFeed(Feed):
    title = 'my blog rss'
    link = '/blog/feed/'
    description ='this is a rss of my blog'

    def item_link(self, item):
        return '/blog/entry/%d/' % item.id

    def item_pubdate(self, item):
        return item.created

    def item_categories(self, item):
        return (item.Category.Title,)

    def items(self):
        return Entries.objects.order_by('-created')[:5]</code></pre>
</blockquote>
<h5>AtomFeed 클래스 만들기</h5>
<p>저차원 방법으로 RSS 기능 구현할 때 RSS 2.01 이든 Atom 이든 django 에서 구현하는 방법은 다를 바 없다고 한 말이 기억났으면 좋겠다. 이 말을 기억한다면 AtomFeed 클래스는 RssFeed 처럼 필요한 기능을 모두 구현할 필요 없이 아주 쉽게 구현할 수 있기 때문이다.</p>
<p>현재 AtomFeed 클래스는</p>
<blockquote><pre><code>class AtomFeed(Feed):
    pass</code></pre>
</blockquote>
<p>이렇게 생겼다. 근데 구현은 RSS 2.01 이든 Atom 이든 같다. 그럼 굳이 AtomFeed 에서 복잡하게 메소드니 뭐니 추가할 필요 없이 RssFeed 클래스 내용을 베껴오면 쉬울 것이다. 그걸 할 수 있게 RssFeed 내용을 상속 받게 하면 된다.</p>
<blockquote><pre><code>class AtomFeed(RssFeed):
    pass</code></pre>
</blockquote>
<p>RssFeed 는 이미 Feed 클래스를 상속 받고 있으므로 AtomFeed 클래스가 RssFeed 클래스를 상속 받으면 Feed 클래스도 쓸 수 있는 셈이다. 그리고 RssFeed 에서 정의한 프로퍼티(RSS 이름이나 설명 등)나 메소드(item_link 니 뭐니)도 함께 상속 받는다.</p>
<p>필요한 내용은 상속 받는 걸로 끝냈으니 놀고 먹자(^^). 다만, <strong>RssFeed 클래스와 완전히 같으면 안되니 RssFeed 와 AtomFeed 를 구분할 프로퍼티 내용을 따로 정의</strong>해주면 된다.</p>
<p>먼저 <strong>feed_type 이라는 프로퍼티</strong>가 있다. 이름에서 알 수 있듯이 feed_type 을 정의한다. 이 값을 따로 정의하지 않으면 기본으로 Rss201rev2Feed 가 들어가서 RSS 2.01 방식이 된다. AtomFeed 는 Atom 1 방식이므로</p>
<blockquote><p><code>feed_type = Atom1Feed</code></p></blockquote>
<p>이렇게 해준다. 아, 또 뜬금없이 Atom1Feed 가 나왔다. 우린 이런거 만든 적도 없으니 어딘가에서 가져와야(import) 한다. django.utils.feedgenerator 여기에 있으니 feeds.py 파일 속 위에</p>
<blockquote><p><code>from django.utils.feedgenerator import Atom1Feed</code></p></blockquote>
<p>라고 해주면 된다.</p>
<p>이로써 다 된 것 같은데 두 가지 프로퍼티를 더 정의해줘야 한다. 바로 title과 description 요소를 위한 템플릿 지정이다. RssFeed 는 슬러그가 rss 이므로 자동으로 rss_title.html 과 rss_description.html 을 가져다 썼다. <strong>AtomFeed 도 마찬가지여서 atom_title.html 과 atom_description.html 을 가져다 쓰려 한다</strong>. 딱히 RssFeed 와 다르게 할 생각이 없으므로 AtomFeed 도 rss_title.html 과 rss_description.html 을 가져다 쓰게 하자.</p>
<blockquote><pre><code>title_template = 'feeds/rss_title.html'
description_template = 'feeds/rss_description.html'</code></pre>
</blockquote>
<p>굳이 설명 안해도 이해할 수 있을 것이다.</p>
<p>이로써 AtomFeed 클래스는</p>
<blockquote><pre><code>class AtomFeed(RssFeed):
    feed_type = Atom1Feed
    title_template = 'feeds/rss_title.html'
    description_template = 'feeds/rss_description.html'</code></pre>
</blockquote>
<p>이렇게 생겼다.</p>
<p>이제 /blog/feed/ 는 저차원 방법으로 RSS 가 작동하고, /blog/feed/rss/ 와 /blog/feed/atom/ 은 고차원 방법으로 RSS 가 작동한다.</p>
<h3>로그인 기능</h3>
<h4>세션 다루기</h4>
<p>드디어 마지막 기능이다. 이번 기능 구현은 숙제도 담고 있다.</p>
<p><strong>로그인 기능은 보통 서버 세션 기능과 웹브라우저 쿠키 기능을 함께 이용</strong>한다. django 역시 이들 기능을 다룰 수 있는 기능을 제공한다. 바로 request 객체를 통하면 된다.</p>
<p>늘 그랬듯이 로그인과 로그아웃을 처리할 주소 규칙을 urls.py 에 넣자. 로그인은 /login , 로그아웃은 /logout 주소로 처리하자.</p>
<blockquote><pre><code>(r'login/$', 'hannal.blog.views.login'),
(r'logout/$', 'hannal.blog.views.logout'),</code></pre>
</blockquote>
<p>그런 뒤 views.py 안에 login 과 logout 함수를 만들자.</p>
<blockquote><pre><code>def login(request):
    request.session['blog_login_sess'] = 'hannal'
    return HttpResponse('[%s] logged in successfully' % request.session['blog_login_sess'])</code></pre>
</blockquote>
<p>먼저 로그인 함수. 간단하다. 넘겨받은 request 객체에 있는 session 프로퍼티에 세션 이름을 키(key)값으로 넣고, 그 세션에 저장될 내용을 넣어주면 된다. 여기서는 blog_login_sess 라고 세션키 이름을 지었다.</p>
<p>로그아웃도 간단하다.</p>
<blockquote><pre><code>def logout(request):
    del request.session['blog_login_sess']
    return HttpResponse('logged out successfully')</code></pre>
</blockquote>
<p>그런데 위 내용은 사실 세션 기능이 작동하고 있는 것은 아니다. 실제 기능으로써(functionality) 자동하게 하려면 따로 설정을 해야 한다.</p>
<h4>세션 설정</h4>
<p>세션에 대한 설정은 settings.py 에서 한다. 먼저 세션 기능을 쓸 수 있게 하려면 <strong>MIDDLEWARE_CLASSES</strong> 에 사용할 미들웨어를 등록해야 한다.</p>
<p>별도 설정을 안했다면</p>
<blockquote><pre><code>MIDDLEWARE_CLASSES = (
    'django.middleware.common.CommonMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.middleware.doc.XViewMiddleware',
)</code></pre>
</blockquote>
<p>이런 모양새일텐데 맨 위에</p>
<blockquote><p><code>'django.contrib.sessions.middleware.SessionMiddleware',</code></p></blockquote>
<p>이런 코드를 넣어서</p>
<blockquote><pre><code>MIDDLEWARE_CLASSES = (
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.middleware.doc.XViewMiddleware',
)</code></pre>
</blockquote>
<p>이렇게 만든다. 이번엔 우리가 만든 blog 어플리케이션을 등록했듯이 세션 어플리케이션도 INSTALLED_APPS 에 등록해야 하는데, 아마 이미 기본으로 들어가 있을 것이다.</p>
<blockquote><p><code>'django.contrib.sessions',</code></p></blockquote>
<p>이 코드가 들어가 있으면 된다. 근데 위 내용은 django 0.96에 해당되는 내용으로 0.97부터는 상황에 따라서 꼭 하지 않아도 괜찮다.</p>
<p>이제 세션 기능은 작동한다. 좀 더 <a href="http://www.djangoproject.com/documentation/sessions/#settings">세밀한 세션 설정은 django 공식 문서에</a> 잘 나와있으니 한 가지만 언급하고 내용을 마치려 한다.</p>
<h4>브라우저가 닫히면 세션도 지우기</h4>
<p>요즘은 이용자가 로그인한 상태에서 웹브라우저를 닫아도 접속을 유지시켜 주는 것이 유행이지만, 그래도 아직은 웹브라우저가 닫히면 로그인 정보를 지워서 접속을 끊어주는 것이 예전부터 지금까지 많은 곳에서 제공하는 사용성이다. 우리도 이 기능, 그러니까 웹 브라우저가 닫히면 로그인이 끊기는 기능을 구현해보자.</p>
<p>서버는 이용자가 웹브라우저를 닫았는지 열어놨는지 알 수 없다. 이용자가 어떤 행위를 하기 전까지는 말이다. 그러므로 서버 세션 정보만으로는 이용자가 웹브라우저를 닫았는지 알아내어 로그인 정보를 끊어야 할 지 말아야 할 지 알아낼 방법이 없다.</p>
<p>쿠키는 다르다. <strong>쿠키는 웹브라우저가 관리하는 정보</strong>이다. 쿠키는 만료일시(expired time)를 가질 수 있는데, 만약 <strong>쿠키 만료일시 값이 없으면 웹브라우저가 닫힐 때 해당 쿠키도 함께 사라진다</strong>. 만료일시가 3시간 뒤라면 웹브라우저가 닫혀도 해당 쿠키는 남아있다.</p>
<p>그렇다면 쿠키와 세션 정보를 어찌 잘 조화하면 이용자가 웹브라우저를 닫으면 로그인도 끊을 수 있지 않을까? 위에서 로그인 기능을 설명할 때 서버 세션 기능과 웹브라우저 쿠키 기능을 이용한다는 말이 바로 이런 상황을 뜻한 것이다.</p>
<p>작동법은 간단하다. 로그인을 하면 로그인 정보는 서버가 세션으로 들고 있고, 로그인을 했다는 표시 정도만 웹브라우저 쿠키로 들게 한다. 이 쿠키가 살아있다면 이용자는 웹브라우저를 닫지 않은 것이다. 그리고 쿠키가 없다면 웹브라우저를 닫은 이후이므로 서버 세션을 지워서 로그인을 끊으면 된다. 이용자가 로그아웃을 하면 해당 쿠키도 지우고 로그인 세션 정보를 지우면 된다.</p>
<p>이용자가 웹브라우저에 있는 쿠키 정보를 조작해도 상관 없다. 어차피 해당 쿠키는 웹브라우저를 닫았었는지 닫은 적이 없는지만 구분하는 쓰임새로 쓸 뿐, 실제 로그인 정보는 서버 세션으로 들고 있기 때문이다.</p>
<p>자, 이제 이런 내용을 소스 코드로 반영해보자. 말은 쉬운데 왠지 처리가 귀찮을 것 같다. 이 귀찮은 상황을 django 에서는 딱 한 줄로 처리해주는데, settings.py 에 <strong>SESSION_EXPIRE_AT_BROWSER_CLOSE</strong> 라는 변수를 통하기만 하면 된다.</p>
<p>이 변수는 이름에서 알 수 있듯이 웹브라우저가 닫힐 때 세션도 만료시킬 것인지를 설정한다. <strong>True 라고 하면 웹브라우저가 닫힐 때 쿠키와 세션을 만료</strong>시킨다. 즉 로그인도 끊기는 셈이다.</p>
<blockquote><p><code>SESSION_EXPIRE_AT_BROWSER_CLOSE = True</code></p></blockquote>
<p><strong>False 라고 하면 웹브라우저가 열려있든 닫혔든 상관없이 세션은 살아있다</strong>. 즉, 기본값으로 지정된 만료시간이나 이용자가 따로 지정한 만료시간을 넘기지만 않았다면 웹브라우저를 몇 번을 닫고 다시 열든 세션 정보는 살아있다. 따로 이 변수를 설정하지 않으면 기본으로 False 로 작동한다.</p>
<p>이 변수의 작동원리는 간단하다. django는 이용자가 서버에 접근할 때 쿠키를 발생시켜서 서버에 접근한 세션 정보를 들게 한다. 그런 뒤 접근이 있을 때 마다 쿠키가 살아있는지 만료되었는지 확인한다. 세션 만료시점은 이 세션과 이어져있는 쿠키 만료시점을 따른다.</p>
<p>이 변수가 True 이면 쿠키 만료 시점을 지정하지 않는데, 쿠키 만료 시점이 없으면 위에서 말했듯이 웹브라우저가 닫힐 때 쿠키가 삭제된다. 그래서 이용자 쿠키가 만료되었는지(웹브라우저가 닫히면 만료됨) 확인해서 만료된 경우 세션 정보도 만료시킬 수 있는 것이다. 이 변수가 False 이면 쿠키는 SESSION_COOKIE_AGE 변수에 담긴 기간만큼 지속되며(만료기간) 이 기간은 따로 설정하지 않을 경우 2주가 된다. 쿠키 만료기간이 있으면 웹브라우저가 닫혀도 해당 쿠키가 살아있으므로 웹브라우저를 열고 닫고 상관없이 쿠키가 살아있는 동안엔 그 쿠키와 이어진 세션 역시 살아있게 된다.</p>
<h4>로그인 기능 숙제</h4>
<p>이 강좌에서 여러분이 받는 마지막 숙제이다. :)</p>
<ul>
<li>이용자가 어딜 가든 로그인이 되어 있는지 확인하고, 로그인이 되어 있지 않으면 로그인을 할 수 있는 곳(/login_page/)으로 보내주자. 어딘가로 보내주려면 HttpRedirect 객체를 쓰면 되며, 이 객체는 HttpResponse 객체가 있는 곳에 있다.</li>
<li>로그인을 할 때엔 계정(id)과 비밀번호를 받아야 한다.</li>
<li>서버에서 비밀번호를 비교할 때엔 md5 모듈을 이용해야 한다. (댓글 쓰기 기능 만들 때 한 번 다뤘다)</li>
<li>웹브라우저가 닫히면 로그인도 끊어야 한다.</li>
<li>로그인이 되어 있어야만 글쓰기 화면이 나타나고, 글을 쓸 수 있다.</li>
</ul>
<p>구현해보자. :)</p>
<hr />
<p>드디어 이 강좌에서 기능 구현은 모두 마쳤다. 여기까지 함께한 여러분이 자랑스럽다. 진심으로 축하한다. 장담은 할 수 없지만, 아마도 여기까지 숙제 꼬박 꼬박하며 강좌를 진행한 이라면 파이썬과 django로 뭔가를 만드는 것이 두려운 단계는 벗어 났으리라 생각한다. 비록 앞으로도 계속 실수하고 벽에 부딪히겠지만, 이 정도까지 해낸 이라면 스스로 인터넷에서 자료를 찾으며 문제를 해결할 기본 소양을 갖췄다고 할 수 있다.</p>
<p>다음 글은 이 강좌를 마치는 글로 강좌 후기와 앞으로 여러분이 어떻게 했으면 좋을지 생각을 정리해 담으려 한다.</p>
<ul>
<li><a href='http://blog.hannal.com/assets/uploads/2008/08/hannal-yi_7.zip'>이번 글에서 다룬 소스 압축파일</a></li>
</ul>
