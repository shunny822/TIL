# JavaScript and DOM

## JavaScript

- 웹 페이지의 동적인 기능을 구현하기 위한 웹 브라우저에서의 JavaScript를 학습

- 웹 브라우저에 내장된 JavaScript 엔진에 의해 브라우저에서 실행됨

- JavaScript 실행환경 : HTML script 태그, js 확장자 파일, 브라우저 Console

<br>

## DOM : Document Object Model

### 개념

- 웹 페이지(document)를 구조화된 객체로 제공하며 프로그래밍 언어가 웹 페이지를 사용할 수 있게 연결시킴

- 브라우저는 HTML 문서를 해석하여 `DOM tree`라는 객체의 트리로 구조화

- DOM에서 모든 요소, 속성, 텍스트는 하나의 객체이며 모두 document 객체(DOM Tree의 진입점)의 자식

- 웹 페이지를 동적으로 만드는 것 == 웹 페이지를 `조작`하는것
  - 조작의 순서 : 조작하고자하는 요소를 `선택` 또는 `탐색` → 선택된 요소의 컨텐츠 또는 속성을 `조작`


### DOM Query(선택)
- `document.querySelector()`
  - 제공한 선택자와 일치하는 element 한개 선택

  - 요소가 없다면 null 반환

- `document.querySelectorAll()`
  - 제공한 선택자와 일치하는 여러 element를 선택

  - 매칭할 하나 이상의 셀렉터를 포함하는 유효한 CSS Selector를 인자로 받음

  - 제공한 CSS selector를 만족하는 NodeList를 반환

- 예시
  ```html
  <h1 class="title heading">DOM 선택</h1>
  <a href="https://www.google.com/">google</a>
  <p class="text">content1</p>
  <p class="text">content2</p>
  <p class="text">content3</p>
  <ul>
      <li>list1</li>
      <li>list2</li>
  </ul>

  <script>
      console.log(document.querySelector('.title'))
      console.log(document.querySelector('.text'))
      console.log(document.querySelectorAll('.text'))
      console.log(document.querySelectorAll('ul > li'))
  </script>
  ```
  - 괄호에는 선택자가 문자열 형태('')로 들어감


### DOM Manipulation(조작)

1. 속성(attribute) 조작

    - 클래스 속성 조작 : 'classList' property

      - 요소의 클래스 목록을 DOMTokenList(유사 배열) 형태로 반환

      - .add()와 .remove() 메서드를 사용해 지정한 클래스 값을 추가 혹은 제거

      ```html
      <script>
          // 변수에 저장 후 조작
          const h1Tag = document.querySelector('h1')
          console.log(h1Tag.classList)

          h1Tag.classList.add('test')
          console.log(h1Tag.classList)

          h1Tag.classList.remove('test')
          console.log(h1Tag.classList)
      ```

    - 일반 속성 조작
      - 조회 : .getAttribute()

      - 설정(수정) : .setAttribute()

      - 삭제 : .removeAttribute()

      ```html
      <script>
          const aTag = document.querySelector('a')
          console.log(aTag)
          console.log(aTag.getAttribute('href'))

          aTag.setAttribute('href', 'https://www.naver.com/')
          console.log(aTag.getAttribute('href'))

          // 클래스도 일반 속성 방식으로 조작 가능
          const pTag = document.querySelector('.text')
          console.log(pTag.getAttribute('class'))
      </script>
      ```

2. HTML 컨텐츠 조작 : 'textContent' property
    ```html
    <script>
        const h1Tag = document.querySelector('.heading')
        console.log(h1Tag.textContent)

        h1Tag.textContent = '컨텐츠 수정'
        console.log(h1Tag.textContent)
    </script>
    ```

3. DOM 조작
    ```html
    <div></div>
    <script>
        // 생성
        const h1Tag = document.createElement('h1')
        h1Tag.textContent = '제목'
        console.log(h1Tag)

        // 추가
        const divTag = document.querySelector('div')
        divTag.appendChild(h1Tag)

        // 삭제
        divTag.removeChild(h1Tag)
    </script>
    ```

4. style 조작 : 'style' property
    - 해당 요소의 모든 스타일 속성 목록을 포함하는 속성
    ```html
    <p>Heading</p>

    <script>
        const pTag = document.querySelector('p')

        pTag.style.color = 'red'
        pTag.style.fontSize = '3rem'
    </script>
    ```

<br>

## 참고사항
### Parsing
- 구문 분석, 해석

- 브라우저가 문자열을 해석하여 DOM Tree로 만드는 과정
