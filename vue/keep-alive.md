### 前言

本篇文章对`vue`的特殊生命周期钩子函数介绍（`activated`、`deactivated`），为什么特殊呢？因为它们是通过keep-alive产生的，不是默认生成的。`keep-alive`，翻译为‘保持活动’，它区别于`vue`中的普通生命周期钩子函数，通过`keep-alive`，我们可以实现动态组件，提升性能

### 介绍

- `Vue` 中的内置组件，能在组件切换过程中将状态保留在内存中，防止重复渲染`DOM`

- 在项目中通过为组件添加`keep-alive`，来对组件进行缓存，从而提升性能

- 使用时，只需要将需要被缓存的组件，被`keep-alive`包裹，当组件被切换时，`activated`、`deactivated`这两个生命周期钩子函数会被执行，每次进入组件都会执行`activated`，每次离开组件都会执行`deactivated`

> 注意：只有组件被`keep-alive`包裹才会触发这两个钩子函数。

### 使用

1. 首先为需要缓存的组件添加`keep-alive`

   ![image-20220528225737598](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220528225737598.png)

2. 定义一个用户信息页面，一个信息详情页面。通过点击用户信息，跳转到信息详情页

   > - 在用户信息页面中，通过`router.push('/detail')`实现跳转到详情页，并传入`id`
   >
   > ```
   > toDetail (id) {
   >     this.$router.push({
   >         path: '/home/detail',
   >         query: { id }
   >     })
   > }
   > ```
   >
   > - 在详情页中，通过`this.$route.query.id`，获取传入的`id`，根据`id`发起网络请求获取对应的用户信息
   >
   > ![keep-alive-1](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/keep-alive-1.gif)

3. 查看页面请求

   > 此时，只有第一次访问才会发起请求，第二次和多次访问都不会发起请求，证明生命周期不会被执行，但是`activated`除外，我们通过打印来查看
   >
   > ```
   >   },
   >   created () {
   >     console.log('---created被执行---');
   >   },
   >   activated () {
   >     console.log('---activated被执行---');
   >   }
   > ```
   >
   > ![gif2](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/gif2.gif)
   >
   > 可以看出，只有在第一次访问用户详情页时，`created`会被执行，第二次及第N次都不会执行，只有`activated`被执行。
   >
   > 所以，我们可以根据这个特性，通过增加条件判断，来实现组件缓存

4. 实现组件缓存

   > 实目标：当访问相同用户信息时，不发起请求，使用缓存；当访问不同用户信息时，发起请求，获取新数据。
   >
   > 思路：由于created只有第一次访问才会执行，activated则是每次都执行，所以我们可以在第一次访问时，获取初始id，在第二次及第N次访问时，获取当前id，判断初始id与当前id是否相同，如果不相同，则表明当前请求的页面不是第一次访问的，需要重新获取数据，如果相同，则使用缓存数据。
   >
   > 实现：
   >
   > ```
   >   created () {
   >     this.getData()
   >     this.id = this.$route.query.id
   >     console.log();
   >   },
   >   activated () {
   >     console.log('初始id：', this.id, '当前id：', this.$route.query.id);
   >     if (this.id != this.$route.query.id) {
   >       console.log('不是同一用户，发起请求');
   >       this.getData()
   >       this.id = this.$route.query.id
   >     } else {
   >       console.log('是同一用户，不发请求');
   >     }
   >   }
   > ```
   >
   > 效果：![GIF4](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/GIF4.gif)

   


### 总结

🤗`keep-alive`就是用来缓存组件，提升性能，如果用户每次点击同一个用户，那么用户详情页就没必要发起重复的请求，直接使用缓存即可，只在请求不同用户时，才发起请求，这样能提升性能，减小服务器压力。