---
title: '网课项目互动连麦功能开发记录'
date: 2022-07-14 16:45:16
tags: [项目总结]
published: true
hideInList: false
feature: 
isTop: false
---
- 腾讯的trtc文档有很多坑，比如引入的库要用在vue项目里需要挂在在window对象上面才会对库里的方法生效
![](https://blog.shaoyunxiang.cn/post-images/1657788346778.png)

- 很多方法的调用有个先后顺序的问题都要放在promise的then后面处理，文档没有标明也没有用async和await的形式
![](https://blog.shaoyunxiang.cn/post-images/1657788367764.png)

- 拉一路或者两路流的时候出现 can not read proporty 'sdp' of null
[SDP解释链接](https://time.geekbang.org/column/article/111337)
SDP（Session Description Protocal）说直白点就是用文本描述的各端（PC 端、Mac 端、Android 端、iOS 端等）的能力。这里的能力指的是各端所支持的音频编解码器是什么，这些编解码器设定的参数是什么，使用的传输协议是什么，以及包括的音视频媒体是什么等等
-- ps：升级trtc的sdk 从4.7.0到4.7.1就好了

- 本地的样式是好的，到了测试环境构建后的样式乱了，原因是因为没有加scoped作用域，其他组件的样式放到全局样式去影响到了当前组件了，所以全局样式不要放在子组件里，单独放一个全局的css，子组件必须要加作用域

- 老师停止和学生连麦后，学生端不仅要断开推流，如果不切换学生的角色，则腾讯云管理后台还会一直有学生的流