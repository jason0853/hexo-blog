---
title: Functional Programming
tags:
  - Javascript
thumbnail:
  - ../../../../images/javascript/javascript-logo.png
categories:
  - Front-end
  - Javascript
date: 2018-02-02 15:37:46
---


![](../../../../images/javascript/javascript-logo.png)

**Functional Programming(함수형 프로그래밍)**은 기존 [객체지향 프로그래밍](https://jason0853.github.io/2018/01/10/OOP-Inheritance/)처럼 객체에 존재하는 property나 method를 사용하는 것과 달리 이름 그대로 함수를 기반으로 프로그래밍하는 기법입니다. 최근 개발자들 사이에서 **함수형 프로그래밍**이 trend로 잡고 있는데 어떤 점이 좋은지 한번 정리해보겠습니다.


### # The advantage of Functional Programming

* 버그가 적음.
* 추론하기가 쉬움.
* 시간 절약.

우선 기본적으로 함수형 프로그래밍을 어떻게 하는지 알아보겠습니다.

``` js
let sum = function(a) {
  return a + 3;
};

let result = sum;

result(3);  // 6
```

위 코드처럼 함수는 변수에 할당될 수 있으며 **입출력이 순수**하기 하기 때문에 **side-effect가 전혀 없습니다.** 그럼 자바스크립트에서 **Higher-Order Function**하면 자주 언급되는 함수들과 필요한 개념들을 한번 살펴보겠습니다.

### # Filter

조건에 부합하는 값들을 필터링해서 새로운 배열을 반환합니다.

``` js
let _ = require('underscore');

let players = [
  { name: 'Kyrie Irving', retire: false },
  { name: 'Kobe Bryant', retire: true },
  { name: 'Tracy Mcgrady', retire: true  },
  { name: 'Aaron Gordon', retire: false },
  { name: 'Tim Duncan', retire: true }
];

let isRetired = function(player) {
  return player.retire === true;
};

let retiredPlayers = players.filter(isRetired);
let currentPlayers = _.reject(players, isRetired);

/* Output: retiredPlayers */
/* 
[ { name: 'Kobe Bryant', retire: true },
  { name: 'Tracy Mcgrady', retire: true },
  { name: 'Tim Duncan', retire: true } ]
*/

/* Output: currentPlayers */
/* 
[ { name: 'Kyrie Irving', retire: false },
  { name: 'Aaron Gordon', retire: false } ]
*/
```

* <code>filter</code>라는 자바스크립트 내장 함수를 사용함으로써 for loop와 if 구문을 사용할 필요가 없어졌습니다. 결론적으로 코드는 좀 더 간결해졌습니다.
* <code>isRetired</code> 변수를 함수형으로 표현함으로써 재사용성이 높아지게 되었습니다.

### # Map

기존 데이터에서 필요한 값들만 추출해서 새로운 배열을 반환합니다.

``` js
let players = [
  { name: 'Kyrie Irving', retire: false },
  { name: 'Kobe Bryant', retire: true },
  { name: 'Tracy Mcgrady', retire: true  },
  { name: 'Aaron Gordon', retire: false },
  { name: 'Tim Duncan', retire: true }
];

let names = players.map(x => x.name);  // arrow function(=>) - es6 문법

// Output: names
/*
["Kyrie Irving", "Kobe Bryant", "Tracy Mcgrady", "Aaron Gordon", "Tim Duncan"]
*/
```

* ES6 문법을 사용하여 코드를 한 줄로 정리하였습니다.
* 파라미터명을 player라고 명명할수도 있지만 **함수형 프로그래밍**에서는 <code>x</code>라고 짧게 하는 것이 일반적입니다.

### # Reduce

기존 데이터 값들을 가지고 연산에 의해 반환되는 값을 구할때 주로 사용하는 메서드입니다.

``` js
let orders = [
  { price: 30000 },
  { price: 1000 },
  { price: 5000 },
  { price: 10000 },
  { price: 20000 },
];

let totalPrice = orders.reduce((sum, order) => sum + order.price, 0);  // arrow function(=>) - es6 문법

// Output: totalPrice
// 66000
```

* <code>reduce</code>의 두번째 파라미터는 초기값을 설정해주기 때문에 변수로 initial 값을 지정해 줄 필요가 없어졌습니다.

### # Closure

**Closure(클로저)**란 외부함수가 실행이 끝났음에도 불구하고 내부함수가 외부함수의 변수에 접근할 수 있는 것을 가리킵니다.

``` js
let count = function() {
  let _number = 0;
  return function() {
    _number++;
    return _number;
  }
};

let increase = count();  // increase -> 클로저
increase();  // 1
increase();  // 2
increase();  // 3
```

* <code>count()</code> 함수가 실행되어 내부함수를 return하여 종료되었음에도 불구하고 <code>increase()</code> 함수는 호출되면서 <code>count</code> 함수의 지역변수인 <code>_number</code>에 접근하게 되어 값이 제대로 출력됩니다.
* <code>increase</code>에서 **클로저**가 생성됩니다.

이번에 조금 다른 예제로 클로저를 다뤄보겠습니다.

``` js
function makeNumber(a) {
  return function(b) {
    return a + b;
  };
}

var sum3 = makeNumber(3);  // sum3 -> 클로저
var sum5 = makeNumber(5);  // sum5 -> 클로저
sum3(10);  // 13
sum5(10);  // 15
```

* <code>makeNumber</code>는 두 개의 새로운 함수들을 만들기 위해 **함수 팩토리**를 사용합니다.
* <code>a</code> 인자를 보통 자유변수라고 부르며 <code>b</code> 인자를 묶인 변수라고 합니다.
* 문법적 환경으로 바라보았을 때 <code>sum3</code>의 <code>a = 3</code>이며, <code>sum5</code>의 <code>a = 5</code>입니다.

### # Currying

**Currying(커링)**은 한 번에 모든 인자를 전달하지 않고 첫번째 인자를 전달하고 두번째 인자를 전달할 새 함수를 반환합니다. 이렇게 인자의 일부만 받도록 하는 기법을 **커링**이라고 부릅니다.

``` js
let person = 
      name => 
        age => 
          job => 
            `I'm ${name}. I'm ${age}. I work as a ${job}.`;

let personName = person('Jason');
let personAge = personName(33);
personAge('Front-end Engineer');

/* Output */
// I'm Jason. I'm 33. I work as a Front-end Engineer.
```

* <code>person</code> 함수는 **커링** 기법을 사용할 수 있게 파라미터 이후에 함수를 계속 리턴해주었습니다.

``` js
const _ = require('lodash');

let person = (name, age, job) => `I'm ${name}. I'm ${age}. I work as a ${job}`;

person = _.curry(person);

let personName = person('Jason'); 
let personAge = personName(33);
personAge('Front-end Engineer');

/* Output */
// I'm Jason. I'm 33. I work as a Front-end Engineer.
```

* lodash 라이브러리를 이용하여 <code>person</code> 함수를 **커링**할 수 있는 함수로 만들어 보았습니다.

위에서 filter 함수를 다뤄보았을 때 underscore 라이브러리를 사용하여 은퇴한 선수들과 현역 선수들을 구분했었습니다. 이번에는 lodash의 curry 함수를 사용하여 구분해보겠습니다.

``` js
const _ = require('lodash');

let players = [
  { name: 'Kyrie Irving', retire: false },
  { name: 'Kobe Bryant', retire: true },
  { name: 'Tracy Mcgrady', retire: true  },
  { name: 'Aaron Gordon', retire: false },
  { name: 'Tim Duncan', retire: true }
];

let retired = _.curry((val, obj) => obj.retire === val);

let retiredPlayers = players.filter(retired(true));
let currentPlayers = players.filter(retired(false));

/* Output: retiredPlayers */
/* 
[ { name: 'Kobe Bryant', retire: true },
  { name: 'Tracy Mcgrady', retire: true },
  { name: 'Tim Duncan', retire: true } ]
*/

/* Output: currentPlayers */
/* 
[ { name: 'Kyrie Irving', retire: false },
  { name: 'Aaron Gordon', retire: false } ]
*/
```

### # Recursion

**Recursion(재귀)**는 함수가 자기 자신을 호출하는 것을 말하며, 어느 조건을 주었을 경우 만족할 때까지 반복문처럼 계속 호출합니다. **함수형 프로그래밍**에서는 반복문 대신 재귀를 사용합니다.

``` js
let count = (num) => {
  if (num === 0) return;
  console.log(num);
  count(num-1);
};

count(10);

/* Output */
/*
10
9
8
7
6
5
4
3
2
1
*/
```

* <code>count</code> 함수는 0이 되기 전까지 계속 자기 자신을 호출하기 때문에 **재귀함수**라고 할 수 있습니다.

이번에는 **재귀함수**를 응용해서 object tree 구조를 한 번 만들어보겠습니다.

``` js
let categories = [
  { id: 'human', parent: null },
  { id: 'men', parent: 'human' },
  { id: 'women', parent: 'human' },
  { id: 'jason', parent: 'men' },
  { id: 'tom', parent: 'men' },
  { id: 'jane', parent: 'women' },
];

/* Result */

{
  human: {
    men: {
      'jason': {},
      'tom': {}
    },
    women: {
      'jane': {},
    }
  }
}
```

<code>categories</code>라는 데이터베이스가 있다고 가정하고 재귀함수를 사용하여 위와 같이 똑같은 결과값이 나오도록 해보겠습니다.

``` js
let categories = [
  { id: 'human', parent: null },
  { id: 'men', parent: 'human' },
  { id: 'women', parent: 'human' },
  { id: 'jason', parent: 'men' },
  { id: 'tom', parent: 'men' },
  { id: 'jane', parent: 'women' }
];

let makeObjectTree = (categories, parent) => {
  let obj = {};

  categories
    .filter(c => c.parent === parent)
    .forEach(c => obj[c.id] = makeObjectTree(categories, c.id));

  return obj;
};

JSON.stringify(makeObjectTree(categories, null), null, 2);

/* Output */
/*
{
  human: {
    men: {
      'jason': {},
      'tom': {}
    },
    women: {
      'jane': {},
    }
  }
}
*/
```

* <code>JSON.stringify()</code>를 사용해서 자바스크립트 값을 JSON 문자열로 변경하였습니다.

***주의) 반복문보다 속도가 느리며 stack을 사용하기 때문에 재귀호출이 많아지면 성능상에 이슈가 발생합니다.***

