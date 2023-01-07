# 컴퓨터 프로그래밍 언어
 ### - 컴퓨터 : caculation(조작) + remember(저장)
 ### - 프로그래밍 : 명령어의 모음(집합)

<br>
<br>


# Python 기초
## 특징
 - easy to learn : 문법이 간결

 - expressive language : C나 java보다 간결한 표현 가능

 - 크로스 플랫폼 언어 : 다양한 os에서 실행 가능

 - 인터프리터 언어(interpreter) : 컴파일 없이 바로 실행 가능

 - 객체 지향 프로그래밍(Object Oriented Programing)

<br>

## 객체와 변수
- 객체(Object) : python이라는 박스에 담는 모든 것! 객체 = 사물, things

- 변수(Variable) : 객체를 참조하기 위해 사용되는 이름, 다른 객체를 언제든 할당 가능

<br>

## 변수
- 할당 연산자(=)를 통해 값을 할당(assingment)

- type() : 변수에 할당된 값의 타입

- id() : 변수에 할당된 값의 고유한 identity이며 메모리 주소

### 변수 할당
- 같은 값을 동시에 할당 가능
```python
x = y = 1004
```
- 다른 값을 동시에 할당 가능
```python
x, y = 1, 2
```
- 변수의 값을 서로 바꾸는 방법1
```python
tmp = x
x = y
y = tem
```
- 방법2, python에서 가능!
```python
x, y = y, x
```

### 식별자(Identifiers)
- python 객체를 식별하는데 사용하는 이름

- 규칙
  - 영어, 언더스코어_, 숫자만을 사용
  - 첫글자에 숫자 X
  - 예약어, 내장함수, 모듈 이름 사용 불가능
  - snake_case 방식 : 단어 연결 시 언더스코어 사용
    cf) 자바는 camel case

<br>

## 객체
<br>

### 자료형(Data Type)

<br>

Type : 객체의 종류

<br>

1. 수치형(Numeric Type)
  - 정수(Int) : 모든 정수의 타입은 int(long없음), 오버플로우 발생하지 않음

     cf) overflow : 데이터 타입별 사용가능한 메모리 크기를 넘어서는 상황
  - 실수(Float) : 정수가 아닌 모든 실수, 부동소수점, 지수표기법, 복소수 참고만

      cf) 값을 비교하는 과정에서 실수일 경우 주의(feat.부동소수)
      ```python
      3.14 - 3.02 == 0.12000000000001
      ```
      cf) 복소수(Complex) : 허수부를 j로 표현

<br>

2. 불린형(Boolean Type)
  - True/False의 값을 가짐(★첫글자 대문자)

  - True는 1, False는 0

  - 비교/논리 연산 시 사용

<br>

3. 연산자(Operator)
  - 산술 연산자(Arithmetic Operator) : +, -, * %, /, //(몫), **(거듭제곱)

  - 복합 연산자(In=place Operator) : 연산과 할당이 함께 이루어짐(+=, //= 등)

  - 비교 연산자(Comperison Operator) : <, <=, >, >=, ==, !=

  - 논리 연산자(Logical Operator) : A and B, A or B, Not(True를 False로, False를 True)

<br>

4. 컨테이너(Container)

    : 여러 개의 값을 담울 수 있는 객체, 서로 다른 자료형 저장 가능

  - 시퀀스형 컨테이너(Sequence Container) : 문자열, 리스트, 튜플, 레인지

  - 컬렉션/비시퀀스 : 세트(유일한 값들의 모음), 딕셔너리(키-값들의 모음)
    - 문자열(String Type) : 모든 문자의 타입, ''나 ""를 사용하여 표기, 변경불가
    - 리스트(List)
      - 변경가능한 값들의 나열된 자료형
      - 다른 타입의 요소 가질 수 있음, 반복 가능
      - 변경 가능 : 추가는 .append(), 삭제는 .pop()
      - s = [ 수현, 산하, ...] 형태
      - Positive index :  0  1  2  3  4  5
      - Negative index : -6 -5 -4 -3 -2 -1

    cf) 시퀀스형 주요 연산자
    - s[i] : s의 i번째 항목(0부터 시작)
    - s[i:j] : i이상 j`미만` 슬라이스
    - s[-n:] : 뒤에서 n개 슬라이스
    - s[i:j:k] : i~j미만 + 스텝k 슬라이스
    - s + t : 이어붙이기
    - s * n or n * s : s를 n번 더함
    - x in s : x가 s의 항목 중 하나와 같으면 True, 아니면 False
    - x not in s : 위와 반대
    - len(s) : s의 길이
    - min(s) : s의 가장 작은 항목
    - max(s) : s의 가장 큰 항목 

    cf) Escape sequence
    - \n, \t(탭), \r(캐리지리턴), \0(Null), \\(\), \', \"

<br>

5. None : 값이 없음을 나타내는 자료형


