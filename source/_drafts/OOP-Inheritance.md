---
title: OOP & Inheritance
tags:
  - Javascript
thumbnail:
  - ../../../../images/javascript/javascript-logo.png
categories:
  - Front-end
  - Javascript
---

![](../../../../images/javascript/javascript-logo.png)

**OOP**란 *Object-oriented programming*(객체지향 프로그래밍)이라고 불리며 대규모 어플리케이션을 제작할 때 보다 유연하고 유지보수하기 높은 프로그래밍 패턴입니다. 각각의 기능을 모듈화를 함으로써 코드 재사용할 수 있는 특징과 객체의 독립성을 유지시켜줍니다. 모듈을 재활용함으로써 하드웨어 처리량을 획기적으로 줄일 수 있습니다. 

자바스크립트에서 **OOP**를 구현하기 위해 두 가지 **Inheritance**(상속) 방법에 대해서 알아보겠습니다.

### # Classical Inheritance

``` javascript
let Human = function(firstname, lastname) {
  this._firstname = firstname;
  this._lastname = lastname;
};

Human.prototype.greet = function() {
  console.log(`Hello, I'm ${this._firstname} ${this._lastname}.`);
}

let Player = function(firstname, lastname, sport) {
  Human.call(this, firstname, lastname);
  this._sport = sport;
};

Player.prototype = Object.create(Human.prototype);
Player.prototype.constructor = Player;

Player.prototype.greet = function() {
  console.log(`Nice to meet you, I'm ${this._firstname} ${this._lastname}.`);
};

Player.prototype.play = function() {
  console.log(`I play ${this._sport}.`);
};

let Guard = function(firstname, lastname, sport, skill) {
  /**
   * [...arguments].slice(0, 3)
   * output: [firstname, lastname, sport]
   */
  Player.apply(this, [...arguments].slice(0, 3));
  this._skill = skill;
};

Guard.prototype = Object.create(Player.prototype);
Guard.prototype.constructor = Guard;

Guard.prototype.showoff = function() {
  console.log(`I'm good at ${this._skill}`);
};

let kyrie = new Guard('Kyrie', 'Irving', 'basketball', 'dribble');

kyrie.greet();  // Hello, I'm Kyrie Irving.
kyrie.play();  // I play basketball.
kyrie.showoff();  // I play
```

![](../../../../images/javascript/oop-inheritance-01.png)

* 생성자함수를 정의할 때 함수선언식이 아닌 익명함수로 정의하였습니다.
***함수선언식 같은 경우 자바스크립트 인터프리터가 스크립트가 로딩되는 시점에 바로 초기화하고 VO(Variable Object)에 저장되기 때문에 위치와 상관없이 호출됩니다. 그렇기 때문에 VO에 함수선언식이 많으면 VO에 저장되는 코드가 많게 되어 어플리케이션 응답속도를 저하시킵니다. 하지만 익명함수는 VO에 저장되지 않고  runtime시에 실행됩니다.***
* <code>call</code>과 <code>apply</code> 함수를 이용하여 현재 생성자 *context*로 바인딩합니다.
* <code>Object.create()</code>을 이용하지 않고 *prototype*을 그대로 상속하였을 경우 <code>prototype</code>에 추가한 <code>greet</code>함수도 같이 업데이트 되기 때문에 값이 변경되버립니다.
* <code>constructor</code> 설정은 필수는 아니지만 내부적으로 설정되지 않는 부분들을 수정해주는 습관을 주는 것이 도움이 됩니다.

### # Prototypal pattern

``` javascript
let employee = {
  company: 'Facebook',
  work: function() {
    console.log(`I work for ${this.company}`);
  },
  master: function() {
    console.log(this.skill);
  }
};

let webEngineer = Object.create(employee);
webEngineer.skill = 'HTML, CSS, Javascript';

let frontendEngineer = Object.create(webEngineer);
frontendEngineer.skill = 'React, Redux, React-Native';
frontendEngineer.master = function() {
  console.log(`Mastered ${this.skill}`);
}
```

``` javascript
let 
```

 

### Reference
<https://developer.mozilla.org/ko/docs/Web/JavaScript/Introduction_to_Object-Oriented_JavaScript>
<https://stackoverflow.com/questions/8453887/why-is-it-necessary-to-set-the-prototype-constructor>
<https://www.youtube.com/watch?v=doXpW5AD60Q&index=7&list=PLoYCgNOIyGABs-wDaaxChu82q_xQgUb4f>
<https://www.youtube.com/watch?v=sWOXYDBbz0g&index=6&list=PLoYCgNOIyGABs-wDaaxChu82q_xQgUb4f>
