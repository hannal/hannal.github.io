---
layout: post
status: publish
published: true
title: Django 템플릿에서 VariableDoesNotExist 예외 오류 대응하기
author:
  display_name: Kay
  login: Kay
  email: kay@hannal.net
  url: ''
author_login: Kay
author_email: kay@hannal.net
date: '2017-06-26 00:00:00'
categories: [devlife]
tags:
- django
- template
- 템플릿
permalink: "/2017/06/hello_2017/"
---

한 줄 요약 : Django 템플릿 엔진은 템플릿 필터에 대해서 항상 조용한 실패 처리(silent failure)를 하진 않는다.

---

Django Template은 없는 템플릿 변수나 템플릿 변수의 속성, 키, 색인이 없어도 오류 상황을 일으키지 않고 조용히 오류 상황을 잠재운다. 일명 Silent failure 동작이다.

```
{% raw %}{{% endraw %}{% raw %}{{% endraw %} lorem.ipsum.hello.world }}
```

`lorem`이라는 템플릿 변수가 없든 `lorem` 템플릿 변수는 있는데 이 객체에 `ipsum`이라는 키나 속성이 없다고 가정하자. 최종 템플릿 맥락이 출력(치환)이면 Django는 변수나 키, 속성이 없다는 오류 상황을 일으키지 않으며, 저 템플릿 변수 위치엔 아무것도 출력되지 않는다. 템플릿 변수를 출력(render)하거나 템플릿 태그에서 사용할 때는 이처럼 Silent failure로 동작한다.

하지만 템플릿 필터를 거치는 경우엔 `VariableDoesNotExist` 예외(exception)가 발생한다. 예외 이름에서 드러나듯이 템플릿 변수가 없다는 뜻이다.

예를 들어, 존재하지 않는 템플릿 변수인 `not_exist_var`를 `divisibleby` 템플릿 필터에 사용하면 예외 오류가 발생한다.

```
{% raw %}{{% endraw %}{% raw %}{{% endraw %} '1234'|divisibleby:not_exist_var }}
```

이에 대해 Django 공식 문서에서는 다음과 같이 설명한다.

> Thus, filter functions should avoid raising exceptions if there is a reasonable fallback value to return. In case of input that represents a clear bug in a template, raising an exception may still be better than silent failure which hides the bug. ( 출처 : [custom-template-tags - writing-custom-template-filter](https://docs.djangoproject.com/en/1.11/howto/custom-template-tags/#writing-custom-template-filters) )

간단히 말해서 템플릿 필터 함수에서는 버그를 숨기는 Silent failure 보다는 예외를 일으키는 게 낫다고 한다. 실제로 Django 내장 템플릿 필터를 보면 대체물을 대신 반환해도 될 만한 경우엔 Exception 처리를 잡아내서 오류 상황을 피하지만, 그 외의 경우엔 Exception이 발생하게 냅둔다. 문제는 그 정책이 예상을 벗어나는 경우에 발생한다. 난 `default` 템플릿 필터에서 조용한 실패 처리를 하지 않는 상황을 만났다. `default` 템플릿 필터는 대개 다음과 같이 사용한다.

```
{% raw %}{{% endraw %}{% raw %}{{% endraw %} empty_var|default:'비었수다' }}
```

나도 비슷하게 사용했다.

```
{% raw %}{{% endraw %}{% raw %}{{% endraw %} apple.attr3|default:lemon.attrdict.color }}
```

Django 템플릿 엔진 동작에 익숙하다면 다음과 같이 동작하길 기대(예상)한다.

1. `apple.attr3`가 없으면 `lemon.attrdict['color']`를 대신 출력
2. `lemon.attrdict`에 `color` 키가 없으면 결국 아무것도 출력하지 않고 Silent failure.

딱히 Exception이 발생할만한 로직이 아니고, `default` 필터 함수를 봐도 인자 두 개 받아서 `return value or arg`로 동작하는 것 뿐이다. 다시 말해 `default` 템플릿 필터인 Python 함수는 두 개 인자를 받는데, 첫 번째 인자로 받는 `|` 앞에 있는 `apple.attr3`가 있으면 해당 객체를 반환하고, 없으면 두 번째 인자로 받는 `:` 뒤에 있는 `lemon.attrdict.color`를 반환한다.

하지만 실제로는 `VariableDoesNotExist` 예외 오류가 발생한다. 이 예외는 `with` 템플릿 태그로 해결하면 된다.

[with 템플릿 태그로 VariableDoesNotExist 예방](https://gist.github.com/hannal/9de33b54a749457d7f29c5f30c5e9136)

`with` 템플릿 태그는 Silent failure 처리를 해주니 `with` 템플릿 태그로 만든 임시 템플릿 변수인 `colour`엔 출력할(render) 게 없는 빈 객체가 할당이 된다. `with` 템플릿 태그를 안 쓴 경우엔 Silent failure를 해주는 놈이 없다보니 Exception이 그대로 나버린 것이다. 이 문제가 까다로운 이유는 Django 디버깅 화면에서는 문제가 있는 템플릿 줄(line)을 가리키지 않고 Exception이 발생한 Django 소스를 보여주는 데 있다. 평범한 속성명이나 키 이름을 쓰다가는 고생하기 십상이다.

흔히 겪는 상황은 아닐 것 같다. 나는 모델에 `JSONField`를 썼고, 이 모델필드의 `dict` 객체에 특정 키(위 예제 기준으로는 `colour`키)가 없어서 발생한 거였다. 뷰 함수에서 넘겨주는 템플릿 변수 이름대로라면 금방 발견했을 것 같다.

삼천포 요약 : 변수 네이밍을 괴랄하게 하면 디버깅에 도움이 된다.
