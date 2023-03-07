# Grid system

## Bootstrap Grid system

- 웹 페이지의 레이아웃을 조정하는데 사용되는 12개의 컬럼으로 구성된 시스템

- 반응형 디자인을 지원해 웹 페이지를 모바일, 태블릿, 데스크탑 등 다양한 기기에서 적절하게 표시할 수 있도록 도움

- 여기서 12 → 적당히 크고 많은 약수를 가진 수

<br>

## class와 기본 구조

### Grid system의 핵심 클래스
```html
<div class="container">
  <div class="row">
    <div class="col-4"></div>
    <div class="col-4"></div>
    <div class="col-4"></div>
  </div>
</div>
```
- 1개의 row안에 12칸의 column영역이 구성됨

- 각 요소는 12칸 중 몇 칸을 차지할 것인지 지정됨

### 중첩
```html
<h2>nesting</h2>
<div class="container">
    <div class="row">
        <div class="box col-4">col</div>
        <div class="box col-8">
            <div class="row">
                <div class="box col-6">col</div>
                <div class="box col-6">col</div>
                <div class="box col-6">col</div>
                <div class="box col-6">col</div>
            </div>
        </div>
    </div>
</div>
```

### offset
```html
<h2>offset</h2>
<div class="container">
    <div class="row">
        <div class="box col-4">col</div>
        <div class="box col-4 offset-4">col</div>
    </div>
</div>
```
- 설정된 offset 칸 만큼 띄우고 시작

### Gutter
```html
<h2>gutter</h2>
<div class="container">
    <div class="row g-5">
        <div class="col-6">
            <div class="box">col</div>
        </div>
        <div class="col-6">
            <div class="box">col</div>
        </div>
        <div class="col-6">
            <div class="box">col</div>
        </div>
        <div class="col-6">
            <div class="box">col</div>
        </div>
    </div>
</div>
```
- Grid system에서 column 사이에 padding 영역

- gx-, gy-, g-로 조절하며 g-0을 주면 요소끼리 붙을 수 있음