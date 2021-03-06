---
layout: post
status: publish
published: true
title: "청) 컨트롤러(뷰) - 댓글 기능 (5편 3/3) - django 강좌"
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
excerpt: "드디어 기능 구현을 하는 마지막 글이다. 글을 쓰고 쓴 글을 나타냈으니, 이젠 이 글에 댓글을 달 것이다. 새로울 원리 없으니 쉽게
  쉽게 만들 수 있다."
wordpress_id: 2318
wordpress_url: http://blog.hannal.com/?p=103
date: '2008-07-20 19:32:28 +0900'
date_gmt: '2008-07-20 10:32:28 +0900'
categories:
- "start_with_django_lectures"
tags:
- django
- jquery
- python
- "파이썬"
- ajax
- javascript
- django입문강좌
permalink: "/2008/7/05_3-python_django_lecture/"
---
<p>드디어 기능 구현을 하는 마지막 글이다. 글을 쓰고 쓴 글을 나타냈으니, 이젠 이 글에 댓글을 달 것이다. 새로울 원리 없으니 쉽게 쉽게 만들 수 있다.</p>
<h3>댓글 쓰기</h3>
<h4>댓글 써보낼 주소</h4>
<p>댓글을 보낼 주소부터 정해야 한다. urls.py 에서 주소 규칙을 추가하자.</p>
<blockquote><p><code>(r'^blog/add/comment/$', 'hannal.blog.views.add_comment'),</code></p></blockquote>
<p>글쓰기 주소인 /blog/add/post/ 와 거의 똑같다. 이번엔 저 주소에 대응하는 함수를 views.py에 만들어야 한다.</p>
<blockquote><pre><code>def add_comment(request):
    pass
</code></pre>
</blockquote>
<p>그리고 댓글 모델(Comments)도 써야 하니 가져오자(import).</p>
<blockquote><p><code>from hannal.blog.models import Entries, Categories, TagModel, Comments</code></p></blockquote>
<h4>댓글 쓰기 폼(form)</h4>
<p>잠시 컨트롤러(뷰)는 놔두고, 글쓰기 폼(form) 영역 html 을 먼저 만들자. templates 폴더에 <strong>comment_form.html</strong> 을 만들고 다음과 같이 html 를 써넣는다.</p>
<blockquote><pre><xmp><form method="post" action="/blog/add/comment/">
	<input type="hidden" name="entry_id" value="{{current_entry.id}}" />

	<p><label for="name">글쓴이</label><input type="text" id="name" name="name" value="" /></p>

	<p><label for="password">비밀번호</label><input type="password" id="password" name="password" value="" /></p>
	<p><textarea id="content" name="content"></textarea></p>
	<p><input type="submit" value="댓글 달기" /></p>
</form></xmp></pre>
</blockquote>
<p>특별한 건 없지만 <code>&lt;input type="hidden" name="entry_id" value="{{current_entry.id}}" /&gt;</code> 이걸 짚고 넘어가야 한다. 댓글을 단다는 뜻은 “<strong>특정 글에 댓글을 다는 것</strong>”이다. 그러므로 댓글을 남길 때 글쓴이 정보나 댓글 내용 뿐 아니라 “어떤 글”에 다는 것인지도(1번 글인지 10번 글인지) 서버로 보내야 한다. 그래서 폼(form) 정보로 entry_id 라는 항목에 글 번호를 넣는다. 다만 이걸 굳이 화면에 드러낼 필요는 없으므로 input 태그에 있는 type 속성을 hidden 으로 해서 감췄다.</p>
<p>다 만든 comment_form.html 은 글 낱장보기 템플릿 파일인 read.html 에서 불러 들인다.</p>
<blockquote><p><code>&lt;div&gt;<br />
&#123;&#37; include 'comment_form.html' &#37;&#125;<br />
&lt;/div&gt;</code>
</p></blockquote>
<p>read.html 파일을 열어서 위 내용을 적당한 곳에 넣으면 된다(난 이전 글, 다음 글 길잡이 영역 바로 위에 두었다).</p>
<p>우리는 지난 번에 템플릿 안에서 for 반복문이나 if조건문을 표현하는 걸 익혔다. 이번엔 include 표현식을 썼는데, 이름에서 알 수 있듯이 <strong>include 문은 다른 템플릿 파일을 가져와서 포함</strong>시킨다. 변수가 아닌 표현식이므로 &#123;&#37; 과 &#37;&#125; 로 감쌌다.</p>
<p>이 include 문은 읽어올 파일 경로를 지정할 수 있다. 예를 들어, comment_form.html 을 templates 폴더 속 또 다른 폴더인 comment 라는 폴더에 넣었다면</p>
<blockquote><p><code>&#123;&#37; include 'comment/comment_form.html' &#37;&#125;</code></p></blockquote>
<p>이렇게 해당 경로를 써넣으면 된다.</p>
<h4>댓글 입력받기</h4>
<p>views.py 안에 있는 def add_comment(request): 에 댓글 입력 받는 기능을 구현할 차례이다. 글쓴이 이름, 비밀번호, 본문 모두 꼭 입력해야 하므로 def add_post 함수에서 값 검사하는 부분을 따다 쓰자.</p>
<blockquote><pre><code># 글쓴이 이름 처리
if request.POST.has_key('name') == False:
    return HttpResponse('글쓴이 이름을 입력해야 한다우.')
