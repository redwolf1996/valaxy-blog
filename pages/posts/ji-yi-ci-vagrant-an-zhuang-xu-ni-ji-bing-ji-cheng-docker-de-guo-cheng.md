---
title: '记一次vagrant安装虚拟机并集成docker的过程'
date: 2022-08-02 09:02:43
tags: [VM]
published: true
hideInList: false
feature: 
isTop: false
---
- 安装virtualbox
- 安装vagrant
- 去vagrant官网下载box
- vagrant add box && vagrant up
- vagrant ssh
- yum update升级到centos7.9和内核3.10
- 去runoob看安装docker的步骤并安装好
- 重新用vagrant打包成自己的box
1. 重新运行新的box，报错，注意打包前要在系统里更改登录方式允许账号密码登录
2. 更改 /etc/ssh/sshd_config里面的 PasswordAuthentication 为 yes
3. Vagrantfile中增加 config.ssh.username="vagrant"  config.ssh.password="vagrant" 改为账号密码登录
- vagrant reload 此时会报错 /sbin/mount.vboxsf: mounting failed with the error: Invalid argument：
4. 安装virtualbox官方的extention_pack
5. vagrant plugin install vagrant-vbguest && vagrant vbguest 等待一会而安装完成
6. 在vagrant ssh大功告成
7. 在Vagrantfile中设置私有网络如192.168.56.10 可以互相固定ip ping通
8. 使用ruby脚本编写Vagrantfile创建多个带docker容器的引擎 即可模拟多服务器集群 
9. 大功告成~