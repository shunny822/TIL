# Controlling event

## 목차
- [Event](#event)
  - [event handler](#event-handler)
  - [event handler 활용](#event-handler-활용)
  - [lodash](#lodash)
- [Event Trigger](#event-trigger)
- [참고 자료](#참고-자료)

<br>

## Event

> 무언가 일어났다는 신호, 사건
> 
> DOM 요소는 event를 받고 '처리'할 수 있음 == 이벤트 핸들러(처리기)

### event handler

: 이벤트가 발생했을 때 실행되는 함수(사용자의 행동에 따른 반응을 JS코드로 표현)

- .addEventListener() : 대표적인 이벤트 핸들러 중 하나
  
  - 기본 형태 : `EventTarget.addEventListener(type(특정 이벤트), handler(콜백함수))`


### event handler 활용
- click 이벤트 : 버튼을 클릭하면 숫자 1씩 증가
  ```js
  <body>
    <button id="btn">버튼</button>
    <p id="counter">0</p>
    
    <script>
      let x = 0
      const btn = document.querySelector('#btn')
      const pTag = document.querySelector('#counter')
      btn.addEventListener('click', function (plus) {
        x = x + 1
        pTag.textContent = x
      })
      
    </script>
  </body>
  ```

- input 이벤트 : 사용자 입력값을 실시간으로 출력
  ```js
  <body>
    <input type="text" id="text-input">
    <p></p>
    
    <script>
      const inputTag = document.querySelector('#text-input')
      const pTag = document.querySelector('p')

      inputTag.addEventListener('input', function (event) {
        pTag.textContent = event.target.value
      })

    </script>
  </body>
  ```

- click & input : 사용자의 입력값 실시간 출력 & 버튼 클릭 시 출력한 값의 스타일 변경
  ```js
  <body>
    <h1></h1>
    <button id="btn">클릭</button>
    <input type="text" id="text-input">

    <script>
      const btn = document.querySelector('#btn')
      const inputTag = document.querySelector('#text-input')
      const h1Tag = document.querySelector('h1')

      inputTag.addEventListener('input', function (event) {
        h1Tag.textContent = event.target.value
      })

      btn.addEventListener('click', function (event) {
        h1Tag.classList.toggle('blue')
      })

    </script>
  </body>
  ```

- 이벤트 취소
  ```js
  <body>
    <h1>정말 중요한 내용</h1>

    <script>
      const h1Tag = document.querySelector('h1')

      h1Tag.addEventListener('copy', (event) => {
        event.preventDefault()
        alert('복사할 수 없습니다.')
      })

    </script>
  </body>
  ```

- todo list 만들기
  ```js
  <body>
    <input type="text" class="input-text">
    <button id="btn">+</button>
    <ul></ul>

    <script>
      const inputTag = document.querySelector('.input-text')
      const btn = document.querySelector('#btn')
      const ulTag = document.querySelector('ul')

      const addTodo = (event) => {
        const inputData = inputTag.value
        if (inputData.trim()) {
          const liTag = document.createElement('li')
          liTag.textContent = inputData
          ulTag.appendChild(liTag)
          inputTag.value = ''
        } else {
          alert('할 일을 입력하세요.')
          inputTag.value = ''
        }
      }

      btn.addEventListener('click', addTodo)

    </script>
  </body>
  ```
  - .trim() : 공백을 제거하는 함수

- 로또 번호 추첨
  ```js
  <body>
    <h1>로또 추첨 번호</h1>
    <button id="btn">행운 번호 받기</button>
    <div></div>

    <!-- Lodash(라이브러리) 가져오기 -->
    <script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
    <script>
      const h1Tag = document.querySelector('h1')
      const btn = document.querySelector('#btn')
      const divTag = document.querySelector('div')

      btn.addEventListener('click', (event) => {
        const numbers = _.range(1, 46)
        const lottoNumbers = _.sampleSize(numbers, 6)
        const ulTag = document.createElement('ul')
        
        lottoNumbers.forEach((n) => {
          const liTag = document.createElement('li')
          liTag.textContent = n
          ulTag.appendChild(liTag)
        })

        divTag.appendChild(ulTag)
      })

    </script>
  </body>
  ```

### lodash
- 모듈성, 성능 및 추가 기능을 제공하는 JS 유틸리티 라이브러리

- [lodash site](https://lodash.com/)


<br>

## Event Trigger

event trigger를 함수로 정의하여 원하는 event를 원하는 element에 발생시킬 수 있다.

```js
const inputEventTrigger = (element) => {
  const event = new Event('input', {
    bubbles: true,
    cancelable: true,
  });
  element.dispatchEvent(event);
}
```
이와 같이 Event 객체를 생성하여 커스텀하고 이를 `dispatchEvent()` method로 element에 적용하는 방식으로 사용된다.

- Event instance의 property는 위에 사용한 것 외에도 `currentTarget`, `defaultPrevented` 등 매우 다양하나 자주 사용되는 `bubbles`, `cancelable`에 대해 알아보자.
    
    - `bubbles` : 이벤트가 DOM을 통해 bubble up(이벤트 발생)되는지 여부 true/false
    - `cancelable` : 이벤트를 취소할 수 있는지 여부 true/false

- `eventTarget.dispatchEvent(event)` : eventTaget에 해당 event를 발생시킴


<br>

## 참고 자료

https://developer.mozilla.org/en-US/docs/Web/Events/Creating_and_triggering_events

https://developer.mozilla.org/en-US/docs/Web/API/Event