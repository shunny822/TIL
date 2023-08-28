# Express

## 목차
- [Express란](#express란)
- [프로젝트 생성](#express로-프로젝트-생성)
- [Express 요소](#express의-요소)
  - [app.js](#appjs)
  - [routing](#routing)

<br>

## Express란

> Fast, unopinionated, minimalist web framework for Node.js

- Node.js의 가장 대중적인 웹 애플리케이션 프레임워크이다.

- 다양한 HTTP 메소드와 미들웨어를 제공하여 빠르고 쉽게 API 구축이 가능하다.

- Unopinionated framework이다.
  - Opinionated(자기 주장이 강한) : 빌드 아키텍처 및 규칙에 대해 비교적 뚜렷한 경로를 제시한다. 러닝커브가 있을 수 있지만 복잡하고 규모가 큰 프로젝트의 설계가 가능하다.

  - Unopinionated(자기 주장이 강하지 않은) : 사용자들이 직접 관리하거나 선택의 옵션이 비교적 많이 허용되고 쉽게 변화를 적용할 수 있어 배우기 편하다. 하지만 규모가 크고 복잡한 프로젝트 시에 코드 유지보수, 확장 등이 까다로울 수 있다.

<br>

## Express로 프로젝트 생성

1. `npm init` : 애플리케이션을 생성할 디렉토리에 npm 생성

2. `npm install express` : express 설치

3. (선택)`npm install ejs` : template 엔진인 ejs 사용 시 먼저 설치

4. `npx express-generator --view=ejs myapp` : views에 ejs를 사용하고 해당 위치에 'myapp' 프로젝트 생성

5. `cd myapp`, `npm install` : myapp에 npm 설치

6. `set DEBUG=myapp:* & npm start` : 실행, 'http://localhost:3000/'으로 브라우저에서 액세스(nodejs는 3000 port)

*참고) 생성된 앱의 디렉토리 구조
```
.
├── app.js
├── bin
│   └── www
├── package.json
├── public
│   ├── images
│   ├── javascripts
│   └── stylesheets
│       └── style.css
├── routes
│   ├── index.js
│   └── users.js
└── views
    ├── error.pug
    ├── index.pug
    └── layout.pug

7 directories, 9 files
```

<br>

## Express의 요소

### app.js

HTTP 요청을 개별적으로 처리하고 응답을 보내는 방식으로 동작하며 위에서 아래의 순서대로 처리한다.


### routing

routing은 애플리케이션의 엔드포인트(URI)가 클라이언트 요청에 응답하는 방식을 나타내는 것이다. 형식은 아래와 같다.
```
app.METHOD(PATH, HANDLER)
```
- app : express의 인스턴스
- METHOD : get, post, put, delete(HTTP request method)
- PATH : 서버에서의 경로
- HANDLER : 해당 경로에서 실행될 함수
- ex) 해당 경로의 GET, POST메서드에 대해 정의
  ```js
  // GET method route
  app.get('/', (req, res) => {
    res.send('GET request to the homepage')
  })

  // POST method route
  app.post('/', (req, res) => {
    res.send('POST request to the homepage')
  })
  ```


## 참고 자료

https://expressjs.com/

https://www.hanl.tech/blog/opinionated-vs-unopionated/