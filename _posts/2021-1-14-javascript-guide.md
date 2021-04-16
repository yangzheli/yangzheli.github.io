---
title: 前端总结 (1) -- JavaScript 指南
categories: [frontend]
comments: true
---

## 1. JavaScript 中数据类型有哪些？

(1) 原始类型：String、Number、Boolean、Undefined、Null、Symbol

- String、Number、Boolean 有包装类型 (特殊的对象)
- Undefined 只有一个值即 undefined，undefined 指变量声明了但未初始化
- Null 只有一个值即 null，null 是一个空对象指针 (typeof of null 返回 object)，同时 null 也是对象原型链的终点
- Undefined 转 Number 结果为 NaN，Null 转 Number 结果为 0
- Symbol 主要用来定义对象的唯一属性名，使用 Symbol() 函数进行初始化

(2) 对象/引用类型：Object (数组和函数也是特殊的对象)

## 2. 变量声明与作用域
