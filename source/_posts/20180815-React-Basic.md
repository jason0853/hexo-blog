---
title: React Basic
tags:
  - React
  - Javascript
thumbnail: ../../../../images/react/react-logo.png
categories:
  - Front-end
  - React
date: 2018-08-15 14:51:56
---


![](../../../../images/react/react-logo.png)

오랜만에 Velopert 님의 인프런 강의(**리액트**)를 보고 정리하는 차원에서 리액트 관련 포스팅을 작성해보겠습니다.

### # Use Create-React-App

```shell
$ npx create-react-app react-basic
$ cd react-basic
$ yarn start
```

- Facebook 에서 제공하는 **create-react-app** CLI 툴을 사용하여 리액트 프로젝트를 사용하였습니다.

### # jsconfig.json and .eslintrc

```shell
$ yarn add --dev prettier-eslint
$ touch jsconfig.json .eslintrc
```

```json jsconfig.json
{
  "compilerOptions": {
    "baseUrl": "./"
  }
}
```

```json package.json
//(중략...)
"scripts": {
    "start": "NODE_PATH=src react-scripts start",
    "build": "NODE_PATH=src react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  },
//(중략...)
```

```json .eslintrc
{
  "extends": ["react-app"],
  "rules": {
    "quotes": ["error", "single", { "avoidEscape": true }],
    "indent": ["error", 2]
  }
}
```

- <code>baseUrl</code> 옵션에 src 폴더를 root 로 지정해줍니다.
- 서버를 실행하는 명령어 구문에 <code>NODE_PATH=src</code>를 추가해줍니다.
- **prettier-eslint**는 코드를 일관성 있게 작성해주는 package 입니다.

### # JSX

React 를 이제 본격적으로 시작해보겠습니다. 리액트에는 **JSX**라는 특이한 문법을 가지고 있는데 이 syntax 에 대해 다뤄보겠습니다.

{% gist 90306ee3a10d7b8d5e762112f6aea996 %}

- <code>class</code>가 아닌 <code>className</code>이라고 사용하는 것이 올바른 convention 입니다.
- 스타일을 작성할 때도 객체로 작성하셔야합니다. 기존 css 에서 스타일 주는 것과 차이점은 property 를 대쉬(-)가 아닌 camelCase 로 작성하셔야합니다.
- 배열의 있는 값들을 반복문으로 순환할 때는 <code>map</code> 메소드를 사용하여 새로운 배열로 반환시킵니다. 주의할 점은 리액트에서 배열을 렌더링을 할 때 꼭 필요한 것이 <code>key</code>값입니다. <code>key</code>라는 고유한 값을 통하여 배열의 값이 변경될 때 성능 최적화를 시킬 수 잇습니다.
- jsx 문법에서 가장 좋은 점은 자바스크립트 문법을 자유롭게 사용할 수 있는 것입니다. 위에서 보신바와 같이 <code>&&</code> 연산자를 사용할 수 있고 삼항연산자를 사용할 수도 있습니다.
  예) <code>completed ? <b>success</b> : <del>fail</del></code>

* 컴포넌트에서 DOM 태그를 리턴시킬 때 항상 닫힌 태그여야하며 상위 Element 가 존재하여야합니다. 상위 Element 를 피하고 싶은 경우에는 <code>Fragment</code>를 이용하시기 바랍니다.

### # Props && State

이제부터는 리액트에서 가장 중요한 개념이라고 생각되는 **props** 와 **state** 에 대해 다뤄보겠습니다.

#### props

리액트에서 자식 컴포넌트로 값을 전달해줄때 props 를 이용합니다. 하지만 값을 전달만 할 뿐이지 직접 수정할 수는 없습니다.

{% gist b4aae8028d3e914145042cd3dcca4d1c %}

- <code>MyName</code> 이라는 컴포넌트를 만들어 <code>App</code> 컴포넌트에서 렌더링 시켜줍니다.

* <code>name</code>이라는 props 를 주게 되면 할당된 값이 전달됩니다. 하지만 props 를 전달해주지 않으면 <code>defaultProps</code>에 미리 정의된 값이 노출됩니다.

* 함수형 컴포넌트로 만들어서 렌더링 시킬 경우 미세하게나마 성능이 향상됩니다.

#### state

컴포넌트 내부에서 state 값을 선언할 수 있으며 수정할 수 있습니다.

{% gist 787953947e4a160fbf15a7860cf1c9e3 %}

- state 값을 선언하고 나서 값을 수정하고 싶은 경우 항상 <code>setState</code>함수를 이용하여 값을 변경해야합니다.
- Arrow function 을 사용해서 함수를 생성한다면 이벤트에 bind 할 때 this 문제를 해결할 수 있습니다.

### # LifeCycle API

아래 코드는 velopert 님 강의에서 나온 코드 위주로 정리한 **LifeCycle API** 자료입니다.

{% gist 39076d4bfd844b863647c033989467b0 %}

### Wrap-up

Velopert 님의 인프런 강의 내용이 워낙 알차다보니 정리하고 넘어가면 좋을것 같아서 포스팅하게 되었습니다. 기초로 돌아가 공부하다 보면 프로젝트 진행하면서 제가 아쉽게 짜던 코드들이 기억이 남습니다. 예를 들면 LifeCycle API 를 잘 사용했다면 좀 더 쉽게 구현할 수 있었을텐데 하는 아쉬움 같은거 말이죠?ㅋㅋㅋ

### Reference

[누구든지 하는 리액트: 초심자를 위한 리액트 핵심 강좌](https://www.inflearn.com/course/react-velopert/)
