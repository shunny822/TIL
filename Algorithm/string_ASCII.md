# 문자열(String)과 아스키(ASCII) 코드

## 문자열 조작

> 문자열은 immutable(변경 불가능한) 자료형!
>
> 사용 시 원본 자체를 바꾸지 못하고 변경한 값을 리턴


### 슬라이싱

|     | a | b | c | d | e | f |
|-----|---|---|---|---|---|---|
|index| 0 | 1 | 2 | 3 | 4 | 5 |
|index| -6| -5| -4| -3| -2| -1|

- s[2:5:2] = 'ce'

- s[-5:-1:3] = 'be'

- s[2:5:-1] = ''

- s[:] = 'abcdef'

- s[::-1] = 'fedcba'

- s[-3:] = 'def'


### 정규 표현식 Regular expression

[정규 표현식](https://wikidocs.net/4308)


### 추가 메서드

- startswith() : 문자열이 지정된 문자/문자열로 시작하면 True, 아니면 False를 반환

- endswich() : 문자열이 지정된 문자/문자열로 끝나면 True, 아니면 False를 반환
  ```python
  s = 'Hello Python!'
  print(s.startswith('Hel'))
  print(s.endswith('!'))
  ```

<br>
<br>

## 아스키 코드

> ASCII, American Standard Code for Information Interchange
>
> 미국 정보교환 표준 부호

- 컴퓨터는 숫자만 이해할 수 있어 문자를 인코딩해야함

- 1bit = 1byte이며 각 문자를 표현하는데 1byte 사용

- 아스키코드 메서드

  - ord(문자) : 문자를 아스키코드로 변환

  - chr(아스키코드) : 아스키코드를 문자로 변환