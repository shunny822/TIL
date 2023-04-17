# Many to Many relationship

## N : M 관계

의사-환자의 관계 예시

### 중개 모델

```python
class Doctor(models.Model):
    name = models.TextField()


class Patient(models.Model):
    name = models.TextField()


class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
```
- N : M 관계인 model을 FK로 작성

- 예약 생성 : `Reservation.objects.create(doctor=doctor1, patient=patient1)`

- 예약정보 조회 : `doctor1.reservation_set.all()`, `patient1.reservation_set.all()`


### ManyToManyField
```python
# hospital/models.py

class Patient(models.Model):         # to
    doctors = models.ManyToManyfield(Doctor)
    name = models.TextField()

    # __str__ 메서드는 객체 자체를 출력시 출력되는 내용
    def __str__(self):
      return f'{self.pk}번 환자 {self.name}'
```

- Patient model 또는 Doctor model 둘 중 하나에 ManyToManyField 작성

- 필드명은 복수형으로 작성 -> N : 1과 구분 가능

- ManyToManyField 작성 시 중개 테이블 자동으로 생성됨

- 생성된 중개 테이블은 해당 테이블의 id, patient_id, doctor_id로 구성됨

- 예약 생성
  - `patient1.doctors.add(doctor1)` : (순) 참조로 생성

  - `doctor1.patient_set.add(patient2)` : patient에 ManyToManyField를 작성했으므로 역참조로 생성

- 예약 삭제 : `patient1.doctors.remove(doctor1)`, `doctor1.patient_set.remove(patient2)`


### 'through' argument

> 중개 테이블에 추가 데이터(필드)를 사용하여 다대다 관계를 연결하려는 경우 사용

```python
# hospital/models.py

class Patient(models.Model):
    doctors = models.ManyToManyfield(Doctor, through='Reservation')
    name = models.TextField()


class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    symptom = models.TextField()
    reserved_at = models.DateTimeField(auto_now_add=True)
```
- 추가로 기록하고 싶은 필드를 더하여 작성

- 예약 생성
  - 방법1 : `reservation1 = Reservation(doctor=doctor1, patient=patient1, symptom='headache')`, `reservation1.save()`

  - 방법2 : `patient2.doctors.add(doctor1, through_defaults={'symptom': 'flu'})`

- 예약 조회와 삭제는 위의 ManyToManyField와 같음


<br>

## ManyToManyField's Arguments

### related_name

: 참조, 역참조 시에 related manager가 겹치므로 역참조 시 사용하는 manager name을 변경!
```python
class Patient(models.Model):
    doctors = models.ManyToManyfield(Doctor, through='Reservation', related_name='patients')
    name = models.TextField()

# 변경 전
doctor.patient_set.all()
# 변경 후
doctor.patients.all()
```


### through
- 중개 테이블을 직접 작성하는 경우 중개 테이블을 지정하는 인자

- 중개 테이블에 추가 데이터를 사용하는 extra data with a many-to-many relationship에 사용됨

### symmetrical
```python
# 예시

class Person(models.Model):
  friends = madels.ManyToManyField('self', symmetrical=False)
```
- '팔로우'와 같이 동일한 모델에서의 ManyToMany관계에서 사용됨

- 기본값은 True이며 이는 source 모델의 인스턴스가 target 모델의 인스턴스를 참조하면 자동으로 target 모델의 인스턴스도 source 모델 인스턴스를 자동으로 참조하도록 함(대칭)

- 대칭을 원하지 않는 경우 False로 설정

### methods
- add() : 관련 객체 집합에 지정된 객체를 추가, 이미 존재하는 관계에 사용하면 관계가 복제되지 않음

- remove() : 관련 객체 집합에서 지정된 모델 개체를 제거

### 참고
- exists()
  - QuerySet에 결과가 포함되어 있으면 True를 반환, 그렇지 않으면 False를 반환

  - exists는 큰 QuerySet에 있는 특정 개체의 존재와 관련된 검색에 유용

<br>

