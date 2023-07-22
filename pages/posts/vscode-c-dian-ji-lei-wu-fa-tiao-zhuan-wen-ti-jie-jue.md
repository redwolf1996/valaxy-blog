---
title: 'vscode c# 点击类无法跳转问题解决'
date: 2023-02-14 15:59:21
tags: [vscode]
published: true
hideInList: false
feature: 
isTop: false
---
1. 安装微软官方C#插件后，更改设置，打开反编译选项
![](https://blog.shaoyunxiang.cn/post-images/1676362979902.png)

2. 按 `ctrl/command + shift + p` 输入 「OmniSharp: Select Project」
3. 按理说如果项目只有一个sln文件，安装插件后会自动分析文件，但是如果有多个sln文件多个csproj文件那么我们需要手动指定，如下图所示：
![](https://blog.shaoyunxiang.cn/post-images/1676363285748.png)
选择后vscode的底部状态栏会出现下图所示的过程
![](https://blog.shaoyunxiang.cn/post-images/1676363391811.png)
等分析完毕就可以实现点击跳转了：
![](https://blog.shaoyunxiang.cn/post-images/1676363500358.png)
![](https://blog.shaoyunxiang.cn/post-images/1676363507602.png)
