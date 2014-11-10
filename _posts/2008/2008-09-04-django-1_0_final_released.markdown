---
layout: post
status: publish
published: true
title: "드디어 Django 1.0 정식판이 나왔습니다."
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 1275
wordpress_url: http://blog.hannal.com/?p=1275
date: '2008-09-04 19:43:00 +0900'
date_gmt: '2008-09-04 10:43:00 +0900'
categories:
- "essay"
tags:
- django
- "정식판"
- "파이썬"
permalink: "/2008/9/django-1_0_final_released/"
---
<p>오래도록 정식판 번호가 0.96에 머물러 있던 <a href="http://www.djangoproject.com">Django</a>가 방금 전에 <a href="http://www.djangoproject.com/weblog/2008/sep/03/1/">1.0 최종판을 정식 발표</a>했네요.</p>
<p>저야 진작 0.97시험판이나 1.0시험판을 써오던 터라 큰 불편없지만, 혹 0.96판을 쓰던 이라면 <a href="http://docs.djangoproject.com/en/dev/releases/1.0-porting-guide/">0.96판을 기반으로 하는 자신의 코드를 1.0으로 손보는 안내 문서</a>를 참조하면 무난히 처리할 수 있습니다.</p>
<p>바뀐 부분이 꽤 많습니다. 가장 반가운 점은 Django 내부에서 문자열 처리는 유니코드로 통일한 점입니다. 예를 들면 0.96에서는 UTF-8 문자열을 받고, 이를 모델로 넘겨서 DB에 넣을 때는 encode('utf-8') 메소드를 이용하여 ASCII 문자열로 인코딩한 UTF-8 문자열을 넣어야 했습니다. 그러나 이제는 그냥 바로 넣으면 됩니다. 또 이와 관련하여 모델에서 쓰던 __str__ 메소드도 없어지고 __unicode__ 만 쓸 수 있습니다.</p>
<p>두 번째는 폼(forms)이 newforms 로 바뀐 점입니다. 이전까지는 0.96에써 쓰던 forms 를 oldforms 로 쓸 수 있었는데 이젠 아예 없어졌습니다.</p>
<p>세 번째는 admin mode 입니다. 예전엔 admin mode 를 사용하려면 models.py 등을 건드려서 코드가 이곳 저곳에 흩어졌는데, 이제는 admin.py 으로 정리하면 됩니다. 그래서 admin mode 를 열고 웹에서 접근하는 방법(urls.py 등)이 꽤 바뀌었습니다.</p>
<p>다국어 환경(i18n) 기능 중 po 파일과 mo 파일 만드는 방법도 살짝 바뀌었습니다. 예전에는 make-messages.py 가 po 파일을 만들고, compile-messages.py 가 mo 파일을 만들었는데, 이젠 이 두 파일에 있던 기능이 django-admin.py 안으로 들어갔습니다. django-admin.py  makemessages 나 compilemessages 라고 하면 됩니다. 근데 make messages는 여전히 파일 확장자가 py 인 것만 잘 추려내서 po 파일로 만드네요. -e html 이렇게 추가 확장자를 넣어도 html 것은 잘 안됩니다. django 쪽 문제는 아닌 것 같지만요. :)</p>
<p>request 객체도 눈 여겨볼 만 합니다. request 객체는 urls.py 에 연결해놓아서 이용자 요청을 처리해주는 view 콜백함수(callback)에 인자로 넘어오는데, 예전엔 별 기능이 없었습니다. 그러나 0.97부터는 이용자가 ajax 방식으로 접근한 것인지를 확인할 수 있는 is_ajax 메소드 등 편리한 기능이 많이 생겼지요. 이용자가 form 으로 올리는 파일을 받아 처리하는 <a href="http://docs.djangoproject.com/en/dev/topics/http/file-uploads/">파일 업로드</a> 부분도 깔끔하게 정리 됐습니다.</p>
<p>재밌는 건 <a href="http://geodjango.org">GeoDjango</a> 라는 모듈입니다. 예전엔 몰랐는데 이런 게 들어가서 /django/contrib/gis 에 예쁘장하게 자리 잡고 있군요. GeoDjango는 간단히 말해서 위치(Geographic) 정보를 다루는 <a href="http://en.wikipedia.org/wiki/Geographic_information_system">GIS</a>를 지원하는 추가 모듈입니다. 예전에 지도와 연동되는(mash up) 시험용 서비스를 만든 적이 있는데, 위치값 처리가 영 귀찮았습니다. 근데 이런 걸 처리해주니 한결 편해지겠네요. <a href="http://geodjango.org/docs/db-api.html#distance-queries">거리 관련 쿼리(Distance queries)</a> 내용을 보시면 쓰임새를 바로 이해하실 수 있습니다. 제 관심사와도 맞는 모듈이라 무척 흥미롭습니다.</p>
<p>이외에도 많은 부분이 개선되고 추가 되었습니다. 단지 제가 0.96을 안쓴 지 좀 돼서 비교를 더 하기 힘들군요. ^^;</p>
<p>아쉬운 점은 이번에도 다중 DB 접속 기능이 안들어갔고, 모델 API로 group_by 가 안들어갔네요. 1.0시험판에도 들어가있지 않았기에 별 기대는 안했지만, 막상 정말로 추가 안된 걸 보니 아쉽긴 합니다.</p>
<p>0.96판도 꽤 좋았지만, 1.0판에 들어서면서 정말 편하고 강력한 모습으로 탈바꿈 했네요. 비록 0.96판을 기준으로 했지만, 실제 코드 작성은 0.97판을 기반으로 해서 1.0정식판에서도 강좌 내용에 별 영향이 없는 <a href="http://blog.hannal.com/01-python_django_lecture/">날로 먹는 Django 웹 프로그래밍 강좌</a>를 보며 보다 강력해진 Django 1.0을 써보세요. :) 추천합니다.</p>
