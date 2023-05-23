# Django multiple Images/Files input

사용자로부터 이미지 또는 파일을 여러 개 입력받을 때 사용할 수 있는 방법을 정리해보았다. 아래의 예시는 이미지 예시이다.

## Model 설정
```python
from imagekit.models import ProcessedImageField
from imagekit.processors import ResizeToFill

class Review(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    store = models.ForeignKey(Store, on_delete=models.CASCADE)
    content = models.CharField(max_length=500)
    rating = models.IntegerField()
    created_at = models.DateTimeField(auto_now_add=True)


class ReviewImage(models.Model):
    review = models.ForeignKey(Review, on_delete=models.CASCADE)

    def review_image_path(instance, filename):  # 이미지 저장 경로 설정
        return f'reviews/{instance.review.user.username}/{filename}'

    image = ProcessedImageField(upload_to=review_image_path, blank=True, null=True,
                                processors=[ResizeToFill(100,100)],
                                format='JPEG',
                                options={'quality': 80})
    
    def delete(self, *args, **kargs):
        if self.image:
            os.remove(os.path.join(settings.MEDIA_ROOT, self.image.path))
            dir_path = os.path.dirname(os.path.join(settings.MEDIA_ROOT, self.image.name))
            if not os.listdir(dir_path):
                os.rmdir(dir_path)
        super(ReviewImage, self).delete(*args, **kargs)
```
- 가게에 대한 리뷰를 작성하는 Review 모델과, 리뷰에 대한 이미지를 저장하는 ReviewImage 모델을 정의

- Review모델과 ReviewImage 모델은 N : 1 관계로 하나의 리뷰에 여러 개의 이미지가 저장될 수 있도록 함

- delete 함수는 저장된 이미지를 제거하는 역할을 함(DB에는 이미지 경로만 저장되어있어 객체를 삭제해도 이미지 자체는 지워지지 않으므로)

- 이를 간단하게 해주는 django-cleanup 존재([공식문서](https://pypi.org/project/django-cleanup/))

### django imagekit

> 이미지가 원하는 크기와 quality로 저장될 수 있도록 가공을 도와주는 모듈(서버에 과도하게 큰 용량의 이미지가 저장되는 것 방지)

[공식문서](https://django-imagekit.readthedocs.io/en/latest/)

```
$ pip install Pillow
$ pip install django-imagekit
```
- 이미지 필드 사용 시 필요한 Pillow와 우리가 사용하려는 django-imagekit 설치

```python
from imagekit.models import ProcessedImageField
from imagekit.processors import ResizeToFill
```
- 사용하려는 app의 models.py에 import

```python
image = ProcessedImageField(upload_to=review_image_path, blank=True, null=True,
                            processors=[ResizeToFill(100,100)],
                            format='JPEG',
                            options={'quality': 80})
```
- imagekit을 사용하려는 field에 이미지의 크기, quality 등의 스펙 설정


<br>

## Form 작성
```python
from .models import Review, ReviewImage

class ReviewImageForm(forms.ModelForm):
    image = forms.ImageField(
        label='리뷰 이미지 업로드',
        widget=forms.ClearableFileInput(
            attrs={
                'class': 'form-control',
                'multiple': True,
            },
        ),
        required = False,
    )
    class Meta:
        model = ReviewImage
        fields = ('image',)
```
- form 위젯의 속성에 ''multiple': True'를 입력하면 여러 개의 값을 받을 수 있음

- bootstrap form-control을 사용하면 필수 항목이 되므로 필수가 아닌 경우 'required = False' 입력

- form을 작성하지 않고 template에서 직접 input tag를 입력하여 값을 입력받을 수 있음(이때도 마찬가지로 multiple 속성 지정해야 함 + name 지정 가능)


<br>

## view 함수에서 사용
```python
def create(request, store_pk):
    store = Store.objects.get(pk=store_pk)

    if request.method == 'POST':
        review_form = ReviewForm(request.POST)
        files = request.FILES.getlist('image')
        if review_form.is_valid():
            review = review_form.save(commit=False)
            review.user = request.user
            review.store = Store.objects.get(pk=store_pk)
            review.save()
            
            for file in files:
                ReviewImage.objects.create(review=review, image=file)
            return redirect('stores:detail', store_pk)
    else:
        review_form = ReviewForm()
        review_image_form = ReviewImageForm()
    context = {
        'review_form': review_form,
        'review_image_form': review_image_form,
        'store': store,
    }
    return render(request, 'reviews/create.html', context)
```
- 사용자로부터 입력받은 이미지는 `request.FILES'에서 getlist메서드를 이용해 받아올 수 있음

- getlist로 받을 때 'image'는 field명(input tag 직접 작성 시 name 지정 가능)

- 이미지 파일들은 for문을 통해 각각의 ReviewImage 객체로 생성