---
title: "angular2开发环境搭建"
layout: page
date: 2016-06-21 00:00
base: target="_blank"
---

[TOC]

### 创建项目
```shell
mkdir environment
cd environment
npm init
```

根据npm init提问，创建package.json文件，创建后去掉不必要的字段，像这样即可：

```json
{
  "name": "environment",
  "version": "1.0.0",
  "description": "I will show you how to set up angular2 development environment",
  "keywords": [
    "angular2",
    "environment"
  ],
  "author": "Howard.Zuo",
  "license": "MIT"
}
```

### 安装运行时依赖
```shell
npm install --save --save-exact @angular/common@2.0.0-rc.2 @angular/core@2.0.0-rc.2 @angular/forms@0.1.0 @angular/http@2.0.0-rc.2 @angular/compiler@2.0.0-rc.2 @angular/platform-browser@2.0.0-rc.2 @angular/platform-browser-dynamic@2.0.0-rc.2 @angular/router@3.0.0-alpha.7 @angular/router-deprecated@2.0.0-rc.2 @angular/upgrade@2.0.0-rc.2 es6-shim reflect-metadata rxjs@5.0.0-beta.6 zone.js core-js bootstrap
```

> * @angular: angular包,可以按需 引入
> * es6-shim: angular2依赖了大量ES2015的特性，这可能导致一些不支持ES2015特性的浏览器无法运行angular2程序(例如：老版本IE)。所以需要该shim来保证老浏览器的正确
> * reflect-metadata: angular2允许开发者使用Decorator，这使得程序具备更好的可读性。无奈Decorator是ES2016里的提案，需要reflect-metadata提供反射API才能使用
> * rxjs: 一个Reactive Programming的JavaScript实现。这里对她的依赖是因为angular2支持多种数据更新模式，比如：flux、Rx.js
> * zone.js: 用来对异步任务提供Hooks支持，使得在异步任务运行之前/之后做额外操作成为可能。在angular2里的主要应用场景是提高脏检查效率、降低性能损耗。


### 安装开发环境依赖

```shell
npm install --save-dev webpack ts-loader typescript lite-server concurrently typings http-proxy-middleware
```
> * webpack: 我们这里使用webpack对源码进行编译、打包，而不是用官网介绍的System.js的运行时加载、解释、执行。合并打包的好处不用我多说吧？减少请求数、uglify、预检查...
> * ts-loader: TypeStrong出品的TypeScript加载器，通过该加载器，TypeScript源码可以顺利被编译成ES5代码
> * typescript: angular2官方推荐的开发语言，我们在教程里也将使用该语言进行代码编写
> * lite-server: 一个轻量级的静态服务器，本章节我们就用它启动程序
> * concurrently: 这是一个可以让多个阻塞命令同时执行、管理的工具。我们将在后面用到
> * typings: typescript定义文件管理系统，由于angular2依赖ES2015的诸多特性，譬如：Promise等，所以需要这些API的支持以及typescript定义
> * http-proxy-middleware: 用于在发送接口请求时做代理转换

### 创建index.html
```shell
touch index.html
```

向刚才创建的index.html里添加如下内容：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>environment</title>
</head>
<body>
    <!--这里引用我们的第一个component-->
    <my-app></my-app>
    <!--加载使用webpack编译后的bundle-->
    <script type="text/javascript" src="/dist/bundle.js"></script>
</body>
</html>
```

### 创建app.ts
```shell
mkdir ts
touch ts/app.ts
```
向刚才创建的ts/app.ts里添加如下内容：
```javascript
import {Component} from '@angular/core';

//声明第一个component
@Component({
    selector: 'my-app',
    template: '<h1>My First Angular 2 App</h1>'
})
export class AppComponent { }
```

### 创建index.ts
```shell
touch ts/index.ts
```
向刚才创建的ts/index.ts里添加如下内容：
```javascript
import 'es6-shim';
import 'reflect-metadata';
import 'zone.js/dist/zone';
import {bootstrap} from '@angular/platform-browser-dynamic';

//引用上一步创建的app.ts
import {AppComponent} from './app';

//启动程序
bootstrap(AppComponent);
```

### 创建webpack.config.js
```shell
touch webpack.config.js
```

```javascript
var path = require('path');

module.exports = {
    entry: {
        index: './ts/index.ts'
    },
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js',
        publicPath: 'dist/'
    },
    module: {
        loaders: [
            {
                test: /\.ts$/,
                loader: 'ts'
            }
        ]
    },
    resolve: {
        extensions: [
            '',
            '.js',
            '.ts'
        ]
    }
};
```

### 创建tsconfig.json
```shell
touch tsconfig.json
```
向刚才创建的tsconfig.json里添加如下内容：

```json
{
    "compilerOptions": {
        "noImplicitAny": true,
        "removeComments": true,
        "module": "commonjs",
        "target": "es5",
        "moduleResolution": "node",
        "emitDecoratorMetadata": true,
        "experimentalDecorators": true,
        "sourceMap": true,
        "declaration": false
    },
    "buildOnSave": false,
    "compileOnSave": false,
    "exclude": [
        "node_modules"
    ],
    "atom": {
        "rewriteTsconfig": true,
        "formatOnSave": true
    }
}
```

### 创建typings.json
```shell
touch typings.json
```
向刚才创建的typings.json里添加如下内容：
```json
{
  "globalDependencies": {
    "es6-collections": "registry:dt/es6-collections#0.5.1+20160316155526",
    "es6-promise": "registry:dt/es6-promise#0.0.0+20160423074304"
  }
}
```

### 创建bs-config.js
该文件为http-proxy-middleware的配置文件,可添加代理转发,以下为配合lite-server的写法,其他的可参考以下链接:
```javascript
var proxyMiddleware = require('http-proxy-middleware');

module.exports = {
    server: {
        middleware: {
            1: proxyMiddleware('/news-at-api', {
                target: 'http://news-at.zhihu.com',
                changeOrigin: true,   // for vhosted sites, changes host header to match to target's host,
                pathRewrite: {
                '^/news-at-api': '/api'
                }
            })
        }
    }
};

```
可参考配置:

[Overview of http-proxy-middleware implementation in different servers](https://github.com/chimurai/http-proxy-middleware/blob/master/recipes/servers.md#lite-server)

[lite-server issues - proxy](https://github.com/johnpapa/lite-server/issues/61)

[npm - http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware)

### 为package.json增加常用命令
向package.json中，增加scripts属性，内容如下：

```json
"scripts": {
  "preinstall": "tsd install",
  "watch": "webpack -w",
  "start": "concurrently \"npm run watch\" \"lite-server\""
}
```

> * preinstall: 执行npm install之前自动执行该脚本，将ES2015的定义文件下载下来
> * watch: 使用npm run watch调用。编译、打包源码，并持续监视源码变动，一旦你做了改动，自动重新编译、打包
> * start: 使用npm start调用。这里用到了之前提到的concurrently包，她使的一条命令可以同时执行两个阻塞操作npm run watch以及lite-server

### 运行
```shell
npm start
```

### 环境配置参考链接:
[Angular2 Quickstart](https://angular.io/docs/ts/latest/quickstart.html){:target="_blank}

[angular2初入眼帘之－搭个环境](https://segmentfault.com/a/1190000004930079){:target="_blank"}
