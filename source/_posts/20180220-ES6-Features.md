---
title: ES6 Features
tags:
  - Javascript
  - ES6
thumbnail:
  - ../../../../images/javascript/javascript-logo.png
categories:
  - Front-end
  - Javascript
date: 2018-02-20 17:57:20
---


![](../../../../images/javascript/javascript-logo.png)

자바스크립트 관련 포스팅을 하면서 **ES6** 문법을 간혹 쓴 경우가 있었는데 이번 기회에 **ES6** 문법 기능을 한번 정리해보겠습니다. 물론 기존 Vanilla Javascript(ES5)도 우수하지만 **ES6** 문법을 혼용해서 쓰면 좀 더 간결하게 코드를 작성할 수 있고 다른 언어에서 지원되는 클래스(물론 무늬만)나 상수도 지원받을 수 있습니다.

### # var, let, const

기본적으로 <code>var</code>는 **function-scope**이고 <code>let</code>, <code>const</code>는 **block-scope**입니다.

``` js
// ES5
var i = 1000;
(function() {
  // var i 호이스팅(hoisting) 
  for (i = 0; i < 3; i++) {
    console.log(i);  // 0, 1, 2
  }

  var i;
})();

console.log('after loop', i);  // after loop 1000

// ES6
var i = 1000;
for (let i = 0; i < 3; i++) {
  console.log(i);  // 0, 1, 2
}

console.log('after loop', i);  // after loop 1000
```

* ES5 문법을 보면 함수 안에 <code>var i;</code>는 나중에 선언되었지만 에러가 나지 않고 **호이스팅(hoisting)**이 일어나면서 함수 최상단으로 끌어올려집니다. 위에서 언급한대로 <code>var</code>는 함수 스코프 단위로 범위가 정해져있기 때문에 loop가 끝나고 나서는 <code>console.log('after loop', i);</code>는 함수 바깥에 선언된 변수 <code>i</code>에 접근하게 됩니다.
* ES6 문법에서는 ES5와 달리 블록 단위로 변수 범위가 정해져있습니다. ES5처럼 굳이 함수를 만들지 않아도 위와 똑같은 결과물이 나옵니다. 하지만 <code>var</code>처럼 변수를 나중에 선언하게 되면은 호이스팅은 되지만 <code>Uncaught ReferenceError: i is not defined</code> 에러가 발생할 것입니다.. 그 이유는 <code>let</code>은 **항상 선언이 된 뒤에 값이 할당되어야 한다는 전제 조건**이 있기 때문입니다.

``` js
let name;
name = 'Jaesung';
name = 'Jason';
console.log(name);

const age;  // Uncaught SyntaxError: Missing initializer in const declaration
```

* <code>let</code>는 **선언은 먼저 하고 값은 나중에 할당**해도 상관없습니다.
* <code>const</code>는 **항상 선언과 값을 함께 할당**해야 에러가 발생하지 않습니다.

``` js
const age = 33;
age = 34;  // Uncaught TypeError: Assignment to constant variable.
```

* <code>const</code>는 **재할당 불가**입니다.

``` js
const person = {
  name: 'Jaesung',
  age: 33
};

person.name = 'Jason';
person.job = true;

person = {  // Uncaught TypeError: Assignment to constant variable.
  job: true
};
```

* <code>const</code>는 **객체의 property 값을 변경 및 새로운 property 값을 추가할 수 있지만 재할당 불가**입니다.

프로그래밍을 할 때 변경 가능한 상태(mutable state)를 최소화하는 습관을 들이는 것이 중요합니다. 왜냐하면 에러가 예상치 못한 곳에서 발생하기 때문에 디버깅하기가 너무 어렵기 때문입니다. 최근 자바스크립트에서는 <code>var</code>를 지양하고 <code>let</code>과 <code>const</code>를 지향하라고 권장합니다. 

### # Destructuring / Template Literals / Spread Operator

