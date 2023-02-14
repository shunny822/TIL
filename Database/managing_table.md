# Managing Table

## Create a table

### CREATE TABLE statement
```sql
CREATE TABLE table_name (
  column_1 data_type,
  column_2 data type,
  ...,
  constraints
);
```
- 각 필드에 적용할 데이터 타입 작성

- 테이블 및 필드에 대한 제약조건(constraints) 작성

### 예시
```sql
CREATE TABLE examples (
    examId INT AUTO_INCREMENT,
    lastName VARCHAR(50) NOT NULL,
    firstName VARCHAR(50) NOT NULL,
    PRIMARY KEY (examId)
);
```
- Constraint : 데이터 무결성(데이터의 정확성과 일관성 보증)을 지키기 위해 데이터를 입력받을 때 실행하는 검사 규칙

  - PRIMARY KEY : 해당 필드를 기본키로 지정

  - NOT NULL : 해당 필드에 NULL값을 저장하지 못하도록 지정

- AUTO_INCREMENT attribute : 테이블의 기본키에 대한 번호 자동 생성

  - 기본키 필드에 사용(고유한 숫자를 부여)

  - 시작 값은 1이며 데이터 입력 시 값을 생략하면 자동으로 1씩 증가(게시판 글 올리는것 같이)

  - 이미 사용한 값을 재사용하지 않음

  - 기본적으로 NOT NULL 제약조건을 포함



### Table 구조 확인
```sql
SHOW COLUMNS FROM examples;
```


### 대표적 MySQL Data Types
- Numeric(숫자형) : INT, FLOAT, ...

- String(문자형) : VARCHAR, TEXT, ...
  - VARCHAR : CHAR는 길이를 50으로 지정하면 어떤 길이의 문자열이 들어와도 길이가 50이나 VARCHAR는 가변길이를 가짐
  - TEXT는 large text data를 받으며 따로 길이를 지정하지 않음(길이 제한은 있으나 매우 큼)

- Data and Time(날짜형) : DATE, DATETIME, ...

<br>

## Delete a table

### DROP TABLE statement
```sql
DROP TABLE table_name;
```
- 테이블 삭제

<br>

## Modifying table fields

### ALTER TABLE statement : 필드 조작

1. ALTER TABLE ADD
  ```SQL
  ALTER TABLE
    table_name
  ADD
    new_column_name column_definition;
  ```
- ADD키워드 이후 추가하고자 하는 필드 이름과 데이터타입 및 제약조건 작성

- 예제
  ```SQL
  ALTER TABLE
    examples
  ADD age INT NOT NULL,
  ADD address VARCHAR(100) NOT NULL;
  ```


2. ALTER TABLE MODIFY
  ```SQL
  ALTER TABLE
    table_name
  MODIFY
    column_name colomn_definition;
  ```
- MODIFY 이후 변경하고자 하는 필드 이름, 데이터타입 및 제약조건 작성

- 예시
  ```SQL
  ALTER TABLE
    examples
  MODIFY lastName VARCHAR(10) NOT NULL,
  MODIFY firstName VARCHAR(10) NOT NULL;
  ```


3. ALTER TABLE CHANGE COLUMN
```SQL
ALTER TABLE
  table_name
CHANGE COLUMN
  original_name new_name colum_definition;
```
- CHANGE COLUMN 이후 기존필드 이름, 변경할 필드 이름, 데이터타입 및 제약조건 작성

- 예시
  ```sql
  ALTER TABLE
  	examples
  CHANGE COLUMN
	country state VARCHAR(100) NOT NULL;
  ```
  - 데이터타입과 제약조건은 기존과 같더라도 생략 불가능, 변경 가능하여 modify의 역할도 함께 


4. ALTER TABLE DROP COLUMN
```SQL
ALTER TABLE
  table_name
DROP COLUMN
  column_name;
```
- DROP COLUMN 이후 삭제하고자 하는 필드이름 작성

- 예시
  ```SQL
  ALTER TABLE
    examples
  DROP COLUMN state,
  DROP COLUMN age;
  ```



<br>

## 참고사항
### NOT NULL
- 데이터베이스를 사용하는 프로그램에 따라 NULL을 저장할 필요가 없는 경우가 많아 되도록 NOT NULL로 정의

- '값이 없다'라는 표현은 테이블에 0이나 빈 문자열로 기록하는 것을 권장!!
