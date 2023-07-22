---
title: 'Vue2中watch和computed异同'
date: 2022-07-19 09:06:37
tags: [web前端]
published: true
hideInList: false
feature: 
isTop: false
---
- ** 都是监测数据变化计算属性 **
- ** computed靠return返回值给出计算属性 不能在里面有任何的异步计算 但是代码相对精简**
- **  watch是手动直接去计算 可以插入异步任务(如：隔一段时间再输出计算结果) 但是代码相对繁琐 复杂对象还需要深度监听 但同时也有立即执行的选项可选 控制的更加精细 **
- ![](https://blog.shaoyunxiang.cn/post-images/1658193202435.png)