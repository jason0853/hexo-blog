---
title: React and Redux with Typescript
tags:
  - React
  - Redux
  - Immer
  - Typescript
thumbnail: ../../../../images/react/react-logo.png
categories:
  - Front-end
  - React
date: 2018-05-12 17:06:26
---


![](../../../../images/react/react-logo.png)

최근 회사에서 **리액트&리덕스**를 사용하여 웹앱 프로젝트를 진행하고 있는 와중에 **타입스크립트**도 개인적으로 공부하고 있었습니다. 그래서 '이 두 가지를 같이 사용하면 어떨까?'라는 생각을 하게 되었습니다. 다음 프로젝트를 위해 미리 포스팅을 작성하면서 공부해보겠습니다.

### # Create react app with typescript

``` shell
$ create-react-app react-redux-ts --scripts-version=react-scripts-ts
$ cd react-redux-typescript-tutorial
$ yarn start
```

* Microsoft에서 미리 react와 typescript 초기 설정에 대한 CLI를 제공해줍니다. **react-redux-typescript-tutorial** 폴더로 이동하고 <code>yarn start</code>를 실행해보겠습니다.
주의) 혹시 브라우저에서 <code>Duplicate identifier 'URL'</code> 에러가 나는 경우 @types/node 버전을 바꿔주시길 바랍니다. -> <code>yarn add --dev @types/node@9.6.7</code>

### # Configure

우선 첫 component를 만들기에 앞서 src 폴더 안에 있는 logo.svg, App.css, App.test.tsx를 삭제하겠습니다.
그리고 나서 App.tsx 파일 내용은 아래와 같이 바꿔주시길 바랍니다.

{% gist 542d2e22bec63c4f892ce0964fb400fc %}

``` json package.json
//(중략...)
"scripts": {
    "start": "NODE_PATH=src react-scripts-ts start",
    "build": "NODE_PATH=src react-scripts-ts build",
    "test": "react-scripts-ts test --env=jsdom",
    "eject": "react-scripts-ts eject"
  },
//(중략...)
```
``` json tsconfig.json
{
  "compilerOptions": {
    "baseUrl": "./src",
//(중략...)
```
``` json tslint.json
{
  "extends": ["tslint-react", "tslint-config-prettier"],
  "linterOptions": {
    "exclude": [
      "config/**/*.js",
      "node_modules/**/*.ts"
    ]
  },
  "rules": {
      "jsx-no-lambda": false
  }
}
```

* 절대 경로로 파일을 불러올 수 있도록 **package.json**, **tsconfig.json**의 파일을 위와 같이 수정해줍니다.
예) <code>import Person from components/Person</code>
* tslint.json 파일 안에 <code>"tslint:recommended"</code>가 들어가 있다면 삭제하고 <code>"jsx-no-lambda" : false</code>로 설정을 추가해줍니다.

``` shell
$ yarn start
```

* 브라우저에 App component가 잘 렌더링됩니다. 

### # Create first component

**Typescript**를 이용하여 component를 생성할 때는 **.tsx** 확장자를 사용합니다. 

``` plain
// 폴더 구조
src
  - components
    Person.tsx
  App.tsx
  index.css
  index.tsx
  registerServiceWorker.ts
```

{% gist 98433a5acb9d72249e342a9defa8771f %}

* 기존 자바스크립트에서는 prop-types를 이용하여 타입 checking을 해왔었습니다. 하지만 타입스크립트에서는 <code>interface</code> 및 <code>type</code>를 이용하여 props 및 state에 타입을 지정해줍니다.
* type 에러가 날 경우 예전과 달리 런타임 전에 error를 확인하실 수 있습니다.
* 함수형 Component로 작성할 경우 <code>React.SFC</code>를 사용하여 타입을 지정해주는 것이 권장사항입니다.

### # Smart component & Dumb component

이번엔 **smart component(state 및 life cycle api가 필요한 컴포넌트)**와 **dumb component(style 및 단순 props 전달받을 컴포넌트)**로 나누어 연습해보겠습니다.

``` plain
// 폴더 구조
src
  - components
    Counter.tsx
    Person.tsx
  - containers
    CounterContainer.tsx
  App.tsx
  index.css
  index.tsx
  registerServiceWorker.ts
```

{% gist cfd3e945d261859457d82c7116d5a051 %}

* CounterContainer(Smart Component)의 <code>state</code> 값과 함수들을 Counter(Dumb Component)의 props로 전달해줍니다.
* <code>interface</code>가 없는 경우엔 Generic 타입을 빈 객체로 지정해줍니다.
예) <{}, State>

### # Create Todo List

Todo List를 한번 만들어 보면서 컴포넌트 최적화 및 여러가지 기능들을 구현해보겠습니다.

