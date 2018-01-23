---
title: Using design pattern to refactory jQuery spaghetti code
tags:
  - Javascript
  - jQuery
thumbnail:
  - ../../../../images/javascript/javascript-logo.png
categories:
  - Front-end
  - Javascript
date: 2018-01-23 14:56:06
---


![](../../../../images/javascript/javascript-logo.png)

**jQuery** 라이브러리를 이용하여 화면 개발을 많이 했었지만 **Javascript** 공부를 제대로 하지 않고 코드를 짜면 엉망진창이 되기 쉽상이었습니다. 특히 프로젝트가 끝나고 유지보수 단계에 들어가면 한군데 수정하면 다른데도 수정해야되고 정말 귀찮았습니다. 그래서 Javascript 기본 개념을 다시 깨우치고 공부하면서 터득한 걸 공유해보겠습니다.

일단 앞으로 만들 결과물을 한 번 공개해보겠습니다.

{% jsfiddle f6zjqge8 result,html,css,js %}

간단히 결과물을 설명해드리면, 좋아하는 선수 이름을 리스트에 추가, 삭제하는 기능과 라디오버튼이 on이였을 경우 count를 하고 off였을 경우는 count를 하지 않는 기능을 넣는 simple한 apllication입니다. CDN을 통해 [jquery 1.12.4](https://cdnjs.com/libraries/jquery/1.12.4)를 추가하였습니다. 이제 js 소스를 한번 살펴보겠습니다.

``` js script.js
var players = [];

checkCount();

$('#player').find('button').on('click', function() {
  var value = $('#player').find('input').val(),
      item = `
        <li>
          <span>${value}</span>
          <button type="button">Delete</button>
        </li>
      `;

  if (!value) {
    alert('Player 이름을 입력해주세요.');
    return;
  }
  
  $('#player').find('ul').append(item);
  players.push(value);
  $('#player').find('input').val('');

  checkCount();
});

$('#player').find('ul').on('click', 'button', function(e) {
  var $remove = $(e.target).closest('li'),
      idx = $('#player').find('ul').find('li').index($remove);

  $remove.remove();
  players.splice(idx, 1);
  checkCount();
});

$('#count').find('.radios').find('input').on('change', function(e) {
  checkCount();
});

function checkCount() {
  var val = $('#count').find('input[type=radio]:checked').val(),
      $result = $('#count').find('.result').find('span');
      
  if (val == 'on') $result.text(players.length);
  else $result.text('?');
}
```

솔직히 코드 양이 많지 않아 spaghetti code처럼 보이지 않지만 한 페이지에 이런 기능들이 많다고 가정해봅시다.

### # The disadvantage of unstructured spaghetti code  

* 코드 양이 한 파일에 너무 많아 가독성이 좋지 않음.
* 유지보수 하기 어려움. - 기능 구분이 명확히 나누어 있지 않기 때문.

우선 간단히 **Object Literal Pattern**을 사용하여 코드를 좀 더 효율적으로 개선해보겠습니다.

### # Object Literal Pattern

#### Rule

* 전역 변수 없애기.
* 관심사 분리하기.
* DRY Code(Don't Repeat Yourself) - 반복되는 코드를 사용하지 않기.
* DOM Cache 하기.
* 메모리 누수 없애기 - 모든 이벤트는 unbind 될 수 있어야 함.

``` js playerCount.js
(function() {
  var peoepleCount = {};
})();

console.log(peopleCount); //  Uncaught ReferenceError: playerCount is not defined
```

* **IIFE(Immediately Invoked Function Expressions)**를 사용하면 전역변수 오염을 방지할 수 있습니다.

``` js playerCount.js
(function() {
  var playerCount = {
    players: [],
    init: function() {
      this.cacheDom();
      this.bindEvents();
      this.render();
    },
    cacheDom: function() {
      this.$player = $('#player');
      this.$count = $('#count');
      this.$form = this.$player.find('.form');
      this.$input = this.$form.find('input');
      this.$button = this.$form.find('button');
      this.$ul = this.$player.find('ul');
      this.$result = this.$count.find('.result');
      this.$radios = this.$count.find('.radios');
      this.$span = this.$result.find('span');
      this.$radioButton = this.$radios.find('input');
    },
    bindEvents: function() {
      this.$button.on('click', this.addPlayer.bind(this));
      this.$ul.on('click', 'button', this.deletePlayer.bind(this));
      this.$radioButton.on('change', this.checkCount.bind(this));
    },
    render: function() {
      this.checkCount();
    }
  };

  playerCount.init();
})();
```

* <code>init</code> - <code>peopleCount</code> 객체를 초기화 시킵니다.
* <code>cacheDom</code> - Dom을 캐싱하기 때문에 성능면에서 효율적입니다.
* <code>bindEvents</code> - 모든 이벤트는 이 함수안에서 바인드됩니다.
* <code>render</code> - count 숫자를 렌더링 해줍니다.

***주의) Event handler를 등록할 때 bind(this) 사용해야합니다.***

아래 예제 코드를 보시겠습니다.

``` js playerCount.js
(function() {
  var playerCount = {
    players: [],
    init: function() {
      this.cacheDom();
      this.bindEvents();
    },
    cacheDom: function() {
      this.$player = $('#player');
      this.$form = this.$player.find('.form');
      this.$button = this.$form.find('button');
    },
    bindEvents: function() {
      this.$button.on('click', this.addPlayer);
    },
    render: function() {
      console.log('redner')
    },
    addPlayer: function() {
      console.log(this); //  <button type="button">Add</button>
      this.render(); //  Uncaught TypeError: this.render is not a function
    }
  };

  playerCount.init();
})();
```

* Event handler 안에 **this**는 event target인 button을 가리키고 있기 때문에 <code>addPlayer</code>안에 있는 **this**는 <code>render</code> method를 찾을 수 없습니다.
* **this**의 context는 <code>playerCount</code>를 가리켜야합니다.

``` js playerCount.js
(function() {
  var playerCount = {
    players: [],
    init: function() {
      this.cacheDom();
      this.bindEvents();
      this.render();
    },
    cacheDom: function() {
      this.$player = $('#player');
      this.$count = $('#count');
      this.$form = this.$player.find('.form');
      this.$input = this.$form.find('input');
      this.$button = this.$form.find('button');
      this.$ul = this.$player.find('ul');
      this.$result = this.$count.find('.result');
      this.$radios = this.$count.find('.radios');
      this.$span = this.$result.find('span');
      this.$radioButton = this.$radios.find('input');
    },
    bindEvents: function() {
      this.$button.on('click', this.addPlayer.bind(this));
      this.$ul.on('click', 'button', this.deletePlayer.bind(this));
      this.$radioButton.on('change', this.checkCount.bind(this));
    },
    render: function() {
      this.checkCount();
    },
    addPlayer: function() {
      var value = this.$input.val(),
          item = `
            <li>
              <span>${value}</span>
              <button type="button">Delete</button>
            </li>
          `;

      if (!value) {
        alert('Player 이름을 입력해주세요.');
        return;
      }
      
      this.players.push(value);
      this.$ul.append(item);
      this.$input.val('');
      this.render();
    },
    deletePlayer: function(e) {
      var $remove = $(e.target).closest('li'),
          idx = this.$ul.find('li').index($remove);

      this.players.splice(idx, 1);
      $remove.remove();
      this.render();
    },
    checkCount: function() {
      var val = this.$radios.find('input[type=radio]:checked').val();
      val == 'on' ? this.$span.text(this.players.length) : this.$span.text('?');
    }
  }

  playerCount.init();
})();
```

이제 모든 코드가 완성되었고 이전과 동일하게 작동합니다. 자바스크립트 코드만 변경되었습니다.

{% jsfiddle c2uk5mfj result,js %}

이전 코드보다 가독성이 좋아졌고 유지보수하기 편해졌습니다. 
하지만 여기서 한 가지 상황을 더 가정해보겠습니다. 기능들이 더 추가될 예정이며 <code>playerCount</code>에 더 많은 속성과 메서드가 추가될 것입니다. 이런 상황들이 반복적으로 이어지다 보면 파일 하나에 너무 많은 기능이 집약되어 있어 지금처럼 한눈에 파악하기가 쉽지 않습니다.
**Revealing Module Pattern**을 사용하여 <code>player</code>, <code>count</code> 두 객체를 따로 나눠보겠습니다.

### # Revealing Module Pattern

#### Rule

* **IIFE**라 불리는 즉시 호출 함수를 사용함.
* 공개해도 지장이 없는 API만 return 시킴.
* 나머지는 접근가능하지 않도록 비공개 처리해줌.

```js player.js
var player = (function() {
  var players = [];

  _render();

  function _render() {
    console.log('render');
  }

  function addPlayer() {
    
  }

  return {
    addPlayer: addPlayer
  }
})();
console.log(player);

/* Output
render
{addPlayer: ƒ}
*/
```

* **Revealing Module Pattern**의 기본 골격입니다.
* <code>players</code> 변수를 공개해버리면 외부에서 조작을 할 수 있기 때문에 비공개로 해놓아야합니다.

```js player.js
var player = (function() {
  var players = [];

  // cache Dom
  var $player = $('#player'),
      $form = $player.find('.form'),
      $input = $form.find('input'),
      $button = $form.find('button'),
      $ul = $player.find('ul');
      
      
  
  // bind events
  $button.on('click', addPlayer);
  $ul.on('click', 'button', deletePlayer);

  _render();

  function _render() {
    count.checkCount(players.length);
  }

  function addPlayer() {
    var value = $input.val(),
        item = `
          <li>
            <span>${value}</span>
            <button type="button">Delete</button>
          </li>
        `;

    if (!value) {
      alert('Player 이름을 입력해주세요.');
      return;
    }

    players.push(value);
    $ul.append(item);
    $input.val('');
    _render();
  }

  function deletePlayer(e) {
    var $remove = $(e.target).closest('li'),
        idx = $ul.find('li').index($remove);

    players.splice(idx, 1);
    $remove.remove();
    _render();
  }
  
  return {
    addPlayer: addPlayer,
    deletePlayer: deletePlayer
  }
})();
```

``` js count.js
var count = (function() {
  var number = 0;
  
  // cache Dom
  var $count = $('#count'),
      $result = $count.find('.result'),
      $radios = $count.find('.radios'),
      $span = $result.find('span'),
      $radioButton = $radios.find('input');

  // bind event
  $radioButton.on('change', function(e) {
    checkCount(number)
  });

  function checkCount(newPlayer) {
    number = newPlayer;

    var value = $radios.find('input[type=radio]:checked').val();
    value == 'on' ? $span.text(number) : $span.text('?');
  }

  return {
    checkCount: checkCount
  }
})();
```

***주의) index.html에 script를 link시킬때 count.js가 player.js보다 위에 있어야됩니다. 그렇지 않으면 Uncaught TypeError: count.checkCount is not a function이라는 에러가 발생합니다.***

