---
title: 'git删除分支'
date: 2023-02-13 15:17:06
tags: [生产力工具,Git]
published: true
hideInList: false
feature: 
isTop: false
---
1. 使用`git branch -a`查看左右分支，分支形如 **『remotes/origin/<branch_name>』** 的的是远程分支，其他的是本地分支，如果要删除某些分支，请先使用`git checkout`切换到这些以外的分支
   
2. 使用`git banch -d <branch_name>`删除本地分支，如果无法删除，可以使用`git branch -D <branch_name>`强制删除
   
3. 使用`git push origin -d <branch_name>`删除远程分支