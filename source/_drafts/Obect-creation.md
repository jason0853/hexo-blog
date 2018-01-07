---
title: Obect creation
tags:
  - Javascript
thumbnail:
  - ../../../../images/javascript/javascript-logo.png
categories:
  - Front-end
  - Javascript
---


![](../../../../images/javascript/javascript-logo.png)

최근 **Javascript**를 기초부터 다시 공부하고 있는 와중에 블로그에도 문서화 시키면 좋을 것 같아서 공부한 내용을 정리해보기로 하였습니다. 이 포스팅을 보시면서 오타가 있거나 잘못된 내용이 있다면 댓글 부탁드립니다.

### # bind and this

``` javascript
let player = {
  sport: 'basketball',
  play: function() {
    console.log(this.sport);
  }
};

player.play(); // basketball

let playFunction = player.play;
playFunction(); // undefined
```

* *undefined*가 로그에 찍히는 이유는 <code>this</code> 키워드를 통해 **context**에 접근할 수 있는데 <code>playFunction</code>은 단순히 함수입니다. 10번째 줄 코드는 아래코드와 동일합니다.

``` javascript
let playFunction = function() {
  console.log(this.sport);
};
```

이제 <code>bind</code>함수를 이용하여 <code>this</code>가 가리키고 있는 **context**를 변경해보겠습니다.

``` javascript
let player = {
  sport: 'basketball',
  play: function() {
    console.log(this.sport);
  }
};

player.play(); // basketball

let playFunction = player.play;
let playFunctionBound = playFunction.bind(player);
playFunctionBound(); // basketball
```

* <code>bind()</code>의 파라미터에 참조해야할 객체인 <code>player</code>를 인자로 전달해주었습니다. 이제 <code>this</code>는 <code>sport</code> 속성에 접근하여 이전과 다르게 *basketball*이 출력되었습니다.

``` javascript
function play() {
  console.log(this.sport);
}

let player = {
  sport: 'basketball'
};

let playBound = play.bind(player);
playBound(); // basketball
```

* 이번에는 함수를 <code>player</code> 객체로부터 분리해놓고 설명해보겠습니다. <code>bind</code> 함수를 사용하기 이전에 코드를 살펴보면은 <code>play</code> 함수 안에 있는 <code>this</code>는 **window** 객체(웹 브라우저) 혹은 **global** 전역 객체(node.js)를 가리킵니다. 그렇기 때문에 <code>this</code>가 <code>player</code> 객체를 바라보도록 컨텍스트를 변경해야합니다.

``` javascript
let play = function() {
  console.log(this.sport);
}

let player = {
  do: play,
  sport: 'basketball'
};

let anotherPlayer = {
  swim: player.do,
  sport: 'swimming'
};

player.do(); // basketball
anotherPlayer.swim(); // swimming
```

* <code>anotherPlayer</code> 객체 하나를 더 생성해 보았습니다. 각 객체의 <code>do</code>, <code>swim</code> 속성에 <code>play</code> 함수가 복사되면서 <code>this</code>가 가리키는 컨텍스트는 각 객체 자신입니다.

{% jsfiddle frdy6twg js,html,result %}

* 만약 <code>bind()</code> 함수를 사용하지 않는다면 <code>this</code>가 가리키는 객체는 button 태그를 가리킵니다.

### # Prototype basic

보통 상속을 할때 클래스를 사용하는데 익숙합니다. 하지만 자바스크립트는 **프로토타입(prototype)**을 사용하여 상속을 구현합니다. <code>Object.setPrototypeOf(obj, prototype);</code> 를 이용하여 프로토타입을 설정해보겠습니다.

``` javascript
function say() {
  console.log(this.greet);
}

let person = {
  say // === say: say 와 동일합니다. es6 syntax
};

let student = {
  greet: 'Hello'
};

let soldier = {
  greet: 'Hello, sir'
};

let strongSoldier = {
  shout: function() {
    console.log(this.greet.toUpperCase());
  }
};

Object.setPrototypeOf(student, person);
Object.setPrototypeOf(soldier, person);
Object.setPrototypeOf(strongSoldier, soldier);

student.say(); // Hello
soldier.say(); // Hello, sir
strongSoldier.shout(); // HELLO, SIR
```

* <code>setPrototypeOf</code>을 이용하여 두번째 파라미터에 <code>person</code> 객체를 전달해주었습니다. 각 객체마다 공유되는 공통 메서드이기 때문입니다.
* <code>strongSoldier</code> 객체에 프로토타입을 설정해주지 않으면 <code>shout</code> 함수안에 있는 <code>this</code>는 <code>greet</code>이라는 속성을 가지고 있지 않기 때문에 에러가 발생하게 됩니다.


