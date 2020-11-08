---
title: 使用webpack打包基础库并发布至npm
tags:
  - webpack
author: GleanWu
---

webpack 除了打包应用，也可以用来打包自己写的一些 js 库

> 单纯只是打包 js 库和组件库的话 使用 rollup 打包也是一个不错的选择。

我们来实现一个简单的打包例子，这个例子需要满足以下几点功能:

- 需要支持打包压缩版(x.min.js)和非压缩版本(x.js)

- 支持 AMD/CJS/ESM 模块引入

- 支持通过 script 脚本直接引入链接

```js

// ESM
import HiRequest Tool from 'hi-request';

//cjs
const HiRequest = require('hi-request');

// AMD
require(['hi-request'],function(){
  ...
})

// script 脚本
<script src="https://xxx.com/hi-request.min.js"></script>

```

## 如何将库暴露出去？

- [library](https://webpack.docschina.org/guides/author-libraries/#expose-the-library)：指定库的全局变量

- [libraryTarget](https://webpack.docschina.org/configuration/output/)：支持库引入的方式

```js
 output: {
    filename: '[name].js',
    library: 'HiRequest',
    libraryTarget: 'umd',
    libraryExport: 'default'
  },
```

### 1. 构建项目

```sh
mkdir hi-reqest

cd hi-reqest

npm init -y

npm i webpack webpack-cli
```

### 2.新建目录 src/index.js,编写我们的工具代码

```js
export function request(el) {
  // 自己随便写点代码
  console.log('发出请求')
}
```

### 3. 安装[terser-webpack-plugin](https://github.com/webpack-contrib/terser-webpack-plugin) 压缩插件

**用 terser-webpack-plugin 替换掉 uglifyjs-webpack-plugin 解决 uglifyjs 不支持 es6 语法问题**

```sh
npm i terser-webpack-plugin -D
```

### 4. 新建 webpack.config.js

```js
const TerserPlugin = require('terser-webpack-plugin')

module.exports = {
  entry: {
    'hi-request.min': './src/index.js'
  },
  output: {
    filename: '[name].js',
    library: 'HiRequest',
    libraryTarget: 'umd',
    libraryExport: 'default'
  },
  mode: 'none',
  optimization: {
    minimize: true,
    minimizer: [
      new TerserPlugin({
        terserOptions: {
          output: {
            comments: false
          }
        },
        extractComments: false,
        include: /\.min\.js$/
      })
    ]
  }
}
```

### 5. 修改 package.json 添加打包命令

```js
{
  "name": "hi-request",
  "version": "1.0.0-rc.1",
  "description": "Promise based HTTP client for the browser",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "prepublish": "webpack"
  },
  "repository": {
    "type": "git",
    "url": "git@github.com:wugaoliang1116/hi-request.git"
  },
  "keywords": [
    "react",
    "request"
  ],
  "author": "HiUI-team",
  "license": "ISC",
  "dependencies": {
    "axios": "^0.18.0",
    "js-cookie": "^2.2.1"
  },
  "devDependencies": {
    "terser-webpack-plugin": "^1.4.5",
    "webpack": "^5.1.3",
    "webpack-cli": "^4.0.0"
  }
}


```

### 6.设置入口文件 不同环境下使用不同的入口文件

```js
// index.js
if (process.env.NODE_ENV === 'production') {
  // 通过环境变量来决定入口文件
  module.exports = require('./dist/hi-request.min.js')
} else {
  module.exports = require('./dist/hi-request.js')
}
```

### 7. 发布

```sh
npm publish // 这个步骤可能要登录  npm login
```

完整代码参见[github](https://github.com/wugaoliang1116/hi-request)地址
