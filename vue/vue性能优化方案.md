1. 路由懒加载

   > 这个比较简单，改变一下引入的方式就行
   >
   > ```
   > const router = new VueRouter({
   >   routes:[
   >     {path:'/foo',component:()=>import('./Foo.vue')}
   >   ]
   > })
   > ```

2. keep-alive

   >通过keep-alive进行页面缓存，如果使用keep-alive，生命周期函数会多两个：activated、deactivated
   >
   >如果组件被keep-alive包裹，那么其他生命周期就不会被执行，而是执行activated、deactivated，进入组件执行：activated，离开组件执行：deactivated
   >
   >根据这个特点，可以实现组件缓存，实现原理就是：判断用户上一次进入的页面和当前进入的页面是否为同一个页面，如果是同一个页面就不发请求。

3. if和for合理使用

   > 在vue2中，for会比if优先执行，因为vue代码中已经规定好了，for就是在if前进行判断
   >
   > ```
   > 顺序是：static->once->for->if->slot
   > ```
   >
   > 如果for比if前执行，就会出现无用功，因为当前元素可能不需要被展示，但是却进行了for循环，消耗了性能
   >
   > 解决办法：通过template包裹，将if设置到template中

4. 第三方插件按需导入

   > 当我们使用ui库时，全局导入会导致项目体积过大，例如：element-ui，可以通过按需导入，减小项目体积
   >
   > 不过在打包上线的时候，可以用cdn解决

5. 合理使用v-if和v-show

   > 频繁切换用v-show
   >
   > 不频繁切换用v-if

6. 图片懒加载

   > 在vue中有一个API `v-lazy`，可以实现图片懒加载
   >
   > ```
   > <img v-lazy="/static/img/logo.png">
   > ```

7. 事件销毁

   > 一些无用的事件，例如定时器，我们不需要使用时就可以进行销毁
   >
   > ```
   > created(){
   >   this.timer = setInterval(this.refresh,2000)
   > },
   > beforeDestroy(){
   >   clearInterval(this.timer)
   > }
   > ```
   >
   > 

8. SSR

   > 服务器渲染，指在服务端完成页面的HTML拼接，直接发送到浏览器中展示
   >
   > 适用场景：
   >
   > - 需要更好的SEO
   > - 需要更快的渲染
   >
   > 实现原理大致是：通过webpack打包成两个bundle，一份给客户端，一份给服务器