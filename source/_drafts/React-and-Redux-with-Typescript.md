---
title: React and Redux with Typescript
tags:
  - git
thumbnail: ../../../../images/react/react-logo.png
categories:
  - Front-end
  - React
---

![](../../../../images/react/react-logo.png)

최근 회사에서 **리액트&리덕스**를 사용하여 웹앱 프로젝트를 진행하고 있는 와중에 **타입스크립트**도 개인적으로 공부하고 있었습니다. 그러는 와중에 '이 두 가지를 같이 사용하면 어떨까?'라는 생각을 하게 되었습니다. 다음 프로젝트를 위해 미리 포스팅을 작성하면서 공부해보겠습니다.

### # Create react app with typescript

``` shell
$ create-react-app react-redux-ts --scripts-version=react-scripts-ts
$ cd react-redux-ts
$ yarn start
```

* Microsoft에서 미리 react와 typescript 초기 설정에 대한 CLI를 제공해줍니다. **react-redux-ts** 폴더로 이동하고 <code>yarn start</code>를 실행해보겠습니다.
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

* 절대 경로로 파일을 불러올 수 있도록 **package.json**, **tsconfig.json**의 파일을 위와 같이 수정해줍니다.
예) <code>import Person from components/Person</code>

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
    - Person.tsx
  App.tsx
  index.css
  index.tsx
  registerServiceWorker.ts
```

{% gist 98433a5acb9d72249e342a9defa8771f %}

* 기존 자바스크립트에서는 prop-types를 이용하여 타입 checking을 해왔었습니다. 하지만 타입스크립트에서는 <code>interface</code>를 이용하여 props 및 state에 타입을 지정해줍니다.
* type 에러가 날 경우 예전과 달리 런타임 전에 error를 확인하실 수 있습니다.
* 함수형 Component로 작성할 경우 <code>React.SFC</code>를 사용하여 타입을 지정해주는 것이 권장사항입니다.

### # Smart component & Dumb component

이번엔 **smart component(state 및 life cycle api가 필요한 컴포넌트)**와 **dumb component(style 및 단순 props 전달받을 컴포넌트)**로 나누어 연습해보겠습니다.

``` plain
// 폴더 구조
src
  - components
    - Counter.tsx
    - Person.tsx
  - containers
    - CounterContainer.tsx
  App.tsx
  index.css
  index.tsx
  registerServiceWorker.ts
```

{% gist cfd3e945d261859457d82c7116d5a051 %}

* CounterContainer(Smart Component)의 <code>state</code> 값과 함수들을 Count(Dumb Component)의 props로 전달해줍니다.
* <code>interface</code>가 없는 경우엔 Generic 타입을 빈 객체로 지정해줍니다.
예) <{}, State>

