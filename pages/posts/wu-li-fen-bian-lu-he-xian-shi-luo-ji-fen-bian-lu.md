---
title: '物理分辨率和显示(逻辑)分辨率'
date: 2020-09-14 09:02:26
tags: [计算机基础]
published: true
hideInList: false
feature: 
isTop: false
---
- 比如大家常说的4k还有前些年常讲的1080p。 这个其实就是说这个屏幕的物理分辨率有多少，再通俗一点就是说这款显示器硬件上有多少个像素小灯泡。
- 什么是逻辑上的分辨率呢？这个其实就是显示分辨率，指的是操作系统在渲染图像时，把这个显示当做 m*n的分辨率来渲染。 因此在这种情况下，我们看到3840是1920的两倍，因此我的操作系统会用显示器上2个物理上的像素点来当做一个像素点来用。

- 如果你注意过你的retina macpro，会发现你的retina mac-pro也不是按照他的实际物理分辨率来显示的。你可以使用截图的方式来查看下(前端同学也可以在浏览器里调用screen api查看)，我发现我的macpro的分辨率在1280*800 (retina 13寸 2015 early款)。 所以其实macbook pro的retina显示屏的逻辑像素和物理像素之间也是个2倍的关系。