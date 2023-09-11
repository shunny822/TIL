# Node.js에 MySQL 연동

## 목차
- [MySQL2 설치 및 연동](#mysql2-설치-및-연동)
- [MySQL2 사용](#mysql2-사용)
- [참고 자료](#참고-자료)

<br>

## MySQL2 설치 및 연동

설치
```
npm install --save mysql2
```

연동
```js
const mysql = require('mysql2/promise');
// 변수 관리
require('dotenv').config();

const dbConfig = {
  host: process.env.DB_HOST,
  port: '13306',
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_NAME
};

const dbPool = mysql.createPool(dbConfig);
```
- 나는 코드를 좀 더 깔끔하게 쓸 수 있도록 mysql2/promise를 사용하였다.

- MySQL과 연결하기 위해 host, port, user, password, database(이름) 정보를 작성하고 풀을 생성했다.
  - host는 현재 local에서 접속하므로 '127.0.0.1'
  - port는 port forwarding으로 설정한 값
  - user와 password는 MySQL에 접근 가능한 사용자 정보

- `createConnection`을 사용하여 연결할 수도 있다. DB에 접근할 때마다 connection을 생성해야 한다.

- `createPool`은 connection을 한 번 생성한 상태로 DB에 접근 시 connection을 가져가고, 작업이 끝나면 반환하여 보관하는 식으로 동작한다. 이렇게 connection을 미리 생성해두고 관리하는 방식은 DB과부하를 방지하고 유동적으로 Connection을 관리할 수 있다.

<br>

## MySQL2 사용

```js
const getTableData = async (tableName) => {
  try {
    const connection = await dbPool.getConnection();
    const [rows, fields] = await connection.execute(`SELECT * FROM ${tableName};`);
    connection.release();
    return rows;
  } catch (error) {
    console.error(error);
    return false;
  }
}
```
- `getConnection()` : dbPool에서 connection을 가져온다.

- 가져온 Connection을 쿼리문을 작성하여 실행(execute)한다.

- try-catch로 error처리를 해주었다.


```js
const deletePayOption = async (optionName) => {
  const query = `
    DELETE FROM payOption
    WHERE optionName = ?;
  `;
  try {
    const connection = await dbPool.getConnection();
    await connection.execute(query, [optionName]);
    connection.release();
  } catch (error) {
    console.error(error);
  }
}
```
처음 예시처럼 query를 literal 자체로 사용할 수 있지만 '?'를 사용하여 변수를 받을 수도 있다. 변수를 넣을 위치에 '?'를 쓰고 execute 함수의 두번째 인자로 변수를 array에 담아 넣어준다. 이렇게 할 경우 literal 자체를 보냈을 때 문자열을 가로챌 경우 발생하는 보안적 위험에 더 안전하다.

<br>

## 참고 자료

https://www.npmjs.com/package/mysql2

https://cotak.tistory.com/105