else:
    if len(request.POST['name']) == 0:
        return HttpResponse('글쓴이 이름을 입력해야 한다우.')
    else:
        cmt_name = request.POST['name']

# 비밀번호
if request.POST.has_key('password') == False:
    return HttpResponse('비밀번호를 입력해야 한다우.')
else:
    if len(request.POST['password']) == 0:
        return HttpResponse('비밀번호를 입력해야 한다우.')
    else:
        cmt_password = md5.md5(request.POST['password']).hexdigest()

# 댓글 본문 처리
if request.POST.has_key('content') == False:
    return HttpResponse('댓글 내용을 입력해야 한다우.')
else:
    if len(request.POST['content']) == 0:
        return HttpResponse('댓글 내용을 입력해야 한다우.')
    else:
        cmt_content = request.POST['content']</code></pre>
</blockquote>
<p>이렇게 구현했다. 좀 더 깔끔한 방법은</p>
<blockquote><pre><code># 글쓴이 이름 처리
cmt_name = request.POST.get('name', '')
if not cmt_name.strip() :
   return HttpResponse('글쓴이 이름을 입력해야 한다우.')

# 비밀번호
cmt_password = request.POST.get('password', '')
if not cmt_password.strip() :
    return HttpResponse('비밀번호를 입력해야 한다우.')
cmt_password = md5.md5(cmt_password).hexdigest()

# 댓글 본문 처리
cmt_content = request.POST.get('content', '')
if not cmt_content.strip() :
    return HttpResponse('댓글 내용을 입력해야 한다우.')
</code></pre>
</blockquote>
<p>이렇게 구현할 수 있다(<a href="http://blog.hannal.com/05_3-python_django_lecture/#comment-6529">스파이크님 의견 참조</a>. 좋은 조언 고맙습니다. :)).</p>
<p>위 코드 중 별 다른 부분은</p>
<blockquote><p><code>cmt_password = md5.md5(request.POST['password']).hexdigest()</code></p></blockquote>
<p>이 코드일 것이다. <a href="http://ko.wikipedia.org/wiki/MD5">md5 는 암호화 모듈</a>(128비트 해시(hash)를 제공하는 암호화 해시 함수인데 이런 어려운 말은 관심있는 사람이 따로 알아보길 권한다)이다. 이 함수에 <strong>md5</strong> 라는 메소드가 있고, 이 메소드로 만든 객체엔 md5 해니 객체를 문자열로 표현해주는 <strong>hexdigest 메소드</strong>가 있다. 이 메소드는 길이가 32글자(byte)인 16진수 문자열로 표현한다. 이를테면</p>
<blockquote><p><code>md5.md5('hannal').hexdigest()</code></p></blockquote>
<p>라고 하면</p>
<blockquote><p>0b01fc13f1d6c3aa5aada3c0f79c4a6e</p></blockquote>
<p>이런 비슷한 문자열로 암호화 된다. 이 md5 모듈을 쓰려면 md5 모듈을 가져와야(import) 한다. views.py 파일 맨 위에</p>
<blockquote><p>import md5</p></blockquote>
<p>라고 해도 되고, 댓글 달 때만 필요하니까 add_comment 함수 안에서 import md5 해도 된다. 이 글에서는 views.py 파일 맨 위에서 import 했다.</p>
<p>이번엔 댓글 달 글 정보를 가져오자. 글 쓸 때 글 갈래 정보(Categories) 가져오는 방법대로 가져오면 된다.</p>
<blockquote><pre><code># 댓글 달 글 확인
if request.POST.has_key('entry_id') == False:
    return HttpResponse('댓글 달 글을 지정해야 한다우.')
