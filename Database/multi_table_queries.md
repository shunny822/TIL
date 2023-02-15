# Multi Table Queries

관계형 데이터베이스에서는 테이블을 분리하여 관리를 용이하게 하고 PK, FK로 연결지어 출력

## Joining tables

### INNER JOIN clause

: 두 테이블에서 값이 일치하는 레코드에 대해서만 결과를 반환

```sql
SELECT
  select_list
FROM
  table1
INNER JOIN table2
  ON table1.fk = table2.pk;
```
- FROM절 이후 메인 테이블(table1) 지정

- INNER JOIN 절 이후 메인 테이블과 조인할 테이블(table2) 지정

- ON 키워드 이후 조인 조건 : table1과 table2 간의 레코드를 일치시키는 규칙 지정

- 예시
  ```sql
  SELECT
    productCode, productName, productlines.textDescription
  FROM
    products
  INNER JOIN productlines
    ON products.productLIne = productlines.productLine;
  ```
  - 조회하려는 필드가 두 테이블에 모두 있으면 어떤 테이블의 필드인지 적어줘야 함(SELECT문 참고)

- GROUP BY 적용 예시
  ```SQL
  SELECT
    t1.orderNumber,
      status,
      SUM(quantityOrdered * priceEach) AS totalSales
  FROM
    orders AS t1
  INNER JOIN orderdetails AS t2
    ON t1.orderNumber = t2.orderNumber
  GROUP BY
    orderNumber;
  ```
  - TABLE에도 AS로 별칭을 붙여 간단하게 쓸 수 있음


### LEFT JOIN clause

: 오른쪽 테이블의 일치하는 레코드와 함께 왼쪽 테이블의 모든 레코드 반환

```SQL
SELECT
  select_list
FROM
  table1
LEFT [OUTER] JOIN table2
  ON table1.fk = table.pk;
```
- FROM절 이후 왼쪽 테이블 지정(table1)

- LEFT JOIN절 이후 오른쪽 테이블 지정(table2)

- ON 키워드 이후 조인 조건(fk와 pk)

- 예시
  ```sql
  SELECT *
  FROM articles
  LEFT JOIN users
    ON articles.userId = users.id;
  ```

- WHERE절을 이용하여 주문내역이 없는 고객 조회
  ```SQL
  SELECT
    contactFirstName,
    orderNumber,
    status
  FROM
    customers AS t1
  LEFT JOIN orders AS t2
    ON t1.customerNumber = t2.customerNumber
  WHERE t2.orderNumber IS NULL;
  ```

- 특징

  - 왼쪽 테이블은 무조건 표시하고 오른쪽 테이블과 매치되는 레코드가 없으면 NULL을 표시
  
  - 왼쪽 테이블 한 개의 레코드에 여러개의 오른쪽 테이블 레코드 일치 시 왼쪽 테이블 레코드 여러 번 표시



### RIGHT JOIN clause

: 왼쪽 테이블의 일치하는 레코드와 함께 오른쪽 테이블의 모든 레코드 반환

```SQL
SELECT
  select_list
FROM
  table1
LEFT [OUTER] JOIN table2
  ON table1.fk = table.pk;
```
- LEFT JOIN과 비교하였을 때 테이블의 위치는 변함 없으며 LEFT JOIN에서 적용되는 것이 반대로 적용
