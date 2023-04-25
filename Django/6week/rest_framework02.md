# Django rest framework 01

Article과 Comment의 예시

## N : 1 관계

### 단일 댓글 생성
```python
# articles/urls.py

urlpatterns = [
    ...,
    path('articles/<int:article_pk>/comments/', views.comments_create),
]
```
```python
# articles/views.py

@api_view(['POST'])
def comments_create(request, article_pk):
    article = Article.objects.get(pk=article_pk)
    serializer = CommentSerializer(data=request.data)
    if serializer.is_valid(raise_exception=True):
        serializer.save(article=article)
        return Response(serializer.data, status=status.HTTP_201_CREATED)
```
- `save(article=article)`: 'article' field는 user에게 입력받는 것이 아니므로 저장 시 해당하는 article 객체를 직접 넣어줘야 함

```python
# articles/serializers.py

class CommentSerializer(serializers.ModelSerializer):
    class Meta:
        model = Comment
        fields = '__all__'
        read_only_fields = ('article',)
```
- __'read_only_fields'__ 를 사용하여 외래키 필드를 읽기 전용필드로 설정해야 함

- 이를 통해 view함수에서 유효성 검사 시 해당 필드를 제외시키면서 데이터 조회 시에는 출력될 수 있도록 함!

<br>

## N : 1 관계의 역참조

### PrimaryKeyRelatedField()
```python
# articles/serializers.py

class ArticleSerializer(serializers.ModelSerializer):
    comment_set = serializers.PrimaryKeyRelatedField(many=True, read_only=True)

    class Meta:
        model = Article
        fields = '__all__'
```
- related manager와 PrimaryKeyRelatedField를 사용하여 역참조 데이터를 가져올 수 있음

- 인자로 읽기전용 속성을 True로 입력해줘야 함

- model에서 related_name 변경 시 위의 comment_set도 같이 변경해줘야 함


### Nested relationships

: 모델 관계 상에서 참조된 대상은 참조하는 대상의 표현에 중첩될 수 있음

```python
# articles/serializers.py

class ArticleSerializer(serializers.ModelSerializer):
  # 원하는 필드만 출력하기 위해 원하는대로 serializer 정의
    class CustomCommentSerializer(serializers.ModelSerializer):
        class Meta:
            model = Comment
            fields = ('id', 'content')
    
    comment_set = CustomCommentSerializer(many=True, read_only=True)
    comment_count = serializers.IntegerField(source='comment_set.count', read_only=True)

    class Meta:
        model = Article
        fields = '__all__'
```
- related manager와 serializer를 사용하여 역참조 데이터를 가져올 수 있음

- 댓글 수 카운트
  - 카운트하기 위해 IntegerField로 필드를 생성

  - 'source'인자에 'related_manager.count'를 입력하여 원하는 값 도출

### 댓글 조회 시 게시글 출력
```python
# articles/serializers.py

class CommentSerializer(serializers.ModelSerializer):
    class ArticleTitleSerializer(serializers.ModelSerializer):
        class Meta:
            model = Article
            fields = ('title',)

    class Meta:
        model = Comment
        fields = '__all__'
```

### [주의사항]

: 특정 필드를 override 혹은 추가한 경우 read_only_fields 동작하지 않음


<br>

## API 문서화

- API 구조를 안내하는 문서

- Swagger : REST 웹 서비스를 설계, 빌드, 문서화 등을 도와주는 오픈소스 소프트웨어 프레임워크

- [공식문서](https://drf-yasg.readthedocs.io/en/stable/readme.html#quickstart) 참고하여 사용

<br>

## 참고

Django shortcuts functions
```python
# articles/views.py
from django.shortcuts import get_objects_or_404, get_list_or_404

...

# 기존 코드
article = Article.objects.get(pk=article_pk)
comment = Comment.objects.get(pk=comment_pk)

# 변경 코드
article = get_objects_or_404(Article, pk=article_pk)
comment = get_objects_or_404(Comment, pk=comment_pk)

# 기존 코드
articles = Article.objects.all()
comments = Comment.objects.all()

# 변경 코드
articles = get_list_or_404(Article)
comments = get_list_or_404(Comment)
```
- 'get_objects_or_404()' : 모델 manager objects에서 get()을 호출하지만, 해당 객체가 없을 때 기존의 DoesNotExist 예외 대신 Http404를 raise함

- 'get_list_or_404()' : 모델 manager objects에서 filter()의 결과를 반환하고 해당 객체 목록이 없을 때 Http404를 raise함