else:
    try:
        entry = Entries.objects.get(id=request.POST['entry_id'])
    except:
        return HttpResponse('그런 글은 없지롱')</code></pre>
</blockquote>
<p>더 건드릴 것이 없으니 저장을 하면 된다.</p>
<blockquote><pre><code>try:
    new_cmt = Comments(Name=cmt_name, Password=cmt_password, Content=cmt_content, Entry=entry)
    new_cmt.save()
    return HttpResponse('댓글 잘 매달았다, 얼쑤.')
except:
    return HttpResponse('제대로 저장하지 못했습니다.')
return HttpResponse('문제가 생겨 저장하지 못했습니다.')</code></pre>
</blockquote>
<p>글 써넣는 과정과 거의 같아서 설명을 많이 생략했지만 여러분은 별 어려움 없이 구현했으리라 본다.</p>
<h3>댓글 보기</h3>
<h4>댓글들 가져오기</h4>
<p>댓글은 글 낱장보기에서 나타내므로 컨트롤러에서는 def read 안에서 처리하면 된다. 꽤 간단하다. 글 목록 가져오듯이 댓글도 가져오면 된다.</p>
<blockquote><p><code>comments = Comments.objects.filter(Entry=entry_id).order_by('created')</code></p></blockquote>
<p>위 코드는 Comments 모델에서 Entry 요소(프로퍼티)와 이용자가 보고 있는 글 일련번호(id)가 같은 <strong>모든</strong> 자료를 가져온다.</p>
<p>모델에서 자료를 가져올 때 all 메소드와 get 메소드로 가져오긴 했어도 filter 메소드로 가져오는 건 이번이 처음이다. 모양새는 get 과 비슷한데, get 과 다른 점이라면 <strong>filter 는 조건에 해당하는 자료를 여러 개 가져온다</strong>. get 메소드는 1개 <strong>가져오거나 가져오지 못한다면</strong> filter 메소드는 <strong>0개 이상을 list 자료형으로</strong> 반환한다. 그래서 get 메소드로 자료를 가져올 때는 가져올 조건에 해당하는 자료가 없는 경우를 대비해서 예외 상황 처리(try, except 문)를 했지만, filter 는 조건에 속하는 자료가 없으면 0개 자료를 반환하므로 따로 예외 상황 처리를 하지 않았다.</p>
<p>좀 더 정석대로(?) 코드를 구현하면 이렇다.</p>
<blockquote><p><code>comments = Comments.objects.filter(Entry=current_entry).order_by('created')</code></p></blockquote>
<p>글번호로 조건을 걸어 자료를 가져오는게 아니라 가져온 글 객체를 조건으로 건 것이다. 그리고 current_entry 객체를 제대로 가져오지 못한 경우를 대비해서, 그러니까 current_entry = Entries.objects.get(id=int(entry_id)) 이 코드에서 자료가 없어 예외 상황 오류가 발생하는 걸 막기 위해 예외 상황 처리문을 써넣어야 한다.</p>
<blockquote><pre><code>try:
    current_entry = Entries.objects.get(id=int(entry_id))
except:
    return HttpResponse('그런 글이 존재하지 않습니다.')</code></pre>
</blockquote>
<h4>가져온 댓글 출력하기</h4>
<p>이번엔 위에서 가져온 댓글을 출력할 차례이다. comments.html 이라는 템플릿 파일을 따로 만들어서 출력할테니 <strong>templates 폴더에 comments.html 파일을 만들고</strong> 다음과 같이 파일 내용을 채운다.</p>
<blockquote><pre><code>&#123;&#37; if comments|length &#37;&#125;
&lt;ul&gt;
&#123;&#37; for comment in comments &#37;&#125;
	&lt;li&gt;{{comment.Name}}님이 {{comment.created}}에 남긴 댓글
	&lt;p&gt;{{comment.Content}}&lt;/p&gt;&lt;/li&gt;
