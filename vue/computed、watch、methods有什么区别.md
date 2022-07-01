### 前言

初学`vue`的时候，我对`computed`、`methods`、`watch`三者使用很容易搞混，虽然了解了它们的使用，但是对于具体使用场景容易混淆，例如：同一个功能，`methods`可以实现，`computed`也可以实现；获取一个数据，`computed`和`watch`获取的效果完全相同。对于这种情况，最好的办法，就是将它们分别做对比，才能更好的在合适的场合使用它们

本篇文章主要介绍`computed`、`methods`、`watch`三者的区别，当然了，`vue`官方文档有很详细的介绍它们之间的区别，但是由于太过官方，所以我希望通过更语义化的方式，介绍它们之间的区别。

### 介绍

- `computed`

  - 介绍

    > 在`computed`中定义计算属性的方法
    >
    > 在`template`中使用{{方法名}}展示内容

  - 原理

    > 底层使用了`Object.defineproperty`中的`getter`和`setter`

  - 使用

    ```
      computed: {
      
        // 简写
        sum () {
          return this.a + this.b
        }
    
        // 完整写法
        sum: {
          get () {
            return this.a + this.b
          },
          set (value) {
            console.log('set', value)
          }
        }
        
      }
    ```

- `watch`

  - 介绍

    > 通过`vue`实例中 `$watch()`或`watch`配置来监视属性
    >
    > 当属性变化，回调函数自动调用，在函数内计算

  - 原理

    > 底层对`watch`属性创建一个`watcher`

  - 使用

    ```
      watch: {
        sum: {
          immediate: true, 
          handler (newValue, oldValue) {
            console.log('sum被修改了', newValue, oldValue)
          }
        }
      }
    ```

- `methods`

  - 介绍

    > 可以通过`vm.方法名`定义或调用方法
    >
    > `可以通过vm.$data.属性名`来访问data数据

  - 使用

    ```
        methods:{
          show: function(){
            console.log(this.sum);
          }
        }
    ```

    

### 区别

#### 1.computed 与 methods 

- `computed`是有缓存的

- `methods`没有缓存

- 举例：

  ```
  <template>
    <div>
      <h1>计算a+b={{sum}}</h1>
      <h1>计算a+b={{sum}}</h1>
      <h1>计算a+b={{sum}}</h1>
      <h1>计算a+b={{sum}}</h1>
    </div>
  </template>
  
  <script>
  export default {
    data () {
      return {
        a: 3,
        b: 2
      }
    },
    computed: {
      // 简写
      sum () {
        console.log('computed：计算了a+b');
        return this.a + this.b
      }
    }
  }
  </script>
  
  <style>
  </style>
  
  ```

  ![image-20220531173401177](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220531173401177.png)

输出了多个`sum`，但是`computed`并不会执行多次，可以看出计算属性有缓存，并且是基于它们的响应式依赖进行缓存的。而`methods`没有缓存。

#### 2.computed 与 watch

- `watch`是监听，数据发生了改变才会被执行，支持异步。一个值影响多个属性（多对一）
- `computed`是计算，会根据新数据计算结果，支持缓存。一个值依赖多个属性（一对多）

`computed`使用场景，计算数据

```
<template>
  <div>
    <h1>计算{{a}}+{{b}}={{sum}}</h1>
    <button @click="a++">点击a+1</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      a: 3,
      b: 2
    }
  },
  computed: {
    sum () {
      console.log('computed：计算了a+b');
      return this.a + this.b
    }
  }
}
</script>
```

![GIF](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/GIF.gif)

`watch`使用场景，观察数据

```
<template>
  <div>
    <h1>计算{{a}}+{{b}}={{sum}}</h1>
    <button @click="a++">点击a+1</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      a: 3,
      b: 2
    }
  },
  computed: {
    sum () {
      // console.log('computed：计算了a+b');
      return this.a + this.b
    }
  },
  watch: {
    sum: {
      immediate: true,
      handler (newValue, oldValue) {
        console.log('sum被修改了', newValue, oldValue)
      }
    }
  }
}
</script>
```



![GIF1 (2)](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/GIF1%20(2).gif)

### 总结

具体使用要根据不同的场景做判断。如果要计算属性，使用`computed`；如果要监听数据或路由变化，使用`watch`

