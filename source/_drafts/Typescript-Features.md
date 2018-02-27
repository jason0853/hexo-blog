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

* <code>any</code> 타입은 어떠한 타입의 값도 들어올 수 있습니다.
* array 타입을 지정할 때는 두 가지 방식이 있습니다. 두번째 방식을 generic array type이라고 합니다.
* **Tuple**이란 array type의 고정된 수의 유형이면서 동일한 타입의 배열이 아닐 때 사용할 수 있습니다.
* **Enum**이란 C#과 같은 언어에서처럼 열거형은 숫자값 집합에 더 친숙한 이름을 지정하는 방법입니다.
* 함수에 <code>void</code>가 선언되어 있을 경우 리턴값이 없어야합니다.

### # class

Typesciprt의 클래스는 ES6의 class와 사용법이 다르니 한번 자세히 다뤄보겠습니다.

``` ts
class Person {
  public readonly name: string;
  private age: number;
  protected job: boolean;
  static hobby: string[] = ['Basketball', 'Swimming', 'Jit Jitsu'];

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

  static greet(): string {
    return 'Hi, How are you?';
  }
}

class Student extends Person {
  constructor(name: string, age: number, job: boolean) {
    super(name, age, job);
  }

  changeJob(newJob: boolean): void {
    this.job = newJob;
  }

  isJob(): boolean {
    return this.job;
  }
}

const p1:Person = new Person('Jason', 33, true);
const p2:Student = new Student('Jane', 18, false);

// p1.name = 'Jason Lee'  // [ts] Cannot assign to 'name' because it is a constant or a read-only property.
console.log(p1.name);  // Jason

p2.setAge(19);
console.log(p2.getAge());  // 19

console.log(p2.isJob());  // false

console.log(Person.greet());  // Hi, How are you?
console.log(Person.hobby);  // ["Basketball", "Swimming", "Jit Jitsu"]
```

* <code>public</code> - 클래스, 서브클래스, 인스턴스 어디에서도 접근 가능.
* <code>private</code> - 클래스 내부 이외의 서브클래스나 인스턴스에서는 접근 불가능.
* <code>protected</code> - 클래스 내부와 서브클래스 내부에서만 접근 가능. 인스턴스 접근 불가능.
* <code>readonly</code> - 읽기 전용 propery.
* <code>static</code> - 클래스의 인스턴스로 호출하지 않고 접근할 수 있음. 인스턴스 접근 불가능.

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

### Wrap-up



### Reference

[타입 선언과 정적 타이핑](http://poiemaweb.com/typescript-typing)