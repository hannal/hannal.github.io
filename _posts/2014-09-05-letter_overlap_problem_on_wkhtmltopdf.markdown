---
layout: post
status: publish
published: true
title: wkhtmltopdf 에서 글자 겹치는 문제
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
wordpress_id: 3132
wordpress_url: http://blog.hannal.com/?p=3132
date: '2014-09-05 00:34:40 +0900'
date_gmt: '2014-09-04 15:34:40 +0900'
categories:
- "essay"
tags: []
permalink: "/2014/9/letter_overlap_problem_on_wkhtmltopdf"
---
<p>wkhtmltopdf (QT 패치 적용한 버전)으로 html을 pdf로 변환할 때 글자가 겹치는 현상이 일어나는 원인.</p>
<h3>letter-spacing css 속성</h3>
<p>letter-spacing이 -1px 이하인 경우 글자가 의도한 것보다 많이 겹치거나 오히려 자간이 벌어진다. -0.5px 정도로 값을 올리면 문제 해결. 하지만 환경에 따라서 이 값은 변동될 수 있으니 letter-spacing 을 적절히 조절해봐야 함.</p>
<h3>text-indent css 속성</h3>
<p>text-indent 속성에 엉뚱한(?) 값을 넣어 글자를 안 보이게 하는 트릭(예 : text-indent: -999999px)은 wkhtmltopdf에서 통하지 않는다. 글자들이 이상하게 커져서 한 곳에 겹쳐지는 등 전혀 엉뚱한 문제가 발생한다.</p>
<p>해결법은 텍스트 자체를 별도 태그로 감싸서 출력을 안 시키거나 감춰야 한다.</p>
<p>&lt;div style="text-indent: -10000em;"&gt;&lt;span style="display: none;"&gt;감출 글자&lt;/span&gt;&lt;/div&gt;</p>
<p>또는</p>
<p>&lt;div style="text-indent: -10000em;"&gt;&lt;span style="visibility: hidden;"&gt;감출 글자&lt;/span&gt;&lt;/div&gt;</p>
<p>webkit renderer를 쓰는 safari와 chrome에서는 발견되지 않으며, wkhtmltopdf에서 발생. 이외에도 다양한 원인이 있겠지만, 현재까지 발견한 원인은 이 두 가지.</p>
