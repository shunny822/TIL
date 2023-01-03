# 형변환(Typecasting)
### 1. 암시적 형 변환(Implicit Typecasting)
- 파이썬 내부적으로 자료형 변환
  ```python
  True + 3  # = 4
  3 + 5.0   # = 8.0
  3 + 4j + 5  # = (8+4j)
  ```

### 2. 명시적 형 변환(Explicit Typecasting)
- str, float → int
- str, int → float
- int, float, list tuple, dict → str

<br>
<br>

# String Formatting

### %-formatting
```python
name = 'Kim'
score = 4.5

print('Hello, %s' % name)
print('내 성적은 %d' % score)
print('내 성적은 %f' % score)  # 타입에 따라
```

### f-string
```python
name = 'Kim'
score = 4.5
print(f'Hello, {name}! 성적은 {score}')  #Hello, Kim! 성적은 4.5

pi = 3.141592
print(f'원주율은 {pi:.3}. 반지름이 2일 때 원의 넓이는 {pi*2*2}')
# 원주율은 3.14. 반지름이 2일 때 원의 넚이는 12.566368
```

<br>
<br>

# Range
- range(n, m, s) : n부터 n-1까지 s만큼 증가
- 숫자 시퀀스를 나타내기 위해 사용
- 변경 불가능 immutable하며 반복가능함 iterable
```python
range(3)  # = range(0, 3)
list(range(0, 3))  # [0, 1, 2]
type(range(3))  # <class 'range'>

list(range(1, 5, 2))  # [1, 3]
# 역순
list(range(6, 1, -1))  # [6, 5, 4, 3, 2]
```

<br>
<br>

# 제어문(Control Statement)
- 위에서 아래로 순차적 명령 수행

- 특정 상황에 따라 코드 선택적 실행/반복하는 제어 필요

- 제어문은 flow chart로 표현 가능

<br>
<br>

## 조건문(Conditional Statement)
### 기본 조건문 형식

else는 선택적 사용
```python
if  expression :
  ~ code ~
else :
  ~ code ~
```

### 복수 조건문
```python
if  expression :
  ~ code ~
elif  expression :
  ~ code ~
elif  expression :
  ~ code ~
else :
  ~ code ~
```

### 중첩 조건
```python
if  expression :
  ~ code ~
  if expression :
    ~ code ~
else :
  ~ code ~
```

<br>

## 반복문
### for문
- 시퀀스를 포함한 순회가능한 객체(iterable)의 모든 요소 순회
- 순회가능한 객체 : string, tuple, list, range, set, dictionary
- 기본 형식
  ```python
  for 변수명 in iterable :
    ~ code ~
  ```

<예시>
```python
chars = input()  # apple

for index in range(len(chars)) :
  print(chars[index], end=" ")
  # a p p l e 
```

cf) print 시 
기본값은 모두 출력 후 \n이며 변경 가능
- end = "" → 모두 출력 후 마지막에 붙는 것
- sep = "" → 각각의 인자 출력 시 마다 붙는 것

### enumerate()
- index와 원소 동시에 접근 가능
  ```python
  list(enumarate(['A', 'B', 'C']))
  # = [(0, 'A'), (1, 'B'), (2, 'C')]
  ```
- in의 뒷부분인 iterable을 enumerate()로 감싸줌
  ```python
  for i, letter in enumerate('apple') :
    print(i, letter)
  # 0, a \n 1, p \n 2, p \n, 3, l \n 4, e
  ```
- 시작 인덱스 변경하고 싶을 때 start= 사용
  ```python
  for i, word in enumerate(['a', 'b', 'c'], start=1) :
    print(i, word)
  # 1, a \n 2, b \n 3, c
  ```


### 반복문 제어
- break : 해당 반복문 종료

- continue : continue 이후의 코드 블럭 실행하지 않고 다음 반복 수행

- for-else : 끝까지 반복문 실행된 이후에 실행됨
  ```python
  for char in 'apple' :
    if char == 'b' :
      print('b!')
      break
  else :
    print('b가 없습니다.')
  ```