&#123;&#37; endfor &#37;&#125;
&lt;/ul&gt;
&#123;&#37; else &#37;&#125;
댓글이 없습니다.
&#123;&#37; endif &#37;&#125;</code></pre>
</blockquote>
<p>생김새는 list.html 과 비슷하다. comments 치환 변수에서 for 반복문으로 하나씩 자료를 꺼내어 comment 에 담고, 이 comment 를 출력하는 것이다. Name 이나 Content 는 Comments 모델을 참조하면 된다.</p>
<p>그런데 if 조건문을 보면 <strong>|length</strong> 라는 부분이 눈에 띈다. 이 <strong>length는 django 템플릿 기능에 내장된(built-in) 필터(filter)</strong>이다. <strong>| 는 파이프</strong>라고 하는데 Linux/Unix 를 어느 정도 다룬 사람이라면 익숙할 것이다.</p>
<p>개념으로는 이렇다. 파이프 앞에 있는 걸 A 라고 하고 파이프 뒤에 있는 걸 B 라고 하면, 파이프를 타고 A를 B로 보내주어 B가 그 내용물을 받아 처리하는 것이다. 마치 B함수에 인자로 A를 보내는 느낌이다. 그러므로 comments|length 는 comments 라는 개체를 length 라는 필터로 보내서 어떤 처리를 하라는 것이다.</p>
<p><strong>length 필터는 문자열을 던져주면 그 문자열의 길이를 반환하고, list 자료형을 던져주면 그 자료형의 방 개수를 반환</strong>한다. 만약 comments|length 에서 comments 가 단순히 hannal 이라는 문자열이라면 6이 반환되고, comments 라는 방이 1023개인 list 자료형이라면 1023이 반환된다.</p>
<p>그럼 왜 필터가 필요할까? 저런 처리를 굳이 템플릿에서 처리하지 않고 컨트롤러(뷰)에서 처리할 수도 있는데 말이다. 답은 방금 문장에서 나왔다. <strong>컨트롤러에서 처리할 수도 있지만 템플릿에서 처리하는 것보다 불편한 경우가 있어서 템플릿에서 처리하고 싶은 건 처리할 수 있게</strong> 필터라는 걸 만든 것이다. length 처럼 문자열 길이를 알아내거나 앞에서부터 10글자만 잘라내거나 HTML tag를 싹 빼는 기능처럼 말이다. <a href="http://www.djangoproject.com/documentation/templates/#built-in-filter-reference">django에는 이런 내장 필터가 꽤 많으며</a>, 직접 만들어 쓸 수도 있다.</p>
<p>참고로 파이프는 우리말로 통, 배관 정도가 되겠고, 필터는 거름기가 되겠다. <strong>파이프를(배관) 통해 색깔이 있는 물을 흘려보내면 필터(거름기)가 색깔을 빼서 깨끗한 물로 만드는 모습을 상상</strong>하면 저 암호같은 문법이 한결 쉽게 머리 속에 그려질 것이다.</p>
<p>어쨌든 &#123;&#37; if comments|length &#37;&#125; 이 말은 comments 라는 list 자료형 변수에 방이 1개 이상인지 확인하는 것이다. 1개 이상이면 논리 조건에서 참(True)가 되므로 if 조건문을 충족하는 것이고, 0개이면 거짓(False)가 되므로 if 조건문을 충족하지 못한다. 즉, 댓글이 1개 이상 있으면 &lt;ul&gt; 과 &lt;/ul&gt;로 둘러쌓인 부분으로 댓글을 출력하고, 0개이면 &#123;&#37; else &#37;&#125; 라는 if 조건문에서 이외 조건(else) 단락 안을 실행하여 “댓글이 없습니다”라는 문자열을 출력한다.</p>
<p>이렇게 만든 comments.html 은 read.html 안에서 include 문으로 가져와 포함시키면 된다. 난</p>
<blockquote><p><code>&#123;&#37; include 'comment_form.html' &#37;&#125;</code></p></blockquote>
<p>이 부분 위에 넣어서</p>
<blockquote><p><code>&lt;div&gt;<br />
&#123;&#37; include 'comments.html' &#37;&#125;<br />
&#123;&#37; include 'comment_form.html' &#37;&#125;<br />
&lt;/div&gt;</code></p></blockquote>
<p>이렇게 만들었다.</p>
<p><img src="http://blog.hannal.com/assets/uploads/2008/07/entry_with_comments.png" alt="" title="" width="500" height="426" class="alignnone size-full wp-image-117" style="border: 1px solid #000;" /></p>
<p>이제 댓글을 남겨보고 댓글을 남긴 글을 열면 댓글이 잘 나온다.</p>
<h3>댓글 수 증가</h3>
<p>근데 뭔가 허전하지 않은가? 댓글을 넣고 꺼내는 건 다 했는데, 댓글을 남길 때마다 댓글을 남긴 글에 댓글 수를 1씩 더해주지 않았다. 그래서 댓글이 있는데도 글에 달린 댓글 수는 여전히 0일 것이다. 그러니 댓글을 달고 나면 글에 달린 댓글 수도 1씩 더해주자.</p>
<p>add_comment 함수를 보면 댓글 달 글이 있는 글인지 확인하려고</p>
<blockquote><p><code>entry = Entries.objects.get(id=request.POST['entry_id'])</code></p></blockquote>
<p>이런 코드를 썼다. 이 글에 있는 댓글 수를 올려 주면 된다. 댓글 수는 댓글이 잘 달렸을 때 달면 되니까 댓글을 저장하는 <strong>new_cmt.save()</strong> 아래에서 처리하면 된다.</p>
<blockquote><p><code>entry.Comments += 1<br />
entry.save()</code></p></blockquote>
<p>이렇게 하면 댓글을 남긴 글에 있는 Comments 요소(댓글 수)에 1을 더한 뒤 바뀐 정보를 갱신(save())한다.</p>
<blockquote><p><code>entry.Comments += 1</code></p></blockquote>
<p>코드는</p>
<blockquote><p><code>entry.Comments = entry.Comments + 1</code></p></blockquote>
<p>과 같다.</p>
<h3>댓글 지우기</h3>
<p>이 기능 만드는 것은 숙제이다. 이용자가 댓글을 남길 때 써넣는 비밀번호를 이용해서 댓글 지우는 기능을 스스로 만들어 보자. 물론, 관련 내용은 여기서 다룬다.</p>
<p>django 모델을 통해 자료를 가져오고 저장하는 걸 꽤 익힌 지금, 자료를 지우는 것 역시 어렵지 않다. 자료를 저장하거나 바꿀 때 모델 클래스를 할당 받은 뒤 그 모델에 있는 save 메소드(new_entry.save() 이런 식)를 이용하듯이, 지우는 것 역시 지우는 메소드를 이용하면 된다. 바로 <strong>delete 메소드</strong>이다.</p>
<p>1번 글을 지운다고 가정하자. 댓글 지우기 요청이 이뤄지면 우선 1번 글 정보를 가져와야 한다.</p>
<blockquote><p><code>del_entry = Entries.objects.get(id=지울글번호)</code></p></blockquote>
<p>그런데 비밀번호를 비교해야 한다. 비밀번호가 서로 맞아야 지울 수 있기 때문이다. 글 정보를 가져온 del_entry 객체에 있는 Password 요소(프로퍼티)를</p>
<blockquote><pre><code>if request.POST['입력한비밀번호'] == del_entry.Password:
    pass</code></pre>