**Destructuring**은 비구조화 할당이라고 하며 객체와 배열을 변수로 변환해주는 기능입니다.
**Template Literals**은 <code>`${}`</code> 문법을 제공하여 좀 더 편리하게 문자열을 처리할 수 있는 기능입니다.
**Spread Operator**은 전개 연산자라 하며 2개 이상의 인자, 요소, 변수들을 확장시키는 기능입니다.

``` js
const movie = {
  title: 'Friends with Benefits',
  genre: 'Romance'
};

const { title: tit, genre: gen, rating = 'R' } = movie;

console.log(tit);  // Friends with Benefits
console.log(gen);  // Romance
console.log(rating);  // R
```

* 위 코드 6번째 줄을 보면 객체의 속성을 별개의 변수로 추출하여 새로운 변수(<code>title: tit</code>, <code>genre: gen</code>) 및 default 값(<code>rating = 'R'</code>)을 제공할 수 있습니다.

``` js
showMovie({
  title: 'Jason Bourne',
  genre: 'Action'
});

// ES5
function showMovie(opt) {
  var tit = opt.title,
      genre = opt.genre,
      rating = opt.rating || 'R';
  console.log('title: ' + opt.title + ', genre: ' + opt.genre + ', rating: ' + rating);
}

// ES6
function showMovie({ title: tit, genre, rating = 'R' }) {
  console.log(`title: ${tit}, genre: ${genre}, rating: ${rating}`);
}

// Output
// title: Jason Bourne, genre: Action, rating: R
```

* ES6 문법을 적용하니 확실히 코드량이 줄어들었습니다. 특히 **Template Literals** 문법을 사용하니 ES5보다 문자열 처리가 간결해졌습니다.
 
``` js
const numbers = [1, 2, 3, 4, 5];
const [first, ...rest] = numbers;

console.log(first);  // 1
console.log(...rest);  // 2 3 4 5

const Person = function(name, age, job) {
  this.name = name;
  this.age = age;
  this.job = job;
};

Person.prototype.log = function() {
  console.log(`name: ${this.name}, age: ${this.age}, job: ${this.job}`);
}

const subInfo = [33, 'Front-end Engineer'];
const info = ['jason', ...subInfo];
// info.push(...subInfo);

const p1 = new Person(...info);
p1.log();  // name: Jason, age: 33, job: Front-end Engineer
```

* <code>...</code> **전개 연산자** 문법입니다.
* **전개 연산자**를 이용했기 때문에 굳이 <code>push()</code> 함수 및 파라미터 전달을 하나하나 할 필요가 없어졌습니다.

이렇게 3가지 새로운 문법에 대해서 간략히 살펴보았는데 코드량이 상당히 줄어들어 가독성이 향상되는 장점이 있는 것 같습니다.

### # Arrow Function

**Arrow Function(화살표 함수)**는 간결하고 **this**가 기존의 ES5와 다르게 작동합니다. 또한 생성자 함수로는 적합하지 않습니다.

``` js
function Movie(title) {
  this.title = title;

  // ES5
  setTimeout(function() {
    console.log(this.title);  // Jason Bourne
  }.bind(this), 1000);

  // ES6
  setTimeout(() => {
    console.log(this.title);  // Jason Bourne
  }, 1000);
}

const m1 = new Movie('Jason Bourne');
```

* ES5 문법에서 <code>bind(this)</code>를 하지 않으면 <code>this</code>는 window 객체를 가리킵니다.
* ES6 문법에서는 <code>this</code>가 새로운 target으로 바인딩되지 않고 <code>Movie</code> 객체를 정확히 참조합니다.

