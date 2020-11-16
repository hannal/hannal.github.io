---
layout: post
status: publish
published: true
title: django-filter용 NULLS LAST 정렬 필터.
author:
  display_name: Kay
  login: Kay
  email: kay@hannal.net
  url: 'https://blog.hannal.com'
author_login: Kay
author_email: kay@hannal.net
date: '2020-11-13 11:00:00'
categories: [devlife]
tags:
- django filter
- nulls last
- django
- postgresql
permalink: "/2020/11/nulls-last-ordering-for-django-filter/"
---

이 글 내용은 PostgreSQL을 기반으로 하며, 다른 RDBMS 에서는 확인 안 해봤다.

### Django ORM으로 NULLS LAST 정렬

`Hannal`이라는 모델이 있고 `name` 필드는 `null`을 허용한다.

```
class Hannal(models.Model):
    name = models.CharField(max_length=32, null=True, blank=True)
    birthday = models.DateField(null=True, blank=True)
```

여러 데이터의 `name` 필드의 값이 다음과 같이 들어갔다고 가정하자.

- `'abc'`
- `'zyx'`
- `None`
- `'lmn'`

이제 `name` 필드로 오름차순(Ascending) 정렬해보자.

```
Hannal.objects.order_by('name')
```

- `None`
- `'abc'`
- `'lmn'`
- `'zyx'`

결과는 `null`이 먼저 나온다. 만약 `null`이 아닌 데이터를 먼저 정렬하고 그 이후에 `null`인 데이터를 나열하려면 `NULLS LAST`로 정렬해야 한다.

```
from django.db.models import F

Hannal.objects.order_by(F('name').desc(nulls_last=True))
```

- `'abc'`
- `'lmn'`
- `'zyx'`
- `None`

우리가 원하는 대로 정렬됐다.

### django-filter의 OrderingFilter

이번엔 이를 [django-filter](https://django-filter.readthedocs.io/)에 적용해보자. django-filter에 정렬 필터인 [OrderingFilter](https://django-filter.readthedocs.io/en/stable/ref/filters.html#orderingfilter)를 사용하면 간편하게 정렬 필터를 적용할 수 있다.

```
import django_filters as filters


class HannalFilter(filters.FilterSet):
    ordering = filters.OrderingFilter(
      fields=(
          ('name', 'name', ),
          ('birthday', 'saengil', ),
      ),
    )
```

자세한 내용은 공식 문서에 나와있지만, 영문 싫어하는 사람도 많고 공식 문서가 썩 친절하진 않으니 설명하겠다. 

`fields`로 모델의 필드와 HTTP Query String에서 넘겨 받을 필드를 짝지어야 한다. 뒤(`name`과 `saengil`)는 Query string으로 받을 값, 앞(`name`과 `birthday`)은 모델의 필드/필드표현식을 뜻한다. 보통은 모델 필드명과 동일하게 하는 게 알아보기 좋지만, 다르게 해야 할 경우도 많다.

우선 요청자(client)측에서 다른 이름을 쓰고 싶은 경우이다. Python 관례에 따르면 snake case 표기를 쓰겠지만, Front-end쪽에서는 camel case 표기를 대개 쓴다. 예를 들어 모델 필드명은 `birth_day`인데 요청자는 굳이 `birthDay`나 `saengil`을 쓰고 싶은 경우이다.

다른 경우가 더 흔한 경우인데, 모델 관계(model relationship)를 필드 표현식으로 다뤄야 하는 경우이다. 예를 들어 `Kay`라는 모델이 있고 `Hannal` 모델과 1:N 관계를 맺고 있다고 하자.

```
class Kay(models.Model):
    name = models.CharField(max_length=32, null=True, blank=True)


class Hannal(models.Model):
    name = models.CharField(max_length=32, null=True, blank=True)
    birthday = models.DateField(null=True, blank=True)
    related = models.ForeignKey('Kay', on_delete=models.CASCADE)
```

그리고 `Kay` 모델의 `name` 필드를 기준으로 정렬한 `Hannal` 모델의 데이터를 가져오려면 다음과 같이 한다.

```
Hannal.objects.order_by('related__name')
```

여기서 `related__name`을 django-filter 에서도 사용할 수 있다.

```
class HannalFilter(filters.FilterSet):
    ordering = filters.OrderingFilter(
      fields=(
          ('name', 'name', ),
          ('birthday', 'saengil', ),
          ('related__name', 'kayname', ),
      ),
    )
```

이렇게 `fields`를 지정하면 `ordering`키의 값으로 `name`, `saengil`, `kayname`을 사용할 수 있다. [Django REST framework](https://www.django-rest-framework.org/)(이하 DRF)에 적용하면 URL을 `?ordering=name`로 하여 `name` 필드에 대해 오름차순 정렬하거나 `?ordering=-name`로 하여 내림차순 정렬한다. 이 뿐만 아니라 여러 개 필드를 정렬할 수도 있다. 예를 들어 `name` 필드는 오름차순, `birthday` 필드는 내림차순으로 정렬하고자 한다면 HTTP Query String 으로 `?ordering=name,-birthday`라고 하면 된다.

### OrderingFilter에 NULLS LAST 적용

편하다. 근데 django-filter `2.4.0` 버전 기준까지는 `NULLS LAST`와 `NULLS FIRST`를 지원하지 않는다. 그러므로 따로 구현해야 한다. `OrderingFilter`가 제공하는 기능은 그대로 쓰되 QuerySet 만들 때 null 정렬만 추가하면 되니 이 클래스를 상속받아 쓴다.

```
from django.db.models import F
import django_filters as filters


class NullsLastOrderingFilter(filters.OrderingFilter):
    def filter(self, qs, value):
        if not value:
            return qs
        for _v in value:
            is_desc = _v.startswith('-')
            field = self.param_map.get(_v[1:] if is_desc else _v)
            if not field:
                continue
            if is_desc:
                qs = qs.order_by(F(field).desc(nulls_last=True))
            else:
                qs = qs.order_by(F(field).asc(nulls_last=True))
        return qs


class HannalFilter(filters.FilterSet):
    ordering = NullsLastOrderingFilter(
      fields=(
          ('name', 'name', ),
          ('birthday', 'saengil', ),
          ('related__name', 'kayname', ),
      ),
    )

    class Meta:
        model = Hannal
        fields = ('name', 'birthday', )
```

위에서 설명한 내용을 모두 반영한 코드이다. `filter()` 메서드는 django-filter 가 넘겨받은 QuerySet 객체이다. DRF에 연동해 사용한다면 DRF가 이런 저런 조치를 취한 QuerySet일테고, 사용자가 만든 filter 들을 거친 QuerySet이기도 하다.

`NULLS FIRST`를 적용하려면 `nulls_last` 대신 `nulls_first`를 `True`로 지정하면 된다.

