---
title: 'Vue监测数组变化的问题'
date: 2022-07-19 09:40:55
tags: [web前端]
published: true
hideInList: false
feature: 
isTop: false
---
```
data(){
    return {
        arr: [
            {name:'zhangsan', age:20},
            {name:'lisi', age:55}
        ]
    }
}
如上所示 假如改变arr数组的第一个对象
arr[0].name = 'zhangsan111'; arr[0].age = 22 这样子页面上是可以同步更新状态
但是设置arr[0] = {name: 'zhangsan111', age:22} 这样整体设置则监听不到，但其实data上挂载的属性已经是更新了 需要用$set去改变了（包括对象后加属性都要用$set去处理，为属性增加getter和setter才能使数据变成响应式）
```