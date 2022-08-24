## 前言

学习框架，仅仅会使用是不够的，想要更熟悉框架，需要对源码有一定的了解。本篇文章通过vue源码解析，实现vue的双向绑定。

## 介绍

单向绑定，是将`Model`绑定到`View`中，如果更新了`Model`，那么`View`中的数据也会跟着改变，此时就是单向绑定

双向绑定，在单向绑定的基础上，如果修改`View`中的数据，`Model`中数据也会跟着改变，此时就是双向绑定

![双向绑定](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/%E5%8F%8C%E5%90%91%E7%BB%91%E5%AE%9A.gif)

```
  <div id="app">
    <h1>{{message}}</h1>
    <input type="text" v-model="message">
  </div>

  <script src="vue.min.js"></script>
  <script>
    new Vue({
      el: '#app',
      data: {
        message: 'helloWorld',
      }
    })
  </script>
```

## 实现

接下来，我们就来介绍一下`vue`中双向绑定`v-model`的原理，并实现

- 创建双向绑定的`DOM`结构

  ```
  <input v-model="message">
  ```

- 在`vue`中，循环每一个节点，找到包含`v-model`的节点

  > 通过`hasAttribute('v-model')`进行判断节点是否添加了 `v-model`

  - 如果节点添加了`v-model`，就获取属性值

    > 通过`getAttribute('v-model')`获取属性名为`v-model`的属性值

  - 还需要判断`data`中是否有`v-model`对应属性值

    > 通过`hasOwnProperty(vmKey)`判断data中是否有`vmKey`属性，这里的`vmKey`是 `message`
    >
    > ```
    > if(this.hasOwnProperty(vmKey)){
    > 	console.log(this,this[vmKey])  
    > }
    > ```
    >
    > 此时的`this`是`vue`对象，通过`vue[vmKey]`可以获取到属性对应的值
    >
    > **问题：**
    >
    > 获取不到`data`中的`message`，只能获取到`data`中`$data`下的`message`
    >
    > `console.log(this,vmKey,this[vmKey],this.$data[vmKey])`
    >
    > ![image-20220615095714106](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220615095714106.png)
    >
    > 所以我就通过判断`data`中的`$data`是否有`vmKey`属性
    >
    > ```
    > if(this.$data.hasOwnProperty(vmKey)){
    >             console.log(this.$data[vmKey])            
    >           }
    > ```
    >
    > ![image-20220615095936020](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220615095936020.png)

    - 如果有对应属性值，那么就把`data`中对应的值取出来，赋值给当前节点的`value`中

      > 例如：`v-model`绑定了`message`，那么还需要判断`data`中是否有`message`属性，如果有，就取出`data`中`message`对应的属性值，将对应属性值赋值给，`v-model`绑定的`input`框的`value`，从而实现`Model`数据传输到`View`中
      >
      > 还需要实现`v-model`绑定的`input`框中`value`值改变时，`data`中对应的属性也要跟着改变，这里需要利用前面说的数据劫持
      >
      > ```
      >  if(this.$data.hasOwnProperty(vmKey)){
      >             // data中对应的值取出来，赋值给当前节点的value中
      >             item.value = this.$data[vmKey]         
      >           }
      > ```
      >
      > ![image-20220615100116836](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220615100116836.png)

    - 为节点添加事件

      > 为节点添加事件，将`input`中的`value`赋值给`data`中的`message`，此时就能实现双向绑定了
      >
      > ```
      >           item.addEventListener('input',e => {
      >             this[vmKey] = item.value
      >           })
      > ```
      >
      > 这里的前提是对数据进行劫持了，当我们把value值赋值给对象的属性中，observe()中的set就会监听到，进而调用update去更新视图

完整代码

```
        // 双向绑定，判断元素节点是否被包含v-model
        if(item.hasAttribute('v-model')){
          // 获取属性名为v-model对应的属性值，去除两边空格
          let vmKey = item.getAttribute('v-model').trim()
          // 判断当前节点中是否包含属性值(例：message)，即data中是否有message属性，这里的this是vue对象
          // 问题：获取不到data中的message，只能获取到data中$data下的message ，这里不太理解
          // console.log(this,vmKey,this[vmKey],this.$data[vmKey])            
          if(this.$data.hasOwnProperty(vmKey)){
            // data中对应的值取出来，赋值给当前节点的value中
            item.value = this.$data[vmKey]         
          }
          // 为节点添加事件，将input中的value赋值给data中的message，此时就能实现双向绑定了
          item.addEventListener('input',e => {
            this.$data[vmKey] = item.value
          })
        }
```

效果

![双向绑定2](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/%E5%8F%8C%E5%90%91%E7%BB%91%E5%AE%9A2.gif)

## 总结

双向绑定是改变Model然后`View`也会改变，改变`View`然后`Model`也会改变，

`Model->View` 实际上是通过`Object.defineProperty`劫持数据的改变，如果数据改变了，就更新视图（数据变化，`set`监听到后调用`update`去更新视图）

`View->Model` 则是通过监听元素节点中包含 `v-model` 的节点，并添加事件，监听`value`变化。例如是`input`绑定了`v-model`，首先找到该`input`，然后就为`input`绑定事件，监听`input`中`value`的变化，只要`value`变化，就将值赋给`vue`对象中的`data`，此时`data`变化又触发`Model->View`