### # Promise

**Promise**는 콜백처럼 비동기를 다루는 방식 중에 하나입니다.

``` js
let test = (str) => new Promise((reslove, reject) => {
  if (typeof str === 'string') {
    reslove(str);
  } else {
    let msg = 'This argument must be string';
    reject(new Error(msg));
  }
});

let arr = [];

Promise.all([ test('test1'), test('test2'), test('test3') ])
  .then(data => {
    data.forEach((val) => arr.push(val));
  })
  .catch(err => console.error(err));

console.log(arr);  // ["test1", "test2", "test3"]
```

* <code>Promise</code>는 ECMA6의 글로벌 객체에 포함되어 있으며 표준으로 채택되었습니다.
* **Promise**의 가장 큰 장점은 콜백 지옥에서 벗어날 수 있다는 점입니다.

### # Functor

**Functor**는 map(매핑)될 수 있는 객체이며, 주로 컴퓨터 공학에서 사용됩니다. 기존 값들은 유지 및 변형시키면서 새로운 결과물(**functor**)를 얻게 됩니다. 아래 예제를 통해 한번 설명을 해보겠습니다.

``` js
const arr = [1, 2, 3, 4, 5];

const newArr = arr.map(num => num * 2);

console.log(newArr);  // [2, 4, 6, 8, 10]
```

