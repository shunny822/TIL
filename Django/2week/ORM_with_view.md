# ORM with View

: Django ORM과 View 함수를 결합하여 웹 애플리케이션의 데이터를 저장 & 렌더링

## todos app 실습
### READ
```python
# todos/urls.py

app_name = 'todos'
urlpatterns = [
    # todos app의 메인 페이지
    path('', views.index, name='index'),
    # 단일 todo 조회 시 보여지는 세부정보 페이지
    path('detail/<int:todo_pk>/', views.detail, name='detail'),
]
```
- 전체 todo list 조회
```python
# todos/views.py

def index(request):
  # 모든 데이터를 불러와 'todos'변수에 저장하여 return
    todos = Todo.objects.all()
    context = {
        'todos': todos,
    }
    return render(request, 'todos/index.html', context)
```
```html
<!-- template -->

<h1 class="mx-3 my-5">✨My Todo List✨</h1>
<!-- todo 생성 페이지 -->
<a href="{% url 'todos:new' %}" class="my-3 ms-5">Todo 생성</a>
<!-- 'todos'변수의 데이터를 모두 나열하고
 각각의 세부 페이지로 갈 수 있도록 해당 데이터의 pk인자와 함께 detail url을 걸어줌 -->
{% for todo in todos %}
  <div class="my-3 ms-5">
    <a href="{% url 'todos:detail' todo.pk %}">할 일 {{ todo.pk }}</a>
  </div>
{% endfor %}
```

- 단일 todo list 조회
```python
# todos/views.py

def detail(request, todo_pk):
  # 받은 인자와 같은 pk값을 가지는 데이터를 찾아 'todo'변수에 저장 후 return
    todo = Todo.objects.get(pk=todo_pk)
    context = {
        'todo': todo,
    }
    return render(request, 'todos/detail.html', context)
```
```html
<!-- template -->

<!-- 각각의 값을 출력 -->
<h1>제목 - {{ todo.title }}</h1>
<h3>내용 - {{ todo.content }}</h3>
<h3>우선순위 - {{ todo.priority }}</h3>
<h3>마감기한 - {{ todo.deadline }}</h3>
<h3>완료 여부 - {{ todo.completed }}</h3><br>

<!-- 메인 페이지로 돌아가기 -->
<a href="{% url 'todos:index' %}">뒤로 가기</a>
```

### CREATE
```python
# todos/urls.py

app_name = 'todos'
urlpatterns = [
    path('', views.index, name='index'),
    path('detail/<int:todo_pk>/', views.detail, name='detail'),
    path('new/', views.new, name='new'),
    path('create/', views.create, name='create'),
]
```
- 새로운 todo 입력(메인페이지의 Todo 생성 url)
```python
# todos/views.py

def new(request):
    return render(request, 'todos/new.html')
```
```html
<!-- template -->

<div class="container">
  <h1 class="m-5">Todo list 작성</h1>
  <!-- input 제출 시 create url로 보냄 -->
  <form action="{% url 'todos:create' %}" method='GET'>
    <!-- 각각의 input을 name을 이용해 key 설정 -->
    <div class="mb-3">
      <label for="title" class="form-label">제목</label>
      <input type="text" class="form-control" name="title" id="title">
    </div>
    <div class="mb-3">
      <label for="content" class="form-label">내용</label>
      <textarea class="form-control" name="content" id="content" rows="3"></textarea>
    </div>
    <div class="d-flex justify-content-between">
      <div class="mb-3 col-5">
        <label for="priority" class="form-label">우선순위</label>
        <input type="number" class="form-control" name="priority" id="priority">
      </div>
      <div class="mb-3 col-6">
        <label for="deadline" class="form-label">마감기한</label>
        <input type="date" class="form-control" name="deadline" id="deadline">
      </div>
    </div>
    <!-- 버튼 누르면 제출되어 create url로 이동 -->
    <div class="d-flex justify-content-center mt-5">
      <button type="submit" class="btn btn-outline-primary w-50">list에 추가</button>
    </div>
  </form>
</div>
```

- 입력받은 데이터로 todo 생성
```python
# todos/views.py

def create(request):
  # 각각의 값을 변수에 저장
    title = request.GET.get('title')
    content = request.GET.get('content')
    priority = request.GET.get('priority')
    deadline = request.GET.get('deadline')
  # 저장한 값을 Todo 인스턴스 생성 시 argument로 넣어줌
    todo = Todo(title=title, content=content, priority=priority, deadline=deadline)
  # 생성된 인스턴스 저장 = DB에 저장
    todo.save()
    return render(request, 'todos/create.html')
```
```html
<!-- template -->

<h1>할 일을 등록하였습니다!</h1>
<!-- 등록 확인 후 메인 페이지로 돌아가기 -->
<a href="{% url 'todos:index' %}">돌아가기</a>
```