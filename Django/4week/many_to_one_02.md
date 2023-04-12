# Many to one relationships 02

Article(N) - User(1) & Comment(N) - User(1)

## 모델 관계 설정

- N대 1 관계에서 Article과 Comment가 User를 참조하므로 Article과 Comment에 User를 참조할 FK 작성

### Model에 user FK 추가
```python
# articles/models.py

# User 모델 참조를 위해 settings import
from django.conf import settings

class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    title = models.CharField(max_length=10)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)


class Comment(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    content = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```
- User 모델을 참조하는 2가지 방법
  - `get_user_model()` : 반환 값이 'User Object'(객체)이며 models.py가 아닌 다른 모든 곳에서 참조할 때 사용됨

  - `settings.AUTH_USER_MODEL` : 반환 값이 'accounts.User'(settings.py에서 설정한 값) 문자열이며 models.py의 모델 필드에서 참조 시 사용됨

- django 내부적으로 모델 생성이 먼저인데 어떠한 모델 생성을 위해 User model을 참조해야하나 아직 User 모델도 생성이 되지 않은 단계이므로 User model 객체가 반환되는 get_user_model이 아니라 settings.AUTH_USER_MODEL을 사용해야 함


<br>

## Article과 User

- 기존의 코드에 참조하는 user값을 채워주는 코드가 필요! (Comment와 User도 마찬가지)

### Article CREATE
```python
# article/views.py

@login_required
def create(request):
    if request.method == 'POST':
        form = ArticleForm(request.POST)
        if form.is_valid():
            article = form.save(commit=False)
            article.user = request.user
            article.save()
            return redirect('articles:detail', article.pk)
    else:
        form = ArticleForm()
    context = {
        'form': form,
    }
    return render(request, 'articles/create.html', context)
```
- save함수에서 'commit=False'를 이용하여 인스턴스만 생성, 이후 user값을 부여하고 DB에 저장


### Article READ
`{{ article.user }}`를 이용해 작성자 출력 가능


### Article UPDATE
```python
# article/views.py

@login_required
def update(request, article_pk):
    article = Article.objects.get(pk=article_pk)
    # 현재 요청의 user가 게시글의 user인 경우만 수정 가능하도록
    if request.user == article.user:
        if request.method == 'POST':
            form = ArticleForm(request.POST, instance=article)
            if form.is_valid():
                form.save()
                return redirect('articles:detail', article.pk)
        else:
            form = ArticleForm(instance=article)
    else:
        return redirect('articles:index')
    context = {
        'article': article,
        'form': form,
    }
    return render(request, 'articles/update.html', context)
```
- 현재 요청의 user가 수정하려는 글의 user가 아닌 경우 index로 돌아가도록 설계

```html
<!-- detail template -->
...
{% if request.user == article.user %}
  <form action="{% url 'articles:delete' article.pk  %}" method="POST">
    {% csrf_token %}
    <input type="submit" value="삭제">
  </form>
  <a href="{% url 'articles:update' article.pk %}">[UPDATE]</a>
{% endif %}
```
- 현재 요청의 user와 게시글의 user가 같은 경우만 삭제, 수정 버튼이 나타나도록 설계


### Article DELETE
```python
# article/views.py

@login_required
def delete(request, artilce_pk):
    article = Article.objects.get(pk=artilce_pk)
    if request.user == article.user:
        article.delete()
    return redirect('articles:index')
```
- 현재 요청의 user와 게시글의 user가 같은 경우만 삭제

<br>

## Comment와 User

### Comment CREATE
```python
# article/views.py

@login_required
def comment_create(request, article_pk):
    article = Article.objects.get(pk=article_pk)
    comment_form = CommentForm(request.POST)
    if comment_form.is_valid():
        comment = comment_form.save(commit=False)
        comment.article = article
        comment.user = request.user
        comment.save()
        return redirect('articles:detail', article_pk)
    context = {
        'article': article,
        'comment_form': comment_form,
    }
    return render(request, 'articles/detail.html', context)
```
- save함수에서 'commit=False'를 이용하여 인스턴스만 생성, 이후 user와 article값을 부여하고 DB에 저장

### Comment READ
`{{ comment.user }}`를 이용해 작성자 출력 가능


### Comment DELETE
```python
# article/views.py

@login_required
def comment_delete(request, article_pk, comment_pk):
    comment = Comment.objects.get(pk=comment_pk)
    if request.user == comment.user:
        comment.delete()
    return redirect('articles:detail', article_pk)
```
- 현재 요청의 user와 댓글의 user가 같은 경우만 삭제

```html
<!-- detail template -->

{% if request.user == comment.user %}
  <form action="{% url 'articles:comment_delete' article.pk comment.pk %}" method="POST">
    {% csrf_token %}
    <input type="submit" value="삭제">
  </form>
{% endif %}
```
- 현재 요청의 user와 댓글의 user가 같은 경우만 삭제버튼이 나타나도록 설계

<br>

## 참고

### onclick
- html tag에는 onclick이라는 속성이 존재

- 'confirm('')'이라는 자바스크립트 함수와 사용할 경우 해당 동작을 한번 더 확인하는 과정을 거침

- ex) `<input type="submit" value="삭제" onclick="return confirm('삭제하시겠습니까?')">