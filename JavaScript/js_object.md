# 참조 자료형 Reference type

## (plain)Object

> key로 구분된 데이터 집합(data collection)을 저장하는 자료형

### 객체의 구조
```js
const user = {
  name: 'Sophia',
  age: 30,
  'key with space': true,
}
```
- 중괄호를 이용해 작성

- 중괄호 안에는 key: value쌍으로 구성된 속성(property)을 여러 개 넣을 수 있음

- key는 문자형, value는 모든 자료형 가능

- 마지막 property에 붙는 콤마는 trailing comma로 붙이면 속성의 추가, 삭제, 이동이 용이해짐(필수는 아님)


### 객체의 속성
위의 user에 대한 property 활용

- 조회
  ```js
  // 점 표기법(key에 공백 없을 시에만 가능)
  console.log(user.age)  // 30
  // 대괄호 표기법
  console.log(user['key with space'])  // true
  ```

- 추가
  ```js
  // 기존에 없는 키
  user.address = 'korea'
  ```

- 수정
  ```js
  // 존재하는 키를 쓰면 value update
  user.age = 20
  ```

- 삭제
  ```js
  delete user.address
  ```

- 'in' : property 존재 여부 확인
  ```js
  console.log('age' in user)   // true
  console.log('country' in user) // false
  ```

- 단축 : 키 이름과 값으로 쓰이는 변수의 이름이 같은 경우 단축 구문 사용 가능
  ```js
  const age = 30
  const address = 'korea'

  const oldUser = {
    age: age,
    address: address,
  }

  const newUser = {
    age,
    address,
  }
  ```

- 계산된 property : 키가 대괄호로 둘러싸여 있는 property로 고정된 값이 아닌 변수의 값을 사용할 수 있음
  ```js
  const product = prompt('물건 이름을 입력해주세요')
  const prefix = 'my'
  const suffix = 'property'

  const bag = {
    [product]: 5,
    // 연산도 가능
    [prefix + suffix]: 'value',
  }
  ```


### 객체와 함수

> Method : 객체 속성에 정의된 함수로 객체를 `행동`할 수 있게 함

```js
// 객체의 속성에 함수 정의
const person = {
  name: 'Sophia',
  greeting: function () {
    return 'Hello'
  },
}

// 메서드 호출
console.log(person.greeting())
```

- `this` keyword
  ```js
  const person = {
    name: 'Sophia',
    greeting: function () {
      return `Hello my name is ${this.name}`
    },
  }

  console.log(person.greeting())
  ```
  - 함수나 메서드를 호출한 객체를 가리키는 키워드

  - 함수 내에서 객체의 속성 및 메서드에 접근하기 위해 사용

  - this는 함수가 호출되는 시점에서 값이 결정됨(동적)

  - this는 함수를 호출하는 방법에 따라 가리키는 대상이 다름

    - 메서드 호출 시 위와 같이 메서드의 객체를 가리킴

    - 단순 호출 시 최상위 전역 객체를 가리킴
      ```js
      const myFunc = function () {
        return this
      }
      console.log(myFunc())  // 최상위 전역 객체인 'window' 출력
      ```

- Nested 함수
  ```js
  const myObj2 = {
    numbers: [1, 2, 3],
    myFunc: function () {
      this.numbers.forEach(function (number) {
        console.log(number)  // 1 2 3
        console.log(this)   // window
      })
    }
  }
  ```
  - forEach의 인자로 들어간 함수는 일반 함수 호출이기 때문에 this가 전역 객체를 가리킴

  ```js
  const myObj3 = {
    numbers: [1, 2, 3],
    myFunc: function () {
      this.numbers.forEach((number) => {
        console.log(number)  // 1 2 3
        console.log(this)   // myObj3
      })
    }
  }
  ```
  - 화살표 함수는 자신만의 this를 가지지 않기 때문에 외부 함수에서 this값을 가져옴


### 참고사항
- JSON (JavaScript Object Notation)
  - Key-Value 형태로 이루어진 자료 표기법

  - JavaScript의 Object와 유사한 구조를 가지고 있지만 JSON은 형식이 있는 '문자열'!!

  - JavaScript에서 JSON을 사용하기 위해서는 Object자료형으로 변경해야 함

  ```js
  const jsObject = {
    coffee: 'Americano',
    iceCream: 'Coodie and Cream',
  }
  ```
  ```js
  // Object -> JSON
  const objToJson = JSON.stringify(jsObject)

  console.log(objToJson)  //{"coffee":"Americano", "iceCream":"Cookie and Cream"}
  console.log(typeof objToJson)  // string
  ```
  ```js
  // JSON -> Object
  const jsonToObj = JSON.parse(objToJson)

  console.log(jsonToObj)  // { coffee: 'Americano', iceCream: 'Coodie and Cream' }
  console.log(typeof jsonToObj)  // object
  ```