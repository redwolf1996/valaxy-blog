---
title: '浏览器检测视频编码格式的代码'
date: 2023-03-13 13:42:44
tags: [DIY]
published: true
hideInList: false
feature: 
isTop: false
---
```
const videoEle = document.querySelector('video');

if (MediaSource.isTypeSupported('video/mp4; codecs=av01.0.05M.08')) {  
    console.log('AV1')
} else if (videoEle.canPlayType('video/mp4; codecs="hevc"')) {
    console.log('H.265/HEVC')
} else {
    console.log('H.264/AVC')
}

```