# SSR과 CSR

## 목차
- [SSR](#ssr)
- [CSR](#csr)
- [참고 자료](#참고-자료)

<br>

## SSR

Server Side Rendering으로 서버에서 렌더링 준비를 마친 상태로 HTML file을 클라이언트에 전달하는 방식이다. 이 때 서버에서 템플릿 엔진을 이용할 수 있다.

클라이언트에 렌더링 준비가 끝난 HTML file이 전달되면 준비가 되어있기 때문에 바로 렌더링 되어 사용자가 화면을 볼 수 있다. 렌더링 과정에서 JS file이 fetch되기 전까지는 사용자가 사이트를 조작할 수 없다.


<br>

## CSR

Client Side Rendering으로 클라이언트 쪽에서 렌더링이 일어난다. 서버는 요청을 받으면 클라이언트에 HTML과 JS를 보내준다.

클라이언트는 HTML과 JS를 다운로드 받고, 다운로드가 완료된 JS가 실행되면서 데이터를 받기 위한 API가 호출된다.

API로부터 받아온 데이터를 placeholder 자리에 넣어주고, 이후로 사용자가 페이지를 조작할 수 있게 된다.

<br>

## SSR과 CSR의 차이

- 첫 페이지 로딩 시간

  CSR은 HTML, CSS, script file을 모두 한 번에 불러오고, SSR은 필요한 부분만 불러오므로 SSR이 빠르다.

- 나머지 페이지 로딩 시간

  CSR은 이미 모든 코드를 받아왔기 때문에 같은 과정을 반복하는 SSR보다 빠르다.

- 서버 자원

  CSR은 클라이언트에 일감을 몰아주고 SSR은 서버에서 처리하므로 서버 자원이 더 많이 사용된다.

<br>

## 참고 자료

https://velog.io/@vagabondms/%EA%B8%B0%EC%88%A0-%EC%8A%A4%ED%84%B0%EB%94%94-SSR%EA%B3%BC-CSR%EC%9D%98-%EC%B0%A8%EC%9D%B4