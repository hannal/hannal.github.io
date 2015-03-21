---
layout: post
status: publish
published: true
title: Python 3에서 함수의 위치 인자와 주석문
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
date: '2015-03-22 01:50:49'
categories: [devlife]
tags:
- Python 3
- 위치 인자
- positional argument
- 키워드 인자
- keywords argument
- annotation
permalink: "/2015/03/positioning_argument_and_annotations_for_python3/"
---

Python 3에 도입된 함수 선언 문법 중 위치 인자 개수를 지정하는 것과 주석문(`annotation`)이 있다. Python의 매력 요소 중 하나가 깔끔하고 명료한 코드라 생각하는데, 이 두 문법은 기호를 남발하는 코드처럼 보여서 좀 불만스럽지만 코드 문맥(context)을 읽는 데엔 참 유익하다. 그나마 `$` 기호가 사용되는 건 아니라서 다행이랄까?! :)

### 위치 인자 개수 지정

Python은 함수 매개 인자 방식으로 위치 인자(positional argument)와 키워드 인자(keywords argument)를 지원한다. 위치 인자는 함수로 전달하는 매개 인자를 순서대로 나열하는 것이고, 키워드 인자는 인자 이름과 인자에 할당할 값을 특정하는 것이다.

```
def args_func(arg1, arg2, arg3):
    print(arg1, arg2, arg3)

args_func('hello', 'world', '!')
args_func('!', arg3='hello', arg2='world')
args_func('world', arg3='!', arg2='hello')
args_func('hello', '!', arg3='world')
```

위 코드에서

- `args_func('hello', 'world', '!')`는 `hello world !`를 출력하고,
- `args_func('!', arg3='hello', arg2='world')`는 `! world hello`를 출력한다.
- `args_func('world', arg3='!', arg2='hello')`는 `world hello !`를 출력하며,
- `args_func('hello', '!', arg3='world')`는 `hello ! world`를

출력한다. 위치 인자, 키워드 인자 순서로 전달만 하면 어떤 인자를 위치 인자로 전달하고, 어떤 인자를 키워드 인자로 전달하는지에 별다른 제한은 없다.

Python 3는 키워드 인자를 강제하는 문법을 지원한다. 바로 `*` 문자를 쓰는 것이다.

```
def args2_func(arg1, *, arg2, arg3):
    print(arg1, arg2, arg3)

args2_func('hello', 'world', '!')
args2_func('!', arg3='hello', arg2='world')
args2_func('world', arg3='!', arg2='hello')
args2_func('hello', '!', arg3='world')
```

`*` 이후에 나열된 매개 인자는 반드시 키워드 인자로 전달돼야 한다. 위 코드에서 `args2_func` 함수를 실행하는 네 개 실행 줄 중 첫 번째와 네 번째는 `TypeError` 예외가 일어난다.

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: args2_func() takes 1 positional argument but 3 were given
```

즉, `args2_func` 함수는 위치 인자를 1개 취하는데, 이 개수보다 많은 인자가 위치 인자로 전달되었다는 뜻이다.

그렇다면 `*` 이전에 나열된 매개 인자를 키워드 인자로 값을 전달하면 어떻게 될까?

```
args2_func(arg1='hello', arg2='world', arg3='!')
```

아무 문제도 발생하지 않는다. 즉, `*`는 위치 인자 개수를 특정하거나(exact) `*` 앞에 나열된 인자를 위치 인자로 강제하는 것이 아니라 `*` 이후에 나열되는 인자는 반드시 키워드 인자로 전달 받도록 강제하는 것이다. 만약 위치 인자를 단 한 개도 허용하지 않고자 한다면 다음과 같이 함수 매개 인자를 선언하면 된다.

```
def kwargs_func(*, arg1, arg2, arg3):
    print(arg1, arg2, arg3)
```

이 함수는 모든 매개 인자를 키워드 인자로 전달해야 한다.


### 주석문 (annotation)

annotation 문법은 함수 매개 인자와 반환 값에 대한 주석(annotation)을 지정하는 것이다.

```
def anno_func(arg1: str, arg2: 'also str', arg3: 1 is True) -> bool:
    print(arg1, arg2, arg3)
