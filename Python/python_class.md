# 사용자 정의 클래스

## 객체 지향 프로그래밍

: 프로그램을 여러 개의 독립된 객체들과 그 객체들 간의 상호작용으로 파악하는 프로그래밍 방법

프로그램을 유연하고 변경이 용이하게 만들어 대규모 소프트웨어 개발에 많이 사용됨

배우기 쉽고, 소프트웨어 개발과 보수를 간편하게 하며, 직관적인 코드 분석 가능

### 객체(Object)의 특징

`객체는 특정 타입의 인스턴스 이다.`

- 타입(type) : 어떤 연산자(operator)와 조작(method)이 가능한가?

- 속성(attribute) : 어떤 상태(data)를 가지는가?

- 조작법(method) : 어떤 행위(함수)를 할 수 있는가?


## 클래스와 인스턴스

`객체의 틀(클래스)을 가지고, 객체(인스턴스)를 생성한다.`

- 기본문법
  ```python
  # 클래스 정의
  class MyClass:

  # 인스턴스 생성
  my_instance = MyClass()

  # 메서드 호출
  my_instance.my_method()
  ```

- 클래스 : 객체들의 분류

- 인스턴스 : 하나하나의 실체/예

- 속성 : 특정 데이터 타입/클래스의 객체들이 가지게 될 상태/데이터를 의미

- 메서드 : 특정 데이터 타입/클래스의 객체에 공통적으로 적용 가능한 행위(함수)

- 예시
  ```python
  class Person:
    def greeting(self):
      return 'hi..'
  
  iu = Person()
  print(type(iu))  # class __main__.Person
  print(iu.greeting())  # hi..
  isinstance(iu, Person) # True
  # iu가 Person 클래스의 인스턴스인지
  ```

### 객체 비교
- `==` : 변수가 참조하는 객체가 동등한 경우 True
- `is` : 두 변수가 동일한 객체를 가리키는 경우 True(주소값이 같아야 함)
  ```python
  a = [1, 2, 3]
  b = [1, 2, 3]
  print(a == b, a is b) # True False

  a = [1, 2, 3]
  b = a
  print(a == b, a is b) # True True
  ```

### 인스턴스

- 인스턴스 변수
  - 인스턴스의 속성(attribute), 각 인스턴스들의 고유한 변수
  - 생성자 메서드에서 self.__로 정의
  ```python
  class Person:
      def __init__(self, name):
        self.name = name  # 인스턴스 변수 의

  suhyun = Person('suhyun')
  print(suhyun.name) # suhyun

  suhyun.name = 'shyunny'
  print(suhyun.name) # shyunny  # 인스턴스 변수 접근 및 할당
  ```


### 메서드
- 인스턴스에 적용하는 메서드

- 호출 시 첫번째 인자로 인스턴스 자기 자신(self)이 전달됨

- 따라서 첫 번째 인자는 self로 정의('self'이름은 암묵적 규칙)

- 생성자(constructor) 메서드
  - 인스턴스 객체가 생성될 때 `__init__` 메서드자동으로 호출
  - 인스턴스 변수들의 초기값 설정

- 소멸자(destructor) 메서드 : 인스턴스 객체가 소멸되기 직전에 호출

- 매직 메서드
  - Double underscore(__)가 있는 메서드는 특수한 동작을 위해 만들어짐
  - 특정 상황에 자동으로 불리는 메서드
  - 예시
    ```python
    __str__(self), __len__(self), __repr__(self)
    __lt__(self, other), __le__(self, other), __eq__(self, other)
    __gt__(self, other), __ge__(self, other), __ne__(self, other)
    ```
    - `__str__` : print함수 호출 시 자동호출 되어 해당 객체의 출력 형태 지정
    - `__gt__` : > (greater than), True/False

<예시>
```python
# 소개팅
class Person:

  def __init__(self, name, age):
    self.name = name
    self.age = age
  
  def greeting(self):
    return f'{self.age}살이구요 {self.name}입니다.'

  def __gt__(self, other):
    if self.age > other.age:
      return self
    else:
      return other
  
  def __str__(self):
    return f'{self.name} ({self.age})'

p1 = Person('재용', 50)
p2 = Person('유영', 48)

print(p1.name)
print(p1.greeting())
print(p1 > p2)  # 연산자가 나오면 __gt__ 메서드 호출됨
print(p1)
```