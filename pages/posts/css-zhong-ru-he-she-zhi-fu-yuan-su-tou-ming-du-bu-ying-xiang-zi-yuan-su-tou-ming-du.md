---
title: 'CSS中如何设置父元素透明度不影响子元素透明度'
date: 2022-07-14 16:51:50
tags: [web前端]
published: true
hideInList: false
feature: 
isTop: false
---
- 原因分析： 使用css的opcity属性改变某个元素的透明度，但是其元素下的子元素的透明度也会被改变，即便重定义也没有用，不过有个方法可以实现，大家可以看看。

可以使用一张透明的图片做背景可以达成效果，但是有没有更简单的方法呢？使用RGBA。
例如：
设置父元素opacity：0.5，子元素不设置opacity，子元素会受到父元素opacity的影响，也会有0.5的透明度。

即使设置子元素opacity：1，子元素的opacity：1也是在父元素的opacity：0.5的基础上设置的，因此子元素的opacity还是0.5。

> *解决方法：为父元素设置background: rgba(0,0,0,0.5)。* 