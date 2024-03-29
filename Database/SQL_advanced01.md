# SQL 심화

## 목차
- [Transaction](#transactions)
  - [개념](#트랜젝션이란)
  - [ACID](#transaction의-성질-acid)
  - [기본 문법](#기본-문법)
- [Trigger](#triggers)
  - [개념](#트리거란)
  - [기본 문법](#기본문법)
- [참고 사항](#참고-사항)
- [참고 자료](#참고-자료)

<br>

## Transactions

### 트랜젝션이란

- 여러 쿼리문을 묶어 하나의 작업처럼 처리하는 방법

- 이때 쿼리문들은 다 성공하던지 or 다 실패하던지!!


### Transaction의 성질: ACID
- Atomicity : 원자성

  트랜젝션 단위의 작업이 모두 반영되거나 모두 롤백된다. 

- consistency : 일관성

  하나의 트랜젝션 이전과 이후에서 데이터베이스의 상태는 이전과 같이 유효해야 한다. 이는 데이터베이스의 제약이나 규칙을 만족하는 상태를 유지해야 한다는 뜻이다.

  ex) '모든 고객은 이름을 가지고 있어야 한다.'와 같은 제약이 있는 경우 이름이 없는 고객을 추가하는 쿼리를 사용하는 경우 제약을 위반하게 된다.

- Isolation : 고립성

  모든 트랜젝션은 다른 트랜젝션들로부터 독립되어야 한다. 동시에 여러 트랜젝션이 수행될 경우 서로 영향을 미치게 되면 결과가 달라질 수 있다.

- Durability : 영구성

  한번 커밋된 트랜젝션은 영구적으로 적용된다.



### 기본 문법
```sql
START TRANSACTION;
state_ments;
...
[ROLLBACK|COMMIT];
```
- START TRANSACTION : 트랜젝션 구문의 시작을 알림

- COMMIT : 모든 작업이 정상적으로 완료되면 한꺼번에 DB에 반영

- ROLLBACK : 부분적으로 작업이 실패하면 트랜젝션에서 실행한 모든 연산을 취소하고 이전으로 되돌림

- TRANSACTION은 임시 데이터 영역에서 실행되며 성공 여부에 따라 COMMIT OR ROLLBACK

- 예시
  ```sql
  -- 자동커밋 비활성화
  SET autocommit = 0;
  -- user table생성
  CREATE TABLE users (
    id INT AUTO_INCREMENT,
      name VARCHAR(10) NOT NULL,
      PRIMARY KEY (id)
  );

  START TRANSACTION;
  INSERT INTO users (name)
  VALUES ('harry'), ('test');

  COMMIT;
  -- 또는 ROLLBACK;

  SELECT * FROM users;
  ```
  - MySQL은 기본적으로 변경사항을 자동 COMMIT하기 때문에 비활성화(활성화하려면 1로 다시 작성)


<br>

## Triggers

### 트리거란

- 특정 이벤트(INSERT, UPDATE, DELETE)에 대한 응답으로 자동실행되는 것

### 기본문법
```SQL
CREATE TRIGGER trigger_name
  {BEFORE | AFTER} {INSERT | UPDATE | DELETE} -- 언제
  ON table_name FOR EACH ROW  -- 어디서
  trigger_body;
```
- CREATE TRIGGER 키워드 다음에 생성하려는 트리거 이름 지정

- 각 레코드의 어느 지점에서 트리거가 실행될 지 지정(삽입, 수정, 삭제 전/후)

- ON 키워드 뒤에 트리거가 속한 테이블의 이름을 지정

- 트리거가 활성화도리 때 실행할 코드를 trigger_body에 지정
  - 여러 명령문을 실행하려면 BEGIN END 키워드로 묶어서 사용

- 생성된 트리거는 `SHOW TRIGGERS;`로 확인, 삭제는 `DROP TRIGGER trigger_name;`
 
- 예시
  ```SQL
  CREATE TABLE articles (
    id INT AUTO_INCREMENT,
      title VARCHAR(100) NOT NULL,
      createAt DATETIME NOT NULL,
      updatedAt DATETIME NOT NULL,
      PRIMARY KEY (id)
  );

  INSERT INTO articles (title, createAt, updatedAt)
  VALUES ('title1', CURRENT_TIME(), CURRENT_TIME());

  DELIMITER //  -- 여기서부터 종료조건은 '//'이다
  CREATE TRIGGER myTrigger
    BEFORE UPDATE
      ON articles FOR EACH ROW
  BEGIN
      SET NEW.updatedAt = CURRENT_TIME();
      ...
  END//
  DELIMITER ; -- 여기서부터는 다시 종료조건이 ';'이다
  ```
  - BEGIN-END 구문 사이에 여러 SQL문이 작성되기 때문에 하나의 트리거로써 작동되도록 DELIMITER로 묶어줌

    - 이때 문장의 끝이 혼동되지 않도록 DELIMETER로 종료조건을 잠시 바꿔줌
  
  - 특정 시점 전/후 값에 접근할 수 있도록 OLD와 NEW 키워드를 필드명에 적
    - INSERT : NEW
    - UPDATE : OLD, NEW
    - DELETE : OLD 사용 가능

<br>

## 참고 사항

### Triggers 생성 시 에러
- 트랜젝션 생성 후 정상적으로 종료되지 않아 발생하는 다음과 같은 에러 해결법
  - Error Code:2013.Lost connection to MySQL server diring query
  - Error Code:2015.Lock wait timeout exceeded; try restarting transaction
```sql
-- 실행중인 프로세스 목록 확인
SELECT * FROM information_schema.INNODB_TRX;

-- 특정 프로세스의 trx_mysql_thread_id 삭제
KILL [trx_mysql_thread_id1];
```

<br>

## 참고 자료

https://hanamon.kr/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98%EC%9D%98-acid-%EC%84%B1%EC%A7%88/