```

표현된 코드를 보면 마치 인자의 형(`type`)을 지정하는 것 같지만, 실제로는 주석이기 때문에 인자의 형이 무엇이 되든 영향을 받지 않아서 다음과 같이 함수를 호출해도 아무 문제가 발생하지 않는다.

```
anno_func(1, True, 'world')
```

반환하는(return) 값의 형도 주석으로 설명한 것과 달라도 무방하다. `anno_func`은 주석으로 반환 값을 `bool`이라 명기했지만, 실제로는 `return`문이 따로 없기 때문에 `None` 값을 반환한다. 물론, 아무 문제도 없다.

이렇게 지정한 주석은 함수 객체에 `__annotations__` 속성에 담겨 있으며, 사전형(`dict`) 객체이다.

```
print(anno_func.__annotations__)
print(anno_func.__annotations__['arg1'])
```

재밌는 점은 `__annotations__`에는 주석으로 지정한 값(`value`)이 그대로 할당되어 있다는 점이다. 이 점을 이용하면 함수 매개 인자를 특정할 수 있다.

```
def static_args_func(arg1: str, arg2: str, arg3: int) -> bool:
    args = locals()
    for _k, _v in args.items():
        arg_type = static_args_func.__annotations__[_k]

        if isinstance(_v, arg_type):
            continue

        raise TypeError(
            "The type of '{}' does not match '{}' type".format(
                _k, arg_type.__name__
            )
        )
    print(arg1, arg2, arg3)

static_args_func(1, 2, 3)
```

`static_args_func` 함수의 `arg1`, `arg2` 인자는 주석으로 `str`형을 명기했다. 그래서 사전형 속성인 `__annotations__`의 `arg1`키에는 명기한 값인 `str`이 할당되어 있다. `__annotations__`에 할당되어 있는 주석 값을 이용해 `arg1`과 같은 함수 매개 인자의 형을(`type`) 검사한 것이 `if not isinstance(_v, arg_type):` 부분이다.

이 함수를 `static_args_func(1, 2, 3)`와 같이 호출하면 `arg1`과 `arg2`에 대해 코드에서 지정한 `TypeError` 예외가 일어난다.

다음 코드는 가변 매개 인자도 형 검사를 한다. 더이상 형 검사를 하지 않는 위치부터 나머지 인자까지는 `Ellipsis` 형(`...`)을 썼다. 즉, 두 번째 인자까지는 형 검사를 하고, 이후 인자는 형 검사를 생략한다.

```
def type_checking_func(*args: (int, int, ...)):
    annotations = type_checking_func.__annotations__

    if (
        not isinstance(annotations, dict) or
        len(annotations) == 0
    ):
        return type_checking_func(*args)

    try:
        _check_index = annotations['args'].index(Ellipsis)
    except ValueError:
        _check_index = len(annotations) - 1

    for i, _v in enumerate(args[:_check_index]):
        arg_type = annotations['args'][i]

        if isinstance(_v, arg_type):
            continue

        raise TypeError(
            "The type of '{}' does not match '{}' type".format(
                _v, arg_type.__name__
            )
        )
    print(*args)

type_checking_func(1, 2, '3', 'a')
```

인자의 형을 검사하는 기능을 장식자(`decorator`)로 만들어서 여러 함수에 간편하게 사용하면 더 낫다.

```
def check_argument_type(func):
    def wrapper(*args):
        annotations = func.__annotations__
        if (
            not isinstance(annotations, dict) or
            len(annotations) == 0
        ):
            return func(*args)

        try:
            check_index = annotations['args'].index(Ellipsis)
        except ValueError:
            check_index = len(annotations['args']) - 1

        for _i, _v in enumerate(args[:check_index]):
            _arg_type = annotations['args'][_i]

            if isinstance(_v, _arg_type):
                continue

            raise TypeError(
                "The type of '{}' does not match '{}' type".format(
                    _v, _arg_type.__name__
                )
            )
        return func(*args)
    return wrapper

@check_argument_type
def hello_func(*args: (int, int, ...)):
    print(*args)

hello_func(1, 2, '3', 'a')
```

Python스러운 구현인 지 아닌 지 모르겠지만, 함수 매개 인자가 어떤 자료형으로 넘어올 지 몰라서 받는 스트레스는 줄어들 것 같다. :)

* [Python 3에서 함수의 위치 인자와 주석문 예제 코드](https://gist.github.com/hannal/12597a1466307f4290a4)
