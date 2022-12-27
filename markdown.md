# 마크다운
## 특징
- 텍스트 기반의 마크업 언어(문서를 구조화)
- 최소한의 단순 텍스트 문법을 사용하여 내용을 작성하며 다양한 환경에서 변환 가능(HTML 등)

## 활용
README.md : 오픈소스의 공식문서나 개인 프로젝트의 소개서 작성, 모든 페이지에 README.md를 넣어 문서를 바로 볼 수 있도록 활용
- 예시
  - 정적 사이트 생성기 기반 블로그 : Jekyll, Gatsby, Velog
  - 마크다운 기반 SW : Notion, Jupyter notebook 등

 ## 문법
### 1. Heading : 제목이나 소제목
  - #의 개수에 따라 h1~h6까지 크기 표현 가능
  - 문서의 구조를 위해 작성되며 글자 크기 조절을 위해 사용하면 안됨

 #### <예시> 
  # h1  
  ## h2
  ### h3
  #### h4
  ##### h5
  ###### h6

### 2. List : 목록
- Tab으로 하위항목을 구성할 수 있음
- 순서가 있는 리스트(ol)
  1. 알람이 울린다.
  2. 눈을 뜬다.
  3. 침대에서 일어난다.
- 순서가 없는 리스트(ul)
  - 로마
  - 피렌체
  - 베네치아

### 3. Fenced Code block : 코드 블럭
- backtick 기호 3개를 활용하여 작성(``````)
- 코드블럭에 특정 언어 명시하여 Syntax Highlighting 적용 가능
```python
"Hello World!"
```

### 4. Inline Code block : 코드 블럭
- backtick 기호 1개를 `인라인`에 활용하여 작성(``)

### 5. link
- `[문자열](url)`을 통해 링크 작성 가능
```
[구글](https://google.com)
[파일1](b/b.txt)
```

### 6. 이미지
- `![문자열](url)`을 통해 이미지 첨부 가능
- 특정 파일 연결 가능
```
![사진2](b/2.png)
```

### 7. 인용문
- `>`를 사용하여 인용문 작성
```
> Hello World!
```

### 8. 테이블
```
| Syntax    | Description |
|-----------|-------------|
| Header    | Title       |
| Paragragh | Text        |
```

### 9. 텍스트 강조
- **굵게** : 2개의 asterisks 또는 underscores
```
  - **asterisk**
  - __underscore__
  ```

- *기울임* : asterisk 또는 underscore 1개
```
  - *asterisk*
  - _underscore_
  ```

### 10. 수평선
- 3개 이상의 asterisks, dashes, underscores
```
***
---
___
```
