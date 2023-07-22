---
title: 'yarn安装依赖网络问题'
date: 2022-07-14 17:04:16
tags: [web前端]
published: true
hideInList: false
feature: 
isTop: false
---
> 当用yarn命令安装依赖的时候报“there appears to be trouble with your network connection. Retrying...”
下面报了代理的问题那么可以删除代理（由于梯子软件自带的复制终端代理命令所致）, 命令如下:
- yarn config delete https-proxy  && yarn config delete proxy

>如果是npm安装则用npm命令删除相应的代理即可
- npm config rm proxy && npm config rm https-proxy
- ps: 能科学上网还是科学上网吧😇
  