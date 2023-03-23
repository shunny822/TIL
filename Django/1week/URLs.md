# django URLs

> URL 패턴을 정의하고 해당 패턴이 일치하는 요청을 처리할 view 함수를 mapping

## 변수와 URL

### Variable Routing

- URL 일부에 변수를 포함시키는 것, 변수는 view 함수의 인자로 전달

- url에서 반복되는 부분이 많다면 이를 한번만 쓰고 바뀌는 부분을 변수로 받음
  ```python
  # 기존의 모습
  urlpatterns = [
    path('articles/1/', ...),
    path('articles/2/', ...),
    path('articles/3/', ...),
  ]

  # variable routing
  urlpatterns = [
    path('articles/<int:num>/', ...),
  ]
  ```

- `<path_converter:variable_name>`의 형태로 작성

- Path converters : str, int 등 URL 변수 타입을 지정(총 5가지)

- 예제
  ```python
  # urls
  urlpatterns = [
    path('number-print/<int:number>', views.number_print),
  ]                 # int type의 number변수

  # views
  def number_print(request, number):
  # number라는 변수가 view함수의 인자로 넘어오게 되어있음
    context = {
        'number': number,
    }
    return render(request, 'nums/number_print.html', context)
  ```

<br>

## App URL mapping

> 각 앱에 URL을 정의하는 것

- 다른 앱에서 사용되어도 URL 이름이 겹치지 않게 조정해야 하므로 번거로움

- 프로젝트와 각각의 앱이 URL을 나누어 관리하여 주소 관리를 편하게 하기 위함

### include()

- 다른 URL들을 참조할 수 있도록 돕는 함수

- URL을 받았을 때 그 시점까지 일치하는 부분을 잘라내고 나머지를 include된 URL로 전달

- 예제
  - 원래 모습
    ```python
    from django.urls import path
    from articles import views
    from pages import views

    urlpatterns = [
      path('admin/', admin.site.urls),
      path('articles/', views.index),
      path('dinner/', views.dinner),
      path('throw/', views.throw),
      path('pages/', views.index),
    ]
    ```
  - url mapping
    ```python
    # 프로젝트 url
    from django.urls import path, include

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('articles/', include('articles.urls')),
        path('pages/', include('pages.urls')),
        # 앞의 url로 시작하는 url을 include가 참조하는 url로 보냄
    ]   # 원래 작성되었던 url들은 해당 app의 urls에 작성
    ```

<br>

## URL 이름 지정

### Naming URL patterns

> path함수의 name 인자를 정의하여 URL에 이름을 지정하는 것

- 주소가 변경되면 그 주소를 사용한 모든 위치를 찾아 변경해야하는 단점을 보완!!
  ```python
  # articles urls
  urlpatterns = [
      path('index/', views.index, name='index'),
      path('dinner/', views.dinner, name='dinner'),
      path('throw/', views.throw, name='throw'),
    ]
  ```

- url tag
  - `{% url 'url_name' arg1 arg2 %}`의 형태
  
  - 주어진 URL 패턴의 이름과 일치하는 절대 경로 주소를 반환

  - 보내는 주소가 변수를 받는 주소라면 argument도 작성

- app_name
  - 다른 앱에 같은 이름의 url이 있는 경우 app_name을 붙여 구분

  - url tag 사용 시 `{% url 'app_name:url_name' arg1 arg2 %}`의 형태

- 예제
  ```python
  # urls
  from django.urls import path
  from . import views

  app_name = 'nums'
  urlpatterns = [
      path('number-print/<int:number>', views.number_print, name='number_print'),
      path('number-print2/', views.number_print2, name='number_print2'),
  ]


  # views
  def number_print(request, number):
    context = {
        'number': number,
    }
    return render(request, 'nums/number_print.html', context)
  
  def number_print2(request):
    return render(request, 'nums/number_print2.html')
  ```
  ```html
  <!-- template -->
  {% extends 'base.html' %}

  {% block content %}
    <a href="{% url 'nums:number_print' 822 %}">822</a>
  {% endblock content %}
  ```


<br>

## 참고

### 기본페이지 설정
```python
# urls
from django.urls import path
from . import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index),
]

# views
def index(request):
    return render(request, 'index.html')
```
- urls에 빈 ''경로를 적고 기본 페이지를 열기 위한 view함수 작성

- view함수에 기본 페이지로 보여질 템플릿 작성


### Trailing Slashes
- django는 URL 끝에 '/'가 없다면 자동으로 붙임

- 'foo.com/bar'와 'foo.com/bar/'는 기술적으로 다른 url이나 같은 곳으로 가도록 설계

cf ) url 앞에 붙는 '/'
- 기본 주소(예를 들어 https://127.0.0.1:8000)뒤에 경로를 붙이는 것(a tag의 href를 생각해보기)

- 앞에 '/'가 없다면 현재 위치에 추가로 경로를 붙이는 것!