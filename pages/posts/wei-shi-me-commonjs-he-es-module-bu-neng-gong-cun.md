---
title: '为什么 CommonJS 和 ES Module 不能共存'
date: 2022-07-14 17:05:46
tags: [web前端]
published: true
hideInList: false
feature: 
isTop: false
---
在 Node 14 的项目里，我们依然能看到混杂着 CommonJS（CJS） 和 ES Modules（ESM） 风格的代码。CJS 使用的是 require() 和 module.exports；ESM 用的是是 import 和 exports。
**首先 ESM 和 CJS 完全是两套不同的设计**。表面上，ESM 使用起来虽然有点接近 CJS，但是实现差异巨大。

### ESM 与 CJS 之间可以相互引用，但是有大量的坑

1.  只能用 import() 调用 ESM 模块，require() 不行，比如 import {foo} from 'foo'
2.  CJS 模块不能使用 import 语法
3.  CJS 模块可以用异步的 dynamic import() 来加载 ESM 模块，但是相对同步的 require 来说，会有一些坑
4.  ESM 模块可以 import CJS 模块，但是只能通过“默认导入”的模式，比如 import _ from 'lodash'，而不支持“命名导入”，比如 import {shuffle} from 'lodash'。
5.  ESM 模块可以 require() CJS 模块，包括“命名导出”的，但是依然会有很多问题，类似 Webpack 或者 Rollup 这样的工具甚至不知道该怎么出处理 ESM 里的 require() 代码。
6.  Node 默认支持的还是 CJS 规范，你需要选择用 .mjs 这样的后缀，或者在 package.json 里设置 "type": "module" 才能开启 ESM 模式。通过 package.json 开启的话，如果有 CJS 规范的文件，就得相反将后缀改成 .cjs。

对于大部分初级 Node 开发者来说，这些规则非常的难以理解，下面会详细对这些展开介绍。
很多 Node 生态的围观群众都把这些问题归结到 ESM 本身，但是接下来我会说明清楚，这些坑都是有其存在的原因，以及未来也很难有完美的解决方案。
最后我也会给框架/库的维护者 3 个建议：

*   提供 CJS 版本
*   基于 CJS 版本简单包一个 ESM 版本出来
*   在项目的 package.json 里添加一个 exports 映射

基本上就能避开大部分坑。

## 背景：CJS 和 ESM 是什么？

Node 从诞生开始就使用了 CJS 规范来编写模块。我们用 require() 引用模块，用 exprts 来定义对外暴露的方法，有 module.exports.foo = 'bar' 或者 module.exports = 'baz'。
下面是一个CJS 的示例，区分两种不同的 exports 方式对于使用上的差异。
**命名导出**：

```
// @filename: util.cjs 
module.exports.sum = (x, y) => x + y; 
// @filename: main.cjs 
const {sum} = require('./util.cjs'); 
console.log(sum(2, 4));

```

**默认导出**：

```
// @filename: util.cjs 
module.exports = (x, y) => x + y; 
// @filename: main.cjs 
const whateverWeWant = require('./util.cjs'); 
console.log(whateverWeWant(2, 4));

```

ESM 规范使用的是 import 和 export，和 CJS 一样也有两种 export 的模式。
**命名导出**：

```
// @filename: util.mjs 
export const sum = (x, y) => x + y; 
// @filename: main.mjs 
import {sum} from './util.mjs' 
console.log(sum(2, 4));

```

**默认导出**：

```
// @filename: util.mjs 
export default (x, y) => x + y; 
// @filename: main.mjs 
import whateverWeWant from './util.mjs' 
console.log(whateverWeWant(2, 4));

```

## ESM 和 CJS 设计差异

CJS 的 require() 是同步的，实际执行的时候会从磁盘或者网络中读取文件，然后立即返回执行结果。被读取的模块有自己的执行逻辑，执行完成后通过 module.exports 返回结果。
ESM 的模块加载是基于 Top-level await 设计的，首先解析 import 和 export 指令，再执行代码，所以可以在执行代码之前检测到错误的依赖。
ESM 模块加载器在解析当前模块依赖之后，会下线这些依赖模块并在此解析，构建一个模块依赖图，直到依赖全部加载完成。最后，按照编写的代码，顺序对应的依赖。
根据 ESM 约定，这些依赖的 ES 模块都是并行下载最后顺序执行。

## Node 默认 CJS 规范是因为 ESM 的不兼容变更