* <code>map</code> 메서드를 통해 변형이 일어납니다. 하지만 기존 <code>arr</code> 배열안에 있는 값들은 바뀌지 않습니다.
* <code>newArr</code>은 <code>map</code> 메서드를 통해 새로운 배열, 즉 매핑될 수 있는 새로운 **functor**를 얻게 됩니다.

아래 코드도 위와 같은 **functor**의 또 다른 예제입니다.

``` js
const arr = [
  { name: 'Jason', age: 33 },
  { name: 'Jane', age: 28 },
  { name: 'Tom', age: 37}
];

const newArr = arr.map(obj => {
  let newObj = {};

  newObj[obj.name] = obj.age;

  return newObj;
});

console.log(newArr);  // [{"Jason":32},{"Jane":30},{"Tom":31}]
```

### Wrap-up

**함수형 프로그래밍**의 특징

* **High Order Function**을 통한 코드 재사용성.
* 항상 부작용이 없는 **Pure Function** 유지.
* <code>for</code>, <code>if</code>구문식보다는 **표현식** 사용. 
* **Immutable**한 데이터 구조.

지금까지 함수형 프로그래밍을 다루면서 필요한 개념들을 다뤄보았습니다. 위 포스팅에 잘못된 내용이나 오타가 있는 부분은 댓글 부탁드립니다.

### Reference

[Mozilla - 클로저](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Closures)
[함수형 프로그래밍이란 무엇인가?](http://sungjk.github.io/2017/07/17/fp.html)