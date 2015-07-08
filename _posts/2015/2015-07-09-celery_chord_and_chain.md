---
layout: post
status: publish
published: true
title: Celery의 Subtask 기능을 이용하여 Chord와 Chain로 작업 분산해서 다루기
author:
  display_name: Kay
  login: Kay
  email: iam@hannal.net
  url: ''
author_login: Kay
author_email: iam@hannal.net
date: '2015-07-08 17:25:00 +0900'
date_gmt: '2015-07-09 2:25:00 +0900'
categories: [devlife]
tags:
- python
- celery
- asynchronous
- distributed
permalink: "/2015/07/celery_chord_and_chain/"
---

### 웹페이지 긁어오기

Python으로 웹페이지 열 곳을 긁어와서 하나로 합쳐 보겠습니다. Python HTTP library인 [requests](http://docs.python-requests.org/en/latest/)를 쓰면 아주 간단합니다.

```
import requests

def fetch_page_by_url(url):
    res = requests.get(url.format(i))

    if int(res.status_code / 100) == 2:
        return res.text

merged_text = []
for i in range(0, 10):
    result = fetch_page_by_url('http://localhost:8000/{}.html'.format(i))

    if result is not None:
        merged_text.append(result)

do_something(merged_text.join(''))
```

### Celery를 이용해 비동기 방식으로 긁어오기

차례대로 긁어오니 열 개 페이지를 모두 가져오기 전까지는 결과를(`do_something(merged_text.join(''))`) 확인하지 못합니다. [multiprocessing](https://docs.python.org/3/library/multiprocessing.html)을 이용해 여러 프로세스로 동시성을 확보해도 되지만, 분산 작업 큐 시스템인 [Celery](http://celery.readthedocs.org/en/latest/)로 쉽고 간편하게 비동기 처리하기도 합니다.

```
from celery import Celery

app = Celery(__name__)

@app.task
def fetch_page_by_url(url):
    res = requests.get(url.format(i))

    if int(res.status_code / 100) == 2:
        return res.text

merged_text = []
for i in range(0, 10):
    result = fetch_page_by_url.apply_async(
        'http://localhost:8000/{}.html'.format(i)
    )

    if result is not None:
        merged_text.append(result)

do_something(merged_text.join(''))
```

이 코드에는 문제가 있습니다. Celery 작업 수행 객체로 장식된(decorated) `fetch_page_by_url` 객체의 `apply_async()` 메서드를[^1] 이용하여 **비동기**로 작업을 수행하는데, 이 메서드가 반환하는 객체는 `res.text`가 아니라 Celery 결과 작업을 다루는 객체입니다. 게다가 비동기로 작업을 수행하고 바로 프로그램 수행 제어권을 호출자에게 반환하므로 `fetch_page_by_url.apply_async(...)` 호출이 되자마자 바로 다음 구문을 수행하는데, 웹 페이지를 가져오는 작업이 끝났는지 여부는 알지 못 합니다.

이 문제를 피하려면 `get()` 메서드를 이용합니다.

```
    if result.get() is not None:
        merged_text.append(result)
```

`get()` 메서드는 비동기로 수행하는 작업 객체(`fetch_page_by_url()`)가 작업을 마치고 값을 반환하기를 **동기식**으로 기다려서 반환합니다. 어?! 이렇게 할 거라면 굳이 Celery를 쓸 필요가 없지요. Celery에게 여러 작업을 맡겨서 비동기로 처리하고, 비동기로 처리한 결과를 받아다 뭔가를 하려면 다른 방법을 써야 합니다. 이 글에서는 `chain`와 `chord`을 사용하겠습니다.

### chain 기능

`chain` 기능은 이름에서 전해지듯이 작업을 체인처럼 줄줄이 수행합니다.

```
from celery import chain

@app.task
def fetch_page_by_url(url, append_text=None):
    res = requests.get(url.format(i))

    if int(res.status_code / 100) == 2:
        if append_text is None
            return res.text
        else:
            res.text + append_text

tasks = []
for i in range(0, 10):
    tasks.append(
        fetch_page_by_url.subtask(
            'http://localhost:8000/{}.html'.format(i)
        )
    )

result = chain(tasks)()
```

`subtask()`는 Celery 작업 객체를 하위 작업으로 수행하는 메서드입니다[^2]. `fetch_page_by_url` 객체를 하위 작업으로 수행하는 작업 열 개를 담아 `chain()`에 전달하면 `chain()`은 순서대로 작업을 수행합니다. 각 작업이 반환하는 객체는 다음 작업자에게 인자로 전달합니다. 첫 번째 `fetch_page_by_url()` 함수가 반환하는 웹페이지 문자열을 두 번째 `fetch_page_by_url()`는 두 번째 인자로 받는 것이죠. 그래서 두 번째 `fetch_page_by_url()`부터는 앞 작업자가 반환하는 결과를 넘겨 받는 것이지요.

다른 예를 들어 보겠습니다. 숫자 두 개를 인자로 전달하면 두 숫자를 더하는 작업자를 쓰겠습니다.

1. 첫 번째 셈은 1 + 1 입니다.
2. 두 번째 셈은 첫 번째 덧셈 결과를 받아서 10을 더합니다.
3. 세 번째 셈은 두 번째 덧셈 결과를 받아서 100을 더합니다.

이걸 `chain()`을 이용하면 다음과 같습니다.

```
do_chain_tasks = chain(add.s(1, 1), add.s(10), add.s(100))
do_chain_tasks()
```

`chain()`도 바로 작업을 수행하는 게 아니라 Celery 작업 객체를 반환하며[^3], 이 작업 객체를 실행해야 합니다. 바로 위 코드는 `chain(...)()`라는 구문을 나눈 것입니다.

재밌는 점은 Celery는 비트 연산으로도 `chain()` 작업 객체를 만들어 준다는 점입니다.

```
(
    fetch_page_by_url.s('http://localhost:8000/0.html') |
    fetch_page_by_url.s('http://localhost:8000/1.html') |
    fetch_page_by_url.s('http://localhost:8000/2.html') |
    fetch_page_by_url.s('http://localhost:8000/3.html')
).apply_async()
```

참 꼼꼼하게 만들어 놨어요. :)

### chord 기능

`chain()`을 이용해 비동기로 열 개 작업을 수행하고 그 결과를 합쳤는데, 아쉬운 마음이 듭니다. 전체 작업 자체는 분명 비동기로 시작한 게 맞지만, 웹페이지를 긁어오는 작업도 동시에 분산해서 처리하면 더 효율이 좋을 겁니다. `chord()`는 하위 작업을 동시에 수행하고, 각 작업자가 반환하는 값을 callback 실행 객체로 전달해줍니다.

```
from collections import MutableSequence
from celery import chord

@app.task
def fetch_page_by_url(url):
    res = requests.get(url.format(i))

    if int(res.status_code / 100) == 2:
        return res.text

@app.task
def merge_text(texts):
    assert(isinstance(texts, MutableSequence))
    return texts.join('')

tasks = []
for i in range(0, 10):
    tasks.append(
        fetch_page_by_url.s('http://localhost:8000/{}.html'.format(i))
    )

do_chain_tasks = chord(tasks)
do_chain_tasks(merge_text.s())
```

`fetch_page_by_url()` 함수가 원래대로(?) 돌아왔고, `merge_text()` 함수가 새로 추가됐습니다. `merge_text()`는 전달받은 인자 `texts`를 합치는 일을 하는데, `fetch_page_by_url()`가 반환하는 문자열을 담은 리스트형(`list`) 객체입니다. 맨 처음에 비동기로 작성한 코드에서 웹페이지 문자열을 리스트로 담은 `merged_text`와 같습니다.

`chord()`는 각 작업자(`fetch_page_by_url()`)가 반환하는 값을 리스트형으로 모아서 callback 객체에게 인자로 전달합니다. `chord()`로 만든 Celery 작업 객체로 callback 객체를 전달할 때 인자를 지정하지 않아도 됩니다. 알아서 넣어 줍니다.

근데 이 코드엔 사소하다면 사소하고 심각하다면 심각한 문제가 있습니다. 작업들을 비동기로 수행하다보니 웹페이지 문자열이 우리가 원하는 순서대로 담겨져 `merge_text()`로 전달된다는 보장이 없습니다. 작업이 먼저 끝나는 순서대로 결과가 담기니 0 - 1 - 2 - 3 ... 순서가 될 지 9 - 4 - 7 - 1 순서가 될 지는 아무도 모릅니다.

여러 해결책이 있겠지만, 각 작업자마다 순번을 주고, `merge_text()`는 이 순번대로 문자열을 합치면 되겠네요.

```
@app.task
def fetch_page_by_url(url, num):
    res = requests.get(url.format(i))

    if int(res.status_code / 100) == 2:
        return res.text, num

@app.task
def merge_text(texts):
    assert(isinstance(texts, MutableSequence))
    texts.sort(key=lambda x: x[1])
    return texts.join('')

tasks = []
for i in range(0, 10):
    tasks.append(
        fetch_page_by_url.s(
            'http://localhost:8000/{}.html'.format(i), i
        )
    )
```

각 `fetch_page_by_url()`에 두 번째 인자로 순번(`i`)을 전달하고, `fetch_page_by_url()`는 받은 순번을 웹페이지 문자열과 함께 그대로 반환합니다. `merge_text()`가 전달받은 `texts`엔 각 `fetch_page_by_url()` 결과가 `[(문자열, 0), (문자열, 3), ...]` 형태로 담깁니다. 그래서 각 항목의 두 번째(`[1]`) 값으로 정렬하고 나서 한 문자열로 합친 것입니다.


- [Canvas: Designing Workflows : The primitives](http://celery.readthedocs.org/en/latest/userguide/canvas.html#the-primitives)
- [Tasks : Avoid launching synchronous subtasks](http://celery.readthedocs.org/en/latest/userguide/tasks.html#avoid-launching-synchronous-subtasks)

----

[^1]: 대개는 `delay()`라는 메서드로 줄여서 수행합니다.

[^2]: 대개는 `s()`로 줄인 메서드 이름을 씁니다.

[^3]: `chain`과 `chord`는 함수처럼 생겼지만 클래스입니다.
