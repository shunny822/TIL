# Workbench 활용 가이드

### ▶ Workbench에서 MySQL 접속
MySQL WorkBench 접속 - MySQL과 연결

![접속](workbench_jpg/%EA%B7%B8%EB%A6%BC01.jpg)

### ▶ 데이터베이스 import
`Administration` - `Data Import/Restore` - `Import from Self-Contained File` - 파일 선택 후 `Start Import`

![database import](workbench_jpg/%EA%B7%B8%EB%A6%BC02.jpg)

`Create a new schema` - 이름 지정 - charset/collation을 utf8과 utf8_bin으로 지정

### ▶ 한글 설정
해당 데이터베이스의 도구모양 - `Charset/Collation`을 utf8과 utf8_bin으로 설정하여 default값으로 지정

![utf8설정](workbench_jpg/%EA%B7%B8%EB%A6%BC03.jpg)

### ▶ 쿼리 실행
해당 데이터베이스 선택하여 쿼리에디터 - 쿼리문 작성 - 쿼리 실행(번개모양)

![쿼리실행](workbench_jpg/%EA%B7%B8%EB%A6%BC04.jpg)

- 쿼리문
  ```
  SELECT * from 조회할 table명;
  ```