``` plain
// 폴더 구조
src
  - components
    Counter.tsx
    Person.tsx
    TodoForm.tsx
    TodoItem.tsx
    TodoList.tsx
  - containers
    CounterContainer.tsx
    TodoContainer.tsx
  App.tsx
  index.css
  index.tsx
  registerServiceWorker.ts
```

{% gist 2ea857eeb604ad7bd9d67832453b6b9b %}

* <code>TodoList</code>, <code>TodoItem</code>을 함수형 컴퍼넌트로 만들지 않은 이유는 **LifeCycle API**인 <code>shouldComponentUpdate(nextProps, nextState)</code>를 이용하여 컴퍼넌트 최적화를 해야하기 때문입니다. 만약 최적화 작업이 이루어지지 않는다면 Virtual Dom에서 필요없는 resource가 렌더링됩니다. 특히 <code>TodoItem</code>의 갯수가 기하급수적으로 늘어난다면 필요없는 자원들로 인해 결국 렌더링 속도가 느려질 것입니다.

### # Combine Redux with Typescript

이제 **Redux**와 **Typescript**를 함께 사용해보겠습니다. 시작하기에 앞서 필요한 패키지들을 설치해보겠습니다.

``` shell
$ yarn add redux react-redux redux-actions immer
$ yarn add @types/redux @types/react-redux @types/redux-actions
```

* **immer** 패키지 안에 type definitions이 이미 내장되어 있기 때문에 타입 패키지를 따로 설치하지 않아도 됩니다.

수정된 파일과 추가된 파일이 너무 많아 아래 깃헙 소스에서 코드를 검토해주세요.ㅎㅎ
Combine Redux with Typescript 커밋 로그를 수정된 소스와 추가된 파일 소스를 바로 확인하실 수 있습니다.

{% githubCard user:jason0853 repo:react-redux-typescript-tutorial width:100% %}

``` shell
// 폴더 구조
src
  - store
    - models
      counter.ts
      index.ts
      todo.ts
    - modules
      counter.ts
      index.ts
      todo.ts
    actionCreators.ts
    configure.ts
    index.ts
```

* 기존 src 폴더 안에 store 폴더를 추가한 다음 redux 관련 코드는 **modules** 폴더에 분류하고 state, action payload, action creator의 type definition을 **models** 폴더에 분류합니다.
* <code>state</code> 및 <code>props</code>는 <code>type</code>(type alias)를 사용하고 actionCreator methods들은 <code>interface</code>로 타입지정을 합니다.
***주의) 액션 함수들을 <code>interface</code>로 타입 지정을 할 경우 <code>[key: string]: any;</code>를 하지 않으면 <code>index signature is missing in type ~~</code>에러가 나기 때문에 추가해줍니다.***
* actionCreators.ts 파일을 따로 만들어 액션 파일들을 따로 관리해주는 이유는 smart component에서 <code>connect</code>함수에 연결시켜서 props로 가져오는 번거러움을 덜어주기 위함입니다.
예) <code>this.props.CounterActions.increase();</code>가 아닌 <code>CounterActions.increase();</code>
* Reducers 코드를 작성할 때 action.payload 타입이 <code>undefined</code> 에러가 나는 경우가 있습니다. 그 이유는 <code>Action</code> 인터페이스가 payload가 없어도 될 수 있도록 설계되어 있기 때문에 예외처리를 따로 해주어야합니다.
  * <code>action.payload!</code> - payload가 항상 있을 경우
  * <code>action.payload && do something</code> - aciton.payload가 참일 경우에만

![](../../../../images/react/react-redux-typescript-tutorial.png)


### Wrap-up

리액트, 리덕스, 타입스크립트를 함께 사용하며 느낀 점은 타입 지정 때문에 기존보다 코딩 시간이 오래 걸리는 것 같이 느끼지만 적응하다보니 좀 더 풍부한 장점을 누릴 수 있었습니다. 특히 vscode랑 같이 쓰다 보니 자동완성 및 마우스 hover를 할 경우 어떤 타입인지 노출시켜줍니다. 그렇기 때문에 파일을 이동하며 확인해 볼 필요가 없어집니다. 또한 저희가 할 수 있는 사소한 실수를 에디터에서 바로 알려줍니다. 저같은 경우는 앞으로 타입스크립트를 도입하여 작업하는 환경이 점점 늘어날 것 같습니다.

### Reference

[TypeScript with React + Redux/Immutable.js 빠르게 배우기](https://velopert.com/3595)
[Immer](https://github.com/mweststrate/immer)
[Interface vs Type alias in TypeScript 2.7](https://medium.com/@martin_hotell/interface-vs-type-alias-in-typescript-2-7-2a8f1777af4c)