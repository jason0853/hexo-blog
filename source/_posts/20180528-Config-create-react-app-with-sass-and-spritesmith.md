---
title: Config create-react-app with sass and spritesmith
tags:
  - React
  - Sass
  - Spritesmith
thumbnail: ../../../../images/react/react-logo.png
categories:
  - Front-end
  - React
date: 2018-05-28 14:58:57
---


![](../../../../images/react/react-logo.png)

이번에 웹앱 프로젝트를 진행하면서 css 모듈화와 이미지 자동화를 위해 **Sass**와 **Spritesmith**를 페이스북에서 제공하는 **create-react-app**과 어떻게 사용하는지 알아보겠습니다.

### # Create-react-app

우선 기본 셋팅을 하기 위해 폴더를 생성하고 생성된 폴더 안에서 **create-react-app**을 사용하여 리액트 프로젝트를 생성해보겠습니다.

``` shell
$ mkdir create-react-app-for-sass-and-spritesmith && cd create-react-app-for-sass-and-spritesmith
$ npx create-react-app .
$ yarn eject
```

* <code>yarn eject</code>를 실행하면 Are you sure you want to eject? This action is permanent. 이런 메세지가 나옵니다. <code>y</code>를 입력해주시면 webpack을 customize 할 수 있게 config 폴더가 생성됩니다.

### # How to config sass with webpack

**Sass** 환경을 위해 필요한 package들을 <code>yarn</code>을 통해 설치해보겠습니다.

``` shell
$ yarn add node-sass sass-loader
```

* node-sass: sass 파일들을 css파일로 변환해주는 역할을 합니다.
* sass-loader: webpack에서 sass 파일들을 읽어오는 역할을 합니다

``` js paths.js
// ...(중략)...
module.exports = {
  // ...(중략)...
  styles: resloveApp('src/styles')  //  styles 폴더 경로 지정
}
```

* scss 모듈들을 쉽게 불러올 수 있도록 styles 폴더를 루트로 지정하였습니다. 이 모듈을 불러와서 webpack에서 설정하겠습니다.

``` js webpack.config.dev.js
// ...(중략)...
{
  test: /\.css$/,
  // ...(중략)...
},
{
  test: /\.scss$/,
  use: [
    require.resolve('style-loader'),
    {
      loader: require.resolve('css-loader'),
      options: {
        importLoaders: 1,
        sourceMap: true
      },
    },
    {
      loader: require.resolve('postcss-loader'),
      options: {
        // Necessary for external CSS imports to work
        // https://github.com/facebookincubator/create-react-app/issues/2677
        ident: 'postcss',
        plugins: () => [
          require('postcss-flexbugs-fixes'),
          autoprefixer({
            browsers: [
              '>1%',
              'last 4 versions',
              'Firefox ESR',
              'not ie < 9', // React doesn't support IE8 anyway
            ],
            flexbox: 'no-2009',
          }),
        ],
      },
    },
    // sass-loader 추가
    {
      loader: require.resolve('sass-loader'),
      options: {
        // path 추가
        includePaths: [paths.styles]
      }
    }
  ],
},
```

* 기존 webpack.config.dev.js 안에 있는 css 설정을 바로 밑에 복사한 다음 <code>/&#92;.scss$/</code>로 확장자를 바꿔줍니다.
  * <code>style-loader</code>: style 태그를 injecting 시킴으로써 DOM에 css를 추가해줍니다.
  * <code>css-loader</code>: <code>@import</code>와 <code>url()</code>을 <code>import</code>와 <code>require()</code>와 같이 해석하여 해결해줍니다.
  * 우리가 입력한 css 코드에 prefix(<code>-webkit-</code><code>-mos-</code><code>-ms-</code>)를 자동으로 붙여줍니다.
* 개발하면서 css 디버깅을 위해 options에 <code>sourceMap: true</code> 코드를 추가해줍니다.
* 맨 마지막으로 <code>sass-loader</code> 설정을 해주고 options에 scss 모듈들을 쉽게 import 할 수 있도록 경로 설정을 추가해줍니다.