ESM 对于 JavaScript 来说是一个巨大的规范变化，ESM 规范默认使用了严格模式，导致 this 指向和作用域都有变化，所以即使在浏览器里，

## CJS 无法 require() 基于 Top-level await 设计的ESM 模块

CJS 无法 require() ESM 模块，最简单的原因就是 ESM 支持 Top-level await，但是 CJS 不支持。
[Top-level await] 支持在非 async 函数中使用 await。
ESM 支持多重解析的加载器，在不带来更多问题的情况下，让 Top-level await 变得可能。引用 [V8 团队博客的内容] [Rich Harris] 写的 > [gist]，表达了一系列对于 Top-level await 的担忧，并抵制 JavaScript 实现这个特性。担忧包括：

*   Top-level await 可能会中断执行。
*   Top-level await 可能会终端资源下载。
*   无法和 CJS 模块互通。

提议的 stage 3 版本直接回应了这些问题：

*   只要模块能够被执行，就不会有中断的问题。
*   Top-level await 在解析模块依赖图的阶段执行。在这个阶段，所有字段都已经下载并建立对应关系，并不会阻断资源下载。
*   Top-level await 限定在 ESM 模块下，不会支持 CJS 模块（没有互通的必要）。

（Rich 现在已经接受了目前的 Top-level await 实现）
由于 CJS 不支持 top-level await，所以基本也无法把 ESM 的 top-level await 编译成 CJS 代码。那么，你会如何用 CJS 重写下面的代码？

```
export const foo = await fetch('./data.json');

```

