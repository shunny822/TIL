# Fundamentals of HTML and CSS

## Intro

Web page : HTML, CSS, JavaScript 등의 웹기술을 이용하여 만들어진 하나의 인터넷 문서

  → Web site : 인터넷에 여러 개의 Web page가 모인 것, 사용자들에게 정보나 서비스를 제공하는 공간

  → World Wide Web : 인터넷으로 연결된 컴퓨터들이 정보를 공유하는 거대한 정보 공간

### Web의 구성

- HTML : Structure

- CSS : Styling

- JavaScript : Behaviour

<br>

## HTML

> HyperText Markup Language, 웹 페이지의 의미와 구조를 정의하는 언어

- Hypertext

  - 웹 페이지를 다른 페이지로 연결하는 링크

  - 참조를 통해 사용자가 한 문서에서 다른 문서로 즉시 접근할 수 있는 텍스트

- Markup Language

  - 태그 등을 이용하여 문서나 데이터의 구조를 명시하는 언어

  - 종류 : HTML, Markdown

### Element
  → opening tag + content + closing tag

### Attributes
```html
<p class="editor-note">My cat is very grumpy</p>
```
여기서 `class="editor-note"` 부분이 속성(이름+값)

- 규칙
  - 요소 이름과 속성 사이에 공백이 있어야 함

  - 하나 이상의 속성들이 있는 경우엔 속성 사이에 공백으로 구분함

  - 속성 값은 큰 따옴표로 감싸야 함

- 목적
  - 나타내고 싶지 않지만 추가적인 기능, 내용을 담고 싶을 때 사용

  - CSS가 해당 요소를 선택하기 위한 값으로 활용

### HTML문서의 구조

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>My page</title>
</head>
<body>
  <a href="https://www.google.co.kr">구글 페이지로 이동!!!</a>
  <img src="https://random.imagecdn.app/500/150" alt="랜덤 이미지">
</body>
</html>
```
1. `<!DOCTYPE html>` : 해당 문서가 html문서라는 것을 나타냄

2. `<html></html>` : 전체 페이지의 콘텐츠를 포함, 최상위 태그

3. `<head></head>` : HTML문서에 관련된 설명과 설정 등, 사용자에게 보이지 않음

    - `<meta charset="">` : 문서에 필요한 설정값들을 쓰는 곳

    - `<title></title>` : 브라우저 탭 및 즐겨찾기 시 표시되는 제목으로 사용

4. `<body></body>` : 페이지에 표시되는 모든 컨텐츠

    - `<a href=""></a>` : hypertext 부분! anchor(닻)라고 부름, 속성에 url작성

    - `<img src="" alt="">` : src(소스)에 이미지 주소 작성, alt(알트)에 대체 텍스트 작성
      
      - 이미지에 문제가 생겼을 때 이미지에 대한 대체 텍스트가 보여짐

      - 시각장애인은 인터넷 사용 시 스캐너가 대체 텍스트를 읽어줌


### Text의 구조
```html
<h1>메인 제목</h1>
<h2>중제목</h2>
<p>이건 <em>emphasis</em>입니다.</p>
<p>이건 <strong>strong</strong>입니다.</p>
<ul>
  <li>파이썬</li>
  <li>알고리즘</li>
  <li>데이터베이스</li>
