# 함수 Function
## 특징
- Abstraction : 복잡한 내용을 숨기고 기능에 집중, 가독성, 생산성
- Decomposition : 기능을 분해, 재사용

__input → ■ → output__

<br>

## 함수 기초
### 함수의 정의
- 특정한 기능을 하는 코드의 조각(묶음)
- 특정 기능의 코드를 매번 작성 X, 필요 시 호출하여 사용
- 사용자 함수 : 구현되어 있는 함수가 없는 경우 사용자가 직접 함수 작성 가능


### 내장함수(Built-in Fucntion)
- print(sep=' ', end='\n') : 출력
  - sep=' ' : sep는 기본값이 space(,사이)
  - end='\n' : end는 기본값이 줄바꿈(모두 출력 후 마지막)

- len(s) : 시퀀스 또는 컬렉션 객체의 길이 반환

- sum(iterable, start=0)
  - iterable(일반적으로 숫자)의 합 + start 값의 합계 반환
  - iterable을 왼쪽에서부터 더하며 start의 기본값은 0

- max(iterable) : iterable에서 가장 큰 항목 반환

- min(iterable) : iterable에서 가장 작은 항목 반환

- abs(x)
  - 숫자의 절댓값 반환
  - 인자 : 정수, 실수, __abs__()를 구현하는 객체

- divmod(a, b) : 두 수를 받아 몫과 나머지 반환
  ```python
  print(divmod(2, 1.2))
  # (1.0, 0.8)
  ```

- pow(base, exp, mod=None) : (base**exp)%mod
  - base ** exp 반환
  - mod : 기본값은 None, base**exp를 mod로 나눈 나머지 반환

- round(number, ndigit=None)
  - number를 ndigit의 정밀도로 반올림한 값 반환
  - ndigit의 기본값은 None, 값이 없는 경우 number를 정수로 반올림
  ```python
  print(round(2.19872, 4))
  # 2.1987
  print(round(2.19872, 2))
  # 2.2
  ```
- sorted(seq) : sequence를 정렬
  ```python
  num = [1, 6, 3]
  print(sorted(num))  # [1, 3, 6]
  ```

- all(iterable) : iterable의 모든 요소가 참이거나 비어있으면 True 반환

- any(iterable) : iterable의 요소 중 하나라도 참이면 True, 비어있으면 False 반환

- map(function, iterable)
  - 순회 가능한 객체의 모든 요소에 함수를 적용하고 그 결과를 map object로 반환
  ```python
  numbers = ['1', '2', '3']
  new_numbers = map(int, numbers)
  print(new_numbers)  # <map object at 0x000~~~~>
  print(list(new_numbers))  # [1, 2, 3]
  ```

- 기타 함수
  - bin(x) : 정수를 '0b' 접두사가 붙은 이진 문자열로 반환
  - hex(x) : 정수를 '0x' 접두사가 붙은 16진수 문자열로 반환
  - oct(x) : 정수를 '0o' 접두사가 붙은 8진수 문자열로 반환
  - ord(c) : 유니코드 문자 c에 대응되는 유니코드 숫자로 반환
  - chr(i) : 유니코드 숫자가 정수 i에 대웅되는 문자를 반환