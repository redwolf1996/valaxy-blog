---
title: '直播客户端弹幕思路'
date: 2020-11-19 15:46:17
tags: [web前端]
published: true
hideInList: false
feature: 
isTop: false
---
>巨人直播客户端弹幕模块思路
- **接收弹幕自定义消息后插入弹幕数组**
- **采用css3动画left属性从calc(100%+n px)到100%**
- **随机计算飞入的弹幕距离顶部的top值，使之不与其他弹幕重合**