``` js webpack.config.prod.js
// ...(중략)...
{
  test: /\.css$/,
  // ...(중략)...
},
{
  test: /\.scss$/,
  loader: ExtractTextPlugin.extract(
    Object.assign(
      {
        fallback: {
          loader: require.resolve('style-loader'),
          options: {
            hmr: false,
          },
        },
        use: [
          {
            loader: require.resolve('css-loader'),
            options: {
              importLoaders: 1,
              minimize: true,
              sourceMap: shouldUseSourceMap,
            },
          },
          {
            loader: require.resolve('postcss-loader'),
            options: {
              // Necessary for external CSS imports to work
              // https://github.com/facebookincubator/create-react-app/issues/2677
              ident: 'postcss',
              plugins: () => [
                require('postcss-flexbugs-fixes'),
                autoprefixer({
                  browsers: [
                    '>1%',
                    'last 4 versions',
                    'Firefox ESR',
                    'not ie < 9', // React doesn't support IE8 anyway
                  ],
                  flexbox: 'no-2009',
                }),
              ],
            },
          },
          {
            loader: require.resolve('sass-loader'),
            options: {
              // path 추가
              includePaths: [paths.styles]
            }
          },
        ],
      },
      extractTextPluginOptions
    )
  ),
  // Note: this won't work without `new ExtractTextPlugin()` in `plugins`.
},
```

* webpack.config.prod.js 파일 또한 기존 css 설정을 바로 밑에 복사한 다음 <code>/&#92;.scss$/</code>로 확장자를 바꿔줍니다.

### # How to config spritesmith with webpack

이번엔 이미지 자동화를 위해 spritesmith를 webpack에 적용해보겠습니다.

``` shell
$ yarn add webpack-spritesmith
```

``` js env.js
// ...(중략)...
// spritesmith plugin 설정
  const configSprite = {
    src: {
      cwd: path.resolve(__dirname, '../src/assets/ico'),
      glob: '*.png'
    },
    target: {
      image: path.resolve(__dirname, '../src/assets/sprites/sprite.png'),
      css: path.resolve(__dirname, '../src/styles/sprite.scss')
    },
    retina: '@2x',
    apiOptions: {
      cssImageRef: "~sprite.png"
    },
  };
  
  return { raw, stringified, configSprite };
}

module.exports = getClientEnvironment;
```

* config 폴더 안에 있는 env.js에서 미리 spritesmith 관련 환경설정을 미리 해줍니다.
* <code>retina: '@2x'</code> 옵션은 sprite@2x.png를 생성시켜줍니다.

``` js webpack.config.dev.js
// ...(중략)...
const SpritesmithPlugin = require('webpack-spritesmith');
  // ...(중략)...
  resolve: {
    // This allows you to set a fallback for where Webpack should look for modules.
    // We placed these paths second because we want `node_modules` to "win"
    // if there are any conflicts. This matches Node resolution mechanism.
    // https://github.com/facebookincubator/create-react-app/issues/253
    // sprites 폴더 경로 추가
    modules: ['node_modules', paths.appNodeModules, 'assets/sprites'].concat(
      // It is guaranteed to exist because we tweak it in `env.js`
      process.env.NODE_PATH.split(path.delimiter).filter(Boolean)
    ),
  // ...(중략)...
  plugins: [
    // ...(중략)...
    // SpritesmithPlugin 추가
    new SpritesmithPlugin(env.configSprite)
  ]
```

* <code>yarn</code>으로 설치한 <code>webpack-spritesmith</code> 플러그인을 불러와 webpack에 설정을 해줍니다.
* <code>'assets/sprites'</code>를 modeuls 속성 배열안에 추가해준 이유는 모듈(~sprite.png) 형태로 불러오는것을 해결해주기 위함입니다.

``` js webpack.config.prod.js
// ...(중략)...
  resolve: {
    // This allows you to set a fallback for where Webpack should look for modules.
    // We placed these paths second because we want `node_modules` to "win"
    // if there are any conflicts. This matches Node resolution mechanism.
    // https://github.com/facebookincubator/create-react-app/issues/253
    // sprites 폴더 경로 추가
    modules: ['node_modules', paths.appNodeModules, 'assets/sprites'].concat(
      // It is guaranteed to exist because we tweak it in `env.js`
      process.env.NODE_PATH.split(path.delimiter).filter(Boolean)
    ),
```

* production 레벨에서는 경로만 추가해줍니다.
* build를 해주기 전에는 <code>yarn start</code>를 한 뒤에 <code>yarn build</code>를 해주시기 바랍니다.

### Wrap-up

처음 환경 셋팅을 할 때 webpack-spritesmith 때문에 삽질을 많이 했었는데 이번에 이렇게 정리하고 나니 다음에는 copy and paste를 하면 될 것 같습니다. 전체 내용 코드는 아래 github repository에서 확인하실 수 있습니다.

{% githubCard user:jason0853 repo:create-react-app-custom width:100% %}

### Reference

[리액트 컴포넌트 스타일링](https://velopert.com/3447)
[webpack-spritesmith](https://www.npmjs.com/package/webpack-spritesmith)