---
title: Typescript Features
tags:
  - Typescript
  - tsconfig
thumbnail:
  - ../../../../images/typescript/typescript-logo.png
categories:
  - Front-end
  - Typescript
date: 2018-03-01 04:34:04
---



![](../../../../images/typescript/typescript-logo.png)

지난 [포스팅](https://jason0853.github.io/2018/02/27/Typescript-Basic-and-Configuration/)에 타입스크립트를 하기 위한 환경을 구축했었습니다. 그럼 본격적으로 **타입스크립트**의 특징에 대해서 하나하나 실습해보겠습니다.

### # Checking Type

자바스크립트는 동적타입 언어이기 때문에 기존 타입에 다른 타입을 대입해도 에러가 나지 않았습니다. 하지만 타입스크립트는 다른 언어(Java)처럼 타입을 시스템을 도입하여 Type 선언을 엄격하게 합니다.
만약 string 타입 변수에 number를 넣으면 아래처럼 에러가 발생합니다.

``` ts
let test1: string = 'Test';
test1 = 3;  // [ts] Type '3' is not assignable to type 'string'.

let test2;
test2 = 'Jaesung';
test2 = 33;

let test3 = 'Jaesung Lee';
test3 = false;  // [ts] Type 'false' is not assignable to type 'string'.
```

* <code>let test1: string</code> 코드를 보면 콜론(:) 뒤에 string이라고 타입을 명시해주었습니다. 즉, string 타입 이외에 다른 타입의 값이 할당되면은 에러를 발생하게 됩니다.
* <code>test2</code> 변수는 선언할 때 값을 할당하지 않았습니다. 이런 경우는 <code>let test2: any;</code> 타입으로 명시해준거랑 같습니다.
* <code>test3</code> 변수는 타입이 선언되지 않았지만 초기값이 string 타입으로 값이 할당되었기 때문에 할당 과정에서 타입이 string으로 결정됩니다. 그렇기 때문에 다른 타입의 값이 재할당되면 에러가 발생하게 됩니다.

``` ts
// string, number, boolean
let pName: string = 'Jason';
let pAge: number = 33;
let isJob: boolean = true;
let profile: string = `Name: ${pName}, Age: ${pAge}, Job: ${isJob}`;

// any
let val: any = 'test';
val = 33;
val = false;

// object
const person: object = {};

// array
let info: any[] = ['Jason Lee', 33, true];
let friends: Array<string> = ['Jason', 'Jane', 'Lilly'];

// Tuple
let player: [string, number];
player = ['Kobe', 8];

// Enum
enum Player {Kobe = 8, Tmac = 1, Kyrie = 11};
let p: Player = Player.Kyrie;
let playerName: string = Player[8];

console.log(p);  // 0
console.log(playerName);  // Kobe

function test(): void {
  console.log('test'); 
}
```

* <code>any</code> 타입은 어떠한 타입의 값도 들어올 수 있습니다. 하지만 실무에서는 any 타입을 지양하는 것을 권고합니다.
* array 타입을 지정할 때는 두 가지 방식이 있습니다. 두번째 방식을 generic array type이라고 합니다.
* **Tuple**이란 array type의 고정된 수의 유형이면서 동일한 타입의 배열이 아닐 때 사용할 수 있습니다.
* **Enum**이란 C#과 같은 언어에서처럼 열거형은 숫자값 집합에 더 친숙한 이름을 지정하는 방법입니다.
* 함수에 <code>void</code>가 선언되어 있을 경우 리턴값이 없어야합니다.

### # function type

함수에 타입을 명시함으로써 argument나 return 타입이 어떤 형태인지 쉽게 알 수 있습니다.

``` ts
function sum(a: number, b: number): number {
  return a + b;
}

function show(): string {
  return 'Calculate';
}

let myCal: (x: number, y: number) => number;
myCal = sum;
console.log(myCal(1, 2));  // 3
myCal = show;  // [ts] Type '() => string' is not assignable to type '(x: number, y: number) => number'. Type 'string' is not assignable to type 'number'.
```

* <code>myCal = show</code> 코드에서 에러가 난 이유는 서로 타입이 다르기 때문입니다. <code>myCal</code>은 인자와 return 타입이 함께 지정되어 있으며 <code>show</code>는 return을 string으로 받는 함수 타입입니다.

### # Object type

``` ts
let info: { name: string; age: number; job: boolean; };
info = { name: 'Jaesung', age: 34, job: true };

// custom type
type profile = { name: string; age: number; job: boolean; };

let p1: profile = { name: 'Jaesung', age: 34, job: true };
let p2: profile = { name: 'Jimmy', age: 19, job: false };
let p3: profile = { name: 'Dennis', age: 22, job: true };
```

* <code>p1</code>, <code>p2</code>, <code>p3</code>는 동일한 타입이지만 변수 <code>info</code>처럼 개별적으로 타입을 선언해주는 것은 좀 별로인것 같습니다. 이때 <code>type</code>과 함께 **custom type**을 명시해줍니다.

### # Union Type

하나 이상의 어떤 타입을 지정하는 방식을 **Union Type**이라고 합니다.

``` ts
let players: string[] | number[] | boolean[] = ['Kobe Bryant', 'Tracy Mcgrady'];

players = [8, 1];
players = [true, false];
```

* <code>any</code>타입을 쓰면 간단하지만 **Union Type**(<code>string[] | number[] | boolean[]</code>)을 쓰는 것을 권장합니다.

### # class

Typesciprt의 클래스는 ES6의 class와 사용법이 다르니 한번 자세히 다뤄보겠습니다.

``` ts
class Person {
  public readonly name: string;
  private age: number;
  protected job: boolean;
  static hobby: string[] = ['Basketball', 'Swimming', 'Jit Jitsu'];
  static word: string = 'Hi, How are you?';

  constructor(name: string, age: number, job: boolean) {
    this.name = name;
    this.age = age;
    this.job = job;
  }

  setAge(newAge: number): void {
    this.age = newAge;
  }

  getAge(): number {
    return this.age;
  }

  move(text: string): void {
    console.log(text);
  }

  static greet(): string {
    return this.word;
  }
}

class Student extends Person {
  constructor(name: string, age: number, job: boolean) {
    super(name, age, job );
    super.move('I can walk.');
  }

  change(isJob: boolean): void {
    this.job = isJob;
  }

  isJob(): boolean {
    return this.job;
  }
}

const p1:Person = new Person('Jason', 33, true);
const p2:Student = new Student('Jane', 18, false);

// p1.name = 'Jason Lee'  // [ts] Cannot assign to 'name' because it is a constant or a read-only property.
console.log(p1.name);  // Jason

// p2.setAge(19);
// console.log(p2.getAge());  // 19

console.log(p2.isJob());  // false

console.log(Person.greet());  // Hi, How are you?
console.log(Person.hobby);  // ["Basketball", "Swimming", "Jit Jitsu"]
```

* <code>public</code> - 클래스, 서브클래스, 인스턴스 어디에서도 접근 가능.
* <code>private</code> - 클래스 내부 이외의 서브클래스나 인스턴스에서는 접근 불가능.
* <code>protected</code> - 클래스 내부와 서브클래스 내부에서만 접근 가능. 인스턴스 접근 불가능.
* <code>readonly</code> - 읽기 전용 propery.
* <code>static</code> - 클래스의 인스턴스로 호출하지 않고 접근할 수 있음. 인스턴스 접근 불가능.
***<code>static greet()</code>메서드 안에 있는 this는 인스턴스가 아닌 클래스 <code>person</code>을 가르킵니다.***

``` ts
abstract class Car {

  constructor(public name: string) {
  }

  abstract brand(): string;

  move(): void {
    console.log('test car');
  }
}

class Genesis extends Car {

  constructor() {
    super('Genesis')
  }

  brand() {
    return 'Hyundai'
  }
}

const g = new Genesis();

g.move();  // test car
console.log(g.name);  // Genesis
console.log(g.brand());  // Hyundai
```

* **추상클래스(abstract class)**는 오직 상속만 가능하며 추상 메소드는 상속하는 클래스에 반드시 구현이 되어 있어야합니다.

### # interface

**인터페이스(interface)**는 custom type과 거의 비슷하지만 몇 가지 효율적인 면이 있습니다. 클래스와 상속에서 유용하게 사용할 수 있습니다.

``` ts
interface Human {
  readonly id: number;
  name: string;
  alive: boolean;
  age?: number;
  say: (arg: string) => string;
}

let person: Human;

person = {id: 111, name: 'Jason', alive: true, say: (lang: string) => lang};

function getName(info: Human): void {
  console.log(info.name)
}

getName(person);  // Jason
console.log(person.say('Korean'));  // Korean

class Student implements Human {
  constructor(
    public id: number,
    public name: string,
    public alive: boolean
  ) {}

  say(lang: string): string {
    return lang
  }
}

const p = new Student(123, '18', true);

console.log(p.say('English'));  // English

interface Gender {
  gender: string;
}

interface Foreigner extends Human, Gender {
  nation: string;
}

let engineer: Foreigner = {id: 111, name: 'Jason', alive: true, say: (lang: string) => lang, nation: 'Korea', gender: 'M'}

// 빈 걕체 초기화
// let engineer = <Foreigner>{};
```

* <code>?</code> **Optional Property**입니다. 인터페이스에 기술한 프로퍼티와 메소드가 반드시 구현되어야 합니다. 하지만 선택적 프로퍼티(?)를 붙이면 생략해도 무방합니다.
* 클래스랑 같이 쓸때 메소드 내부를 반드시 구현해야합니다.
* 여러 인터페이스 상속이 가능합니다.
* <code>engineer</code> 변수를 초기화하지 않고 비어있는 객체로 설정하고 싶다면 **Generic** 문법을 사용하여 빈 객체로 둔 상태에서 property를 추가해줄수 있습니다.

### # Generic

**제네릭(Generic)**은 위에서 배운 타입 선언과 달리 미리 선언을 명시하지 않고 생성한 뒤에 타입이 정해집니다. 또한 타입도 여러가지 타입이 올 수 있습니다.

``` ts
// function test1(arg: number): number {
//   return arg;
// }

// function test2(arg: string): string {
//   return arg;
// }

// function test3(arg: boolean): boolean {
//   return arg;
// }

function test<T>(arg: T): T {
  return arg;
}

let answer = test<string>('Generic');

console.log(answer);  // Generic

let myAnswer: <T>(arg: T) => T = test;
// let myAnswer2: { <T>(arg: T): T } = test;  // 변수 myAnswer와 같음

console.log(myAnswer('Generic'));  // Generic
```

* 주석친 부분을 보시면 같은 함수인데 타입만 다를뿐 안에 구현되는 내용은 같습니다. 이럴때 좀 더 간편하게 쓸 수 있는 것이 **Generic**입니다. 선언 시점에 타입을 명시하는 것이 아니라 생성 시점, 즉 <code>test</code>함수가 실행되면서 타입이 string으로 정해집니다.

### Wrap-up

지금까지 간략하게 **타입스크립트**의 특징에 대해서 알아보았습니다. 자바스크립트만 하다가 타입을 명시하면서 코딩을 하니까 몬가 어색했지만 프로젝트를 진행할 때 좀 더 안정적으로 개발할 수 있을것 같다는 생각이 들었습니다. 특히 인터페이스나 타입이 지정되어 있기 때문에 새로운 팀원이 투입될 시 빠르게 코드 분석을 할 수 있을 것 같습니다. 타입스크립트를 계속 관심만 가지고 지켜보다가 비로소 이번 포스팅으로 인해 첫걸음을 뗀 것 같습니다. 이번에 다루지 못하고 지나간 부분들은 다음 포스팅때 좀 더 공부해서 공유해보도록 하겠습니다.

### Reference

[타입스크립트 공식 홈페이지](https://www.typescriptlang.org)
[타입 선언과 정적 타이핑](http://poiemaweb.com/typescript-typing)