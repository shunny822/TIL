# Many to Many relationship 02

## 동일한 모델과의 N : M 관계

User와 User의 관계 예시

### profile 페이지 구현
```python
# accounts/urls.py

urlpatterns = [
  ...
    path('<str:username>/', views.profile, name='profile'),
]
```
- 여기서 주의할 점은 url주소의 `<str:username>/`은 str 타입으로 다른 index, create도 모두 str으로 인식해버림

- django는 url을 위에서부터 아래로 읽으면서 해당하는 url이 나오면 그 url로 가므로 profile url을 맨 아래에 두거나 `profile/<str:username>/`와 같이 앞에 profile을 붙여줘야 함

```python
# accounts/views.py

# username도 유일한 값이므로 pk와 같은 역할을 할 수 있음
def profile(request, username):
    User = get_user_model()
    person = User.objects.get(username=username)
    context = {
        'person': person,
    }
    return render(request, 'accounts/profile.html', context)
```
```html
<!-- accounts/profile.html -->

<div>
  <h1>
    {{ person }}님의 프로필 페이지
  </h1>
</div>
```


### User간의 Follow 구현
```python
# accounts/models.py

class User(AbstractUser):
    followings = models.ManyToManyField('self', symmetrical=False, related_name='followers')
```
- 대체만 했던 User model에 필요한 field를 추가하여 커스텀

- 동일한 모델의 ManyToManyField의 'to'인자는 'self'

```python
# accounts/urls.py

urlpatterns = [
    ...
    path('<int:you_pk>/follow/', views.follow, name='follow'),
    path('<str:username>/', views.profile, name='profile'),
]
```
- 상대 user의 pk값을 인자로 받음

```python
# accounts/views.py

@login_required
def follow(request, you_pk):
    you = get_user_model().objects.get(pk=you_pk)
    me = request.user
    if you != me:
        if me in you.followers.all():
            you.followers.remove(me)
        else:
            you.followers.add(me)
    return redirect('accounts:profile', you)
```
- User 모델은 get_user_models()을 통해 받아옴

```html
<!-- accounts/profile.html -->

<div class="d-flex align-items-center">
  <div class="me-3">
    <!-- Button trigger modal -->
    <!-- 버튼을 누르면 팔로잉, 팔로우 목록이 모달로 뜨도록 함 -->
    <button type="button" class="border-0 bg-transparent" data-bs-toggle="modal" data-bs-target="#following">
      팔로잉 {{person.followings.all|length}}
    </button>
    <button type="button" class="border-0 bg-transparent" data-bs-toggle="modal" data-bs-target="#follower">
      팔로워 {{person.followers.all|length}}
    </button>
  </div>
  
  <!-- 로그인 되어있고 자신의 계정이 아닌 경우 팔로우 버튼이 보이도록 함 -->
  {% if request.user.is_authenticated and request.user != person %}
    {% if request.user in person.followers.all %}
      <form action="{% url 'accounts:follow' person.pk %}" method="POST">
        {% csrf_token %}
        <button type="submit" class="btn btn-primary" style="heigh: 30px;">
          팔로우
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-check2-circle" viewBox="0 0 16 16">
            <path d="M2.5 8a5.5 5.5 0 0 1 8.25-4.764.5.5 0 0 0 .5-.866A6.5 6.5 0 1 0 14.5 8a.5.5 0 0 0-1 0 5.5 5.5 0 1 1-11 0z"/>
            <path d="M15.354 3.354a.5.5 0 0 0-.708-.708L8 9.293 5.354 6.646a.5.5 0 1 0-.708.708l3 3a.5.5 0 0 0 .708 0l7-7z"/>
          </svg>
        </button>
      </form>
    {% else %}
      <form action="{% url 'accounts:follow' person.pk %}" method="POST">
        {% csrf_token %}
        <input type="submit" class="btn btn-outline-primary" value="팔로우">
      </form>
    {% endif %}
  {% else %}
    <button class="btn btn-secondary" disabled>팔로우</button>
  {% endif %}
</div>
</div>

<!-- Modal -->
<!-- 팔로잉, 팔로우 목록이 뜨는 모달창, 누르면 해당 user profile로 가도록 설계 -->
<div class="modal fade" id="following" tabindex="-1" aria-labelledby="followingLabel" aria-hidden="true">
<div class="modal-dialog">
  <div class="modal-content">
    <div class="modal-header">
      <h1 class="modal-title fs-5" id="followingLabel">팔로잉</h1>
      <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
    </div>
    <div class="modal-body">
      {% for following in person.followings.all %}
        <a href="{% url 'accounts:profile' following %}" class="text-decoration-none text-black">{{followings}}</a>
      {% endfor %}
    </div>
  </div>
</div>
</div>
<div class="modal fade" id="follower" tabindex="-1" aria-labelledby="followerLabel" aria-hidden="true">
<div class="modal-dialog">
  <div class="modal-content">
    <div class="modal-header">
      <h1 class="modal-title fs-5" id="followerLabel">팔로워</h1>
      <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
    </div>
    <div class="modal-body">
      {% for follower in person.followers.all %}
        <a href="{% url 'accounts:profile' follower %}" class="text-decoration-none text-black">{{follower}}</a>
      {% endfor %}
    </div>
  </div>
</div>
```

