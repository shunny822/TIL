# Basic Syntasx

## 개요
- ECMAScript 2015(ES6) 이후의 문서 참고하기

### Style guide
- 들여쓰기 시 2칸 공백

- 문자열에 작은 따옴표를 사용

- 예약어 뒤에는 공백을 추가

- 연산자 사이에 공백

- 세미콜론 사용하지 않음

<br>

## 문법
### 1. 변수
- 작성 규칙
  - 반드시 문자, $, _ 로 시작

  - 대소문자를 구분하며, 클래스명 외에는 모두 소문자로 시작

  - 예약어 사용 불가(for, if, function)

  - 카멜 케이스(camelCase) : 변수, 객체, 함수에 사용

  - 파스칼 케이스(PascalCase) : 클래스, 생성자에 사용

  - 대문자 스네이크 케이스(SNAKE_CASE) : 상수(constants, 변하지 않는 값)에 사용

- 변수 선언 키워드
  - let : 블록 스코프를 갖는 지역 변수를 선언, 재할당 가능 & 재선언 불가능
    ```js
    let number = 10
    number = 20      // 재할당 가능
    let number = 30  // 재선언 불가능
    ```
  
  - const : 블록 스코프를 갖는 지역 변수를 선언, 재할당 불가능 & 재선언 불가능
    ```js
    // 선언 시 초기값 설정 필요
    const number // -> const' declarations must be initialized.
    ```

  - block scope : if, for, 함수 등의 중괄호'{}' 내부를 가리킴, 이 내부의 변수는 바깥에서 접근 불가능

  - 기본적으로 const 사용을 권장하며 재할당을 해야 하는 경우만 let을 사용


### 2. 데이터 타입
- 원시 자료형(Primitive type) : 변수에 값이 직접 저장되는 자료형(불변, 값이 복사)
  - Number : 정수 또는 실수형 숫자를 표현하는 자료형

  - String
    - 문자열 표현, 곱셈, 나눗셈, 뺄셈은 안되고 덧셈으로 문자열끼리 연결 가능

    - Template Literal : backtick과 $를 이용해 f-string과 같이 문자열에 변수 삽입 가능
      ```js
      const age = 10
      const message = `홍길동은 ${age}세 입니다.`
      console.log(message)
      ```
    
  - null : 변수의 값이 없음을 의도적으로 표현할 때 사용

  - undefined : 변수 선언 이후 직접 값을 할당하지 않으면 자동으로 할당

    - null과 undefined 두 개가 존재하는 이유는 js설계 실수

    - 또 하나의 bug : null이 원시 자료형임에도 불구하고 자료형이 object로 출력됨

  - Boolean : true / false, 조건문 또는 반복문에서 undefined, null, 0, NaN, 빈 문자열은 false, 그 외는 true


- 참조 자료형(Reference type) : 객체의 주소가 저장되는 자료형(가변, 주소가 복사)

### 3. 연산자
- 할당 연산자 : 오른쪽에 있는 피연산자의 평가 결과를 왼쪽 피연산자에 할당

- 비교 연산자 : 피연산자들을 비교하여 결과값을 boolean으로 반환

- 동등 연산자(==)
  - 두 피연산자가 같은 값으로 평가되는지 비교 후 boolean값 반환

  - 비교 시 암묵적으로 같은 타입으로 변환 후 비교됨

  - 예상치 못한 결과가 발생할 수 있으므로 특별한 경우를 제외하고 사용하지 않음

- 일치 연산자(===)
  - 두 피연산자의 값과 타입이 모두 같은 경우 true 반환

  - 엄격한 비교! 암묵적 형변환 발생하지 않음

- 논리 연산자 : &&(and), ||(or), !(not) → 단축 평가 지원(어떤 경우 앞의 조건만으로 결과값 반환)


### 4. 조건문
- 기본 문법
  ```js
  if (조건문) {
    명령문
  } else if (조건문) {
    명령문
  } else {
    명령문
  }
  ```


### 5. 반복문
- while : 조건문이 참이기만 하면 문장을 계속해서 수행
  ```js
  while (조건문) {
    // code
  }
  ```

- for : 특정한 조건이 거짓으로 판별될 때까지 반복
  - 기본 문법
    ```js
    for ([초기문]; [조건문]; [증감문]) {
      // code
    }
    ```
  
  - 예시
    ```js
    for (let i = 0; i < 6; i++) {
      console.log(i)
    }
    ```

- for...in : 객체(object)의 속성을 순회할 때 사용
  - 기본 문법
    ```js
    for (variable in object) {
      statements
    }
    ```

  - 예시
    ```js
    const fruits = {a: 'apple', b: 'banana'}

    for (const key in fruits) {
      console.log(key)
      console.log(fruits[key])
    }
    ```
  - 배열도 순회 가능하지만 인덱스 순으로 순회한다는 보장이 없으므로 권장하지 음

- for...of : 반복 가능한 객체(배열, 문자열 등)를 순회할 때 사용
  ```js
  const numbers = [0, 1, 2, 3]

  for (const number of numbers) {
    console.log(number)
  }

  const myStr = 'apple'

  for (const str of myStr) {
    console.log(str)
  }
  ```

<br>

## 참고 사항
### for...in과 for...of의 차이
```js
const arr = [3, 5, 7]

for (const i in arr) {
  console.log(i)  // 0 1 2
}

for (const i of arr) {
  console.log(i)  // 3 5 7
}
```
: for...in은 '속성 이름'을 통해 반복, for...of는 '속성 값'을 통해 반복


### 반복문 사용 시 const 및 let
- for문
  - 최초 정의한 i를 재할당하면서 사용
  
  - 따라서 const를 사용하면 에러 발생

- for...in, for...of
  - 재할당이 아니라 매 반복 시 해당 변수를 새로 정의하여 사용

  - const를 사용해도 에러가 발생하지 않음