</ul>
```
1. 제목
    - h1 ~ h6까지 여섯 단계

    - 단순히 텍스트 크기 조절이 아닌 텍스트의 구조와 의미를 제공

2. 문단
    - `<p></p>` : 문단을 나눔

3. 목록
    - `<ol></ol>` : ordered list로 순서가 있는 목록

    - `<ul></ul>` : unordered list로 순서가 없는 목록

    - `<li></li>` : ol/ul 목록의 각 항목을 둘러쌈

4. 텍스트 강조
    - `<em></em>` : 글자 기울임

    - `<strong></strong>` : 굵은 글자


cf) 태그에 태그가 포함된 경우 부모태그와 자식태그 관계

<br>

## CSS

> Cascading Style Sheet, 웹 페이지의 디자인과 레이아웃을 구성하는 언어

### CSS 구문
```css
/* h1 태그를 선택해서 색을 blue로, 크기를 15px로 변경 */
h1 {
  color: blue;
  font-size: 15px;
}
```
- h1 : 선택자(Selector)

- `color: blue;` : 선언(Declaration)

- `font-size: 15px;` : 속성(property)과 값(value)


### CSS 적용 방법
1. 인라인(Inline) 스타일 : h1 태그 작성시 같이 작성
    ```html
    <h1 style="color: blue; font-size: 15px;">Hello World!</h1>
    ```
    - 우선순위에 혼란을 줄 수 있어 사용 지양!

2. 내부(Internal) 스타일 시트 : head부분에 위치하며 `<style></style>` 사이에 작성
    ```html
    <style>
      h1 {
        color: blue;
        font-size: 15px;
      }
    </style>
    ```

3. 외부(External) 스타일 시트 : `<link>`를 통해 별도의 CSS파일 생성 후 불러오는 방식

### CSS Selectors

> HTML 요소를 선택하여 스타일을 적용할 수 있도록 함

```html
...
<head>
  <style>
    /* 전체 선택자 */
    * {
      color: red;
    }

    /* 요소 선택자 */
    h2 {
      color: orange;
    }

    h3,
    h4 {
      color: blue;
    }

    /* 클래스 선택자 */
    .green {
      color: green;
    }

    /* id 선택자 */
    #purple {
      color: purple;
    }

    /* 자식 결합자 */
    .green > span {
      color:hotpink;
      font-size: 50px;
    }

    /* 자손 결합자 */
    .green li {
      font-size: 50px;
    }
  </style>
</head>
<body>
  <h1 class="green">Heading</h1>
  <h2>선택자 연습</h2>
  <h3>Hello</h3>
  <h4>Nice to meet you</h4>
  <p id="purple">과목 목록</p>
  <ul class="green">
    <li>파이썬</li>
    <li>알고리즘</li>
    <li>웹
      <ol>
        <li>HTML</li>
        <li>CSS</li>
        <li>JS</li>
      </ol>
    </li>
  </ul>
  <P class="green">Lorem, <span>ipsum</span> dolor.</P>
</body>
...
```

1. 기본 선택자

  - 전체(*) 선택자 : 문서의 태그 전체 선택

  - 요소(tag) 선택자 : 지정한 모든 태그 선택

  - 클래스(class) 선택자
    - 주어진 클래스 속성을 가진 모든 요소를 선택
    
    - `.class_name {}`으로 만들고 적용 시 속성에 class="class_name" 작성

  - 아이디(id) 선택자
    -  주어진 아이디 속성을 가진 요소 선택
    
    - 클래스 선택자와 사용이 비슷하나 주어진 아이디를 가진 요소가 하나만 있어야 함

    - `#id_name {}`으로 만들고 적용 시 속성에 id="id_name" 작성

  - 속성(attr) 선택자

2. 결합자 (Combinations)

  - 자식 결합자(>)
    - 첫 번째 요소의 직계 자식 요소만 선택

    - 예시) `ul > li`는 `<ul>` 안에서 한단계 낮은 모든 `<li>`를 선택

  - 자손 결합자(" "(space))
    - 첫 번재 요소의 모든 자손 요소들 선택

    - 예시) `p span`은 `<p>`안에 있는 모든 `<span>`을 선택


### 우선순위

`!important` → 인라인 스타일 → id 선택자 → class 선택자 → 요소 선택자

- 이 안에서 우선순위가 같다면 style 작성 시 나중에 작성된 것이 우선순위가 높음

- 'important'는 cascade의 구조를 무시하고 모든 우선순위를 무효화하기에 사용하지 않는 것을 권장

### 상속
- 기본적으로 부모 요소의 속성을 자식에게 상속함

- 상속되는 속성 : font, color, text_align 등 Text 관련 요소, opacity, visibility

### 참고사항
MDN 문서 참고