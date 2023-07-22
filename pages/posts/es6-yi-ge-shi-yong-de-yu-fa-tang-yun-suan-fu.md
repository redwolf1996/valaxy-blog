---
title: 'ES6--一个实用的语法糖运算符【？？】'
date: 2022-11-21 09:55:22
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
>  ** ?? 非空运算符(如果第一个参数不是 null/undefined, 将返回第一个参数，否则返回第二个参数) **
```
console.log(undefined ?? 5) // 5
console.log(null ?? 5) // 5
console.log(0 ?? 5) // 0
console.log(NaN ?? 5) // NaN
console.log(false ?? 5) // false
```

