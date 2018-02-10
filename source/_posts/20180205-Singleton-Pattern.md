---
title: Singleton Pattern
tags:
  - Javascript
  - Design Pattern
thumbnail:
  - ../../../../images/javascript/javascript-logo.png
categories:
  - Front-end
  - Javascript
date: 2018-02-05 17:34:34
---


![](../../../../images/javascript/javascript-logo.png)

**Singleton Pattern(싱글톤 패턴)**은 'single'이라는 단어 그대로 **오직 하나의 객체 생성만을 허용**하는 것을 뜻합니다. 

### # Object Literal

``` js
const p1 = {
  name: 'Jason',
  sayName: function() {
    return this.name;
  }
};

const p2 = {
  name: 'Jason',
  sayName: function() {
    return this.name;
  }
};

console.log(p1 == p2);  // false
console.log(p1 === p2);  // false
```

* <code>p1</code>, <code>p2</code> 객체의 속성과 메서드는 같지만 객체를 비교해보면 <code>false</code>가 나옵니다.
* **Object Literal**을 이용한 객체 생성 방법은 이미 싱글톤 패턴과 동일합니다.
***주의) Object Literal은 객체의 속성들이 전부 공개되어 외부에서 조작할 수 있는 단점이 있습니다.***

### # Singleton Pattern

그럼 이제 제대로 된(비공개) 싱글톤 패턴을 만들어 보겠습니다.

``` js
const singleton = (function() {
  let instance;

  function init() {
    const name = 'single';

    function show() {
      return name;
    }

    return {
      name: name,
      show: show
    }
  }

  return {
    getInstance: function() {
      if (!instance) {
        instance = init();
      }
      return instance;
    }
  }
})();

const single1 = singleton.getInstance();
const single2 = singleton.getInstance();

console.log(single1 == single2);  // true
console.log(single1 === single2);  // true

single1.name = 'Singleton Pattern';
console.log(single2.name);  // 'Singleton Pattern'
```

* **IIFE(즉시 호출 패턴)**을 사용하여 외부에서 접근하지 못하도록 막아줍니다.
* <code>init()</code> 함수 내부에 private(비공개) 변수 및 함수를 선언해주고 반환시켜줍니다.
* <code>getInstance</code>는 유일한 공개 메서드입니다. 처음 호출할 때 <code>instance</code> 변수가 <code>undefined</code>이기 때문에 <code>init()</code> 함수를 호출한 뒤에 <code>instance</code> 변수에 <code>init</code> 함수 리턴값을 저장시켜줍니다. 두번째 호출할 때는 <code>instance</code>값에 처음 호출된 객체값이 저장되어 있기 때문에 <code>init</code> 함수를 호출하지 않고 바로 리턴시킵니다.
* 싱글톤이기 때문에 <code>single1.name</code> 속성이 바뀌면 <code>single2.name</code>도 변경됩니다.


### Wrap-up

**싱글톤 패턴** 특징

* 하나의 객체만 생성.
* 동일한 객체의 다수의 인스턴스 허용.
* 여러 객체를 만들지 못하도록 제한하고 첫번째 객체가 생성된 후에는 첫번째 객체 자체를 리턴시킵니다.

### Reference

[4 JavaScript Design Patterns You Should Know](https://scotch.io/bar-talk/4-javascript-design-patterns-you-should-know)