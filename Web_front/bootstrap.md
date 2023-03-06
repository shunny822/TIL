# Bootstrap

## 개요

> 프론트엔드 라이브러리 Toolkit
> 
> 반응형 웹 디자인 & CSS 및 JS 기반의 컴포넌트와 스타일 제공

<br>

## 자주 사용하는 클래스

### Spacing
```html
<p class="mt-5">Hello, world!</p>
<!-- "mt-5" = property + sides + '-' + size -->
```
- Property : m(margin) / p(padding)

- Sides : t(top) / b(bottom) / s(left) / e(right) / y(top, bottom) / x(left, right) / blank(4sides)

- Size : 0(0rem) / 1(0.25rem) / 3(0.5rem) / 4(1.5rem) / 5(3rem)


### Typography : 문서 상의 제목, 본문 텍스트, 목록 등
- Headings : HTML h1~h6 태그와 스타일을 일치시키고 싶으나 그에 맞는 HTML 태그를 더 사용할 수 없는 경우에 사용
  ```html
  <p class="h1">h1. Bootstrap heading</p>
  <p class="h2">h2. Bootstrap heading</p>
  <p class="h3">h3. Bootstrap heading</p>
  ```

- Display headings : 기존 Heading보다 더 눈에 띄는 제목이 필요할 경우
  ```html
  <h1 class="display-1">Display 1</h1>
  <h2 class="display-2">Display 2</h2>
  <h3 class="display-3">Display 3</h3>
  ```

- List
  ```html
  <ul class="list-unstyled">
    <li>This is a list.</li>
    <li>It appears completely unstyled.</li>
  </ul>
  ```


### color : Text, Border, Background 및 다양한 요소에 사용하는 Bootstrap의 색상 키워드
<예제>
```html
<div class="box bg-info border border-2 border-dark"></div>
```

<br>

## Component
- Bootstrap에서 제공하는 UI관련 요소(버튼, 폼, 카드, 드롭다운, 네비게이션 바 등)

- 일관된 디자인, 쉬운 프로토타입 제작 및 사용자 경험 제공

<br>

## CDN (Content Delivery Network)

> 지리적 제약 없이 빠르고 안전하게 컨텐츠를 전송할 수 있는 전송 기술

- 지리적으로 사용자와 가까운 CDN 서버에 컨텐츠를 저장해서 사용자에게 전달

- 서버와 사용자 사이의 물리적인 거리를 줄여 컨텐츠 로딩에 소요되는 시간을 최소화(웹페이지 로드 속도 높임)

- Bootstrap을 사용할 때도 CDN을 이용하여 bootstrap css와 js 파일 가져옴


<br>
<br>

### 참고

- rem : 브라우저의 font-size을 기준으로하는 상대단위

- 브라우저 font-size가 16px이면 1rem은 16px