令人沮丧的是，绝大多数 ESM 代码并没有用到 top-level await 的写法，不过这不是一个需要纠结的问题。
[目前还有一个如何 require() ESM 模块的讨论]在评论前尽量阅读完整的文章内容以及对应的讨论链接）。如果你深入了解，会发现 top-level await 并不是唯一的问题。如果你同步 require 了一个 ESM 模块，而这个模块又异步 import 了一个 CJS 模块，然后这个 CJS 模块又同步 require 了一个 ESM 模块，你能设想执行结果么。
所以，最后的结论还是在任何情况下不要用 require() 来引入一个 ESM 模块。[

## CJS 可以 import() ESM，但也不是一个好主意

如果你要在 CJS 代码里 import 一个 ESM 模块，需要使用异步的 dyniamic import()。

```
(async () => {  
    const {foo} = await import('./foo.mjs'); 
})();

```

这么写或许没啥问题，只要你不需要 exports 一些执行结果。如果需要，那么你需要对外导出一个 Promise，[对使用者来说就是一个不小的成本]
```
module.exports.foo = (async () => {  
  const {foo} = await import('./foo.mjs');  
  return foo; 
})();

```

## ESM 不能引入导出命名变量的 CJS 模块否则 CJS 代码执行顺序会和期望的不同

你可以在 ESM 里引入一个如下的 CJS 模块：

```
import _ from './lodash.cjs'

```

但是你不能引用一个 CJS 模块具体导出的接口

```
import {shuffle} from './lodash.cjs'

```

这是因为 CJS 代码是在执行的时候计算导出结果，但是ESM是在解析期进行。
不过我们也有一些应对方案，虽然有点烦，但至少能用，就像下面的代码：

```
import _ from './lodash.cjs'; 
const {shuffle} = _;

```

这样的代码没啥缺点，CJS 库甚至可以被封装成 ESM 模块。
这样挺好，不过还可以有一些更好的方式。[](https://link.juejin.cn?target=)

### 执行顺序不可控会导致一些糟糕的问题

有些开发者提议过在执行 ESM 导入之前执行 CJS 导入。按照这个模式，CJS 的命名式导出就可以和在 ESM 的解析期执行。
但是这样会[引入另外一个问题]：

```
import {liquor} from 'liquor'; 
import {beer} from 'beer';

```

如果 liquor 和 beer 都是 CJS 模块，那么将 liquor 改成 ESM 会将原来 liquor, beer 的执行顺序改成 beer, liquor，如果 beer 依赖 liquor 的一些执行结果，就会有问题。

### 动态模块可以解决问题，但也会带来其他坑

有一些另外的提议来想办法解决执行顺序问题，叫做动态模块。
在 ESM 规范中，通过静态声明的方式声明了所有命名导出。在动态模块规范下，引用模块时可以定义导出的名字。ESM 加载器会默认信任动态模块（CJS 代码）会暴露所有需要的命名导出，如果没有暴露，就会抛出错误。
不幸的是，动态模块需要 JavaScript 语言做一些修改才能被 TC 39 委员会接受，然而[并没有被接受]
比较特别的是，ESM 代码支持这样的写法：

```
export * from './foo.cjs'

```

这样意味着会覆盖原来导出的名字，这样叫做“星号导出 ”。
可惜在这个写法下，加载器依然不知道具体导出了什么。
动态模块也给规范的可塑性上带来了问题，比如

```
export * from 'omg';  
export * from 'bbq';

```

这样写会导致 omg 和 bbq 下同名的导出冲突。允许名字被开发者重新定义，也意味着导出校验基本可以忽略不用了。
动态模块的支持者提议去掉“星号导出”，但是 [TC39 委员会拒绝了]。其中一个 TC39 成员称这个提议像“语法毒药”，因为“星号导出”会因为动态模块带来一些副作用。

（我认为我们一直处于语法毒药的世界，在 Node 14 下，命名导出是有副作用的，在动态模块下，星号导出也是有副作用的。由于命名导出使用的频繁但星号导出用的少，所以动态模块对生态的影响相对更小）
这也是并不是动态模块的尽头。有一个提议是所有 Node 模块都应该是动态模块，即使是 ESM 模块，也就是要放弃 ESM 的多重解析加载器。令人意外的是，这个提议并没有明显的副作用，除了会有一些性能问题，毕竟ESM 加载器是面向弱网环境设计的。
不过不幸的是，动态模块的 Github 讨论 issue 已经因为[一年没有讨论而关闭了]
社区里还有另外一个提议，[升级 CJS 模块解析器来支持解析导出内容]不过这个常识基本不太可能实现（最近的一次 PR对应的测试结果，只能在 npm 排名前 1000 的模块中通过62%）。由于该方案的可靠性不足，部分 Node 工作组的成员反对了这个方案。

## ESM 可以 require()，但并不值得这么做

ESM 模块默认没有 require 方法，但是你可以[很简单地实现这个方法

```
import { createRequire } from 'module'; 
const require = createRequire(import.meta.url);  
const {foo} = require('./foo.cjs');

```

这样写的意义不大，并且还比原来的写法要多谢几行代码，并且 Webpack 和 Rollup 这样的工具并不知道该怎么处理 createRequire 的类型。

## 同时支持 CJS 和 ESM 包最佳实践是什么

如果你当前维护了一个同时支持 CJS 和 ESM 的库，你可以根据下面的指南做的更好。

### 提供一个 CJS 版本

这样可以确保你的库在旧版本 Node 下跑的更好。
（如果你写的是 TypeScript 或者其他需要编译到 JS 的语言，那么编译到 CJS。）

### 基于 CJS 封装到 ESM 版本

（将 CJS 封装到 ESM 很容易，但是 ESM 库是没法封装到 CJS 库的。）

```
import cjsModule from '../index.js'; 
export const foo = cjsModule.foo;

```

把 ESM 封装放到 esm 子目录下，同时在 package.json 里声明 {"type": "module"} 。（在 Node 14 下你也可以用 .mjs 后缀，不过有一些工具不一定支持 .mjs 文件，建议还是用子目录的方式）

### 避免双重编译

如果你在用 TypeScript，是可以把 TypeScript 编译出 CJS 和 ESM 两个版本，但是这样可能会导致开发者不小心同时引用了 ESM 和 CJS 版本。
Node 通常会做一些模块的合并，但是无法判断同个库的 CJS 和 ESM 文件是否是同一个文件，那么真正执行的时候，这些代码会被执行两遍，造成一些不可预期的问题

### 在 package.json 里增加 exports 映射

如下：

```
"exports": {  "require": "./index.js",  "import": "./esm/wrapper.js" }

```

**注意：增加 exports 映射是一个不兼容变更**。
默认情况下，开发者是可以访问到依赖包里的任何文件，包括那么包开发者原本只是期望内部使用的。 exports 映射确保了开发者只能引用到明确的入口文件。
这样很好，但是确实是一个[不兼容变更]。
（如果你本来就允许开发者来引用更多的文件，那么可以设置多个入口，可以参考 [ESM 文档]）
*确保 exports 映射的文件是包含明确后缀的*。用 "index.js" 而不是 "index" 或者类似 "./build" 这样的目录。
**如果你按照上面的指南做，可以避开大部分问题。**

