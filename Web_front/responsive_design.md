# Responsive Web Design

## 개요

> 디바이스 종류나 화면 크기에 상관없이 일관된 레이아웃 및 사용자 경험을 제공하는 디자인 기술
> 
> Bootstrap grid system의 12개 column과 `6개 breakpoints`를 사용하여 반응형 웹 디자인 구현

<br>

## Grid system breakpoints

- 웹 페이지를 다양한 화면 크기에서 적절하게 배치하기 위한 분기점

- 화면 너비에 따라 6개의 분기점 제공 : xs, sm, md, lg, xl, xxl

| | xs(<576px) | sm(>=576px) | md(>=768px) | lg(>=992px) | xl(>=1200px) | xxl(>=1400px) |
|---|---|---|--|--|--|--|
|container `max-width`|None(auto)|540px|720px|960px|1140px|1320px|
|class prefix|`.col-`|`.col-sm-`|`.col-md-`|`.col-lg-`|`.col-xl-`|`.col-xxl-`|

- 각각 breakpoints마다 설정된 최대 너비 값 `이상으로`화면이 커지면 grid system 동작이 변경됨

- 예제
  ```html
  <div class="container">
      <h2>breakpoint</h2>
      <div class="row g-0">
          <div class="col-12 col-sm-6 col-md-2 col-lg-3 col-xl-4">
              <div class="box">col</div>
          </div>
          <div class="col-12 col-sm-6 col-md-8 col-lg-3 col-xl-4">
              <div class="box">col</div>
          </div>
          <div class="col-12 col-sm-6 col-md-2 col-lg-3 col-xl-4">
              <div class="box">col</div>
          </div>
          <div class="col-12 col-sm-6 col-md-12 col-lg-3 col-xl-12">
              <div class="box">col</div>
          </div>
      </div>
  </div>
  ```

- +offset 예제
  ```html
  <div class="container">
      <div class="row g-0">
          <div class="col-12 col-sm-4 col-md-6">
              <div class="box">col</div>
          </div>
          <div class="col-12 col-sm-4 col-md-6">
              <div class="box">col</div>
          </div>
          <div class="col-12 col-sm-4 col-md-6">
              <div class="box">col</div>
          </div>
          <!-- offset 사용 시 크기가 넘어갈 때 다시 지정해줘야 함 -->
          <div class="col-12 col-sm-4 offset-sm-4 col-md-6 offset-md-0">
              <div class="box">col</div>
          </div>
      </div>
  </div>
  ```