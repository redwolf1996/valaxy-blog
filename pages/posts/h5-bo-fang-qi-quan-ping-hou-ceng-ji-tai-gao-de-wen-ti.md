---
title: 'H5播放器全屏后层级太高的问题'
date: 2020-11-17 15:19:48
tags: [web前端]
published: true
hideInList: false
feature: 
isTop: false
---
>腾讯云的播放器在点击自带的全屏控件或者点击时候调用H5的全屏API，video全屏后层级始终在最上面，导致一些自定义的界面如弹幕、互动答题等被覆盖
- **解决办法：**
微信内置浏览器(android: x5浏览器，ios:chrome浏览器)、chrome浏览器，先隐藏video里面的自带控件，然后手动写个全屏按钮，点击这个全屏按钮的时候手动让 width和height跟全屏的尺寸一致而不要调用H5提供的全屏API
```
ps: 某些浏览器仍然不能很好的解决兼容性问题
```