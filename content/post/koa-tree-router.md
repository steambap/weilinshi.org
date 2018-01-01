+++
title = "Koa Tree Router"
date = 2018-01-01T18:44:23+08:00
draft = false
+++

## koa-tree-router

最近有个非常火的框架叫[Fastify](https://github.com/fastify/fastify)。它自称自己高性能的杀手锏之一就是使用了他们自己研发的路由[find-my-way](https://github.com/delvedor/find-my-way)。这个路由说是利用基数树的数据结构，查找时间为 O(k)，比基于正则的路由要快很多。

我好奇的看了一下他们的实现，居然是基于 Golang 里面一个叫做 echo 的框架的路由修改的。而 Golang 的路由实现当中，当属 [julienschmidt/httprouter](https://github.com/julienschmidt/httprouter) 这个项目里面的实现性能最高，比 echo 快 3~5 倍。所以我就做了一个小的实验，基于 httprouter 修改一版基数树的路由，但结果一般：

```
  koa-router  x   399,951 ops/sec ±0.41% (93 runs sampled)
  find-my-way x 1,854,722 ops/sec ±0.50% (96 runs sampled)
  tree-router x 4,875,122 ops/sec ±0.68% (95 runs sampled)
```

只比 find-my-way 快了两倍多，考虑到他们的实现肯定会有为他们的框架妥协的部分，就算改进了，也不会给他们的框架总体带来多大的提升。

其实现在 Node 社区用的最多的还是 express 和 koa。express 的路由是内置的基于正则的路由，应该不会轻易改动，而 koa 是没有基于基数树的路由的，所以我把我实验性的路由改成专门适配 koa 的。

实现了基本功能之后，我决定使用意大利人开发的[autocannon](https://github.com/mcollina/autocannon)试试效果。

![意大利炮](/images/cannon.jpg)

autocannon -c 100 -d 40 -p 10 localhost:8080/test
- koa-router

- koa-tree-router

可以看出 io 有明显提升。

[项目地址](https://github.com/steambap/koa-tree-router)