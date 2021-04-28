---
title: 贝塞尔曲线介绍
categories: [frontend]
comments: true
---

## 简介

贝塞尔曲线是 1962 年法国数学家 Pierre Bezier 研究发现的，它是依据四个任意位置点坐标绘制出来的一条光滑曲线。

## 三阶贝塞尔曲线

## CSS 中的贝塞尔曲线

CSS 中使用的贝塞尔曲线为三阶贝塞尔曲线，由四个点 P0、P1、P2、P3 确定，其中 P0 (0, 0) 是曲线的起点，P3 (1, 1) 是曲线的终点，由于它们位置固定，所以该贝塞尔曲线由 P1、P2 坐标决定 cubic-bezier(x1, y1, x2, y2)

## 参考

- [CSS 贝塞尔曲线可视化](https://cubic-bezier.com/)