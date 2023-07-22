---
title: 'Javascript中的Label语句'
date: 2020-05-22 18:30:31
tags: [web前端]
published: true
hideInList: false
feature: 
isTop: false
---
> **在javascript中，我们可能很少会去用到 Label 语句，但是熟练的应用 Label 语句，尤其是在嵌套循环中熟练应用 break, continue 与 Label 可以精确的返回到你想要的程序的位置**

- label语句的语法 **Label: statement** 如：
```
TestLabel: for (var i = 0; i < 10 ; i++ ){
  alert(i);
}
```
- 举一个比较典型的例子，看完后即明白 Label 的应用（未添加 Label）：
```
var num = 0;
for (var i = 0; i < 10; i++) {
  for (var j = 0; j < 10; j++) {
    if (i == 5 && j == 5) {
      break;
    }
    num++;
  }
}
alert(num); // 循环在 i 为5，j 为5的时候跳出 j循环，但会继续执行 i 循环，输出 95
```
- 对比使用了 Label 之后的程序（添加 Label 后）：
```
var num = 0;
outPoint:
for (var i = 0; i < 10; i++) {
  for (var j = 0; j < 10; j++) {
    if (i == 5 && j == 5) {
      break outPoint;
    }
    num++;
  }
}
alert(num); // 循环在 i 为5，j 为5的时候跳出双循环，返回到outPoint层继续执行，输出 55
```
- 对比使用了break、continue语句：
```
var num = 0;
outPoint:
for (var i = 0; i < 10; i++) {
  for (var j = 0; j < 10; j++) {
    if (i == 5 && j == 5) {
      continue outPoint;
    }
    num++;
  }
}
alert(num);  //95  
```

####从alert(num)的值可以看出，continue outPoint;语句的作用是跳出当前循环，并跳转到outPoint（标签）下的for循环继续执行。