``` js
const playerInfo = [
  { position: 'PG', name: 'Kyrie Irving', score: 20, retired: false },
  { position: 'SG', name: 'Kobe Bryant', score: 30, retired: true },
  { position: 'SG', name: 'Tracy Mcgrady', score: 25, retired: true },
  { position: 'PF', name: 'Aaron Gordon', score: 20, retired: false },
  { position: 'PF', name: 'Tim Duncan', score: 23, retired: true },
];

// ES5
const sgTotalScore = playerInfo
  .filter(function(info) {
    return info.retired === true;
  })
  .filter(function(info) {
    return info.position === 'SG';
  })
  .map(function(info) {
    return info.score;
  })
  .reduce(function(prev, score) {
    return prev + score;
  }, 0);

// ES6
const sgTotalScore = playerInfo
  .filter(info => info.retired === true)
  .filter(info => info.position === 'SG')
  .map(info => info.score)
  .reduce((prev, score) => (prev || 0) + score);

console.log(sgTotalScore1);  // 55
console.log(sgTotalScore2);  // 55
```

* **화살표 함수** 또한 ES5로 작성한 코드보다 코드량이 확실히 줄어들었습니다. 
* parameter가 한 개이면 <code>()</code>안에 매개변수를 넣을 필요가 없습니다.
* brackets <code>{}</code>이 없으면 <code>return</code>한 것과 같은 문법입니다.

### # class keyword

자바스크립트 [Object creation](https://jason0853.github.io/2018/01/09/Object-creation/)에 대해 다룬 포스팅에 있으니 참고바랍니다.

### # Generator

**Generator**는 비동기코드를 동기적으로 프로그매밍 할 수 있도록 도와주는 편한 기능입니다.

``` js
const data = [
  { name: 'Jason', 
    profile: {
      age: 33, 
      gender: 'Male' 
    }
  },
  { name: 'Jane', 
    profile: {
      age: 29, 
      gender: 'Female'
    }
  }
];
```

``` sequence
Get name->Get profile: name
Get profile->Get gender: profile
Note right of Get gender: gender
```

위와 같은 데이터 구조(<code>data</code>)를 참조하여 다이어그램 순서대로 처리해서 결과값(gender)를 도출해야된다고 가정해봅시다.
**제너레이터**를 좀 더 쉽게 사용하기 위해 [co 라이브러리](https://www.npmjs.com/package/co)를 사용해서 실습해보겠습니다.

``` js
const co = require('co');

function getName(name) {
  return data.filter((e) => e.name === name );
}

function getProfile(name) {
  return name.map((e) => e.profile);
};

function getGender(profile) {
  const gender = profile[0].gender;
  return Promise.resolve(gender);
};

co(function* () {
  const name = yield getName('Jason');
  const profile = yield getProfile(name);
  const gender = yield getGender(profile);
  return gender;
}).then(result => {
  console.log(result);  // Male
});
```

* <code>yeild</code> 표현을 통해 함수의 return 값을 반환합니다.
* 만약 콜백으로 처리했다면 코드는 스파게티 코드처럼 더럽혀져 있었을겁니다.

이번에는 co 라이브러리를 사용하지 않고 16 - 23번째 줄을 **async/ await** 문법으로 바꿔 똑같은 결과물을 도출해보겠습니다.

``` js
// (중략)
async function showGender(personName) {
  const name = await getName(personName);
  const profile = await getProfile(name);
  const gender = await getGender(profile);
  return gender;
}
showGender('Jason').then(res => console.log(res));  // Male
```

* <code>function*</code> -> <code>async function</code>, <code>yield</code> -> <code>await</code> 두 개의 키워드만 바뀌었고 Promise로 반환하는 것까지 똑같습니다. 
* 라이브러리의 도움없이 사용할 수 있는 장점이 있지만 아직 브라우저의 표준 스펙은 아닙니다.

### Wrap-up

이전 회사에서 ES6로 자바스크립트 개발을 해본 경험이 있습니다. 하지만 그 당시에는 제대로 개념을 익히지 않고 개발을 진행하다보니 ES6 문법 및 기능을 최대한 활용하지 못했던 것 같습니다. 만약 그 당시로 돌아가서 개발한다면 '코드량을 확실히 줄이고 가독성도 향상시켰을텐데' 하는 아쉬움이 있습니다. ㅎㅎ 

### Reference

[비구조화 할당](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
[ES6의 제너레이터를 사용한 비동기 프로그래밍](http://meetup.toast.com/posts/73)