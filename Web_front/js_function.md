# 참조 자료형 Reference type

## Function

> 참조 자료형에 속하며 모든 함수는 Function object 자료형

### 함수의 구조
```js
function name ([param[, param,[..., param]]]) {
  statements
  return value
}
```
- 함수 이름, 매개변수, body를 구성하는 statement로 구성

- return이 없다면 undefined를 반환

### 함수의 정의
1. 선언식 function declaration
    - 기본 형태
      ```js
      function funcName () {
        statements
      }
      ```
    - 예시
      ```js
      function add (num1, num2) {
        return num1 + num2
      }

      console.log(add(2, 7))
      ```


2. 표현식 function expression
    - 기본 형태
      ```js
      const funcName = function () {
        statements
      }
      ```
    - 예시
      ```js
      const sub = function (num1, num2) {
        return num1 - num2
      }

      console.log sub(7, 2)
      ```
  
3. 특징
    - 함수 선언식은 익명 함수 사용 불가, 표현식은 익명 함수 사용 가능

    - 선언식으로 정의한 함수는 호이스팅 O, 표현식으로 정의한 함수는 호이스팅 X

    - 선언식의 호이스팅은 코드 유지보수에 어려움을 주므로 표현식 사용 권장!
  

### 기본함수 매개변수

: 값이 없거나 undefined가 전달될 경우 이름 붙은 매개변수를 기본값으로 초기화
```js
const greeting = function (name = 'Anonymous') {
  return `Hi ${name}`
}

console.log(greeting())
```


### 매개변수와 인자의 개수 불일치
- 매개변수 < 인자 개수
  ```js
  const noArgs = function () {
    return 0
  }
  console.log(noArgs(1, 2, 3))  // 0 출력

  const twoArgs = function (arg1, arg2) {
    return [arg1, arg2]
  }
  console.log(twoArgs(1, 2, 3))  // [1, 2] 출력
  ```
  - 매개변수를 넘어선 인자값은 무시

- 매개변수 > 인자 개수
  ```js
  const threeArgs = function (arg1, arg2, arg3) {
    return [arg1, arg2, arg3]
  }
  console.log(threeArgs())     // [undefined, undefined, undefined]
  console.log(threeArgs(1))    // [1, undefined, undefined]
  console.log(threeArgs(2, 3)) // [2, 3, undefined]
  ```
  - 모자란 매개변수는 undefined

### 나머지 매개변수 Rest parameters

: 무한한 수의 인자를 '배열'로 허용하여 가변함수를 나타내는 방법

```js
const myFunc = function (arg1, arg2, ...restArgs) {
  return [arg1, arg2, restArgs]
}

console.log(myFunc(1, 2, 3, 4, 5)) // [1, 2, [3, 4, 5]]
console.log(myFunc(1, 2))   // [1, 2, []]
```
- 나머지 매개변수는 `...restArgs`로 표현

- 함수 정의에는 하나의 나머지 매개변수만 있을 수 있음


### 화살표 함수 표현식 Arrow function expressions

: 함수표현식의 간결한 표현

- 화살표 함수 표현식 작성 순서
  ```js
  // 원래의 함수 표현식
  const arrow = function (name) {
    return `hello, ${name}`
  }

  // 1. funcrion 키워드 삭제 후 매개변수와 중괄호 사이에 화살표 작성
  const arrow2 = (name) => { return `hello, ${name}` }

  // 2. 인자가 1개일 경우에만 () 생략 가능
  const arrow3 = name => { return `hello, ${name}` }

  // 3. 함수 본문의 표현식이 한 줄이라면 '{}'와 'return' 제거 가능
  const arrow4 = name => `hello, ${name}`
  ```

- 화살표 함수 표현식 응용
  ```js
  // 1. 인자가 없다면 () or _로 표시 가능
  const noArgs1 = () => 'No args'
  const noArgs2 = _ => 'No args'

  // 2-1. object를 return한다면 return을 명시적으로 작성해야 함
  const returnObject1 = () => { return { key: 'value' }}

  // 2-2. return을 적지 않으려면 소괄호로 감사야 함
  const returnObject2 = () => ({ key: 'value' })
  ```