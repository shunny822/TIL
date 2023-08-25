# 참조 자료형 Reference type

## Array

> 순서가 있는 데이터 집합을 저장하는 자료구조(본질은 객체)
> 
> 숫자형 키(인덱스)를 사용함으로써 순서가 있는 컬렉션 제어

### 배열의 구조
```js
const fruits = ['apple', 'banana', 'coconut',]

console.log(fruits[0])
console.log(fruits[1])
console.log(fruits[2])
console.log(fruits.length)
```
- 대괄호를 이용해 작성

- length를 사용해 배열에 담긴 요소의 개수 알 수 있음

- 배열 요소의 자료형에 제약이 없음

- 배열의 마지막 요소에 객체와 마찬가지로 trailing comma 사용 가능

- 반복
  ```js
  // for
  for (let i = 0; i< fruits.length; i++) {
    console.log(fruits[i])
  }

  // for...of
  for (const fruit of fruits) {
    console.log(fruit)
  }
  ```


### 배열과 메서드
- pop : 배열 끝에 요소를 삭제하고 반환
  ```js
  console.log(fruits.pop())
  ```

- push : 배열 끝에 요소를 추가
  ```js
  fruits.push('orange')
  ```

- shift : 배열의 맨 앞 요소를 제거하고, 제거한 요소를 반환
  ```js
  console.log(fruits.shift())
  ```

- unshift : 배열의 맨 앞에 요소를 추가
  ```js
  fruits.unshift('melon')
  ```

- forEach : 인자로 주어진 함수(콜백 함수)를 배열 요소 각각에 대해 실행
  ```js
  array.forEach(function (item, index, array) {
    // do something
  })
  ```
  - 콜백함수 : 다른 함수에 인자로 전달되는 함수

  - 콜백 함수의 3가지 매개변수 : item(배열의 요소), index(배열요소의 인덱스), array

  - 반환값 : undefined

  - 예시
    ```js
    const fruits = ['apple', 'banana', 'coconut']
    fruits.forEach(function (item, index, array) {
      console.log(`${item} / ${index} / ${array}`)
    })

    // 화살표 사용
    fruits.forEach((item, index, array) => {
      console.log(`${item} / ${index} / ${array}`)
    })
    ```

- map : 배열 요소 전체를 대상으로 함수(콜백함수)를 호출하고, 함수 호출 결과를 모아 새로운 배열 반환
  ```js
  const result = array.map(function (item, index, array) {
    // do something
  })
  ```
  - 기본적으로 forEach와 구조가 같으며 forEach와는 달리 새로운 배열 반환

  - 예시
    ```js
    const fruits = ['apple', 'banana', 'coconut']

    const result = fruits.map(function (fruit) {
      return fruit.length
    })

    console.log(result) // [5, 6, 7]
    ```
    ```js
    const numbers = [1, 2, 3]

    const doubleNumber = numbers.map((number) => {
      return number * 2
    })

    console.log(doubleNumber)  // [2, 4, 6]
    ```

<br>

## 참고

### 콜백함수

- 함수의 재사용
  - 함수를 호출하는 코드에서 콜백 함수의 동작을 자유롭게 변경 가능

  - 이때, 콜백함수는 각 요소를 변환하는 로직을 담당하므로 map함수를 호출하는 코드가 간결하고 가독성 높아짐

- 비동기적 처리
  ```js
  consol.log('a')

  setTimeout(() => {
    console.log('b')
  }, 3000)

  console.log('c')
  // a c b 순으로 출력
  ```
  - setTimeout 함수는 콜백 함수를 인자로 받아 일정 시간이 지난 후에 실행됨

  - 이때, setTimeout 함수는 비동기적으로 콜백 함수를 실행하므로 다른 코드의 실행을 방해하지 않음