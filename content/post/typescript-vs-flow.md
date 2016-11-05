+++
date = "2016-10-30T11:40:57+08:00"
title = "Typescript vs Flow"
draft = false
image = "ts-flow.png"

+++

## Typescript 和 Flow
从编译型语言转型的工程师常常抱怨 Javascript 没有类型，不安全。我自己在工作中
也经常遇到过对象上的一个属性在n个模块之间传递之后变成了 undefined 。  
  
微软和脸书的工程师开发自己的开源的方案来解决这种痛点，这就有了微软的
Typescript 和脸书的 Flow 。

### Typescript
Typescript 被称作“有类型的并且编译成原生的Javascript的超集”。
Typescript 现在是 github 上最受欢迎的语言之一，并且现在在 npm 上有 2M 的下载量。  
  
所有的 Javascript 语法在 Typescript 中都是有效的，但是不要以为你把已有的
.js文件重命名为.ts就可以万事大吉了。Typscript不会像 Less 或者 Sass 那样接受 css 。
如果想用 Typescript ，我必须从新建一个项目开始。

按照 [Typescript 的文档](https://www.typescriptlang.org/docs/tutorial.html) ，
我可以较为轻松的构建一个简单的项目，并给我的变量和对象添加类型。

但是，如果你想做一些更加复杂的事情就没有那么简单了。你肯定需要获取你的依赖的类型定义，
不然 Typescript 会认为  

> import assert from 'assert';

这里的 assert 未定义，不能编译。

所以我花了九牛二虎之力，浪费了好久，查了typings和tsd之后，不知咋的就算勉强搞定了。
但是更糟糕的是，我不能像类似这样：

> mocha --compilers ts:typescript

就运行我的 mocha 测试。我查了以下 github 上面的一些 Typescript 项目，
大家都是先编译再运行测试。到这时我只能怒删 Typescript 了。

### Flow
Flow 是一个给 Javascript 添加了静态类型检查的工具，现在在 npm 上有 1.8M 的下载。
所有的 Javascript 文件都可以让 Flow 检查，你只需要在文件的顶部加上这一行

> // @flow

有点类似 'use strict';  
  
如果想要充分利用 Flow 的静态类型分析，我就必须要添加 Flow 的静态类型标注，像是：

> const str: string = 'Hello, world!';

所以，部分已有的项目是不能有效的使用 Flow 的。 但是基于 babel 的项目是完全可以的，
只需要一个 [babel 插件](https://babeljs.io/docs/plugins/transform-flow-strip-types/)
  
而构建一个新的 Flow 项目和基于 babel 的项目是一样的，你只是加了一个 babel 的插件而已。
Flow 的语言和 Typescript 的语法类似，甚至大部分时候是完全一样的。到了测试环节，
还是 babel 项目的老办法：

> mocha --compilers js:babel-register

Flow 最大的优点是，只要语法正确，babel 仍然能正常编译，如果你不喜欢，你可以去掉
Flow 的指令让它不再检查那个文件就可以了。
  
## 总结
虽然两个工具都很受欢迎，但是我还是偏好 Flow 。也许我打开 Typescript 的姿势不对，
但是 Typescript 总归是个新的语言。不安它的规则走，你就别指望有能用的代码。
相对的， Flow 像是你的额外的Linter,它会对你的文件进行静态类型分析，如果你认为
它说的不对，可以忽略掉。所以，除非有 Angular 2 的项目，不然，如果项目有类型安全的
需求，我肯定会选择 Flow，毕竟 babel 怎么都会用得上。
