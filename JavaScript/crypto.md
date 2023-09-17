# crypto

## 목차
- [crypto란?](#crypto란)
  - [salt](#salt)
- [crypto-pbkdf 사용법](#crypto-pbkdf-사용법)
- [참고 자료](#참고-자료)

<br>

## crypto란?
crypto는 node.js의 내장 모듈로 정보를 hashing하여 암호화한다. 단방향 암호화로 같은 입력값은 같은 출력값이 나오게 되고, 출력값으로는 입력값을 유추할 수 없다. 일반적으로 비밀번호는 plane text로 저장하지 않고, 해시 알고리즘으로 암호화한 후 저장해야 한다.

암호화 방법에는 pbkdf, scrypt, bcrypt이 있으며 비밀번호와 같이 보안이 중요한 개인정보는 보안과 호환성을 고려하여 bcrypt가 널리 사용된다고 한다.

### salt

보안을 더 강화(해커의 복호화 방지 등)하기 위해 salt를 사용할 수 있다. 입력으로 들어가는 비밀번호에 추가 문자열(salt)을 덧붙여 암호화 하는 방법이다.

기본적으로 같은 입력에 대해 같은 출력이 나오는 단방향 암호화가 salt 사용으로 인해 다른 출력이 나오게끔 하는 것이다. 사용자마다 고유의 salt값을 가지고 있기 때문에 비밀번호가 같은 A, B 중 A의 비밀번호가 노출되어도 B는 안전할 수 있다.

<br>

## crypto-pbkdf 사용법

여기서는 pbkdf 사용법을 알아보겠다.

```js
const crypto = require('crypto');
```
module import

```js
// 회원가입 시
const salt = crypto.randomBytes(32).toString('hex');
const hashedPW = crypto.pbkdf2Sync(password1, salt, 100000, 64, 'sha512').toString('hex');
```
- `crypto.randomBytes(size[, callback])`

  : 랜덤으로 원하는 byte의 salt 생성(여기서는 16진수로 생성함)

- `crypto.pbkdf2Sync(password, salt, iterations, keylen, digest)`

  : password와 salt를 결합해 설정한 iterations만큼 hashing 수행

→ 이렇게 생성된 salt와 password를 DB에 저장한다.

```js
// 로그인 시
const inputPWHashing = crypto.pbkdf2Sync(password, user.salt, 100000, 64, 'sha512').toString('hex');

if (user.password !== inputPWHashing) {
  res.redirect('/login');
}
```
→ 위와 회원가입과 같이 input password를 사용자의 salt값과 함께 같은 방식으로 hashing하여 DB에 저장된 password와 일치하는지 확인한다.


<br>

## 참고 자료

https://nodejs.org/api/crypto.html

https://inpa.tistory.com/entry/NODE-%F0%9F%93%9A-crypto-%EB%AA%A8%EB%93%88-%EC%95%94%ED%98%B8%ED%99%94