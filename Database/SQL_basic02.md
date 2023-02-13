# SQL basic

## Filtering data

### DISTINCT

: 조회 결과에서 중복된 레코드 제거

- SELECT 키워드 바로 뒤에 작성

- SELECT DISTINCT 키워드 뒤에 고유한 값을 선택하려는 하나 이상의 필드 지정
  ```SQL
  SELECT DISTINCT
    lastName
  FROM
    employees;
  ```


### WHERE clause

: 조회 시 특정 검색조건을 지정

- WHERE syntax

  - FROM clause 뒤에 위치

  - search_condition은 비교연산자 및 논리연산자[AND(&&), OR(||), NOT(!)]를 사용

- 비교연산자와 논리연산자 사용
  ```sql
  SELECT
    lastName, firstName, officeCode, jobTitle
  FROM
    employees
  -- 5 미만이며 jobTitle이 'Sales Rep'이 아닌 값으로 지정
  WHERE
    officeCode < 5
    AND jobTitle != 'Sales Rep';
  ```

- BETWEEN A AND B : A ~ B(A와 B 포함)
  ```sql
  SELECT
    lastName, firstName, officeCode, jobTitle
  FROM
    employees
  WHERE
    officeCode BETWEEN 1 AND 4
  ORDER BY
    officeCode;
  -- WHERE과 ORDER BY 같이 사용 시 WHERE을 먼저 씀
  ```

- IN operators : 값이 특정 목록 안에 있는지 확인
  ```sql
  SELECT
    lastName, firstName, officeCode
  FROM
    employees
  WHERE
    officeCode IN (1, 3, 4);
  ```

- LIKE operator : 값이 특정 패턴에 일치하는지 확인, Wildcard Characters(%, _)
  - '%' : 0개 이상의 문자열과 일치여부 확인
```sql
-- employees에서 lastName이 son으로 끝나는 데이터
SELECT
  lastName, firstName
FROM
  employees
WHERE
  lastName LIKE '%son';
```
  - '_' : 단일 문자와 일치여부 확인(자리수를 알아야 함)
```sql
-- employees에서 firstName이 4자리이며 y로 끝나는 데이터
SELECT
  lastName, firstName
FROM
  employees
WHERE
  firstName LIKE '___y';
```


### LIMIT clause

: 조회하는 레코드 수를 제한

```sql
SELECT
  select_list
FROM
  table_name
LIMIT [offset,] row_count;
```
- offset : 데이터 조회 시작 부분에서 조회하지 않을 데이터의 수, 쓰지 않아도 되며 쓰지 않을 경우 처음부터 조회

- row_count : 조회할 최대 레코드 수 지정
  ```sql
  -- creditLimit 기준 내림차순으로 4번째부터 7번째 데이터 조회
  SELECT
    contactFirstName, creditLimit
  FROM
    customers
  ORDER BY
    creditLimit DESC
  LIMIT 3, 4;
  -- (LIMIT 4 OFFSET 3;)과 같은 코드임
  ```

<br>

## Grouping data

### GROUP BY clause

: 레코드를 그룹화하여 요약본 생성(집계함수 사용)

- Aggregation Functions

  - 값에 대한 계산을 수행하고 단일한 값을 반환하는 함수

  - SUM, AVG, MAX, MIN, COUNT

- FROM, WHERE 절 뒤에 위치

- GROUP BY 절 뒤에 그룹화할 필드 목록 작성
  ```sql
  -- jobTitle을 그룹화하고 각 그룹에 대해 집계된 값 count
  SELECT
    jobTitle, COUNT(*)
  FROM
    emplyees
  GROUP BY
    jobTitle;
  ```
  ```sql
  -- country field 그룹화 후 각 그룹에서 creditLimit의 평균값 내림차순 조회
  SELECT
    country, AVG(creditLimit) AS avgOfCreditLimit
  FROM
    customers
  GROUP BY
    country
  ORDER BY
    avgOfCreditLimit DESC;
  ```

- HAVING : GROUP BY와 함께 사용, 집계 항목에 대한 세부 조건 지정
  ```sql
  -- country 필드 그룹화 후 creditLimit의 평균값이 80000 초과인 데이터 조회
  SELECT
    country, AVG(creditLimit)
  FROM
    customers
  GROUP BY
    country
  HAVING
    AVG(creditLimit) > 80000;
  ```

<br>

## 참고사항

### SELECT statement 작성 순서

SELECT - FROM - WHERE - GROUP BY - HAVING - ORDER BY - LIMIT

### 정렬 시 NULL

MySQL에서 NULL은 오름차순 정렬 시 가장 먼저 출력