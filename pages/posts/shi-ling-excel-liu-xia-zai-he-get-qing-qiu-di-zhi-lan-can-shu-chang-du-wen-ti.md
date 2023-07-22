---
title: '拾零：excel流下载和get请求地址栏参数长度问题'
date: 2020-09-24 18:34:51
tags: [web前端]
published: true
hideInList: false
feature: 
isTop: false
---
- **流文件下载**
1.下载文件可能的方式有两种，一种是请求接口，后端生成文件然后返回文件的地址，直接用window.open打开地址或者动态创建a标签去请求返回的链接

2.ajax请求后返回二进制的流文件，用new blob装载后再下载，不过有一个注意点是
请求体里面要有 responseType: 'arraybuffer' 或者 'blob'  这个选项

- **get请求地址栏长度问题**
(2020-09-24)实际的项目是百家云唤起客户端，在safari浏览器下地址栏参数过多长度过长会被截断一部分，引起客户端调用参数不全的问题，而chrome浏览器允许的长度显然比safari长
