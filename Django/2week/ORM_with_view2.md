# ORM with View 2

## HTTP request methods

> HTTP : 네트워크 상에서 데이터를 주고 받기 위한 약속
> 
> HTTP request methods : 데이터에 어떤 요청을 원하는지를 나타내는 것

### GET method
- 특정 리소스를 조회하는 요청(반드시 조회 시에만 사용)

- 입력 길이 255자 제한

- GET으로 데이터 전달 시 Query String 형식으로 보내짐

### POST method
- 특정 리소스에 변경 사항을 만드는 요청

- 입력 길이 제한 X

- POST로 데이터 전달 시 HTTP Body에 담겨 보내짐

- 이는 데이터 베이스에 변경사항을 만드는 요청이므로 CSRF 등의 공격을 막기위해 토큰 사용(최소한의 신원 확인)

- Security Token(CSRF Token)
  - Cross-Site-Request-Forgery(사이트 간 요청 위조) : 사용자의 의지와 무관하게 공격자가 의도한 행동을 하여 특정 웹 페이지를 보안에 취약하게 하거나 수정, 삭제 등의 작업을 하게 만드는 공격 방법

  - CSRF Token은 CSRF의 대표적인 방어 방법

  - 서버가 사용자 입력 데이터에 임의의 난수값(token)을 부여하여 함께 서버로 전송 시키고 전달된 token이 유효한지 검증

  - form tag 안에 `{% csrf_token %}` 태그 작성하여 토큰 값 부여

<br>

## todos app 실습2
### DELETE
- 데이터 삭제
```python
# todos/urls.py

urlpatterns = [
    path('', views.index, name='index'),
    path('<int:pk>/', views.detail, name='detail'),
    path('new/', views.new, name='new'),
    path('create/', views.create, name='create'),
    path('<int:pk>/delete/', views.delete, name='delete'),
]
```
```python
# todos/views.py
def delete(request, pk):
    todo = Todo.objects.get(pk=pk)
    todo.delete()
    return redirect('todos:index')
```
  - redirect() : 인자에 작성된 주소로 다시 요청을 보냄

### UPDATE
```python
# todos/urls.py

urlpatterns = [
    path('', views.index, name='index'),
    path('<int:pk>/', views.detail, name='detail'),
    path('new/', views.new, name='new'),
    path('create/', views.create, name='create'),
    path('<int:pk>/delete/', views.delete, name='delete'),
    path('<int:pk>/edit/', views.edit, name='edit'),
    path('<int:pk>/update/', views.update, name='update'),
]
```
- 수정할 내용 입력
```python
# todos/views.py

def edit(request, pk):
  # 수정할 데이터 조회하여 변수에 저장
    todo = Todo.objects.get(pk=pk)
    context = {
        'todo': todo,
    }
    return render(request, 'todos/edit.html', context)
```
```html
<!-- template -->

<div class="container">
  <h1 class="m-5">Todo list 수정</h1>
  <!-- form 제출 시 todo.pk의 값과 함께 데이터를 update url로 보냄 -->
  <!-- 변경사항을 만드므로 POST method 사용 -->
  <form action="{% url 'todos:update' todo.pk %}" method='POST'>
    <!-- POST method 사용 시 토큰태그 작성 -->
    {% csrf_token %}
    <div class="mb-3">
      <label for="title" class="form-label">제목</label>
      <!-- value에 값을 넣어 기존 상태 보여줌 -->
      <input type="text" class="form-control" name="title" id="title" value={{ todo.title }}>
    </div>
    <div class="mb-3">
      <label for="content" class="form-label">내용</label>
      <textarea class="form-control" name="content" id="content" rows="3">{{ todo.content }}</textarea>
    </div>
    <div class="d-flex justify-content-between">
      <div class="mb-3 col-5">
        <label for="priority" class="form-label">우선순위</label>
        <input type="number" class="form-control" min="1" name="priority" id="priority" value={{ todo.priority }}>
      </div>
      <div class="mb-3 col-6">
        <label for="deadline" class="form-label">마감기한</label>
        <!-- date type은 value에 {{ todo.deadline|date:"Y-m-d" }} dateFormat 넣어줘야 함 -->
        <input type="date" class="form-control" name="deadline" id="deadline" value={{ todo.deadline|date:"Y-m-d" }}>
      </div>
    </div>
    <div class="d-flex justify-content-center mt-5">
      <button type="submit" class="btn btn-outline-primary w-50">저장하기</button>
    </div>
  </form>
</div>
```

- 수정한 내용 저장
```python
# todos/views.py

def update(request, pk):
  # 수정한 데이터 조회하여 저장
    todo = Todo.objects.get(pk=pk)
  # 수정된 데이터를 가져와 각각의 필드에 저장
    todo.title = request.POST.get('title')
    todo.content = request.POST.get('content')
    todo.priority = request.POST.get('priority')
    todo.deadline = request.POST.get('deadline')
  # 변경사항 저장
    todo.save()
  # 종료 시 해당 데이터의 detail 페이지로 감
    return redirect('todos:detail', pk)
```

### Todo 목록의 완료 여부 구현
```html
<!-- index.html template -->
<div class="container mt-5 mx-auto">
  <h1 class="mb-5">✨My Todo List✨</h1>
  <ul>
    {% for todo in todos %}
      <li class="mb-3">
        <a href="{% url 'todos:detail' todo.pk %}">{{ todo.title }}</a>
        <!-- 현재 상태 출력 -->
        / 완료여부 - {{ todo.completed }}
        <!-- if문으로 완료 여부에 따라 다른 text 보이게 함 -->
        {% if todo.completed %}
          <a href="{% url 'todos:complete' todo.pk %}">완료 취소</a>
        {% else %}
          <a href="{% url 'todos:complete' todo.pk %}">완료 하기</a>
        {% endif %}
      </li>
    {% endfor %}
  </ul>
  <a class="btn btn-outline-primary m-3" href="{% url 'todos:new' %}" role="button">Todo 추가하기</a>
</div>
```
```python
# todos/urls.py

urlpatterns = [
    path('', views.index, name='index'),
    path('<int:pk>/', views.detail, name='detail'),
    path('new/', views.new, name='new'),
    path('create/', views.create, name='create'),
    path('<int:pk>/delete/', views.delete, name='delete'),
    path('<int:pk>/edit/', views.edit, name='edit'),
    path('<int:pk>/update/', views.update, name='update'),
    # 해당 pk값을 받아 complete 함수 실행
    path('<int:pk>/complete/', views.complete, name='complete'),
]
```
```python
# todos/views.py

def complete(request, pk):
    todo = Todo.objects.get(pk=pk)
    # 완료이면 False, 미완료이면 True로 바꾼 후 저장
    if todo.completed:
        todo.completed = False
    else:
        todo.completed = True
    todo.save()
    return redirect('todos:index')
```