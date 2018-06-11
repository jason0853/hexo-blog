---
title: Javascript high performance and optimization
tags:
  - Javascript
  - Performance
  - Optimization
thumbnail:
  - ../../../../images/javascript/javascript-logo.png
categories:
  - Front-end
  - Javascript
---


![](../../../../images/javascript/javascript-logo.png)

웹 어플리케이션의 성능을 끌어올리고 싶다면 브라우저 성능 이슈나 최적화에 대한 고민을 누구나 한번쯤은 해봤을겁니다. 오늘은 이런 고민을 하고 있는 자바스크립트 개발자들을 위해 포스팅을 작성해보겠습니다.

### # Loading and Execuation

<code>&#60;script&#62;&#60;/script&#62;</code>위치는 어디에 삽입하는게 좋을까요?

``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Script Example</title>
  <link rel="stylesheet" type="text/css" href="style.css" />
</head>
<body>
  <!-- 권장 위치 -->
  <script type="text/javascript" src="main.js"></script>
</body>
</html>
```

* 스크립트 태그의 적절한 위치는 <code>&#60;/body&#62;</code> 끝나기 시점 전에 삽입하기를 권장합니다.
* HTTP 요청이 있을 때마다 성능 부담이 증가하기 때문에 외부 자바스크립트의 파일 수를 줄이는 게 좋습니다.

웹 어플리케이션이 커질수록 자바스크립트 코드 소스는 늘어납니다. 그렇다고 한 파일로 묶어서 HTTP 요청이 한 번만 일어나게 한다헤도 파일이 크기 때문에 브라우저는 멈춘 현상이 생깁니다. 이러한 이슈를 처리하는 비차단 자바스크립트에 대해 알아보겠습니다.

``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Script Example</title>
</head>
<body>
  <!-- defer 지연 스크립트  -->
  <script defer>
    alert('defer')
  </script>
  <script>
    alert('script');
  </script>
  <script>
    window.onload = function() {
      alert('load');
    };
  </script>
</body>
</html>
```

* IE브라우저와 파이어폭스 3.5 이상에서만 defer 속성이 지원됩니다. 이 경우 <code>alert</code> 실행 순서는 <code>script, defer, load</code> 순서로 호출됩니다.
* <code>defer</code> 속성이 있는 <code>script</code> 태그는 DOM이 로드되긴 전에는 실행되지 않습니다.
* 이외에도 **스크립트 태그를 동적으로 생성해서 코드를 내려받아 실행**하거나 **XHR 객체로 자바스크립트 코드를 내려받아 삽입**합니다.

### # Data Access

* 자바스크립트에서 데이터를 저장할 수 있는 장소는 **리터럴 값, 변수, 배열 항목, 객체 멤버**입니다. 각 데이터 저장 위치마다 데이터를 읽고 쓰는 시간이 다릅니다. 실행 속도가 중요한 부분에서는 **리터럴 값**이나 **지역 변수**를 사용하는 것이 속도 향상에 도움이 됩니다.
* 전역 변수는 항상 스코프 체인 마지막에 있으므로 접근하는데 가장 많은 시간이 걸립니다.
* 속성이나 메서드가 프로토타입 체인 깊이 있을수록 접근이 느려집니다.
* 자주 사용하는 객체 멤버나 배열 항목, 스코프 밖의 변수를 지역 변수에 저장하면 대부분의 경우 성능을 올릴 수 있습니다.



### Reference

[자바스크립트 성능 최적화](http://www.hanbit.co.kr/store/books/look.php?p_code=B8190352411)