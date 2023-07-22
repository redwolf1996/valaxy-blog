---
title: 'css如何传递变量给js'
date: 2020-11-02 15:43:27
tags: [web前端]
published: true
hideInList: false
feature: 
isTop: false
---
>**主要还是用过webpack黑魔法实现**
```
// var.scss
$theme: blue;

:export {
  theme: $theme;
}
```
```
// test.js
import variables from '@/styles/var.scss'
console.log(variables.theme) // blue
```
<!-- more -->
