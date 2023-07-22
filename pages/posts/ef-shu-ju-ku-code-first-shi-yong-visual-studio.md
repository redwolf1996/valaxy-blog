---
title: 'ef数据库code first 使用visual studio'
date: 2023-02-09 17:44:45
tags: [后端]
published: true
hideInList: false
feature: 
isTop: false
---
1. vs 菜单栏-》NuGet包管理器-》程序包管理控制台 （项目选择包含有DbContext文件的目录）
2. 在控制台下输入  add-Migration <custom_name> 生成数据库迁移和文件
3. 在控制台下输入  update-Database 更新数据库迁移和文件