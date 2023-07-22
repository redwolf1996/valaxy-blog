---
title: 'EF- code first 数据库迁移(cli 方式)'
date: 2023-02-09 08:45:39
tags: [后端]
published: true
hideInList: false
feature: 
isTop: false
---
> 注意：必须要关闭vscode的debug运行模式才能进行操作 否则会失败
由于visual studio在本人的电脑上太卡，所以用vscode写.net项目 在vscode的生成efcore数据库迁移文件
1. 首先安装全局的ef cli工具 dotnet tool install --global dotnet-ef
   
2. 然后使用 dotnet ef migrations add test_migration -p "./FSMOM.EntityModel/FSMOM.EntityModel.csproj" -s "./FSMOM.Entry/FSMOM.Entry.csproj"（-p是--project的简称，后面是跟的相对目录默认当前文件夹，如果目录下有多个csproj文件需要指定一个；-s是--startup-project的简称，后面也是跟的相对目录默认当前文件夹，如果目录下有多个csproj文件也需要指定一个）生成迁移文件

3. 使用dotnet ef database update -p "./FSMOM.EntityModel/FSMOM.EntityModel.csproj" -s "./FSMOM.Entry/FSMOM.Entry.csproj" 将迁移文件写入数据库

4. 删除：
dotnet ef migrations remove test_migration -p "./FSMOM.EntityModel/FSMOM.EntityModel.csproj" -s "./FSMOM.Entry/FSMOM.Entry.csproj"
<!-- more -->
