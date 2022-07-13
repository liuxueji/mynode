### 介绍

- Express是基于 Node.js 平台，快速、开放、极简的 Web 开发框架

- Koa 是一个新的 web 框架，由 Express 幕后的原班人马打造， 致力于成为 web 应用和 API 开发领域中的一个更小、更富有表现力、更健壮的基石。 通过利用 async 函数，Koa 帮你丢弃回调函数，并有力地增强错误处理。 Koa 并没有捆绑任何中间件， 而是提供了一套优雅的方法，帮助您快速而愉快地编写服务端应用程序。

### 使用

express

```
const express = require('express');
const app = express();

/* 中间件 */
app.use((req, res, next) => {
    console.log('hi');
    next();
    console.log('hellow');
});

/* 路由部分 */
const router = express.Router();
router.get('/', (req, res) => {
    res.send('Home');
});

app.use(router);

/* 静态文件 */
app.use(express.static('./'));

app.listen(3000);
```

koa

```
const Koa = require('koa');
const Router = require('koa-router');
const serve = require('koa-static');

const app = new Koa();
const router = Router();

/* 中间件 */
app.use(async (ctx, next) => {
    console.log('hi');
    next();
    console.log('hellow');
});

/* 路由部分 */
router.get('/', (ctx) => {
    ctx.body = 'Home';
});
app.use(router.routes());

/* 静态文件 */
app.use(serve('./'));

app.listen(3000);
```

### 区别

1. 用法区别：

   - express是基于回调的
   - koa是利用Async/Await

2. 中间件区别：

   - express是线性模型
   - koa是洋葱模型

3. 集成度：

   - express自带Router和Static中间件

   - koa需要自行安装

     > 这也是为什么koa代码量小的原因

4. 社区活跃度：

   - express高于koa