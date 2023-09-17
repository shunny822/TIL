# Semantic Web

> 웹 데이터를 의미론적으로 표현하고 연결하는 개념
> 
> for 컴퓨터가 데이터의 내용과 문맥을 더 효율적으로 이해하고 더 지능적으로 활용
> 
> 이 요소가 가진 목적과 역할은 무엇인가?

## Semantic in HTML

### 각자의 책임과 역할

- HTML : 채워질 데이터를 나타내기 위함

- CSS : 어떻게 보여야 하는지


### 페이지 구조화를 위한 대표적 semantic elemant
```html
<body>
  <header>
    <h1>Heading</h1>
  </header>
  
  <nav>
    <ul>
      <li><a href="#">nav</a></li>
      <li><a href="#">nav</a></li>
      <li><a href="#">nav</a></li>
    </ul>
  </nav>

  <main>
    <article>
      <div></div>
      <section></section>
    </article>
    <article></article>
    <aside></aside>
  </main>

  <footer></footer>
</body>
```
- header

- nav

- main

- article

- section

- aside

- footer

<br>

## Semantic in CSS

### OOCSS

: Obfect-Oriented CSS, 객체 지향적 접근법을 적용하여 CSS를 구성하는 방법론

- 각각의 속성을 잘게 쪼개어 class로 정의하고 적용 시 class들을 조합하여 적용

- 중복되 style 코드를 여러 번 작성하지 않아도 됨

- 코드 재사용성 증가


### BEM

: Block Element Modifier, 블록, 요소, 수정자를 사용해 클래스 이름을 구조화하는 방법론

```css
/* 이름의 형 */
.block {}
.block__element {}
.block__modifier {}
```

- Block
  - 문단 전체에 적용된 요소 또는 요소를 담고 있는 컨테이너

  - 재사용 가능한 독립적 블록, 가장 바깥쪽 상위요소

  - 재사용을 위해 margin 또는 padding을 적용하지 않음(따로 정의해야 함)

- Element
  - block이 포함하고 있는 한 조각

  - 블록을 구성하는 종속적인 하위요소

- Modifier
  - block또는 element의 속성

- 예제
  ```html
  <head>
    ...
    <style>
      /* Block */
      .card {
        display: flex;
        flex-direction: column;
      }

      /* Element */
      .card__title {
        font-size: 2rem;
      }

      .card__list {
        margin: 0;
      }

      .card__button {
        font-size: 1rem;
        padding: 1rem;
        cursor: pointer;
      }

      /* Modifier */
      .card__list-item--none {
        list-style: none;
      }

      .card__button--red {
        background-color: crimson;
      }
    </style>
  </head>

  <body>
    <div class="card">
      <h2 class="card__title">제목</h2>
      <ul class="card__list">
        <li class="card__list-item--none">항목 1</li>
        <li class="card__list-item--none">항목 2</li>
      </ul>
      <button class="card__button card__button--red">버튼</button>
    </div>
  </body>
  ```

<br>

## 참고

### 의미론적 마크업의 이점
- 검색 엔진이 해당 웹 사이트를 분석하기 쉽게 만들어 검색 순위에 영향을 줌

- 시각 장애 사용자가 스크린 리더기로 웹 페이지를 사용할 때 추가적으로 도움


### OOCSS & BEM의 목적
- 재사용 가능한 모듈로 분리함으로써 유지보수성과 확장성을 향상

- 개발자 간의 협력이 향상되어 공통언어와 코드 이해를 확립

### emmet : 자동완성 기능

emmet cheat sheet 참고

<예시>

`div.wrapper>ul.list>li.item.item-$*5>a{link $}`
```html
<div class="wrapper">
  <ul class="list">
    <li class="item item-1"><a href="">link 1</a></li>
    <li class="item item-2"><a href="">link 2</a></li>
    <li class="item item-3"><a href="">link 3</a></li>
    <li class="item item-4"><a href="">link 4</a></li>
    <li class="item item-5"><a href="">link 5</a></li>
  </ul>
</div>
```