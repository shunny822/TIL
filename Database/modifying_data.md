# Modifying Data

## Insert data into table

### INSERT statement : 레코드 삽입
```sql
INSERT INTO table_name(c1, c2, ...)
VALUES (v1, v2, ...)
```
- INSERT INTO 절 뒤에 데이터 이름과 괄호 안에 필드 목록 작성

- VALUES 뒤 괄호 안에 해당 필드에 삽입할 값 목록 작성

- 필드와 값의 순서가 일치해야 함

- 예제
  ```sql
  INSERT INTO articles (title, content, createAt)
  VALUES
	('title1', 'content1', '1900-01-01'), 
  ('title2', 'content2', '1800-01-01'),
  ('myTitle', 'myContent', CURDATE());
  ```
  - CURDATE() : 현재 날짜를 자동으로 입력해주는 Date Function

<br>

## Update data in table

### UPDATE statement : 레코드 수정
```sql
UPDATE table_name
SET column_name = expression, 
WHERE
  condition;
```
- SET절 다음에 수정할 필드와 새 값을 지정

- WHERE 절에서 수정할 레코드를 지정하는 조건 작성(작성하지 않으면 모든 레코드 수정)

- 예제 1
  ```SQL
  UPDATE articles
  SET
    title = 'newTitle2',
    content = 'newContent2'
  WHERE
    id = 2;
  ```

- REPLACE : 지정된 문자열 변경
  ```sql
  UPDATE
    articles
  SET
    content = REPLACE(content, 'content', 'TEST');
  -- 필드명 = REPLACE(필드명, '기존 문자열', '바꿀 문자열')
  ```

<br>

## Delete data from table

### DELETE statement : 레코드 삭제
```sql
DELETE FROM table_name
WHERE
  condition;
```
- DELETE FROM 뒤에 테이블 이름 작성

- WHERE 절에서 삭제할 레코드를 지정하는 조건 작성(작성 안하면 모든 레코드 삭제)

- 예제 1
  ```SQL
  DELETE FROM
    articles
  WHERE
    id = 1;
  ```

- 예제 2 : 가장 최근에 작성된 레코드 2개 삭제하기
  ```SQL
  DELETE FROM
    articles
  ORDER BY
    createAt DESC
  LIMIT 2;
  ```
