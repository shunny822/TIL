# Django Argon2 hasher

## Argon2 란?

서비스에서 비밀번호를 저장할 때는 정보 보호를 위해 암호화/hashing 하여 저장함

Argon2는 차세대 해싱 알고리즘으로 2015년 Password Hashing Competition에서 우승하였으며 이 라이브러리를 사용함으로써 쉽게 보안 강화 가능


### 사용 방법

1. argon2 설치
    ```bash
    $ pip install argon2-cffi
    ```

2. settings.py 설정
    ```python
    PASSWORD_HASHERS = [
        "django.contrib.auth.hashers.Argon2PasswordHasher", # Argon2가 가장 위에 있어야 함
        "django.contrib.auth.hashers.PBKDF2PasswordHasher",
        "django.contrib.auth.hashers.PBKDF2SHA1PasswordHasher",
        "django.contrib.auth.hashers.BCryptSHA256PasswordHasher",
        "django.contrib.auth.hashers.ScryptPasswordHasher",
    ]
    ```

3. view함수에서 사용
    ```python
    from argon2 import PasswordHasher

    ph = PasswordHasher()
    hash = ph.hash('비밀번호')
    ph.verify(hash, '비밀번호')
    ph.check_need_rehash(hash)
    ```
    - hash(password) : input을 hashing하여 반환

    - verify(hash, password) : hash(hashing된 값)와 password(보통 사용자 input)이 일치하면 True, 아니면 False를 반환

    - check_need_rehash(hash) : input이 outdated된 값이면 True, 아니면 False를 반환


### 적용

<암호가 있는 그룹 생성>
```python
from argon2 import PasswordHasher as ph

def group_create(request):
    form = GroupForm(request.POST)
    password1 = request.POST.get('password1') # 새 암호
    password2 = request.POST.get('password2') # 새 암호 확인

    if form.is_valid() and password1 == password2:
        group = form.save(commit=False)
        group.chief = request.user
        group.password = ph().hash(password1)  # 비밀번호 hashing해서 저장
        group.save()
        return redirect('groups:group_detail', group.pk)
    else:
        messages.error(request, '정보를 정확하게 입력하세요.')
        return redirect('groups:index')
```

<암호 변경>
```python
old_password = request.POST.get('old-password')
password1 = request.POST.get('new-password1') # 새 암호
password2 = request.POST.get('new-password2') # 새 암호 확인

# 기존 암호의 hash값과 사용자에게 받은 input이 일치하는지 확인
if ph().verify(group.password, old_password) and password1 == password2:
    group.password = ph().hash(password1)  # 비밀번호 hashing해서 저장
    group.save()
```


### 참고 문서
- [Django 문서](https://docs.djangoproject.com/en/4.2/topics/auth/passwords/)

- [pypi 문서](https://pypi.org/project/argon2-cffi/)
