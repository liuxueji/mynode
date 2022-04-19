前言：我的 Vue CLI 创建 vue 项目时没有runtime选项，默认都为 `runtime-only`

```
用vue cli2创建项目时，往往会碰到让我们选择runtime-compiler模式还是runtime-only模式:
Runtime - Compiler: recommended for most users
运行时+编译器:推荐给大多数用户

Runtime-only: about 6KB lighter min+gzip, but templates (or any Vue-specificHTML) are ONLY allowed in .vue files - render functions are required elsewhere
仅运行程序: 比上面那种模式轻大约 6KB，但是 template (或任何特定于vue的html)只允许在.vue文件中使用——其他地方用需要 render 函数

```

# runtime+compiler和runtime-only的区别

> **运行时 + 编译器 vs只包含运行时**
>
> ```vue
> // 需要编译器
> new Vue({
>   template: '<div>{{ hi }}</div>'
> })
> 
> // 不需要编译器
> new Vue({
>   render (h) {
>     return h('div', this.hi)
>   }
> })
> ```

#### 1. runtime-only

> **runtime-only，我的项目用的都是这个**
>
> 优点：
>
> ​	1.运行效率高
>
> ​	2.源代码量更少
>
> 缺点：
>
> ​	灵活度不够。只能用render

#### 2. runtime+compiler

> runtime+compiler，当我需要动态生成vm实例，就需要这个，因为new Vue(options)，options包含template
>
> 优点：
>
> 	1. 选择更灵活。可以选用 template 或 render 
>
> 缺点：
>
> 	1. 体积略大
>  	2. 性能略差

#### 3. 区别

> 两者只在src文件下的main.js里面有区别：
>
> runtime-compiler模式里的Vue实例的模板，和注册的组件，都被一个render函数替换掉了

![](C:\Users\Shinelon\Desktop\随堂笔记\image\Snipaste_2022-04-19_16-54-38.png)

#### 4.运行过程

> runtime + compiler 中 Vue 的运行过程

```
对于 runtime-compiler 来说，它的代码运行过程是：template -> ast -> render -> virtual dom -> UI

    1. 首先将vue中的template模板进行解析解析成abstract syntax tree （ast）抽象语法树
    2. 将抽象语法树在编译成render函数
    3. 将render函数再翻译成virtual dom（虚拟dom）
    4. 将虚拟dom显示在浏览器上
```

> runtime-only 中 Vue 的运行过程

```
对于 runtime-only来说，它是从 render -> virtual dom -> UI

    1. 可以看出它省略了从template -> ast -> render的过程
    2. 所以runtime-only比runtime-compiler更快，代码量更少
    3. runtime-only 模式中不是没有写 template ，只是把 template 放在了.vue 的文件中了，并有一个叫 vue-template-compiler 的开发依赖时将.vue文件中的 template 解析成 render 函数。 因为是开发依赖，不在最后生产中，所以最后生产出来的运行的代码没有template
```

> render 函数

```
在Vue中，我们使用模板HTML来组建页面，使用render()我们就可以在逻辑行为中使用JS来构建DOM。

Vue的核心技术使用了虚拟DOM，所以在项目中的template模板是需要解析编译成虚拟DOM的，而转化为虚拟DOM的过程就需要用到render()函数。

当使用render()描述虚拟DOM时，Vue提供一个函数，这个函数就是构建虚拟DOM所需要的工具，官网上给它起了个名字createElement(),但通常简写为h()。

new Vue({
  el: "#app",
  render: h => h(App)
});
```

#### 5.官方介绍

> 在npm包的 dist/目录 你将会找到很多不同的 Vue.js 构建版本，这里列出它们之间的差别：

![](C:\Users\Shinelon\Desktop\随堂笔记\image\Snipaste_2022-04-19_17-13-10.png)

![](C:\Users\Shinelon\Desktop\随堂笔记\image\Snipaste_2022-04-19_17-13-27.png)

![](C:\Users\Shinelon\Desktop\随堂笔记\image\Snipaste_2022-04-19_17-13-37.png)

![](C:\Users\Shinelon\Desktop\随堂笔记\image\Snipaste_2022-04-19_17-13-48.png)

> 因为在` Vue.js 2.0 `中，最终渲染都是通过` render 函数`，如果写 `template 属性`，则需要编译成 render 函数，那么这个编译过程会发生在运行时，所以需要带有编译器的版本。很显然，这个编译过程对性能会有一定损耗，所以通常我们更推荐使用 `Runtime-Only 的 Vue.js`。

#### 如何将runtime-only转为runtime+compiler模式

 在 `src/main.js` 下添加` vue.config.js` 文件，并设置

```js
module.exports = {
  runtimeCompiler: true
}
```