## 감정표현, 좋아요 실습
```python
# articles/models.py
from django.db import models
from django.conf import settings

class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    emote_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='emote_articles', through='Emote')
    title = models.CharField(max_length=80)                         # 중개 모델을 직접 작성하였으므로 through로 명시
    content = models.TextField(null=False)


class Comment(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_comments')
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    content = models.TextField(null=False)


class Emote(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    emotion = models.CharField(max_length=10)
```
```python
# articles/urls.py
urlpatterns = [
    ...,
    path('<int:article_pk>/emotes/<int:emotion>/', views.emotes, name='emotes'),
    path('<int:article_pk>/comments/<int:comment_pk>/likes/', views.likes, name='likes'),
]
```
```python
# articles/views.py

# 표현할 반응 정의
EMOTIONS = [
    {'label': '좋아요🥰', 'value': 1},
    {'label': '신나요😆', 'value': 2},
    {'label': '화나요😡', 'value': 3},
    {'label': '슬퍼요😭', 'value': 4},
]

def detail(request, article_pk):
    article = Article.objects.get(pk=article_pk)
    emotions = []

    # EMOTIONS의 모든 요소를 개수, 존재여부를 emotions에 담아 context로 보내줌
    for emotion in EMOTIONS:
        if request.user.is_authenticated:
            exist = Emote.objects.filter(article=article, emotion=emotion['value'], user=request.user)
        else:
            exist = None
        emotions.append(
            {
            'label': emotion['label'],
            'value': emotion['value'],
            'count': Emote.objects.filter(article=article, emotion=emotion['value']).count(),
            'exist': exist
            }
        )
    comments = article.comment_set.all()
    comment_form = CommentForm()
    context = {
        'article': article,
        'emotions': emotions,
        'comments': comments,
        'comment_form': comment_form,
    }
    return render(request, 'articles/detail.html', context)


@login_required
def emotes(request, article_pk, emotion):
    article = Article.objects.get(pk=article_pk)
    # 해당 조건의 Emote객체가 있다면 삭제, 없다면 생성 후 detail로 redirect
    filter_emotion = Emote.objects.filter(article=article, user=request.user, emotion=emotion)
    if filter_emotion.exists():
        filter_emotion.delete()
    else:
        Emote.objects.create(article=article, user=request.user, emotion=emotion)
    return redirect('articles:detail', article.pk)


@login_required
def likes(request, article_pk, comment_pk):
    article = Article.objects.get(pk=article_pk)
    comment = Comment.objects.get(pk=comment_pk)
    # 해당 댓글의 like_users에 지금 요청을 보낸 user가 있다면 삭제, 없다면 추가
    if request.user in comment.like_users.all():
        comment.like_users.remove(request.user)
    else:
        comment.like_users.add(request.user)
    return redirect('articles:detail', article.pk)
```
```html
<!-- articles/detail.html -->

...
<!-- 게시글 감정표현 -->

<!-- 로그인된 경우 -->
{% if request.user.is_authenticated %}
  <div class="d-flex">
    {% for emotion in emotions %}
      <!-- 내가 버튼을 누른 경우 -->
      {% if emotion.exist %}
        <form action="{% url 'articles:emotes' article.pk emotion.value %}" method="POST">
          {% csrf_token %}
          <button type="submit" class="btn btn-primary me-3">{{emotion.label}} {{emotion.count}}</button>
        </form>
      <!-- 내가 버튼을 안누른 경우 -->
      {% else %}
        <form action="{% url 'articles:emotes' article.pk emotion.value %}" method="POST">
          {% csrf_token %}
          <button type="submit" class="btn btn-outline-primary me-3">{{emotion.label}} {{emotion.count}}</button>
        </form>
      {% endif %}
    {% endfor %}
  </div>
<!-- 로그인 안된 경우 버튼 disabled -->
{% else %}
  <div class="d-flex">
    {% for emotion in emotions %}
      <button type="button" class="btn btn-outline-primary me-3" disabled>{{emotion.label}} {{emotion.count}}</button>
    {% endfor %}
  </div>
{% endif %}
...

<!-- 댓글 좋아요 -->

<!-- 로그인된 경우 -->
{% if request.user.is_authenticated %}
  {% if request.user in comment.like_users.all %}
    <form action="{% url 'articles:likes' article.pk comment.pk %}" method="POST">
      {% csrf_token %}
      <button type="submit" class="btn btn-danger me-3">♥ {{comment.like_users.count}}</button>
    </form>
  {% else %}
    <form action="{% url 'articles:likes' article.pk comment.pk %}" method="POST">
      {% csrf_token %}
      <button type="submit" class="btn btn-outline-danger me-3">♥ {{comment.like_users.count}}</button>
    </form>
  {% endif %}
<!-- 로그인 안된 경우 버튼 disabled -->
{% else %}
  <button type="button" class="btn btn-outline-secondary me-3" disabled>♥ {{comment.like_users.count}}</button>
{% endif %}
```