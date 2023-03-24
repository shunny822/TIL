# Template

## Template system

> 데이터 표현을 제어하며 표현과 관련된 로직 담당

### Django Template Language

: Template에서 조건, 반복, 변수, 필터 등의 프로그래밍적 기능을 제공하는 시스템

1. Variable
    - views에 정의된 render함수에서 세번째 인자로 딕셔너리 타입을 넘길 수 있음(context)

    - 딕셔너리의 key에 해당하는 문자열이 template에서 사용 가능한 변수명이 됨

    - template에서 `{{ variable }}`과 같이 중괄호 두개로 감싸서 사용

    - view에서 함수 정의 시 dot(.)을 이용해 변수 속성에 접근

2. Filters
    - 표시할 변수를 수정할 때 `{{ variable|filter }}`의 형태로 사용

    - chained 가능(filter 여러 개 붙여서 사용)하며 인자를 받는 필터도 있음 : ex) `{{ name|truncatewords:30|filter }}`

    - 약 60개의 built-in template filter 제공 → [공식문서](https://docs.djangoproject.com/en/3.2/ref/templates/builtins/#built-in-filter-reference)

3. Tags
    - 반복 또는 논리를 수행하여 제어 흐름을 만드는 등 변수보다 복잡한 일들을 수행

    - ex) `{% if %} {% endif%}`, `{% extends "" %}`

    - 약 24개의 built-in template tags 제공 → [공식문서](https://docs.djangoproject.com/en/3.2/ref/templates/builtins/#built-in-tag-reference)

4. Comments : 주석
    - `{# #}`, `{% comment %} {% endcomment %}`

### 템플릿 상속

> 페이지의 공통요소를 포함하고, 하위 템플릿이 재정의할 수 있는 공간을 정의하는
> 
> 기본 'skeleton' 템플릿을 작성하여 상속 구조 구축

- extends tag
  - `{% extends 'path' %}`

  - 자식 템플릿이 부모 템플릿을 확장한다는 것을 알림(= 상속 받는 것)

  - 이 tag는 반드시 최상단에 한 개만 작성

- block tag
  - `{% block name %}{% endblock name %}`

  - 자식 탬플릿에서 재정의(overridden)할 수 있는 블록을 정의

- 예제
  - 부모 탬플릿
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
      ...
      {% block content %} bootstrap CDN 생략{% endblock content %}
    </head>
    <body>
      {% block content %}
      {% endblock content %}

      {% block content %} bootstrap CDN 생략{% endblock content %}
    </body>
    </html>
    ```

  - 자식 탬플릿
    ```html
    <!-- extends로 상속 -->
    {% extends 'base.html' %}

    <!-- content block에 작성된 내용은 부모 탬플릿에서 블럭 이름이 같은 곳에 위치 -->
    {% block content %}
    <div>오늘 저녁 추천 메뉴는 <span class="text-danger"><b>{{ today_menu }}</b></span>입니다.</div>
    {% endblock content %}
    ```

<br>

## 요청과 응답 with HTML form

: HTML form은 HTTP 요청을 서버에 보내는 가장 편리한 방법

### form element
- 사용자로부터 할당된 데이터를 서버로 전송

- 웹에서 사용자 정보를 입력하는 여러가지 방식을 제공(text, password 등)

- 핵심 속성
  - action : 입력 데이터가 전송될 URL을 지정, 지정하지 않을 시 현제 form이 있는 페이지로 전송

  - method : 데이터를 어떤 방식으로 보낼 것인지 GET, POST 중에 지정

### input element
- text, number 등 type 속성 값에 따라 사용자로부터 다양한 유형의 데이터를 입력받음

- 핵심 속성 name : 데이터를 제출했을 때 서버는 name 속성에 설정된 값을 통해 사용자가 입력한 데이터에 접근

### Throw & Catch 실습

: 사용자 입력 데이터를 받아 그대로 출력

- urls.py에 throw 작성
  ```python
  urlpatterns = [
      path('admin/', admin.site.urls),
      path('throw/', views.throw),
  ]
  ```
- views.py에 throw 함수 정의
  ```python
  def throw(request):
      return render(request, 'throw.html')
  ```
- throw template 작성
  ```html
  {% extends 'base.html' %}

  {% block content %}
  <div class="container">
    <h1>Throw</h1>
    <form action="/catch/" method="GET" class="d-flex">
      <input type="text" name="message" class="form-control w-25">
      <button type="submit" value="Submit" class="btn btn-outline-primary ms-4">Throw</button>
    </form>
  </div>
  {% endblock content %}
  ```
  - input을 받아 catch로 보낼 것이므로 form action에 현재 app 이후의 경로 작성

  - method의 GET은 데이터를 url에 담아 보내는 역할

  - input name은 message로 지정(input 데이터가 catch로 보내질 때 key 값으로 사용)

  - button의 type이 submit으로 버튼을 누르면 input 값이 action의 주소로 보내짐

- urls.py에 catch 작성
  ```python
  urlpatterns = [
      path('admin/', admin.site.urls),
      path('throw/', views.throw),
      path('catch/', views.catch),
  ]
  ```
- views.py에 catch 함수 정의
  ```python
  def catch(request):
      message = request.GET.get('message')
      context={
          'message': message,
      }
      return render(request, 'catch.html', context)
  ```
  - request : 모든 요청 데이터를 담고 있는 HTTP request 객체(class)이며 GET method를 통해 데이터에 접근 가능

  - GET으로 input data(딕셔너리)에 접근하여 'message'라는 key값을 이용해 value를 얻을 수 있음

  - 이를 context에 저장하여 return함

- catch template 작성
  ```html
  {% extends 'base.html' %}

  {% block content %}
  <h1>Catch</h1>
  <p>{{ message }}를 받았습니다!</p>
  {% endblock content %}
  ```

<br>

## 참고
### 추가 템플릿 경로 지정
- settings.py에서 TEMPLATES 부분에 'DIRS'가 빈 리스트로 존재함

- 기본적으로 app의 templates를 시작 경로로 하는데 'DIRS' 리스트에 추가적으로 경로 지정 가능

- `'DIRS': [ BASE_DIR / 'templates' ]`를 추가 시 최상단 경로의 바로 하위에 있는 templates 폴더도 시작 경로로 추가됨

- 이로 인해 여러 앱에서 extends하는 부모 템플릿 관리가 용이함

- 각 app/templates에 app_name 폴더를 추가하고 그 안에 템플릿 작성 시 경로 지정이 편함 ex) pages/templates/pages