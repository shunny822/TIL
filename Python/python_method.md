# 튜플 Tuple

- 불변한 값들의 나열

- 순서를 가지며, 서로 다른 타입의 요소를 가질 수 있음
- 변경 불가능(immutable), 반복 가능(iterable)
- 소괄호(()) 혹은 tuple()을 통해 생성
- 인덱스로 값에 대한 접근 가능, 값 변경 불가능(추가/삭제 포함)
- tuple은 우리가 만들어쓰는 것 보다 파이썬 내부적으로 사용

<br>
<br>

# 세트 Set

- 유일한 값들의 모음(collection)

- 순서가 없고 중복된 값 없음
- 변경 가능(mutable), 반복 가능(iterable)

### 생성
- 중괄호({}) 또는 set()을 통해 생성
- {}은 딕셔너리가 되기에 빈 set을 만들기 위해서는 set()사용
- 순서가 없어 별도의 값에 접근 불가능

### 추가/삭제
- .add()로 값 추가
- .remove()로 값 삭제

### 활용
- 다른 컨테이너에서 중복된 값 제거
- 이후 순서 무시되므로 순서가 중요한 경우 사용 불가
```python
my_list = ['서울', '서울', '대전', '광주']
len(set(my_list))  # 3
set(my_list)  # ['서울', '대전', '광주']
```

<br>
<br>

# 메서드 Method
`타입.메서드()`

## 객체의 타입별 메서드

### 문자열 탐색/검증
- s.find(x) : x의 첫 번째 위치를 반환, 없으면 -1 반환

- s.index(x) : x의 첫 번째 위치를 반환, 없으면 `오류` 발생
- s.`is`alpha() : 알파벳 문자 여부
- s.`is`upper() : 대문자 여부
- s.`is`lower() : 소문자 여부
- s.`is`title() : 타이틀 형식 여부
  cf) 'is'가 붙으면 반환값이 boolean type

### 문자열 변경
- s.replace(old, new, count) : 글자 바꾸기, count 지정 시 해당 개수만큼 시행

- s.strip([chars])
  - strip('chars') : 문자열 앞/뒤의 'chars(문자)' 제거, 괄호가 빈 경우 공백('\n'포함) 제거
  - lstrip('chars') : left, 선행 문자 제거
  - rstrip('chars') : right, 후행 문자 제거
  ```python
  text = ",,,,,123.....water....pp"
  print(text.lstrip(',123.p'))  # water....pp
  print(text.rstrip(',123.p'))  # ,,,,,123.....water
  print(text.strip(',123.p'))   # water
  ```
  인자로 여러 문자 전달 시 해당 위치의 일치하는 모든 문자 제거

- s.split(sep=None) : 문자열을 특정한 단위로 나눠 리스트로 반환, sep 지정되지 않을 시공백 기준 분리
- 'separator'.join([iterable]) : 컨테이너 요소들을 seperator로 합쳐 문자열 반환
  ```python
  print(''.join(['3', '5']))  # 35
  ```
- s.capitalize() : 첫 번째 글자 대문자로 변경
- s.title() : '나 공백 이후를 대문자로 변경
- s.upper() : 모두 대문자로 변경
- s.lower() : 모두 소문자로 변경
- s.swapcase() : 대문자를 소문자로, 소문자를 대문자로 변경

### 리스트
- L.append(x) : 마지막 항목에 x를 추가

- L.insert(i, x) : 인덱스 i에 항목 x를 삽입
- L.remove(x) : 가장 왼쪽 x를 제거, x 존재하지 않을 시 ValueError
- L.pop(i) : 인덱스 i 항목 반환 후 삭제, 빈 경우 마지막 항목 반환 후 삭제
- L.clear() : 모든 항목 삭제
- L.extend(m) : L + m시퀀스
- L.index(x, start, end) : 리스트에 있는 항목 중 가장 왼쪽 x의 인덱스 반환
  ```python
  numbers = [1, 2, 3, 4]
  print(numbers.index(3)) # 2
  ```
- L.reverse() : 순서를 반대로 뒤집음, None 반환
- L.sort(key)

   : 원본 리스트 정렬, None 반환, 정렬의 기준으로 key를 사용할 수 있으며 key에는 단일인자를 반환하는 함수가 와야함(보통 lambda 사용)
  ```python
  # sort
  numbers = [3, 2, 5, 1]
  result = numbers.sort()
  print(numbers, result)  # [1, 2, 3, 5] None (원본 변경)

  # sorted
  result = sorted(numbers)
  print(numbers, result) # [3, 2, 5, 1] [1, 2, 3, 5] 
  # 원본 변경 없이 정렬된 리스트 반환
  ```
  ```python
  # key 사용
  ranked_movies = [{...}, {...}, {...}] # 요소가 딕셔너리 형태의 영화 정보인 리스트
  ranked_movies.sort(key = lambda l: l['vote_average'])
  ```
- L.count(x) : x의 개수 반환

### 세트
- s.copy() : 세트의 얕은 복사본 반환

- s.add(x) : x 추가
- s.pop() : 랜덤으로 항목을 반환 후 제거
- s.remove(x) : x 삭제
- s.discard(x) : x 삭제
- s.update(t) : 세트 t에 있는 항목 중 세트 s에 없는 항목 추가
- s.clear() : 모든 항목 제거
- s.isdisjoint(t) : 세트 s와 세트 t의 교집합 없는 경우 True 반환
- s.issubset(t) : 세트 s가 세트 t의 하위세트인 경우 True 반환
- s.issuperset(t) : 세트 s 가 세트 t의 상위 세트인 경우 True 반환

### 딕셔너리
- d.clear() : 모든 항목 제거

- d.keys() : 모든 키를 담은 뷰 반환
- d.values() : 모든 값을 담은 뷰 반환
- d.items() : 모든 키-값의 쌍을 담은 뷰 반환
- d.get(k) : key의 값을 반환, key가 존재하지 않을 시 None반환(get 안쓰면 원래는 Key Error)
  ```python
  drama = {'name' = '더글로리', 'writer' = '김은숙' }
  print(drama['director'])  # KeyError
  print(drama.get('director'))  # None
  ```
- d.get(k, v) : key의 값을 반환, key가 존재하지 않을 경우 v 반환
- d.pop(k) : key의 값을 반환 후 삭제, 없는 경우 KeyError
- d.pop(k, v) : key의 값을 반환 후 삭제, 없는 경우 v 반환
- d.update([other]) : 딕셔너리의 값을 매핑하여 업데이트