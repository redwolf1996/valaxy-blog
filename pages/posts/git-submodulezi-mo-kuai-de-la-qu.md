---
title: 'Git submodule(子模块)的拉取'
date: 2022-12-13 09:21:26
tags: [Git]
published: true
hideInList: false
feature: 
isTop: false
---
-  如果首次克隆仓库及其模块，使用：
```
git clone --recursive 仓库地址
```
- 对于仓库首次拉取模块，可以使用：
```
git submodule update --init --recursive
```
- 更新子模块(适用于git 1.8.2及以上版本)：
```
git submodule update --recursive --remote
```
- 更新子模块(适用于git 1.7.3及以上版本)：
```
git submodule update --recursive  或者  git pull --recurse-submodules
```