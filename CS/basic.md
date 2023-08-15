# 네트워크 기초 개념

## 목차

- [JSON](#json)
- [Socket](#socket)
- [ByteStream](#bytestream)
- [Buffer](#buffer)

### JSON

JavaScript Object Notation으로 데이터를 저장하거나 전송할 때 많이 사용되는 경량 data 교환 형식이다. JS에서 객체를 만들 때 사용하는 표현식으로 데이터 통신 방법이 아닌 단순히 데이터를 표현하는 방식을 말한다. 사람과 기계 모두 이해하기 쉽고 용량이 작아 최근에는 XML을 대체하여 데이터 전송에 사용되며 주로 서버와 클라이언트 간의 통신에 사용된다.

- JS에서 객체와 JSON 간의 변환
  ```js
  // JSON -> JS string
  JSON.parse(jsonData);

  // JS object -> JSON
  JSON.stringify(jsObject);
  ```

### Socket

네트워크 소켓은 컴퓨터 네트워크를 경유하는 프로세스 간 통신의 종착점이다. 네트워크 통신을 위한 프로그램들은 소켓을 생성하고 소켓으로 데이터를 교환한다. 즉, 통신의 출입구인 파일이다. 이는 OS가 제공하고 관리하며 소프트웨어적으로 구축되었다.


### ByteStream

입력과 출력에서 이동하는 byte의 흐름으로 스트림은 읽기 스트림과 쓰기 스트림이 있다. 콘솔의 입출력, 파일 읽기/쓰기, 서버와 클라이언트의 요청과 응답(TCP/IP에서 소켓을 이용한 데이터 통신) 등이 있다. ex) nodejs의 fs모듈


### Buffer

데이터를 한 곳에서 다른 곳으로 전송하는 동안 일시적으로 그 데이터를 보관하는 메모리의 영역

<br>

## 참고 자료

https://velog.io/@ym1085/JSON-%EC%82%AC%EC%9A%A9%EB%B2%95

https://m.blog.naver.com/ginameee/220839976258