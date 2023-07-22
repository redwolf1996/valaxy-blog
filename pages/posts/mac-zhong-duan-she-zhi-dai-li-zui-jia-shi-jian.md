---
title: 'mac终端设置代理最佳实践'
date: 2022-08-18 15:51:58
tags: [生产力工具]
published: true
hideInList: false
feature: 
isTop: false
---
> 在开发中用各种包管理工具安装东西非常慢，除了设置国内源之外，还可以设置终端代理

1. 在 ~ 下面建立proxy.sh脚本内容如下：
```
# 开启代理
function px(){
    # port 根据你的代理工具自行设置
    export https_proxy=http://127.0.0.1:7890
    export http_proxy=http://127.0.0.1:7890
    export all_proxy=socks5://127.0.0.1:7890
    echo -e "已开启代理...:)"
}
# 关闭代理
function upx(){
    unset ALL_PROXY
    unset http_proxy
    unset https_proxy
    echo -e "已关闭代理...:("
}

```
2. vim ~/.zshrc 增加内容如下：
```
source $HOME/.term_proxy.sh
```

3. 重启 或者source .zhsrc文件后生效
- **px** 开启代理  **upx**关闭代理
   