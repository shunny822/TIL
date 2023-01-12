# 컬렉션 Collection

## 딕셔너리 Dictionary

- 키-값(key : value) 쌍으로 이루어진 모음(collection)

- 키는 불변 자료형만 가능, 값은 모든 형태 가능

- 변경 가능하며(mutable), 반복 가능함(iterable)

- 기본형태
  ```python
  dic = {key : value, key : value, key : value ...}  # 중괄호 
  ```

### 키-값 추가 및 변경
```python
score = {'홍길동': 100, '최수현': 90}
score['홍길동'] = 50  # {'홍길동': 50, '최수현': 90} 변경
score['김철수'] = 80  # {'홍길동': 50, '최수현': 90, '김철수': 80} 추가
```

### 키-값 삭제
```python
score = {'홍길동': 50, '최수현': 90, '김철수': 80}
score.pop('홍길동')
# score = {'최수현': 90, '김철수': 80}
```

### 딕셔너리 순회
- 딕셔너리는 기본적으로 key순회
```python
grades = {'john': 80, 'eric': 90}

for name in grades:
    print(name)    # john, eric
```
```python
grades = {'john': 80, 'eric': 90}

for name in grades:
    print(name, grades[name])  # john 80, eric 90
```

- 메서드 사용
  - keys() : key로 구성된 결과
  - values() : value로 구성된 결과
  - item() : (key, value)의 튜플로 구성된 결과
  ```python
  grades = {'john': 80, 'eric': 90}
  print(grades.keys())    # dict_keys(['john', 'eric'])
  print(grades.values())  # dict_values([80, 90])
  print(grades.items())   # dict_items([('john', 80), ('eric', 90)])
  ```

<br>
<br>

# 모듈 module

> __<모듈> 다양한 기능을 하나 파일로__
>
> __<패키지> 다양한 파일을 하나의 폴더로__
>
> __<라이브러리> 다양한 패키지를 하나의 묶음으로__
>
>  라이브러리 관리자 : pip

<br>

## <파이썬 표준 라이브러리>
모듈 사용을 위해 import로 가져와야 함
ex) import random

- random : 숫자/수학 모듈, 임의의 숫자 선택/생성 등
  - random.randint(a, b) : a이상 b 이하 임의의 정수 반환
  - random.choice(seq) : 시퀀스 임의의 요소 반환(비어있는 경우 indexError)
  - random.shuffle(seq) : 시퀀스 섞음
  - random.sample(population, k) : 어떤 집단에서 k길이의 무작위 리스트 반환
  ```python
  lotto = range(1, 46)
  print(sorted(random.sample(lotto, 6)))
  ```

- datetime : 날짜와 시간을 조작
  - datetime.date(year, month, day) : 모든 인자 필수
  - datetime.date.today() : 현재 지역 날짜 반환
  - datetime.datetime.today() : 현재 지역 datetime을 반환, now()로 타임존 설정 가능
  ```python
  print(datetime.datetime.now())
  # 2023-01-05 11:05:21.75826
  print(datetime.date(2023, 1, 5))
  # 2023-01-05
  ```
  ```python
  today = datetime.date(2023, 1, 5)
  end = datetime.date(2023, 6, 14)
  print(f'우리가 개발자가 되는 시간 {end - today}')
  ```

- os : OS(운영체제) 조작을 위한 인터페이스 제공
  - os.listdir(path='.') : 디렉토리 항목을 리스트로 반환
  ```python
  print(os.listdir())
  ```
  - os.mkdir(path) : path라는 디렉토리 생성
  - os.chdir(path) : path를 변경