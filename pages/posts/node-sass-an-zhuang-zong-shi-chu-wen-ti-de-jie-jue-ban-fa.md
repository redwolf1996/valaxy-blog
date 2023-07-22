---
title: 'node-sass安装总是出问题的解决办法'
date: 2022-07-14 17:03:06
tags: [web前端]
published: true
hideInList: false
feature: 
isTop: false
---
1. 翻墙
2. 用 dart-sass代替node-sass
3. 如果不想更换package.json内的node-sass包名 又想将其替换成node-sass 可以使用如下的办法
yarn add --dev node-sass@npm:dart-sass
![](https://blog.shaoyunxiang.cn/post-images/1657789430443.png)

<!-- more -->