</blockquote>
<p>비교해도 되지만, 모델을 통해 DB에서 글 정보를 가져올 때 아예 비밀번호도 비교를 해서 가져오도록 해보자.</p>
<blockquote><p><code>del_entry = Entries.objects.get(id=지울글번호, Password=request.POST['입력한비밀번호'])</code></p></blockquote>
<p>이 조건에 만족하는 자료가 있으면 글 정보를 가져와서 del_entry 에 담을테고, 없으면 예외 상황 오류가 발생할 것이다.</p>
<p>아참, 댓글을 저장할 때 비밀번호는 md5 모듈로 암호화를 한 걸 잊으면 안된다.</p>
<blockquote><p><code>del_entry = Entries.objects.get(id=지울글번호, Password=md5.md5(request.POST['입력한비밀번호']).hexdigest())</code></p></blockquote>
<p>글 정보를 잘 가져와 del_entry 에 담았다면 이제 글을 지우면 된다. 앞서 말한 delete 메소드를 쓰면 된다.</p>
<blockquote><p>del_entry.delete()</p></blockquote>
<p>이걸로 끝이다. 무척 간단하다. 자, 어떻게 구현을 해야 하는지 다 나왔다. 직접 코드로 반영하고 적용하는 건 스스로 해보자.</p>
<ul>
<li><a href='http://blog.hannal.com/assets/uploads/2008/07/hannal-cheong_5-3.zip'>이번 글에서 다룬 소스 압축파일</a></li>
</ul>
<hr />
<p>“<a href="http://blog.hannal.com/05_1-python_django_lecture/">청) 컨트롤러(뷰) - 글 목록과 글 보기 기능 (5편 1/3) - django 강좌</a>”과 “<a href="http://blog.hannal.com/05_2-python_django_lecture/">청) 컨트롤러(뷰) - 글 쓰기 기능 (5편 2/3) - django 강좌</a>” 글에서 숨가쁘게 많은 내용을 익힌 덕에 이번 글은 참 쉽게 강좌를 진행할 수 있었다. 이로써 우리는 블로그 기능 대부분을 만들었다. 좀 더 마무리 해야 할 부분이 많지만, 여기까지 진행했다면 스스로 해결할 수 있을 것이라 생각하기에 굳이 강좌 글 공간을 써가며 다루지는 않았다. 이를테면 글/댓글 수정 기능 말이다.</p>
<p>다음 글에서는 잠시 django 기능 구현에서 벗어나 HTML 과 CSS, 그리고 Javascript 를 다룬다. 한동안 파이썬을 익히다 다른 언어를 접하므로 혼란스러울 수 있겠지만, 웹 기획자에겐 더 실용성 있을 것이고 새내기 웹 개발자라면 템플릿과 컨트롤러 구성을 짜는 데 이해력을 높이는 기회가 될 것이다.</p>
<h3>django 모델의 save() 메소드</h3>
<p>django 모델에서 제공하는 save 메소드는 어떻게 글 새로 입력하는 것(INSERT)과 갱신하는 것(UPDATE)을 구분할까? 둘 다 save 메소드로 하는데 말이다. 난 입력하고 싶은데 혹시 save 메소드가 상황을 잘못 이해해서(?) 변경을 하진 않을까? 혹은 그 반대는?</p>
<p>우리가 실수하지 않는 이상 그럴 일은 없다. <strong>save 메소드는 우리가 값을 다룰 모델에서 쓰는 기본키(Primary key) 값으로 입력할 지 갱신할 지를 구분</strong>한다. 우리가 따로 모델 안에서 기본키를 지정하지 않았다면 기본키는 id 가 된다.</p>
<blockquote><p>entry = Entries.objects.get(id=3)</p></blockquote>
<p>id 가 3인 글이 있다면 위 entry 객체의 id, 그러니까 entry.id 는 3이 되어 있다. 이렇게 entry.id (모델 객체의 기본키 요소(프로퍼티))에 값이 들어가 있는 상태에서 save 메소드를 호출하면 갱신(Update)을 실행한다. 그런데 기본키 값이 없다면 입력(Insert)를 실행한다.</p>
<p>실제로 위와 같이 글을 직접 가져온 뒤 기본키 값을 없앤 뒤 save 메소드를 실행하면 그 글 내용을 갱신하지 않고 새로 입력한다.</p>
<blockquote><p>entry = Entries.objects.get(id=3)<br />
entry.id = None<br />
entry.save()
</p></blockquote>
<p>이러면 입력을 한다. 자, 여기서 연습 문제 하나 풀어보자. 위 설명을 이해했다면 참 쉽다. 1번 글을 있는 그대로 100번 복사해서 DB에 넣어보자.</p>
