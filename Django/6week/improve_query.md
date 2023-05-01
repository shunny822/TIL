# Django Improve Query

> [공식문서](https://docs.djangoproject.com/en/3.2/ref/models/querysets/) 참고

```python
def detail(request, store_pk):
    stores = Store.objects.get(pk=store_pk)
    context = {
      'stores': stores,
    }
    return render(request, stores/detail.html, context)
```
- 기존의 방법과 같이 article 객체만을 넘겨주면 detail template에서 article의 user 참조, article의 comment 참조, 각 comment의 user 참조 등... 같은 쿼리문이 반복적으로 발생하게 됨

- 특정 queryset API를 사용하여 같은 결과에 대한 쿼리 개수를 줄임으로써 성능을 개선할 수 있음

### 예시 모델
```python
class Store(models.Model):
    name = models.CharField(max_length=20)
    like_users = models.ManyToManyField(to=settings.AUTH_USER_MODEL, related_name='like_stores')
    info = models.TextField(blank=True, null=True)


class Review(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    store = models.ForeignKey(Store, on_delete=models.CASCADE)
    emote_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='emote_reviews', through='Emote')
    content = models.CharField(max_length=500)
    rating = models.IntegerField()


class Emote(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    review = models.ForeignKey(Review, on_delete=models.CASCADE)
    emotion = models.CharField(max_length=10)
```

### annotate(*args, **kwargs)

: SQL의 GROUP BY절 활용, 주석을 다는 것과 같이 필드를 추가하여 단일값, 평균, 개수 등의 집계식을 붙일 수 있음

```python
from django.db.models import F, Count

# 단일값
reviews = Review.objects.annotate(
    store_name=F('store__name')  # 외래키 + __ + 원하는 field name
)

# count
stores = Store.objects.annotate(Count('review'))  # 필드명을 지정하지 않으면 review__count(필드+집계함수)
stores = Store.objects.annotate(review_cnt=Count('review')) # 필드명 지정 가능
```
- F( ) : 모델 필드의 값을 참조하는 쿼리표현식으로 python 메모리로 가져오지 않고 DB에서 작업을 수행([공식문서](https://docs.djangoproject.com/en/4.2/ref/models/expressions/#f-expressions))

- Count 외에도 Sum, Avg 등의 함수로 합계, 평균을 구할 수 있음


### aggregate(*args, **kwargs)

: 집계식을 사용하여 값을 반환하나 annotate와 달리 단일한 값을 반환함(전체 쿼리셋에 대한 평균, 합계, 개수 등)

```python
from django.db.models import Count

store = Store.objects.aggregate(review_cnt=Count('review')).get(pk=store_pk)
```


### select_related(*fields)

: 1:1 또는 N:1 참조 관계(내가 참조하는)에서 사용하며 SQL의 INNER JOIN절 활용

```python
reviews = Review.objects.select_related('user') # Review의 외래키
```
- review가 참조하는 user의 데이터도 함께 조회 가능


### prefetch_related(*lookups)

: M:N 또는 N:1 역참조(나를 참조하는 것을 내가 참조) 관계에서 사용하며 SQL이 아닌 Python을 사용한 JOIN

```python
stores = Store.objects.prefetch_related('review_set') # related_manager
```
- store를 참조하고 있는 review도 한번에 조회 가능


### Prefetch(lookup, queryset=None, to_attr=None)

: Prefetch object는 prefetch_related()의 작업을 control하는데 사용됨

```python
reviews = Review.objects.filter(store=store).prefetch_related(
        Prefetch('emote_set', queryset=Emote.objects.filter(emotion=1), to_attr='likes'),
        Prefetch('emote_set', queryset=Emote.objects.filter(emotion=2), to_attr='dislikes'),
    )
```
- lookup : 참조관계 기술

- queryset : prefetch 시 쿼리셋에 filter로 조건을 걸거나 select_related로 다른 필드를 덧붙일 수 있음

- to_attr : prefetch한 데이터를 필드로 설정


### Q() objects

: F()와 같이 Python 메모리가 아닌 DB에서 작업하며 SQL의 조건을 나타냄(|(OR), &(AND)등과 함께 사용 가능, [공식문서](https://docs.djangoproject.com/en/3.2/topics/db/queries/#complex-lookups-with-q))