코드를 분할해 모듈화를 시켜보았습니다. 결과는 이전과 동일합니다.

{% jsfiddle xx2hz9pv result,js %}

마지막으로 한가지 상황을 더 가정해보겠습니다. 아래 코드를 보고 설명해보겠습니다.

``` js player.js
var players = [];
// (중략)
_render();

function _render() {
    count.checkCount(players.length);
    header.showCount(players.length);
    footer.displayCount(players.length);
    sidebar.count({ count: player.length });
    // 이런식으로 반복 ...
  }
// (중략)
```
이런식으로 <code>header</code>, <code>footer</code>, <code>sidebar</code> 이런 세개의 화면을 작업하여 모듈화 작업을 했다고 가정해봅시다. 그리고 count를 이 세군데에 보여주어야합니다. 그래서 <code>player</code>코드 객체 안에 함수들을 호출하였습니다. <code>players</code> 배열을 통해 count를 셀 수 있는데 비공개이기 때문에 다른 객체에서 접근할 수가 없습니다. 그렇다고 계속 이런식으로 호출하면 <code>_render</code>함수 안에 너무 많은 함수 리스트가 쌓입니다. 더 좋은 방법이 없을까요? 
바로 **PubSub Design Pattern**을 사용하여 해결해보겠습니다.

