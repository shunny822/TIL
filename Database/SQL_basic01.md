# SQL basic

## SQL

- Structure Query Language : 테이블 형태로 `구조화`된 관계형 데이터베이스에 정보 `질의(요청)`하는 언어

- 데이터베이스에 정보를 저장하고 처리하기 위한 프로그래밍 언어

### SQL Syntax
- 구글에 'how old is earth'라고 검색하는 것 처럼 SQL에서 다음과 같이 작성
  ```sql
  SELECT column_name FROM table_name;
  ```

- SQL 키워드는 대소문자를 구분하지 않으나 가독성을 위해 대문자로 작성

- 각 SQL statements의 끝에 세미콜론(;)으로 표시(각각의 statements 구분)

### SQL Statements
데이터베이스에서 수행 목적에 따라 대체로 4가지 범주로 나뉨

|             유형              |            역할             |       SQL 키워드     |
|-------------------------------|----------------------------|----------------------|
|DDL(Data Definition Language)  |데이터의 기본구조 및 형식 변경| CREATE, DROP, ALTER  |
|DQL(Data Query Language)       |         데이터 검색         |       SELECT         |
|DML(Data Manipulation Language)|데이터 조작(추가, 수정, 삭제) |INSERT, UPDATE, DELETE|
|DCL(Data Control Language)     |데이터 및 작업에 대한 사용자권한 제어|COMMIT, ROLLBACK, GRANT, REVOKE|

<br>

## Querying data
### SELECT statement : 테이블에서 데이터 조회
<기본 문법>
```sql
SELECT
  field_name
FROM
  table_name;
```
- SELECT 키워드 뒤에 필요로 하는 필드명 하나 이상 지정, 모든 필드의 데이터 조회는 *(asterisk)

- FROM 키워드 다음에 데이터를 선택하려는 테이블이름 지정
  ```sql
  SELECT * FROM employees;
  ```

- AS 키워드를 이용해 필드에 별칭을 지정하여 출력할 수 있음
  ```sql
  SELECT firstName AS '이름' FROM employees;
  -- firstName 필드명이 '이름'으로 바뀌어 출력됨
  ```

- 데이터 간의 사칙연산 가능
  ```sql
  SELECT
    productCode,
    quantityOrdered * priceEach AS '주문 총액'
  FROM
    orderdetails;
    -- '주문총액'이라는 이름으로 quantityOrdered와 priceEach을 곱한 값들이 출력됨
  ```


<br>

## Sorting data
### ORDER BY clause(절) : 조회 결과의 레코드 정렬
<기본 문법>
```sql
SELECT
  field_name
FROM
  table_name
ORDER BY
  column1 [ASC|DESC], 
  column2 [ASC|DESC];
```
- FROM clause 뒤에 위치

- 하나 이상의 컬럼을 기준으로 결과를 오름차순 또는 내림차순으로 정렬(ASC(오름차순)이 기본값)
  ```sql
  SELECT
    lastName, firstName
  FROM
    employees
  ORDER BY
    lastName DESC,
    firstName;
    -- lastName 기준 내림차순으로 정렬 후 lastName의 같은 항목을 firstName 기준 오름차순 정렬
    -- lastName 1순위, firstNane 2순위로 생각하기
  ```
  ```sql
  SELECT
    quantityOrdered * priceEach AS totalSales
  FROM
    orderdetails
  ORDER BY
    totalSales DESC;
  ```