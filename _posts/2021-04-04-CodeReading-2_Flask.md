---
title: "CodeReading - 2. Flask"
layout: single
classes: wide
date: 2021-04-04 10:00

category: 
    - Code Reading

tag:
    - code reading
    - framework
    - python
    - flask
    - web application

toc: true
toc_label: "Table of Contents"
toc_icon: "list-alt"
toc_sticky: false
---

> Code Reading은 잘 작성되어 있는 프레임워크, 라이브러리, 툴킷 등의 다양한 프로젝트의 내부를 살펴보는 시리즈 입니다. 프로젝트의 아키텍처, 디자인철학이나 코드 스타일 등을 살펴보며, 구체적으로 하나하나 살펴보는 것이 아닌 전반적이면서 간단하게 살펴봅니다. 

## Series.

- [CodeReading - 1. PyTorch](https://dongjunlee.github.io/code%20reading/CodeReading-1_PyTorch/)
- [CodeReading - 2. Flask](https://dongjunlee.github.io/code%20reading/CodeReading-2_Flask/)

---

 [코드읽기 1편](https://dongjunlee.github.io/code%20reading/CodeReading-1_PyTorch/)에 이어서 이번에 살펴보고자 하는 라이브러리는 Python Web 개발의 양대산맥 중 하나인 [Flask](https://github.com/pallets/flask)를 다뤄보고자 합니다. 코드 읽기를 추천하는 많은 사람들이 추천하는 라이브러리이기도 합니다!

 Flask를 소개하는 한줄의 문장은 다음과 같습니다.

> Flask is a lightweight WSGI web application framework.

 Flask가 lightweight 라고 말을 하는 이유는 사용법이 정말 간단하기 때문입니다. 아래 단 몇 줄의 코드 만으로 Web Application 하나를 만들어낼 수 있습니다.

```python
# save this as app.py
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello, World!"

# $ flask run
# > * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

 그렇다면 차근차근 Flask에 대해서 알아보도록 하겠습니다.


## WSGI

 코드를 읽기 전에 먼저 대략적인 개념들을 잡고가는 것이 도움이 될 것 같습니다. Flask는 Web 개발에 사용되는 프레임워크이며, WSGI(Web Server Gateway Interface)라는 통신규격을 지키고 있습니다. 이는 기존의 웹서버와 웹 애플리케이션 간의 제약사항을 허물어 주면서, 각각의 역할에 따라서 동작할 수 있도록 하였습니다.

![images]({{ site.url }}{{ site.baseurl }}/assets/images/code_reading-2_flask/Untitled.png){: .align-center}
<figcaption class="caption">출처: https://medium.com/analytics-vidhya/what-is-wsgi-web-server-gateway-interface-ed2d290449e</figcaption>

 이렇게 구조를 가져가면서 얻을 수 있는 장점은 주로 1**) 성능 향상**과 **2) 확장성** 에 있습니다. 이 포스트에서는 코드를 읽는 것이 주이므로, 간단하게만 언급하고 넘어가도록 하겠습니다.

 위 인터페이스의 규격은 아래의 코드로 확인할 수 있습니다. `Request` 라는 `Callable Object` 를 Web Application에서 받아서 `Response` 라는 결과를 반환하는 것입니다.
(아래의 예제 코드의 `werkzeug` 는 `flask` 의 코어가 되는 라이브러리로, 같은 개발자가 만들었습니다.)

```python
from werkzeug.wrappers import Request, Response

@Request.application
def application(request):
    return Response('Hello, World!')

if __name__ == '__main__':
    from werkzeug.serving import run_simple
    run_simple('localhost', 4000, application)
```


## The Pallets Projects

 현재 flask는 [The Pallets Projects](https://palletsprojects.com/) 라는 Organization에서 관리가 되고 있습니다. 이 페이지에 들어가 보시면,  사용해봤을 법한 몇가지 유명한 라이브러리들이 같이 만들어졌음을 알 수 있습니다.

- [click](https://github.com/pallets/click) :  Command Line Interface Creation Kit 의 약자로서, 간단하게 cli 를 조작할 수 있습니다. (10.6k Star)
- [jinja](https://github.com/pallets/jinja) :  HTML 템플릿 엔진으로, 다양한 기능들과 Python과 비슷한 syntax 그리고 데이터 연결이 가능하게 해줍니다. (7.6k Star)
- [werkzeug](https://github.com/pallets/werkzeug) : WSGI Web 개발에 핵심이 되는 라이브러리 (5.7k Star)

 위의 3가지 프로젝트가 대표적이죠. flask 뿐만 아니라, 어디선가 봤던 유명한 라이브러리들이 포함되어 있다는 것이 신기하였습니다. 또한 이 모든 프로젝트들이 [Armin Ronacher](https://palletsprojects.com/people/mitsuhiko/) 에 의해서 만들어졌다는 것 입니다. 개발자들의 생산성은 개개인마다 굉장히 차이가 크다는 것은 실험 결과를 통해 입증되었지만, 이렇게 개인이 만든 라이브러리들이 전세계적으로 사용되는 것을 보면 참 대단하다는 생각이 듭니다.

 만든이가 동일한 사람이기 때문에, 라이브러리의 성격들이 비슷하다. click과 flask를 보면, 사용성에 초점이 확 맞춰져 있는 것을 느낄 수 있다. 위 모든 프로젝트에 들어가있는 `A Simple Example` 을 보면 확인할 수 있는 점이기도 합니다.


## Flask

 (Flask 의 코드는 '1.1.2' Tag를 기준으로 살펴봅니다.)

 위에서 살펴본 것처럼, Flask는 The Pallets Projects의 다양한 라이브러리를 통해서 이루어져 있습니다. 간단하게는 [werkzeug](https://github.com/pallets/werkzeug)를 감싸서 만든 Web Application Framework 라고 할 수 있습니다.

```python
install_requires=[
    "Werkzeug>=0.15",
    "Jinja2>=2.10.1",
    "itsdangerous>=0.24",
    "click>=5.1",
],
```

### 디자인 철학

 프레임워크는 전반적으로 단순함을 기초로 하고 있습니다. 의존성을 최소로 하고 있는데, 흔히 Web 개발에 사용되는 DB 또한 내장되어 있지 않고 (Django는 postgreSQL을 기본으로 사용하도록 되어있습니다.) 최소한으로 템플릿은 사용하므로 위의 Jinja2를 템플릿 엔진으로 의존하고 있는 정도입니다. 확장은 사용자에게 달려있는 것이죠.

 그리고 명시적인 Application 객체를 사용하고 있습니다. 여기에 대해서는 다음 3가지 이유가 있습니다.

1. 암시적인 Application 객체가 한 번에 하나의 인스턴스만 있어야 한다는 것.
2. 패키지 이름과 연동하여 리소스 할당.
3. 일반적으로 명시적인 것이 암묵적인 것보다 낫기 때문.

 만약에 명시적인 Application 객체를 사용하지 않으면, 아래와 같은 코드가 될 것 입니다.

```python
from hypothetical_flask import route

@route('/')
def index():
    return 'Hello World!'
```

 Application의 범주를 정의하는 것이 모호할 것이며, 하나의 객체에 여러개의 application이 동작하면서 혼동을 줄 수도 있을 것 입니다. 그리고 명시적인 Application 객체는 최소한으로 기능을 할당해서 Unit test도 진행할 수 있는 장점이 있다고 저자는 말하고 있습니다.

### Code

 코드는 전반적으로 함수와 객체가 역할에 맞춰서 잘 나뉘어 있으며, Python 언어가 제공하는 기본 기능들을 최대한 활용하여 (Pythonic한), 가독성에 굉장히 신경을 쓴 것을 코드 전반적으로 느낄 수 있습니다. 다음은 몇가지 세부 코드들은 들어가서 살펴보겠습니다.

[**helpers.py**](http://helpers.py) : 다양한 유틸 함수 및 클래스를 선언

```python
# https://github.com/pallets/flask/blob/1.1.2/src/flask/helpers.py

def get_env():
    """Get the environment the app is running in, indicated by the
    :envvar:`FLASK_ENV` environment variable. The default is
    ``'production'``.
    """
    return os.environ.get("FLASK_ENV") or "production"

def get_debug_flag():
    """Get whether debug mode should be enabled for the app, indicated
    by the :envvar:`FLASK_DEBUG` environment variable. The default is
    ``True`` if :func:`.get_env` returns ``'development'``, or ``False``
    otherwise.
    """
    val = os.environ.get("FLASK_DEBUG")

    if not val:
        return get_env() == "development"

    return val.lower() not in ("0", "false", "no")

def url_for(endpoint, **values):
    ...

def get_template_attribute(template_name, attribute):
    ...

def total_seconds(td):
    """Returns the total seconds from a timedelta object.
    :param timedelta td: the timedelta to be converted in seconds
    :returns: number of seconds
    :rtype: int
    """
    return td.days * 60 * 60 * 24 + td.seconds
```

 위 코드들에서 확인할 수 있는 것처럼, 자주 사용하는 유틸리티성의 함수들이 직관적인 네이밍과 docstring과 함께 작성이 되어있습니다. docstring 에는 입력에 대한 type과 출력에 대한 type 또한 명시가 되어있습니다. 이 부분들은 Python 3.5 버전부터 추가된 [typing](https://docs.python.org/3/library/typing.html) 라이브러리를 활용하는 것도 가능하겠네요.

- [templating.py](https://github.com/pallets/flask/blob/1.1.2/src/flask/templating.py) : Jinja2를 통해서 템플릿 랜더링 **(코드 작성 방식)**

 이 부분은 템플릿 엔진이 동작하는 로직을 담고 있습니다. 여기서 다루고자 하는 부분은 코드의 작성 방식입니다. [<<클린코드>>](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=34083680) 에서는 코드를 위에서 아래로 보는, 신문 기사에 비유를 합니다. 먼저 핵심 요약을 위에서 다루고, 아래에서 세부적인 내용들을 다루는 것 입니다.

```python
def get_source(self, environment, template):
    if self.app.config["EXPLAIN_TEMPLATE_LOADING"]:
        return self._get_source_explained(environment, template)
    return self._get_source_fast(environment, template)

def _get_source_explained(self, environment, template):
    ...

def _get_source_fast(self, environment, template):
    ...
```

 Flask 역시 위에서 아래로 읽을 수 있도록 코드가 작성되어 있고, 객체지향에서 접근권한을 명시하는 방식 또한 적용되어 있는 것을 보실 수 있습니다. Python은 코드차원에서 이를 지원하지는 않지만, 암묵적인 규칙이 있습니다.

```python
def method_name(): # public
def _method_name():  # protected or private
def __method_name(): # private
```


[**wrappers.py**](https://github.com/pallets/flask/blob/1.1.2/src/flask/wrappers.py)  : werkzeug 를 감싸서 객체 선언 (객체의 확장방식)

 이 코드에서는 [1편 PyTorch](https://dongjunlee.github.io/code%20reading/CodeReading-1_PyTorch/)의 Function의 객체의 확장과 거의 같은 방식을 보실 수 있습니다.

```python
# https://github.com/pallets/flask/blob/1.1.2/src/flask/wrappers.py

from werkzeug.wrappers import Request as RequestBase
from werkzeug.wrappers import Response as ResponseBase

class JSONMixin(_JSONMixin):
    ...

class Request(RequestBase, JSONMixin):
    ...

class Response(ResponseBase, JSONMixin):
    ...
```

 바로 `Base` 객체를 두고 `Mixin` 을 통해서 인터페이스 및 속성을 확장해 나가는 방식입니다. (이 Mixin에 대해서는  [1편 PyTorch](https://dongjunlee.github.io/code%20reading/CodeReading-1_PyTorch/) 에서 간단하게 설명하고 있습니다.)

 한가지 재미있는 점은 Mixin 뿐만 아니라, Meta Class를 활용하는 방식 또한 PyTorch에서 봤던 것과 동일합니다. 

```python
# https://github.com/pallets/flask/blob/1.1.2/src/flask/_compat.py#L60

def with_metaclass(meta, *bases):
    """Create a base class with a metaclass."""
    # This requires a bit of explanation: the basic idea is to make a
    # dummy metaclass for one level of class instantiation that replaces
    # itself with the actual metaclass.
    class metaclass(type):
        def __new__(metacls, name, this_bases, d):
            return meta(name, bases, d)

    return type.__new__(metaclass, "temporary_class", (), {})
```

 이 `with_metaclass` 는 객체의 빌트인 함수들의 동작을 바꿔서, 원하는 방식대로 동작하는 객체를 만드는 데 사용이 됩니다. PyTorch에서 Function 객체를 위 메서드를 통해서 정의하고 있죠. (자세한 것은 [1편](https://dongjunlee.github.io/code%20reading/CodeReading-1_PyTorch/)을 참고해주세요.) 프로젝트 시기상 Flask가 훨씬 먼저 만들어지고, 이후에 PyTorch가 만들어졌기 때문에.. PyTorch의 메인테이너들은 Flask의 코드스타일을 비슷하게 따라갔고 몇가지 그대로 사용하는 케이스도 있었던 것으로 생각이 됩니다. Flask의 코드들이 가독성이 좋고 사용성 또한 직관적이였던 것처럼, PyTorch 역시 그럴마한 이유가 있었네요.


[**signals.py**](https://github.com/pallets/flask/blob/1.1.2/src/flask/signals.py) : Application이 동작하는 시그널을 감지

```python
# https://github.com/pallets/flask/blob/1.1.2/src/flask/signals.py#L54

# Core signals.  For usage examples grep the source code or consult
# the API documentation in docs/api.rst as well as docs/signals.rst
template_rendered = _signals.signal("template-rendered")
before_render_template = _signals.signal("before-render-template")
request_started = _signals.signal("request-started")
request_finished = _signals.signal("request-finished")
request_tearing_down = _signals.signal("request-tearing-down")
got_request_exception = _signals.signal("got-request-exception")
appcontext_tearing_down = _signals.signal("appcontext-tearing-down")
appcontext_pushed = _signals.signal("appcontext-pushed")
appcontext_popped = _signals.signal("appcontext-popped")
message_flashed = _signals.signal("message-flashed")
```

 이 부분은 동작에 대한 신호를 판별하는 부분으로, 거의 유일하게 Python의 내장 라이브러리와 자신이 만든 라이브러리를 제외하고 다른 사람이 만든 것을 활용하고 있습니다. 그리고 그 마저도 꼭 필요한 dependency는 아니기도 합니다. 

 라이브러리를 개발할때, 필요하다면 이와 같이 [Blinker](https://github.com/jek/blinker) 라는 dispatching system을 사용할 수 있을 것 같습니다.

**[werkzeug/datastructures.py**](https://github.com/pallets/werkzeug/blob/master/src/werkzeug/datastructures.py) : (번외로) werkzeug 에서 유틸리티처럼 정의한 데이터구조 클래스들

 이번 Flask 코드를 살펴보면서 자연스럽게 연결되어 있는 라이브러리에 대해서도 살짝 살펴보게 되었습니다. 그 중에서 한가지 참고하기에 가장 좋다고 생각되는 코드를 소개시켜드리고자 합니다.

```python
# https://github.com/pallets/werkzeug/blob/1.0.1/src/werkzeug/datastructures.py

class ImmutableListMixin:
    ...

class ImmutableList(ImmutableListMixin, list):
    ...

class OrderedMultiDict(MultiDict):
   ...

    def __eq__(self, other):
        ...

    def __getstate__(self):
        ...

    def __setstate__(self, values):
        ...
```

 위와 같은 방식으로 Mixin을 정의해서 필요한 데이터구조를 확장하기도 하고, 기본으로 사용하는 생성자 `__init__` 이외에도 `__eq__`, `__ne__`, `__getstate__`, `__setitem__`, `items`, `pop` 등의 기본 내장되어 있는 Built-in 함수들을 필요에 맞춰서 재구현한 것들을 볼 수 있습니다.

```python
# https://github.com/pallets/werkzeug/blob/1.0.1/src/werkzeug/datastructures.py#L919

class Headers(object):
    """An object that stores some headers.  It has a dict-like interface
    but is ordered and can store the same keys multiple times.
    This data structure is useful if you want a nicer way to handle WSGI
    headers which are stored as tuples in a list.
    ...
    """
```

 Request에 사용되는 Headers 또한 기본 Built-in 함수들을 직관적으로 사용할 수 있도록 재구현이 되어 있습니다. 이 코드들을 보다보니, PyTorch의 [Module](https://github.com/pytorch/pytorch/blob/v1.8.1/torch/nn/modules/module.py#L204) 클래스가 생각나기도 하네요!

 그 외에도 다양하게 decorator가 구현되고 직관적으로 사용되는 점들도 있으니, 코드를 살펴보시면서 참고하시면 좋겠습니다. ([@setupmethod](https://github.com/pallets/flask/blob/1.1.2/src/flask/app.py#L82) 예시)


## 끝으로

 이번에 Flask를 살펴보면서.. 한명의 개인이 전 세계적으로 사용되는 다양한 오픈소스를 만들어내는 것도 신기하였고, [좋은 코드를 읽는데 있어서](https://docs.python-guide.org/writing/reading/) Flask를 추천하는 것이 이해가 되었습니다. Pythonic한 코드스타일의 정말 좋은 표본이라고 생각하고 앞으로도 많이 참고를 하게 될 것 같습니다.

 그리고 위에서 PyTorch가 Flask에 영향을 정말 많이 받은 만큼, Flask를 먼저 살펴보고 PyTorch로 넘어가는 것도 괜찮았을 것 같네요!


## References

- [wsgi를 왜 쓰나요 - 조아하는모든것](https://uiandwe.tistory.com/1268)
- [What is WSGI (Web Server Gateway Interface)? - Medium](https://medium.com/analytics-vidhya/what-is-wsgi-web-server-gateway-interface-ed2d290449e)
- [PEP 3333 -- Python Web Server Gateway Interface v1.0.1](https://www.python.org/dev/peps/pep-3333/)
- [The Pallets Projects](https://palletsprojects.com/)
- [Flask Documentation](https://flask.palletsprojects.com/en/1.1.x/)