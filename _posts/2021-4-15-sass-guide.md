---
title: SASS 指南
categories: [frontend]
comments: true
---

## 简介

上文 [《CSS 指南》](/2021-03/css-guide) 中介绍了 CSS 的相关知识，在写 CSS 时常常会因为相同的代码而苦恼，CSS 预处理器为原生 CSS 增加了一些不具备的特性，能够让 CSS 的结构更加可读和易于维护。[Sass](https://sass-lang.com/) 是目前最主流的 CSS 预处理器之一，它在 CSS 的基础上增加了变量、嵌套、混合、导入等高级功能。

&#x1F449; [官方文档](https://sass-lang.com/documentation)

## 语法格式

Sass 有两种语法格式：

- Scss (Sassy CSS)，以 `.scss` 作为拓展名，更加常见

```scss
$hightlight-color: #f90;

.container {
  font-size: 1em;

  &:hover {
    background: $hightlight-color;
  }
}
```

- Sass (Indented CSS)，以 `.sass` 作为拓展名，与 Scss 不同的是它使用**缩进**代替 `{}`，使用**换行**代替 `;`

```sass
$hightlight-color: #f90

.container
  font-size: 1em

  &:hover
    background: $hightlight-color
```

## 使用

### HTML 文件中使用

先安装 Ruby，Ruby 安装完成后使用命令行安装 Sass

```
gem install sass
```

再使用命令行将编写好的 .scss 文件编译成 .css 文件，并在 HTML 文件中引入编译后的 .css 文件

```
sass index.scss index.css
```

或监听 sass 文件，修改保存后自动编译

```
sass --watch index.scss index.css
```

### Webpack 中使用

详见 [《Vue Loader 使用 -- 单文件组件构建》](/2021-03/single-file-component#使用-css-预处理器)

## 变量

Sass 使用 **`$`** 来标识变量，示例：

```scss
$hightlight-color: #f90;

#main {
  color: $hightlight-color;
}
```

变量作用域：

- 局部变量：变量定义在规则块内，只能在该规则块内使用 (可以添加 !global 声明将局部变量转为全局变量)

- 全局变量：变量定义在规则块外，可以在任何地方使用

同时可以使用插值语句 **`#{}`** 在选择器或属性名中使用变量，示例：

```scss
$attr: border;

#main {
  #{$attr}: 1px solid #f90;
}
```

## 嵌套

Sass 允许将 CSS 样式嵌套进另一个样式中，避免重复输入父选择器

### 父选择器 &

有时也需要直接使用嵌套外层的父选择器，使用 **`&`** 代表嵌套外层的父选择器

```scss
a {
  &:hover {
    text-decoration: underline;
  }
}
```

### 属性嵌套

像 border-style、border-width 之类的属性也可以进行嵌套

```scss
#main {
  border: {
    width: 3px;
    style: solid;
    color: #f90;
  }
}
```

## 注释

除了支持 CSS 的多行注释 **`/* */`** 外，Sass 还提供单行注释 **`//`** 

多行注释 **`/* */`** 会被完整保留到编译后的 CSS 文件中，单行注释 **`//`** 编译后则会被抹去

将 **`!`** 作为多行注释第一个字符时，即使是压缩模式，编译后该条注释也将被保留

## 混合 (mixins)

混合器使用 **`@mixin`** 标识符定义，使用 **`@include`** 标识符来引入混合样式

## 继承

使用 **`@extend`** 来继承另一个选择器的样式

## 参考

[Sass: Documentation](https://sass-lang.com/documentation)
