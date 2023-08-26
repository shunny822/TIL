# script async & defer

## 필요성

브라우저는 html 파일을 받으면 한 줄 한 줄 해석하여 DOM 요소로 변환(parsing)하며 script tag를 만나면 해당 js file을 불러오게 된다.

script tag를 head에 포함하는 경우 js file을 모두 불러온 후 화면이 보여지게 되는데 file의 용량이 크거나 인터넷이 느리면 사용자가 화면을 보게 되기까지 시간이 오래 걸릴 수 있다.

script tag를 body 하단에 포함하는 경우 DOM이 모두 생성된 후 js file을 불러오지만 해당 화면이 js file에 의존적인 경우 의미있는 화면을 보기까지 시간이 걸릴 수 있다.

시간을 단축하기 위해 HTML parsing과 js file fetching을 병렬적으로 수행하는 async와 defer를 사용할 수 있다.

<br>

## async

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <script async src="main.js"></script>
</head>
<body>
  <div></div>
</body>
</html>
```
head에 script tag를 포함시키고 async 속성을 쓸 수 있다.

js file을 병렬적으로 불러오는데 HTML파일을 parsing하다 js file을 모두 불러오면 parsing을 멈추고 js file을 실행시킨다. 실행이 완료되면 멈췄던 HTML parsing을 다시 한다.

### 단점

- 멈추는 시간이 존재하기 때문에 여전히 사용자가 화면을 보는데까지 시간이 걸릴 수 있다.

- HTML을 parsing하는 것 보다 먼저 js file이 실행되기 때문에 DOM 조작을 하거나 queryselector를 사용하는 것들이 작동하지 않을 수 있다.

<br>

## defer

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <script defer src="main.js"></script>
</head>
<body>
  <div></div>
</body>
</html>
```
head에 script tag를 포함시키고 defer 속성을 쓸 수 있다.

HTML 파일을 parsing하면서 js file을 병렬적으로 불러오며, parsing이 완료되었을 때 js file이 실행된다. 멈추는 시간이 없기 때문에 다른 것들보다 빠르게 화면을 보여줄 수 있다.

또한 여러 파일을 불러올 때, async는 먼저 다운로드된 순서대로 js file이 실행되나 defer는 작성한 순서대로 실행되어 오류를 방지할 수 있다는 장점이 있다!


<br>

## 참고 자료

https://www.youtube.com/watch?v=tJieVCgGzhs