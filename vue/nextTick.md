### 前言

在写项目的时候，会遇到更新了`DOM`，但是获取到的是为更新前的数据，此时就需要用放到`Vue`提供的`nextTick`，本篇文章聊一聊`nextTick`，但是在讲`nextTick`之前，需要先了解一下`JS`的执行机制

### 介绍

1. `js`执行机制

   `JavaScript`是单线程执行的，将需要执行的任务，分为同步任务和异步任务

   - 同步任务：`JS`会将同步任务放入主线程，形成一个栈，并且是依次执行
   - 异步任务：不进入主线程，并且异步任务可以分为宏任务与微任务
     - 宏任务：主代码块、`setTimeout`、`setInterval`、`setImmedia`
     - 微任务：`process.nextTick()`、`Promise.then`、`catch`

   浏览器在执行`JS`代码时，是按照：同步、异步（微任务、宏任务、微任务...）的顺序依次执行

2. `nextTick`

   将传入的函数，变成异步任务，如果通过`nextTick`包裹的方法获取`DOM`，获取到的是更新后的`DOM`

   示例

   ```
     created () {
       console.log('a');
       console.log('c')
     },
     mounted () {
       console.log('b')
       console.log('d')
     }
   ```

   执行顺序：`a->c->b->d`

   ```
     created () {
       console.log('a');
       this.$nextTick(() => {
         console.log('c')
       })
     },
     mounted () {
       console.log('b')
       this.$nextTick(() => {
         console.log('d')
       })
     }
   ```

   执行顺序：`a->b->c->d`

   可以看出，添加了`nextTick`，会将同步任务转为异步任务，从而延迟了执行时间

3. `nextTick`和`setTimeout`的比较

   之前我将事件转为异步任务使用的是`setTimeout`，那么`nextTick`和`setTimeout`有什么区别吗？

   `nextTick`源码中利用的是`Promise.resolve()`实现的，所以，区别在于`setTimeout`是宏任务，`nextTick`是微任务，上面我们已经说过，`JS`的执行顺序是 `同步、异步（微任务、宏任务、微任务...）`，所以`nextTick`的执行顺序优先于`setTimeout`，当然`setTimeout`也可以替代`nextTick`

### 使用场景

当我们修改`DOM`，并且获取`DOM`内容时，如果直接获取，那么获取到的时未更新的`DOM`，此时就需要用到`$nextTick`，实现异步获取

代码：

```
<template>
  <div>
    <h1 ref="name">{{name}}</h1>
    <button @click="btn">点击获取name</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      name: '张三'
    }
  },
  methods: {
    btn () {
      this.name = '李四';
      console.log('直接获取修改后的DOM' + this.$refs.name.innerHTML);
      this.$nextTick(() => {
        console.log('通过nextTick异步获取修改后的DOM' + this.$refs.name.innerHTML);
      })
    }
  },
}
</script>
```

![image-20220530092644375](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220530092644375.png)

2. 点击按钮，显示元素，并获取元素的高度

   ```
   <template>
     <div>
       <div type="text"
            ref="name"
            v-if="isShow">box</div>
       <button @click="btn">点击获取高度</button>
     </div>
   </template>
   
   <script>
   export default {
     data () {
       return {
         isShow: false
       }
     },
     methods: {
       btn () {
         this.isShow = true
         console.log(this.$refs.name.offsetHeight);
   
         this.$nextTick(() => {
           console.log(this.$refs.name.offsetHeight);
         })
       }
     },
   }
   </script>
   ```

   ![image-20220530095857560](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220530095857560.png)

​	如果没有添加`nextTick`，获取元素属性会报错，因为，此时DOM还没更新，`this.$refs.name`还不存在，所以会报错。如果添加`nextTick`，会异步执行，先操作`DOM`，后执行回调。

### 总结

当我们操作`DOM`，并且需要获取到的是，`DOM`改变后的数据时，就需要使用`nextTick`。