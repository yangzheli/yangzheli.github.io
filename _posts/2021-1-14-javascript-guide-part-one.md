---
title: JavaScript 指南 -- Part I
categories: [frontend]
comments: true
---

JavaScript 指南总共分为两部分：

- [Part I](/2021-01/javascript-guide-part-one) 主要讲 JavaScript 的核心

- [Part II](/2021-04/javascript-guide-part-two) 主要讲浏览器中的 JavaScript

## 简介

## 数据类型

### 原始类型

ECMAScript 有 6 种原始类型：String、Number、Boolean、Undefined、Null、Symbol

- String、Number、Boolean 有包装类型 (特殊的对象)
- Undefined 只有一个值即 undefined，undefined 指变量声明了但未初始化
- Null 只有一个值即 null，null 是一个空对象指针 (typeof of null 返回 object)，同时 null 也是对象原型链的终点
- Undefined 转 Number 结果为 NaN，Null 转 Number 结果为 0
- Symbol 主要用来定义对象的唯一属性名，使用 Symbol() 函数进行初始化

### 引用类型

除了上述 6 种原始类型外，ECMAScript 还有一种引用类型：Object (数组和函数也是特殊的对象)

### 数据类型判断

- typeof 

- instanceof

## 变量声明与作用域

### 变量声明

ECMAScript 中可以使用 var、let 和 const 这 3 个关键字声明变量

### 变量作用域

### 闭包

### 执行上下文

## 对象

## 继承

## 类

## 函数

### 函数声明

### 箭头函数

### 函数柯里化

Currying

### 尾递归与优化

## Proxy 和 Reflect 

## Promise

## iteration 和 generators

## 模块

## 正则表达式

[《JavaScript 正则表达式及其优化》](/2021-03/regular-expression)

## 参考

**下一篇**

&#x23E9; [《JavaScript 指南 -- Part II》](/2021-04/javascript-guide-part-two)