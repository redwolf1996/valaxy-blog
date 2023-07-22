---
title: '腾讯云服务器 centos7 root用户 ssh密钥vscode远程登录'
date: 2022-12-13 17:15:41
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
> 为了体验一下vscode的remote-ssh插件的能力， 所以......
## 基本原理是私钥解公钥，所以方式有两种：
1. 本地用ssh-keygen 生成密钥对 用id_rsa作为私钥解密, 将id_rsa.pub公钥的内容放到服务器 
`/root/.ssh/authorized_keys`里面
2. 服务器用ssh-keygen 生成密钥对 将id_rsa.pub公钥的内容放到服务器的
`/root/.ssh/authorized_keys`里面，并且将id_rsa私钥发送给客户端，客户端使用服务器端生成的私钥解密

- 客户端的 sshconfig配置如下
`vim /Users/syx/.ssh/config` 其中id_rsa2是自定义新建的文件内容就是服务端生成的私钥内容
```
Host 121.4.56.221
    HostName 121.4.56.221
    User root
    Port 22
    IdentityFile ~/.ssh/id_rsa2
```
显然如果很多用户使用一个服务器 需要远程登录的话第二种方式具有更好的复用性 但是坏处在于私钥大量的提供，第一种方式则需要在服务器的authorized_keys里面不停的添加不同客户端的公钥，压力给了服务端 请根据实际情况自行选择

### 最蛋疼的地方在于我们需要在服务器上配置如下
编辑 ```vim /etc/ssh/sshd_config```文件 配置以下选项
```
PermitRootLogin yes
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
PasswordAuthentication yes
GSSAPIAuthentication no
GSSAPICleanupCredentials no
UsePAM yes
```
然后service sshd restart 使得配置生效 值的一提的是 sshd_config  authorized_keys 以及客户端的config文件的权限需要设置为chmod 600或者更低的chmod 400
接着去登录才能成功

登录成功后再把服务器sshd_config文件的 PasswordAuthentication yes 改成no 以禁用密码登录只允许ssh登录 才能最后成功

这也是腾讯云最傻逼的地方 搞了我一天 妈的.......
