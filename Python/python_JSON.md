# 파일의 입출력

 > __Python을 활용하여 파일의 입출력 가능__

## 파일 가져오기
```python
f = open('file명', mode='r', encoding=None)
# mode와 encoding은 생략 시 'r'과 'None'
```
- f.close()로 파일 객체를 종료시켜야 오류발생 X
- mode : 텍스트 모드
  - r : open for reading (default)
  - w : open for writing, 동일 파일 없을 시 파일 생성 후 writing, 있을 시 내용 삭제 후 writing
  - a : open for writing, appending to the end of file(존재하는 file)
  - x : open for exclusive creation, failing if the file already exists
  - b : binary mode
  - t : text mode (default)
  - '+' : open for updating (reading and writing)
- encoding : 인코딩 방식, 일반적으로 utf8 사용

## with 키워드 활용
```python
with open('file명', 'r', encoding=utf8)
```
f.close()를 쓰지 않아도 오류 발생 시 파일이 올바르게 닫힘.(f.close쓰면 파일 실행X)

## 파일 객체 메서드
### 1. read(size)
```python
story = f.read()
print(story)
```
- 파일의 내용을 읽음
- size는 선택이며 생략되거나 음수일 경우 전체 내용을 읽어서 반환
- 파일의 마지막에 도달 시 빈문자열('')을 반환

### 2. readline()
```python
f.readline()  # 첫째줄 읽기
f.readline()  # 둘째줄 읽기(print찍어보기)
```
- 파일에서 한줄씩 읽는 것
- 개행문자\n은 문자열 끝에 보존됨
- 파일의 마지막에 도달 시 빈문자열('')을 반환

### 3. line 별로 읽기
```python
for line in f:
  print(line)
```

### 4. line을 리스트로
- 방법1
```python
story = list(f)
```
- 방법2
```python
story = f.readlines()
```

### 5. write()
```python
f.write('This is a test\n')  # 15
```
- string 내용을 file에 쓰고 출력된 문자 개수를 반환

<br>
<br>

# JSON
자바스크립트 객체 표기법으로 개발환경에서 많이 활용되는 데이터양식

웹 어플리케이션에서 데이터를 전송할 때 일반적으로 사용

문자기반(text) 데이터 포멧으로 다수의 프로그래밍 환경에서 쉽게 활용 가능

## JSON 파일 활용

### dumps()
```python
import json
a = [1, 'sample', 'list']
json.dumps(a)  # '[1, 'sample', 'list']'
```
객체를 text file로 직렬화

### 데이터 load
```python
x = json.load(f)
```

### pprint
```python
from pprint import pprint
pprint(x)
```
JSON 데이터를 한줄씩 끊어서 출력

<br>
<br>
[파이썬 공식문서](https://docs.python.org/ko/3/tutorial/inputoutput.html#reading-and-writing-files)