### # PubSub Design Pattern

**PubSub Design Pattern**은 발행(publish)/구독(subscribbe) 모델로 알려져 있으며 어떤 대상의 상태값이 변경되었을 경우 이를 감지하고 있는 구독자들은 이것을 인지하고 자동으로 업데이트 해줍니다. 분산 이벤트 시스템을 구현할 때 널리 사용됩니다.

{% gist d60d8c0612c6f74d17462667c65072a7 %}

* <code>subjects</code> - 구독할 이벤트를 저장하는 공간이며 비공개로 처리하였습니다. 비공개 멤버인 이유는 외부에서 조작할 수 없도록 하기 위함입니다.
* <code>subscribe</code> - 구독할 이벤트를 <code>subjects</code> 객체에 저장합니다.
* <code>unsubscribe</code> - 구독할 이벤트를 <code>subjects</code> 객체에서 삭제합니다.
* <code>publish</code> - 저장된 구독이벤트들에게 알림을 보내고 함수를 호출합니다.

간단히 **PubSub Design Pattern**을 구현해보았습니다.
우선 header 하나가 추가된다는 전제하에 코드를 수정해보고 <code>pubsub</code>을 이용하여 구독, 취소, 발행 기능을 추가해보겠습니다.

``` html index.html
<!-- (중략) -->
<!-- 추가된 부분 -->
<div id="header">
  인원 <span></span>
</div>
<hr>
<!-- 추가된 부분 -->
<div id="player">
<!-- (중략) -->
```

