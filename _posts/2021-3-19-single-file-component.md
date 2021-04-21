---
title: Vue Loader 使用 -- 单文件组件构建
categories: [frontend]
comments: true
---

## 简介

> 使用 Vue.component 定义全局组件在很多中小型项目中运行的很好，但在更复杂的项目中，下面这些缺点将会非常明显：
>
> - 全局定义
> - 字符串模板
> - 不支持 CSS
> - 没有构建步骤
>
> 文件扩展名为 .vue 的单文件组件 (SFC) 为以上问题提供了解决方案

.vue 文件包含三种类型的块 `<template> <script> <style>`，还允许添加自定义块

一个文件名为 HelloWorld.vue 的简单实例：

```vue
<template>
  <div>{% raw %}{{ msg }}{% endraw %}</div>
</template>

<script>
export default {
  data: function() {
    return {
      msg: "Hello world"
    };
  }
};
</script>

<style>
div {
  color: red;
}
</style>
```

## 构建

构建单文件组件，需要通过 Vue Loader 手动配置 Webpack

### 安装依赖

```
npm install webpack webpack-cli --save-dev
npm install vue vue-loader vue-template-compiler --save-dev
npm install babel-loader css-loader --save-dev
```

### 配置 Webpack

创建以下目录结构、文件和内容:

```
|-- vue-loader-demo
    |-- index.html
    |-- package-lock.json
    |-- package.json
    |-- webpack.config.js
    |-- src
        |-- App.vue
        |-- main.js
```

**webpack.config.js**

```javascript
const path = require("path");
const VueLoaderPlugin = require("vue-loader/lib/plugin");

module.exports = {
  mode: "development",
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "build.js"
  },
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: "vue-loader"
      },
      {
        test: /\.js$/,
        loader: "babel-loader"
      },
      {
        test: /\.css$/,
        use: ["vue-style-loader", "css-loader"]
      }
    ]
  },
  plugins: [new VueLoaderPlugin()]
};
```

**package.json**

添加 npm scripts

```json
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "start": "webpack serve --open",
  "build": "webpack"
}
```

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <div id="app"></div>

    <script type="module" src="./dist/build.js"></script>
  </body>
</html>
```

**src/main.js**

```javascript
import Vue from "vue";
import App from "./App.vue";

new Vue({
  el: "#app",
  render: h => h(App)
});
```

**src/App.vue**

```vue
<template>
  <div>{% raw %}{{ msg }}{% endraw %}</div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      msg: "Hello world"
    };
  }
};
</script>

<style scoped>
div {
  color: red;
}
</style>
```

### 成功输出

![hello-world](../assets/img/single-file-component/hello-world.png)

## 处理图片/文件

下面以图片为例介绍 Vue Loader 如何处理资源路径

### 配置 Webpack

配置 Webpack 使用 file-loader 或 url-loader 进行处理

**webpack.config.js**

新增规则

```javascript
rules: [
  {
    test: /\.(png|jpg|gif)$/i,
    loader: "url-loader"
  }
];
```

**src/App.vue**

引用资源 URL

```vue
<template>
  <div>
    <span>{% raw %}{{ msg }}{% endraw %}</span>
    <img src="./assets/luffy.jpg" />
  </div>
</template>
```

### 报错

控制台提示 [object%20Module] 错误，原因是 Vue Loader 将遇到资源的 URL 转换为 Webpack 模块请求，即 `require('xxx.png')` ，而 file-loader 或 url-loader 默认采用 ECMAScript 模块语法，即 import 'xxx.png'，二者不一致

可以通过将 url-loader 中 esModule 属性设置为 false，关闭 ECMAScript 模块语法解决该问题

**webpack.config.js**

修改后

```javascript
rules: [
  {
    test: /\.(png|jpg|gif)$/i,
    use: [
      {
        loader: "url-loader",
        options: {
          esModule: false
        }
      }
    ]
  }
];
```

### 图片成功显示

![luffy](../assets/img/single-file-component/luffy.png)

## 使用 CSS 预处理器

CSS 预处理器增加了一些原生 CSS 不具备的特性，在 Webpack 中，使用预处理器需要匹配对应的 loader，主流预处理器有 Sass、Less、Stylus 和 PostCSS。下面以 Sass 为例介绍如何在 Webpack 中使用预处理器：

### 安装依赖

首先需要安装 node-sass 和 sass-loader

```
npm install node-sass sass-loader --save-dev
```

安装太慢可以使用淘宝镜像

```
npm install -g mirror-config-china --registry=http://registry.npm.taobao.org
npm install node-sass sass-loader --save-dev
```

注意：安装 node-sass 模块时需要 [node-gyp](https://github.com/nodejs/node-gyp) 进行编译，因此如果因缺少环境报错则需要额外进行安装和配置，详情见 [node-gpy 的安装](https://github.com/nodejs/node-gyp#installation)

### 配置 Webpack

**webpack.config.js**

新增规则

```javascript
rules: [
  {
    test: /\.scss$/,
    use: ["vue-style-loader", "css-loader", "sass-loader"]
  }
];
```

### 成功

配置 Webpack 后就可以在 Vue 组件中使用 Scss 块，示例：

```vue
<style lang="scss" scoped>
$hightlight-color: #f90;

.container {
  font-size: 1em;

  &:hover {
    background: $hightlight-color;
  }

  button {
    color: $hightlight-color;
  }
}
</style>
```

## 参考

[Vue Loader](https://vue-loader.vuejs.org/zh/)

[Webpack](https://webpack.docschina.org/guides/)
