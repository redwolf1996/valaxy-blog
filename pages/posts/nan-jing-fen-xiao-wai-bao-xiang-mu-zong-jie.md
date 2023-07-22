---
title: '南京分销外包项目总结'
date: 2020-09-08 19:58:23
tags: [项目总结]
published: true
hideInList: false
feature: 
isTop: false
---
- 本项目是采用uniapp开发的H5应用，本人负责了C端前端部分逻辑开发
- 改项目的起因是客户原来用的小鹅通平台续费到期，且有些东西客户想自定义，还有就是价格方面的因素
- uniapp有些值得肯定的东西，一个是多端覆盖，一个是unicloud目前处于公测免费的阶段，另外HbuilderX确实比较接地气，不过无论编辑器插件的生态还是项目插件的生态都不如vscode和vue，还有很多莫名其妙的bug有待修复
### 开发中遇到的坑
- **腾讯X5浏览器的确是个垃圾**
安卓手机在微信里看H5用的是X5内核，当时有个需求是记录视频播放的进度，用uniapp的字面量传参 和 api控制都不能很好的操控进度，后来只能用H5原生的方法去操作进度，而且安卓还要在ontimeupadte事件触发后才生效
```
      let myPlayer = document.getElementsByClassName('uni-video-video')[0]
      if (isiOS) {
            myPlayer.currentTime = ti
          } else {
            myPlayer.ontimeupdate = function () {
              if (setTimeFlag == 1) {
                myPlayer.currentTime = ti
                setTimeFlag = 2
              }
          }
      }
```
- **微信H5分享也很蛋疼**
问题在于H5端的微信不能原生的调起微信分享，必须用过自己弹出个引导让用户去右上角点分享，体验相当差
![](https://blog.shaoyunxiang.cn/post-images/1657784598795.webp)
![](https://blog.shaoyunxiang.cn/post-images/1657785019304.png)
当后端用easywechat这个库返回给前端权限信息的时候，默认是不需要设置当前url的，但不设置就会出错，必须手动设置：
![](https://blog.shaoyunxiang.cn/post-images/1657785037688.png)
由于前端的是一个vue单页应用，所以要这样把url传给后端：
![](https://blog.shaoyunxiang.cn/post-images/1657785055455.png)

- **生成海报**
![](https://blog.shaoyunxiang.cn/post-images/1657784997398.png)

需要的工具 一个是二维码生成工具，一个是html2canvas
生成的顺序必须等 头像、主图、二维码渲染完成之后再给跟id用html2canvas导出png或者jpg等，比较裂开的地方在于其中涉及到远程网络的图片，还必须要转换成本地的url才能成功合成文件，为此封装了一个promise用于转换网络图片到本地图片
```
    // 转换网络图片为本地图片
    transformNetImageToLocal(url) {
      return new Promise((resolve, reject) => {
        let canvas = document.createElement('canvas')
        let ctx = canvas.getContext('2d')
        let img = new Image()
        img.src = url
        img.crossOrigin = 'Anonymous' //设置image对象可跨域请求
        img.src = url + '?timeStamp=' + new Date().getTime() //解决缓存引起访问失败需要添加时间戳

        img.onload = () => {
          canvas.height = img.height
          canvas.width = img.width
          ctx.drawImage(img, 0, 0)
          const dataURL = canvas.toDataURL('image/png')
          resolve(dataURL)
        }
        img.onerror = reject
      })
    }
```
- **请求时间太长的问题**
manifest.json文件下面有个async选项，整个去掉即可，uniapp的坑

- **微信授权登陆一直来回跳转的问题**
window.location = 'xx.com'
location.href = 'yy.com'
location.reload()
等等诸如此类的表达式，实际只是一个普通的赋值或者函数执行，并不会中断下面代码的执行，所以跳转后一定要加上return结束后面的脚本运行
![](https://blog.shaoyunxiang.cn/post-images/1657785073774.png)
