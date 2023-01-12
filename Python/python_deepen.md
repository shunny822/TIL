클래스 메서드, 스태틱 메서드, 상속은 django할때 다시!

# 파이썬 심화

## 조건 표현식(Conditional Expression)
조건에 따라 값을 할당할 때 활용
```python
<true인 경우 값> if <expression> else <false인 경우 값>
```

<예시>
```python
# 1
if num >= 0:
  value = num
else:
  value = -num
# 조건표현식
value = num if num >= 0 else -num

# 2
num = 2
result = '홀' if num % 2 else '짝'
```


## List Comprehension
표현식과 제어문을 통해 특정한 값을 가진 리스트를 간결하게 생성
```python
[<expression> for <변수> in <iterable>]
[<expression> for <변수> in <iterable> if <조건식>]
```

<예시>
```python
# 1~3의 세제곱 결과가 담긴 리스트 만들기
cubic_list = []
for number in range(1, 4):
  cubic_list.append(number**3)
print(cubic_list)

# list comprehension
[number**3 for number in range(1, 4)]
```


## Dictionary Comprehension
표현식과 제어문을 통해 특정한 값을 가진 딕셔너리를 간결하게 생성
```python
{key: value for <변수> in <iterable>}
{key: value for <변수> in <iterable> if <조건식>}
```

<예시>
```python
# 1~3의 세제곱 결과가 담긴 딕셔너리 만들기
cubic_dict = {}
for number in range(1, 4):
  cubic_dict[number] = number ** 3
print(cubic_dict)

#dictionary comprehension
cubic_dict = {}
{number: number ** 3 for number in range(1, 4)}
```


## lambda 함수
- 표현식을 계산한 결과값을 반환하는 함수로 이름이 없어 익명함수라고도 불림
- return문, 간편조건문 외의 조건문, 반복문 가질 수 없음
- 함수를 정의하여 사용하는 것보다 간결
- def를 사용할 수 없는 곳에서도 사용 가능
```python
lambda [parameter] : 표현
```

<예시>
```python
numbers = [[2, 1], [1, 3]]
print(list(map(sorted, numbers)))
# [[1, 2], [1, 3]] 
# map의 첫번째 칸에 함수, 메서드는 arg필요해서 X

numbers = [2, 4]
# 2로 나눈 몫으로 구성하려면

# 1
def div_2(n):
  return n//2
print(list(map(div_2, numbers)))

# 2
print(list(map(lambda n: n//2, numbers)))
```

<br>
<br>

# 파이썬 버전별 업데이트

### Type annotation
- 동적 타입 언어인 파이썬에서 각 변수(3.6+)/함수(3.5+)마다 Type에 대한 설명 덧붙임
- 정적 타입으로 변경되진 않지만, IDE/텍스트 에디터를 통해 경고 확인, 코드 작성 시 도움 받을 수 있음
```python
hello: str = "hello world!"

def add(x: int, y: int) -> int:
  return x + y

result: int = add(7, 4)
```

### Positional-only parameters
- 함수를 정의할 때 어떻게 호출해야 하는지를 함께 지정
- a, b는 위치만 / c, d는 위치 및 키워드 모두 / e, f는 키워드만
```python
def f(a, b, /, c, d, *, e, f):
  print(a, b, c, d, e, f)
```