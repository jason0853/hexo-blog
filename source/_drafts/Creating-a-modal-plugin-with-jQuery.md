---
title: Creating a modal plugin with jQuery
tags:
  - Javascript
  - Design Pattern
  - jQuery
thumbnail:
  - ../../../../images/javascript/javascript-logo.png
categories:
  - Front-end
  - Javascript
---

![](../../../../images/javascript/javascript-logo.png)

UI 개발을 하다 보면 재사용하는 기능들을 **플러그인**으로 만들어서 사용하면 굉장히 편리합니다. 지난 포스팅해서 다뤘던 **디자인 패턴(Design Pattern)**들 응용해서 간단한 **Modal Plugin**을 제작해보겠습니다.

### # Getting ready to build plugin

* jQuery 라이브러리 - 필수는 아님. 브라우저 호환성 및 코드의 간결성을 위해
* 옵션

``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style type="text/css">
    html, body, div { height: 100%; position: relative; margin: 0; padding: 0; box-sizing: border-box; }
  </style>
</head>
<body>
  <div id="modal"></div>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/1.12.4/jquery.js"></script>
  <script src="modal.js"></script>
</body>
</html>
```