``` js header.js
var header = (function() {

  // cache Dom
  var $header = $('#header'),
      $span = $header.find('span');

  pubsub.subscribe('changedCount', showCount);

  function showCount(num) {
    $span.text(num);
  }

  function shareData() {
    return $span.text();
  }

  return {
    shareData: shareData
  }
})();
```

* <code>shareData</code> - <code>count</code>에서 카운트 값을 공유할 API 입니다.

``` js count.js
var count = (function() {
  var number = 0;
  
  // cache Dom
  var $count = $('#count'),
      $result = $count.find('.result'),
      $radios = $count.find('.radios'),
      $span = $result.find('span'),
      $radioButton = $radios.find('input');

  // bind event
  pubsub.subscribe('changedCount', checkCount);
  
  $radioButton.on('change', function(e) {
    var value = $(e.target).val() == 'on' ? 1 : 0;
    
    if (value) {
      resume();
    } else {
      destroy();
    }
  });

  function checkCount(newPlayer) {
    number = newPlayer;

    var value = $radios.find('input[type=radio]:checked').val();
    value == 'on' ? $span.text(number) : $span.text('?');
  }

  /* pubsub event */
  function resume() {
    number = header.shareData(); 
    $span.text(number);
    pubsub.subscribe('changedCount', checkCount);
  }

  function destroy() {
    pubsub.unsubscribe('changedCount', checkCount);
    $span.text('?');
  }
})();
```

* <code>pubsub</code>을 이용했기 때문에 굳이 외부로 checkCount를 공개 API롤 설정하지 않아도 됩니다.
* 구독취소(<code>unsubscribe</code>)를 하면 다시 구독하게 되었을 때 기존의 있던 값을 노출시키기 때문에 <code>header.shareData()</code> 호출하여 카운트값을 공유합니다.

``` js player.js
// (중략)
function _render() {
  pubsub.publish('changedCount', players.length);
}
// (중략)
```

* 발행 기능을 추가함으로써 기존 모듈 하나하나를 추가할 때마다 <code>_render</code> 함수안에서 호출하지 않아도 됩니다. 코드가 훨씬 깔끔해졌습니다.

각 모듈들을 pubsub.js -> count.js -> header.js -> player.js 순서로 불러와야 오류없이 작동됩니다.
제대로 작동하는지 한번 확인해보겠습니다.

{% jsfiddle rbr1bgg7 result,js,html %}

### Wrap-up

자바스크립트 디자인 패턴을 적용하게 되면은 규모가 큰 어플리케이션에서 좀 더 생산적이고 효율적으로 작업을 할 수 있습니다. 포스팅을 보시고 수정 혹은 잘못된 부분은 댓글 부탁드립니다.

### Reference
<https://stackoverflow.com/questions/30793066/how-to-avoid-memory-leaks-from-jquery>
<https://hackernoon.com/observer-vs-pub-sub-pattern-50d3b27f838c>
