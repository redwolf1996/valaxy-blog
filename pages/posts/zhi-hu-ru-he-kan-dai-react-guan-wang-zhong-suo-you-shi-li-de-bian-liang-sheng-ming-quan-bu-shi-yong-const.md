---
title: '【知乎】如何看待React官网中所有示例的变量声明全部使用const？'
date: 2022-07-18 09:37:15
tags: [web前端]
published: true
hideInList: false
feature: 
isTop: false
---
> 如果你在用 ts 的话，const 和 let 两者，对于 literal，infer 出来的类型是不一样的。
```
declare const a: number;
const b = a > 0 ? 'positive' : 'negativeOrZero'; // $ExpectType: "positive" | "negativeOrZero"
let c = a > 0 ? 'positive' : 'negativeOrZero'; // $ExpectType: string
```
所以，我几乎都是用 const，为了更精确的 type infer。
---
如果你接受了 functional programming 那套，例如说 Haskell，那你已经习惯了语言中只有 const 没有 let。一旦你习惯了这种写法，你回来用 JavaScript 这种允许 functional 但不强迫 functional 的语言时你会继续坚持 functional 的习惯。这时候你的所有赋值都是一次性的，你不会再去改变它，于是你可以统一写 const 而不是 let。这背后实际发生的是一种编程思维模式的改变。过去你会用一个变量名来代表一个存储空间，在一系列操作的过程中不断改变这个存储空间的内容，
```
就好像这样子：let result = initialValue;
result += adjustment1;
result += adjustment2;
return result;
```
适应 functional 的过程中，因为所有赋值都是 const，你会停止把变量看作存储空间，你会把变量看作一个对计算结果的取名而已。就好比说「把距离除以时间的计算结果取名为速度」一样，目的是方便你将来在别的计算中用「速度」这个简单易懂的名字取代「距离除以时间」。习惯 functional 的思维模式后，你会把上述代码写成这样子：
```
const afterAdjustment1 = initialValue + adjustment1;
const afterAdjustment2 = afterAdjustment1 + adjustment2;
return afterAdjustment2;
```
你会对计算步骤取名字，而不是对存储空间取名字。忘却存储空间这件事情，假设 JavaScript 都能帮你打理好，不会太浪费实际存储空间，然后你会为每一个计算步骤取一个独一无二的名字，并且全部用 const。
