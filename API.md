# API (Application Programming Interface)

## API란?
- 컴퓨터나 컴퓨터 프로그램 사이의 연결

- 일종의 소프트웨어 인터페이스이며 다른 종류의 소프트웨어에 서비스를 제공

- 사용하는 방법을 기술하는 문서나 표준은 API 사양/명세(specification)

- 주소로 정보를 요청하면 문서(JSON)로 응답! 즉! `http`(Hyper Text Transfer Protocol)

- 웹과 파이썬 API 사용 비교

  - 웹 : 브라우저를 통해 주소로 요청을 보내고, 응답 결과를 브라우저가 웹화면으로 랜더링
  - 파이썬 : 파이썬을 통해 주소로 요청을 보내고, 응답 결과를 파이썬으로 조작(이때 쓰는 것이 requests)


## API 활용
- 요청하는 방식에 대한 이해

  - 인증방식

  - URL 생성 : 기본주소, 원하는 기능에 대한 추가 경로, 요청 변수(필수와 선택)

  - 기본 구조 : `URL/기능추가경로?변수이름=값&변수이름=값&변수이름=값`

  <예시>
  
  `https://api.themoviedb.org/3` : 기본 주소
  
  `/movie/popular` : 원하는 기능에 대한 추가 경로(예시는 인기영화)
  
  `?api_key=<<api_key>>` : '<<api_key>>'를 내가 인증받은 API Key 값으로 변경

  `&language=ko-KR` : 요청 변수(Query String 참고)


- 응답 결과에 대한 이해

  - 응답 결과 타입(JSON)

  - 응답 결과 구조


## Requests
> simple HTTP library `for Python`

`pip install requests`로 설치한 후 사용

[Requests 공식 문서](https://requests.readthedocs.io/en/latest/)

```python
import requests

BASE_URL = 'https://api.themoviedb.org/3'
path = '/movie/popular'
parameters = {
    'api_key' : '885~~~~~d'
    'language' : 'ko-KR'
    'region' : 'KR'
}  # '요청변수=값'들을 딕셔너리 형태로 정리 

# URL에서 response 받아오기
r = requests.get(BASE_URL+path, params = parameters).json()
# 'params'키워드를 사용 시 딕셔너리에 저장된 키와 값이 주소 뒤에 붙음
# 받아온 response를 JSON형식 문서로 디코딩
print(r)
```