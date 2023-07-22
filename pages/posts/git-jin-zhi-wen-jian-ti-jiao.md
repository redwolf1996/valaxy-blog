---
title: 'git禁止文件提交'
date: 2022-07-14 16:46:42
tags: [Git]
published: true
hideInList: false
feature: 
isTop: false
---
```
命令：git update-index --assume-unchanged 文件名

作用：忽略文件的改动，但是不加入.gitignore 文件中，这样可以达到仅在本地目录中忽略，不影响其他团队成员的工作。

命令：git update-index --no-assume-unchanged 文件名

作用：上一个命令的逆操作，重新追踪文件改动

上次学会使用update-index --assume-unchanged后，大量用update-index --assume-unchanged来忽略文件，等到项目结束要提交代码时才疯了，哪些文件被我忽略了？由于之前没有做记录，忽略的文件完全没有印象，只好想办法啦，目前只找到下面这个方法：

git ls-files -v | grep '^h'

可以将所有被update-index --assume-unchanged关闭了跟踪的文件。但文件太多了，不想手工一条一条敲，只好先将路径提取出来再与命令拼装，如下：

git update-index --no-assume-unchanged $(git ls-files)

或者

git ls-files -v|grep '^h' |awk '{print $2}' |xargs git update-index --no-assume-unchanged



1、编辑用户目录下的.zshrc文件，即vi ~/.zshrc

2、在.zshrc中添加想要添加的快捷方式。

alias gdisable='git update-index --assume-unchanged'

alias genable='git update-index --no-assume-unchanged'

#d

disable = update-index --assume-unchanged

enable = update-index --no-assume-unchanged

disabled = !git ls-files -v | grep \"^[a-z]\"

```