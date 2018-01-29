---
title: Functional Programming
tags:
  - Javascript
thumbnail:
  - ../../../../images/javascript/javascript-logo.png
categories:
  - Front-end
  - Javascript
---

![](../../../../images/javascript/javascript-logo.png)

**Functional Programming(함수형 프로그래밍)**은 기존 [객체지향 프로그래밍](https://jason0853.github.io/2018/01/10/OOP-Inheritance/)처럼 객체에 존재하는 property나 method를 사용하는 것과 달리 이름 그대로 함수를 기반으로 프로그래밍 하는 기법입니다. 최근 개발자들 사이에서 **함수형 프로그래밍**이 trend로 잡고 있는데 어떤 점이 좋은지 한번 정리해보겠습니다.


### # The advantage of Functional Programming

* 버그가 적음.
* 추론하기가 쉬움.
* 시간이 절약.

우선 기본적으로 함수형 프로그래밍을 어떻게 하는지 알아보겠습니다.

``` js
let sum = function(a) {
  return a + 3;
};

let result = sum;

result(3);  // 6
```

위 코드처럼 함수는 변수에 할당될 수 있으며 **입출력이 순수**하기 하기 때문에 **side-effect가 전혀 없습니다.** 그럼 자바스크립트에서 **higher-order function**하면 자주 언급되는 함수들을 한번 살펴보겠습니다.

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

* <code>reduce</code>의 두번째 파라미터는 초기값을 설정해주기 때문에 변수에 initial 값을 지정해 줄 필요가 없어졌습니다.

### # Closure

**Closure(클로저)**란 

``` js
let count = function() {
  let _number = 0;
  return function() {
    _number++
    return _number;
  }
};

let increase = count();
increase();
```

*