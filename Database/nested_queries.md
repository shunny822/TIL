# Nested Queries

## Subquery

> 단일 쿼리문에 여러 테이블의 데이터를 결합하는 방법

- 조건에 따라 하나 이상의 테이블에서 데이터를 검색하는데 사용

### 예제
- 가장 많은 돈을 소비한 고객 번호 조회
  ```sql
  SELECT customerNumber, amount
  FROM payments
  WHERE amount = (
    SELECT MAX(amount)
      FROM payments
  );
  ```

- 미국에 있는 사무실에서 근무(offices table)하는 직원의 이름과 성(employees table) 조회
  ```sql
  SELECT lastName, firstName
  FROM employees
  WHERE officeCode IN (
    SELECT officeCode
      FROM offices
      WHERE country = 'USA'
  );
  ```

- 소비를 한 고객들 중 지불한 금액이 가장 높은 고객의 customerNumber, amount, contactFirstName을 조회(customers, payments table)
  ```sql
  SELECT customerNumber, amount, contactFirstName
  FROM (
    SELECT t1.customerNumber, amount, contactFirstName
    FROM payments AS t1
    INNER JOIN customers USING (customerNumber)
  ) AS mySub
  WHERE amount = (
    SELECT MAX(amount)
    FROM payments
  );
  ```
  - FROM절에 서브쿼리로 새로 만든 테이블에는 별칭을 붙여줘야 함


### EXISTS operator

: 쿼리문에서 반환된 레코드의 존재 여부를 확인
```sql
SELECT
  select_list
FROM
  table
WHERE
  [NOT] EXISTS (subquery);
```
- 서브쿼리가 하나 이상의 행을 반환하면 EXISTS연산자는 true를, 그렇지 않으면 false 반환

- 주로 WHERE 절에서 서브쿼리의 반환값 존재 여부를 확인하는데 사용

- 예시
  ```SQL
  SELECT customerNumber, customerName
  FROM customers
  WHERE EXISTS (
    SELECT *
    FROM orders
    WHERE customers.customerNumber = orders.customerNumber
  );
  ```

<br>

## Conditional statement

### CASE statement

: SQL문에서 조건문을 구성

```SQL
CASE case_value
  WHEN when_value1 THEN statements
  WHEN when_value2 THEN statements
  ...
  [ELSE else_statements]

END CASE;
```
- case_value가 when_value와 동일한 것을 찾을 때까지 순차적으로 비교, 동일하면 THEN 절 실행

- 동일한 값이 없으면 ELSE 절 실행

- 예제
  ```SQL
  SELECT contactFirstName, creditLimit,
    CASE
      WHEN creditLimit > 100000 THEN 'VIP'
      WHEN creditLimit > 70000 THEN 'Platinum'
      ELSE 'Bronze'
    END AS grade
  FROM customers;
  ```