<br>

## django Paginator
☆ [공식문서 참고](https://docs.djangoproject.com/en/4.2/ref/paginator/)

```python
# articles/views.py
from django.core.paginator import Paginator

def index(request):
    articles = Article.objects.order_by('-pk')  # 최신순 정렬
    page = request.GET.get('page', '1')         # page번호 get, 없으면 1
    per_page = 5                                # 페이지에 포함할 최대항목 수
    pagination = Paginator(articles, per_page)  # Paginator 객체 생성
    page_obj = pagination.get_page(page)        # pagination 객체에서 해당 page의 개체 목록

    context = {
        'articles': page_obj,
        'last_page': pagination.num_pages,   # .num_pages = 페이지의 길이 = 마지막페이지
    }
    return render(request, 'articles/index.html', context)
```
```html
<!-- articles/index.html -->

<ul class="pagination justify-content-center">
  <li class="page-item">
    <!-- 첫 페이지이므로 page=1 -->
    <a class="page-link" href="?page=1">처음</a>
  </li>
  <!-- 이전 페이지가 있는 경우 -->
  {% if articles.has_previous %}
    <li class="page-item">
      <a class="page-link" href="?page={{ articles.previous_page_number }}">
        <span aria-hidden="true">&laquo;</span>
      </a>
    </li>
  <!-- 없는 경우 비활성화 -->
  {% else %}
    <li class="page-item disabled">
      <a class="page-link" tabindex="-1" aria-disabled="true" href="#">
        <span aria-hidden="true">&laquo;</span>
      </a>
    </li>
  {% endif %}

  <!-- 페이지 버튼을 차례로 구현 -->
  {% for page_number in articles.paginator.page_range %}
    <!-- 페이지 수가 많을 경우를 대비해 +-5까지만 생성되도록 함 -->
    {% if page_number >= articles.number|add:-5 and page_number <= articles.number|add:5 %}
      {% if page_number == articles.number %}
        <li class="page-item active" aria-current="page">
          <a class="page-link" href="?page={{ page_number }}">{{ page_number }}</a>
        </li>
      {% else %}
        <li class="page-item">
          <a class="page-link" href="?page={{ page_number }}">{{ page_number }}</a>
        </li>
      {% endif %}
    {% endif %}
  {% endfor %}

  {% if articles.has_next %}
    <li class="page-item">
      <a class="page-link" href="?page={{ articles.next_page_number }}">
        <span aria-hidden="true">&raquo;</span>
      </a>
    </li>
  {% else %}
    <li class="page-item disabled">
      <a class="page-link" tabindex="-1" aria-disabled="true" href="#">
        <span aria-hidden="true">&raquo;</span>
      </a>
    </li>
  {% endif %}
  <li class="page-item">
    <a class="page-link" href="?page={{ last_page }}">마지막</a>
  </li>
</ul>
```

<br>

## Fixtures

> 모델에 초기 데이터를 제공!

- Django가 데이터베이스로 가져오는 방법을 알고있는 데이터 모음

- Django가 직접 만들기 때문에 데이터베이스 구조에 맞춰 작성되어 있음

### dumpdata

- 데이터 베이스의 모든 데이터를 출력, 여러 모델을 하나의 json file로 만들 수 있음

- 명령어 : `python manage.py dumpdata --indent 4 app_name.model_name app_name.model_name ... > filename.json`

- indent 4를 통해 json file 생성시 들여쓰기가 적용되도록 함

- 모델을 나열하여 하나의 json file에 데이터 생성 가능

- 하지만 추후 관리를 위하여 각각 생성하는 것을 추천

### loaddata

- fixtures 데이터를 데이터베이스로 불러옴

- fixtures의 기본경로는 'app_name/fixtures/'로 되어있기 때문에 Django는 설치된 모든 app의 fixtures 폴더 이후의 경로로 파일을 찾아 load함

- fixtures 파일이 어떤 app의 fixtures 폴더에 있는지는 상관없음

- 명령어 : `python manage.py loaddata file_name.json file_name.json ...`

- loaddata를 한번에 실행하지 않고 하나씩 실행한다면 모델 관계에 따라 순서가 중요함(comment는 article에 대한 key 및 user에 대한 key가 필요한 것 처럼)

- 따라서 user → articles → comment 순으로 load해야 오류가 발생하지 않음

- loaddata 시 encoding codec error가 발생하는 경우 2가지 해결 방법
  - dumpdata 시 추가옵션 작성 : `python -Xutf8 manage.py dumpdata ....`

  - loaddata 후 메모장으로 json 파일을 열기 → 다른 이름으로 저장 클릭 후 인코딩을 UTF8로 선택 후 저장