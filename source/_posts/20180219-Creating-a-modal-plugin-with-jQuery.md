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
date: 2018-02-19 00:21:37
---


![](../../../../images/javascript/javascript-logo.png)

UI 개발을 하다 보면 재사용하는 기능들을 **플러그인**으로 만들어서 사용하면 굉장히 편리합니다. 지난 포스팅해서 다뤘던 **디자인 패턴(Design Pattern)**들 응용해서 간단한 **Modal Plugin**을 제작해보겠습니다.

### # Getting ready to build plugin

보통 UI 플러그인을 제작할 때 고려해야 하는 것들은 다음과 같습니다.

* CSS 파일을 제공할 것인가?
* 플러그인을 적용할 대상 태그는 무엇인가?
* 서드파티(third party) 라이브러리가 필요한가?
* 어떠한 메서드를 제공할 것인가?
* 어떠한 옵션을 제공할 것인가?

``` html index.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <!-- CSS 내용 -->
  <style type="text/css">
    html, body, div { height: 100%; position: relative; margin: 0; padding: 0; box-sizing: border-box; }
  </style>
</head>
<body>
  <!-- 적용 대상 태그 -->
  <div id="modal"></div>

  <button id="btn-show">show</button>
  <!-- third party library 제공 -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/1.12.4/jquery.js"></script>
  <script src="modal.js"></script>
</body>
</html>
```

위 코드에서 보듯이 고려해야할 세가지 사항을 반영하였습니다. 이제 본격적으로 modal.js 를 만들어 보겠습니다.

### # Let's build

``` js modal.js
;(function($) {
  'use strict';
  
}(jQuery));
```

* jQuery의 <code>$</code> 별칭을 사용할 때 다른 라이브러리와의 충돌을 방지하기 위해 **IIFE** 즉시 호출 함수 표현식을 사용하여 문제를 해결하는 것을 권장합니다.
* <code>'use strcit'</code>를 함수 안에 적용함으로써 자바스크립트 코드를 좀 더 엄격하게 검사하고 적은 에러를 발생시킬 수 있습니다.

``` js modal.js
;(function($) {
  'use strict';

  var modal = null,
      params = null,
      instance;

  // Constructor
  var Modal = function(opt) {

  };

  // Public
  Modal.prototype = {
    version: 'v1.0.0'
  }

  // private
  modal = {
    init: function() {
      this.cacheDom();
      this.bindEvents();
      this.render();
    },
    cacheDom: function() {
      this.$window = $(window);
      this.$modal = $('#modal');
    },
    bindEvents: function() {
      
    },
    render: function() {
      
    },
  };

  window.Modal = Modal;
}(jQuery));
```

