---
title: '浏览器中 js使用require无效的问题'
date: 2020-04-13 11:30:34
tags: [web前端]
published: true
hideInList: false
feature: 
isTop: false
---
>**浏览器端的模块系统无法使用，不只是`require`了，即使是es6标准的`import`很多低版本的浏览器也无法支持，尤其是IE**
- 原因在于目前模块之间的依赖树的处理方式上还有一个明显的难题没有解决：
因为只有在浏览器完全下载完一个js文件，并且宿主引擎解析到`require`或`import`这些关键字的时候，才知道还有依赖需要下载并解析。然而该文件依赖的这个模块可能还依赖于其他模块，理论上依赖树可以有无限长，目前这种依赖的同步加载方式**会带来严重的进程阻塞和极高的网络开销**
- 目前并没有很好的解决方案使得浏览器端自然的使用各个模块系统，只能使用webpack等工具预先将所有的依赖打包，最终在浏览器换进中运行