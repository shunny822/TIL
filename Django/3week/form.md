# Django Form

## Form과 ModelForm

> 사용자 입력 데이터를 수집하고, 처리 및 유효성 검증을 수행하기 위한 도구

- django에서 유효성 검사를 단순화하고 자동화할 수 있는 기능을 제공

- 유효성 검사 : 수집한 데이터가 정확하고 유효한지 확인하는 과정

- Form은 사용자 입력 데이터를 DB에 저장하지 않을 때 사용. ex) 로그인

- ModelForm은 사용자 입력 데이터를 DB에 저장해야 할 때 사용. ex) 회원가입

### Form
- Form class 선언
  ```python
  # app 폴더에 forms.py 생성 후 작성
  from django import forms

  # 받으려는 input에 적합한 Form 정의
  class ArticleForm(forms.Form):
      title = forms.CharField(max_length=10)
      content = forms.CharField(widget=forms.Textarea)
  ```
  - Widgets : HTML의 input type의 표현을 담당, [widget의 종류](https://docs.djangoproject.com/ko/3.2/ref/forms/widgets/#built-in-widgets)

- Form class 적용
  ```python
  # todos/views.py

  # 정의한 ArticleForm import
  from .forms import ArticleForm

  def new(request):
    # 인스턴스 생성
      form = ArticleForm()
    # 이 형태의 form 인스턴스를 rendering
      context = {
          'form': form,
      }
      return render(request, 'articles/new.html', context)
  ```
  ```html
  <!-- template -->
  <body>
    <h1>New</h1>
    <form action="{% url 'articles:create' %}" method="POST">
      {% csrf_token %}
      {{ form.as_p }}
      <input type="submit">
    </form>
    <a href="{% url 'articles:index' %}">[back]</a>
  </body>
  ```
  - 기존에 input tag들을 `{{ form.as_p }}` 하나로 대체

  - rendering option([공식문서 참고](https://docs.djangoproject.com/en/3.2/topics/forms/))
    - {{ form.as_table }} will render them as table cells wrapped in <tr> tags
    - {{ form.as_p }} will render them wrapped in <p> tags
    - {{ form.as_ul }} will render them wrapped in <li> tags

→ 페이지에서 개발자도구로 보면 input tag와 name, required 속성 등이 자동으로 생성됨을 확인 가능!!

### ModelForm
- ModelForm class 선언
  ```python
  # app 폴더에 forms.py 생성 후 작성
  from django import forms
  # 사용할 model import
  from .models import Todo

  class TodoForm(forms.ModelForm):
      class Meta:
          model = Todo
          fields = '__all__'
  ```
  - Meta class : meta는 데이터에 대한 데이터 정도의 의미로 Meta class는 ModelForm의 정보를 작성

  - field & exclude : field와 exclude로 필요한 속성만 지정하거나 제외할 수 있음
    ```python
    class TodoForm(forms.ModelForm):
      class Meta:
        model = Todo
        fields = ('title',)
        # exclude = ('title',)
    ```

- ModelForm 적용
  - is_valid() : 여러 유효성 검사를 실행하고 그 결과를 boolean으로 반환

  - save() : 데이터베이스 객체를 만들고 저장, 키워드 인자인 instance 여부를 통해 생성인지 수정인지 결정

  - create
    ```python
    # todos/views.py

    # 정의한 TodoForm import
    from .forms import TodoForm

    def create(request):
      # POST method인 경우
      if request.method == 'POST':
        # TodoForm으로 온 input을 form 변수에 저장
          form = TodoForm(request.POST)
          # form의 유효성 검사
          if form.is_valid():
            # 통과한 경우 저장하면서 todo에 저장(pk값을 넘기기 위함)
              todo = form.save()
              return redirect('todos:detail', todo.pk)

      # GET method인 경우
      else:
        # TodoForm의 형식을 변수에 할당해 rendering
          form = TodoForm()
      
      # GET method인 경우와 유효성 검사를 통과하지 못한 경우
      context = {
          'form': form,
      }
      return render(request, 'todos/create.html', context)
    ```
    ````html
    <!-- template -->
    <div class="container">
      <h1 class="my-5">Todo List 생성</h1>
      <form action="{% url 'todos:create' %}" method="POST">
        {% csrf_token %}
        {{ form.as_p }}
        <input class="btn btn-outline-warning" type="submit" value="저장하기">
        <a class="btn btn-outline-primary " href="{% url 'todos:index' %}" role="button">뒤로가기</a>
      </form>
    </div>
    ```

  - update
    ```python
    # todos/views.py

    def update(request, pk):
    # todo에 pk값에 해당하는 인스턴스 할당하고 시작
      todo = Todo.objects.get(pk=pk)

      # POST method인 경우
      if request.method == 'POST':
        # TodoForm의 인자로 'instance=todo'를 넣으면 save함수가 데이터 생성이 아닌 
        # todo 데이터를 수정해야 함을 판단할 수 있음
          form = TodoForm(request.POST, instance=todo)
          if form.is_valid():
              # 수정이므로 새로운 변수에 할당하지 않고 form 자체를 save
              form.save()
              return redirect('todos:detail', todo.pk)
      
      # GET method인 경우
      else:
        # 'instance=todo'를 인자로 써줌으로써 기존의 값을 불러와 form에 저장 후 rendering
          form = TodoForm(instance=todo)
      
      # GET method인 경우와 유효성검사를 통과하지 못한 경우
      context = {
          'todo': todo,
          'form': form,
      }
      return render(request, 'todos/update.html', context)
    ```
    ```html
    <!-- template -->
    <div class="container">
      <h1 class="my-5">Todo List 수정</h1>
      <form action="{% url 'todos:update' todo.pk %}" method="POST">
        {% csrf_token %}
        {{ form.as_p }}
        <input class="btn btn-outline-warning" type="submit" value="저장하기">
        <a class="btn btn-outline-primary " href="{% url 'todos:index' %}" role="button">뒤로가기</a>
      </form>
    </div>
    ```

<br>

## Widget 응용

- ModelForm 작성 시 witget으로 type을 작성하고 그 안에 속성을 딕셔너리 형태로 추가 가능
```python
from django import forms
from .models import Todo

class TodoForm(forms.ModelForm):
    title = forms.CharField(
        widget=forms.TextInput(
            attrs={'placeholder': '제목 입력'},
        ),
    )
    content = forms.CharField(
        label='내용',
        widget=forms.Textarea(
            attrs={'class': 'my-content'},
        ),
    )
    priority = forms.IntegerField(
        widget=forms.NumberInput(
            attrs={'min': 1, 'max': 5, 'value': 3},
        )
    )
    deadline = forms.DateField(
        widget=forms.DateInput(
            attrs={'type': 'date'},
        ),
    )

    class Meta:
        model = Todo
        fields = '__all__'
```