* 변수 <code>modal</code>은 **Object Literal Pattern**을 사용하여 각 기능을 작성할 예정입니다. 객체 리터럴이 생소하신 분들은 [지난 포스팅](https://jason0853.github.io/2018/01/23/Using-design-pattern-to-refactory-jQuery-spaghetti-code/)을 참고해주시기 바랍니다.
* <code>params</code>는 옵션에 해당하는 객체의 결과값을 할당 받을 변수입니다.
* <code>instance</code> 변수는 **Singleton Pattern**을 적용하기 위해 미리 선언해두었습니다. 싱글톤 패턴에 대해 생소하신 분들은 [지난 포스팅](https://jason0853.github.io/2018/02/05/Singleton-Pattern/)을 참고해주세요.
* <code>Modal.prototype</code>에 공개할 API 메서드를 return 할 예정입니다.

UI 구조상 같은 유형의 모달을 동시에 여러개 띄우는 경우는 없으므로 **싱글톤 패턴**으로 만드는 것이 적합합니다. 그럼 constructor 부분애 코드를 추가해보겠습니다.

``` js modal.js
;(function($) {
  'use strict';

  var modal = null,
      params = null,
      instance;

  // Constructor
  var Modal = function(opt) {

    var _self = this;

    function initialize() {
      opt = opt || {};
      
      var config = {
        width: opt.width || 500,
        height: opt.height || 'auto',
        bgColor: opt.bgColor || '#eee',
        title: opt.title || 'Title',
        content: opt.content || 'Content'
      };

      params = $.extend(config, opt);
  
      modal.init();

      return _self;
    }

    return function() {
      if (!instance) {
        instance = initialize();
      }
      
      return instance;
    }();
  };

  // (중략)
  
  window.Modal = Modal;
}(jQuery));
```

* <code>$.extend()</code> 메서드를 이용하여 <code>config</code> 객체(default)와 <code>opt</code> 객체(사용자가 변경할 옵션)를 merge 시킵니다.
* <code>instance = initialize()</code> 값을 할당시키고 난 이후 새로운 객체를 만들려고 해도 <code>instance</code>가 더이상 undefined가 아니기 때문에 새로운 객체를 만들 수가 없어 싱글톤 패턴을 구현하게 됩니다.

``` js modal.js
;(function($) {
  'use strict';

  // (중략)

  // Public method
  Modal.prototype = {
    version: 'v1.0.0',
    show: function() {
      modal.show();
    },
    hide: function() {
      modal.hide();
    },
    change: function(title, content) {
      modal.change(title, content);
    }
  };

  // private
  modal = {
    init: function() {
      this.cacheDom();
      this.bindEvents();
      this.render();
    },
    cacheDom: function() {
      this.$window = $(window);
      this.$modal = $('#modal');
    },
    bindEvents: function() {
      this.$window.on('resize', this.center.bind(this));
      this.$modal.on('click', '#modal-dim', this.hide.bind(this));
    },
    render: function() {
      this.addDim();
      this.create();
      this.addTitle();
      this.addContent();
      this.center();
      this.$modal.hide();
    },
    create: function() {
      var $modalBox = $('<div class="modal-box"></div>'),
          boxStyles = { zIndex: 100, width: params.width, height: params.height, background: params.bgColor };
      $modalBox.css(boxStyles).appendTo(this.$modal);
    },
    addDim: function() {
      var $target = $('<div id="modal-dim"></div>'),
          styles = { position: 'absolute', left: 0, top: 0, right: 0, bottom: 0, width: '100%', height: '100%', background: '#000', opacity: .7 };
      $target.css(styles).appendTo(this.$modal);
    },
    addTitle: function() {
      var $target = $('<div id="modal-title"></div>'),
          $parentElem = this.$modal.find('.modal-box'),
          styles = { width: '100%', height: '50px', lineHeight: '50px', fontSize: '1.5rem', fontWeight: 'bold', textAlign: 'center', borderBottom: '1px solid #ccc' };
      $target.css(styles).appendTo($parentElem).text(params.title);
    },
    addContent: function() {
      var $target = $('<div id="modal-content"></div>'),
          $parentElem = this.$modal.find('.modal-box'),
          styles = { width: '100%', height: 'auto', padding: '1.5rem', wordWrap: 'break-word' };
      $target.css(styles).appendTo($parentElem).text(params.content);
    },
    center: function() {
      var $target = this.$modal.find('.modal-box'),
          wid = this.$window.width(),
          hei = this.$window.height(),
          targetWid = $target.width(),
          targetHei = $target.height(),
          styles = { 'position': 'absolute', 'left': (wid - targetWid) / 2, 'top': (hei - targetHei) / 2};
      $target.css(styles);
    },
    show: function() {
      this.$modal.show();
    },
    hide: function() {
      this.$modal.hide();
    },
    change: function(title, content) {
      var $title = this.$modal.find('#modal-title'),
          $content = this.$modal.find('#modal-content');
      
      params.title = title;
      params.content = content;

      $title.text(title);
      $content.text(content);
    }
  }
  
  window.Modal = Modal;
}(jQuery));
```

* <code>modal</code> 객체에 method로 있는 <code>show</code>, <code>hide</code>, <code>change</code>를 <code>Modal.prototype</code>에 공개 API로 추가해줌으로써 새로운 인스턴스 객체가 생성되었을 때 <code>Modal</code> 생성자 속성 및 메서드에 접근할 수 있게 됩니다.

아래코드는 modal.js 의 전체 코드입니다.

{% gist c08a84293c05272dff8840be8869fda2 %}

### # How to use

``` js
var myModal = new Modal({
  width: 300,
  title: '타이틀',
  content: '내용'
});

$('#btn-show').on('click', myModal.show);
```

아래와 같이 show 버튼을 클릭하면 modal이 노출되고 dim부분을 클릭하면 modal이 사라집니다.

{% jsfiddle cb3o6gta result %}

이번엔 <code>change()</code> 메서드를 이용하여 title과 content 파라미터를 변경해보겠습니다.

``` js
var myModal = new Modal();

$('#btn-show').on('click', function() {
  myModal.change('타이틀', '내용');
  myModal.show();
});
$('#btn-error').on('click', function() {
  myModal.change('에러', '에러입니다.');
  myModal.show();
});
```

{% jsfiddle p300ujj7 result %}

### Wrap-up

간단한 **플러그인**이지만 실제로 이렇게 만들어 보는 것이 중요한 것 같습니다. 자바스크립트 이론을 공부하면서 '실제로 개발할 때 어디에 적용해야되는걸까?'라는 의문을 던지게 됩니다. 예를 들어 싱글톤 패턴을 포스팅하면서도 개념만 익혔지 어디에 활용해야될지 사실 잘 몰랐습니다.ㅠㅠ 반성하게 됩니다.ㅎㅎ 포스팅이 생각보다 길어져서 중간중간 실수한 흔적이 보일 수도 있습니다. 혹시 수정해야할 부분이 있다면 댓글 부탁드립니다.

### Reference

[자바스크립트에서 strict mode를 사용해야 하는 이유](http://blog.aliencube.org/ko/2014/01/02/reasons-behind-using-strict-mode-while-coding-javascript/)
모던 웹을 위한 JavaScript jQuery 입문 - 한빛미디어
