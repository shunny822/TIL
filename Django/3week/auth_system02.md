# Authentication System 02

## 회원가입과 회원정보 수정

> User 객체를 create, update

### Custom Form

- built-in ModelForm인 UserCreationForm()과 UserChangeForm()을 사용

- 이 두 Form의 모델 기본값은 원래의 User model이기 때문에 우리가 Custom한 User model을 적용시켜야 함 == Custom Form 작성

  ```python
  # accounts/forms.py

  # built-in form import
  from django.contrib.auth.forms import UserCreationForm, UserChangeForm
  from django.contrib.auth import get_user_model

  # Form과 Meta에 각 함수 상속
  class CustomUserCreationForm(UserCreationForm):
      class Meta(UserCreationForm.Meta):
          model = get_user_model()


  class CustomUserChangeForm(UserChangeForm):
      class Meta(UserChangeForm.Meta):
          model = get_user_model()
  ```
  - get_user_model() : 현재 프로젝트에서 활성화된 사용자 모델을 반환하는 함수

  - `from .models import User`로 직접 참조하지 않는 이유???


### UserCreationForm()
```python
# accounts/views.py
from django.contrib.auth import login as auth_login
from .forms import CustomUserCreationForm

def signup(request):
    if request.user.is_authenticated:
        return redirect('accounts:index')
    
    if request.method == 'POST':
        form = CustomUserCreationForm(request.POST)
        # 유효한 경우 회원 생성과 동시에 로그인
        if form.is_valid():
            user = form.save()
            auth_login(request, user)
            return redirect('accounts:index')
    else:
        form = CustomUserCreationForm()
    context = {
        'form': form
    }
    return render(request, 'accounts/signup.html', context)
```
```html
<!-- template -->
<h1>회원 가입</h1>
<form action="{% url 'accounts:signup' %}" method="POST">
  {% csrf_token %}
  {{ form.as_p }}
  <input type="submit">
</form>
```


### UserChangeForm()
```python
# accounts/views.py
from .forms import CustomUserChangeForm

def update(request):
    if request.method == 'POST':
        form = CustomUserChangeForm(request.POST, instance=request.user)
        if form.is_valid():
            form.save()
            return redirect('accounts:index')
    else:
        form = CustomUserChangeForm(instance=request.user)
    context = {
        'form': form,
    }
    return render(request, 'accounts/update.html', context)
```
```html
<!-- template -->

<h1>회원정보 수정</h1>
<form action="{% url 'accounts:update' %}" method="POST">
  {% csrf_token %}
  {{ form.as_p }}
  <input type="submit">
</form>
```
- UserChangeForm은 admin 인터페이스에서 사용되는 Form으로 제약을 걸지 않으면 일반 사용자가 접근해서는 안 될 정보들까지 모두 수정이 가능해짐

- 이를 방지하기 위해 UserChangeForm을 Custom할 때 접근 가능한 필드를 조정해야 함
  ```python
  class CustomUserChangeForm(UserChangeForm):
      class Meta(UserChangeForm.Meta):
          model = get_user_model()
          fields = ('email', 'first_name', 'last_name')
  ```

<br>

## 회원 탈퇴

> User 객체를 delete

```html
<!-- template -->

<form action="{% url 'accounts:delete' %}" method="POST">
  {% csrf_token %}
  <input type="submit" value="회원 탈퇴">
</form>
```
```python
# accounts/views.py
from django.contrib.auth import logout as auth_logout

def delete(request):
    request.user.delete()
    # 탈퇴 시 유저의 세션정보도 지우고 싶을 때 logout함께 사용
    auth_logout(request)
    return redirect('accounts:index')
```

<br>

## 비밀번호 변경

### PasswordChangeForm()
```python
# accounts/urls.py

app_name = 'accounts'
urlpatterns = [
    ...
    path('password/', views.password_change, name="password_change"),
]
```
- django는 UserChangeForm에서 별도의 주소(/accounts/password/)를 통해 비밀번호 변경 페이지로 안내함

- 이 주소에 맞춰 url을 'password/'로 지정하기

```python
# accounts/views.py

from django.contrib.auth.forms import AuthenticationForm, PasswordChangeForm
from django.contrib.auth import update_session_auth_hash

def password_change(request):
    if request.method == 'POST':
        form = PasswordChangeForm(request.user, request.POST)
        if form.is_valid():
            user = form.save()
            update_session_auth_hash(request, user)
            return redirect('accounts:index')
    else:
        form = PasswordChangeForm(request.user)
    context = {
        'form':form,
    }
    return render(request, 'accounts/password_change.html', context)
```
- PasswordChangeForm은 인자의 순서가 request.user가 먼저임

- 암호 변경 시 기존 세션과 회원 인증 정보가 일치하지 않기 때문에 로그인 상태가 유지되지 못함

- 로그인이 풀리지 않게 하기 위해서는 update_session_auth_hash() 함수를 사용해야 함

- update_session_auth_hash(request, user) : 암호 변경 시 새로운 password의 session data로 기존 session을 업데이트

```html
<!-- template -->

<h1>비밀번호 변경</h1>
<form action="{% url 'accounts:password_change' %}" method="POST">
  {% csrf_token %}
  {{ form.as_p }}
  <input type="submit">
</form>
```

<br>

## 로그인 사용자에 대한 접근 제한

### is_authenticated

- 사용자가 인증되었는지 여부를 알 수 있는 User model의 속성

- 모든 User 인스턴스에 대해 항상 True인 읽기 전용 속성이며 AnonymousUser에 대해서는 항상 False

- 권한과는 관련이 없으며 사용자가 활성화 상태이거나 유효한 세션을 가지고 있는지도 확인하지 않음

- 이전 시간과 같이 template에서 if문과 쓰일 수 있으며 view함수에서도 적용 가능
  ```python
  # 인증된 사용자인 경우 로그인/회원가입 로직 수행할 수 없도록
  def login(request):
    if request.user.is_authenticated:
        return redirect('accounts:index')
    ...


  def signup(request):
    if request.user.is_authenticated:
        return redirect('accounts:index')
    ...
  ```

### login_required
```python
from django.contrib.auth.decorators import login_required

# accounts/views.py
@login_required
def logout(request):
    ...

@login_required
def update(request):
    ...

# articles/views.py
@login_required
def create(request):
    ...
```
- 인증된 사용자에 대해서만 view함수를 실행시키는 데코레이터

- 로그인하지 않은 사용자의 경우 '/accounts/login/' 주소로 redirect 시킴


<br>

## 참고
### Decorator

: 기존에 작성된 함수에 기능을 추가하고 싶을 때, 해당 함수를 수정하지 않고 기능만을 추가해주는 함수

<예시>
```python
def hello(func):
    def wrapper():
        print('HIHI')
        func()
        print('HIHI')
    
    return wrapper


@hello
def bye():
  print('byebye')

bye()
# 출력
HIHI
byebye
HIHI
```