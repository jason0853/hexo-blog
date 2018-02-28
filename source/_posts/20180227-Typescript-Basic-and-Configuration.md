---
title: Typescript Basic and Configuration
tags:
  - Typescript
  - tsconfig
thumbnail:
  - ../../../../images/typescript/typescript-logo.png
categories:
  - Front-end
  - Typescript
date: 2018-02-27 13:25:40
---


![](../../../../images/typescript/typescript-logo.png)

**타입스크립트**는 Microsoft에서 만든 정적 타입언어이며, 자바스크립트의 한계를 보완해줄 수 있습니다. 개인적으로 가장 큰 장점은 자바스크립트(ES5, ES6)와 타입스크립트 문법을 공존해서 쓸 수 있는 것이라고 생각합니다. 한 마디로 **Typescript는 Javascript의 super set**이라고 정의하는데 자바스크립트 개발자들에게는 반가운 소식일 것 같습니다. 이번 포스팅은 타입스크립트를 시작하는 방법에 대해서 알아보겠습니다.

### # Getting started with Typescript

우선 typescript를 install하고 제대로 설치되었는지 확인해보겠습니다. 그리고 Microsoft사에서 제공하는 에디터인 [vscode](https://code.visualstudio.com/)를 설치해서 실습을 진행하겠습니다.

``` shell
$ npm install -g typesciprt
$ tsc -v
```

간단하게 test.ts 파일을 하나 만들어서 js 파일로 compile 해보겠습니다.

``` ts test.ts
function test(lang: string): string {
  return lang;
}

console.log(test('Typescript'));
```

``` shell
$ tsc test.ts
```

``` js test.js
function test(lang) {
  return lang;
}
console.log(test('Typescript'));
```

node로 test.js를 실행시켜보겠습니다.

``` shell
$ node test.js
Typescript
```

'Typesciprt'가 log에 출력됩니다. 간단히 ts 파일을 만들고 실행까지 시켜보았습니다. 이번에는 **tsc** 몇가지 명령어에 대해서 정리해보겠습니다.

``` shell
$ tsc *.ts
$ tsc 'filename' --watch
$ tsc 'filename' -t 'target'
```

* asterisk(\*)을 사용하여 모든 ts 파일을 js 파일로 complie합니다.
* <code>watch</code> 옵션은 해당 파일을 계속 감지하여 js 파일로 변경시켜줍니다.
* <code>t</code> 옵션은 target의 약자로 타입스크립트의 기본 compile system은 ES3로 target이 정해져 있습니다. 만약 ES5로 바꾸고 싶다면 아래와 같이 명령어를 실행시켜주세요.
예) <code>tsc test.ts -t ES5</code>

### # tsconfig.json

타입스크립트로 프로젝트를 시작할 때 **tsconfig.json**에 다양한 옵션을 정의하여 환경설정을 기술할 수 있습니다. 어떤 옵션들이 있는지 한 번 살펴보겠습니다.

{% gist 3b63b6888123e3bd3b0bcfd64049cb48 %}

* <code>baseUrl</code> : 기본 url 설정
* <code>rootDir</code> : root directory 설정 (outDir 옵션과 함께 사용)
* <code>outDir</code> : 결과 파일을 저장할 directory 지정
* <code>target</code> : target 변경
* <code>module</code> : module mode 지정
* <code>removeComments</code> : 주석 삭제
* <code>strict</code> : 모두 엄격하게 type checking 
(noImplicitAny, noImplicitThis, alwaysStrict, strictNullChecks, strictFunctionTypes, strictPropertyInitialization)
* <code>sourceMap</code> : map 파일 비활성화 (vscode에서 디버깅할려면 true로 변경해야함)
* <code>pretty</code> : error message colorfully 출력
* <code>noEmitOnError</code> : compile 대상 리스트
* <code>include</code> : error 발생시 결과 파일 저장 안함
* <code>exclude</code> : compile 대상 제외 리스트

이 옵션 이외에도 다양한 옵션들이 많이 있습니다. [여기](http://www.typescriptlang.org/docs/handbook/compiler-options.html)를 참고해주세요.

### Wrap-up

이제 typescript를 공부하기 위한 최소한의 개발환경을 셋팅해보았습니다. 다음 포스팅부터 본격적으로 타입스크립트의 문법 및 기능들을 살펴보겠습니다.

### Reference

[Visual Studio Code에서의 TypeScript 개발 환경 구축](http://poiemaweb.com/typescript-vscode)
