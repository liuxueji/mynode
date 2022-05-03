`slot`是插槽的意思，语义化很明确，就像萝卜坑。

`slot`插槽是子组件的模板标签，至于插槽是否显示、如何显示就由父组件决定。简单说就是：坑的位置在子组件中定义，萝卜由父组件是否决定插入和如何插入。

`slot`插槽由三种：`默认插槽（匿名插槽）`、`具名插槽`、`作用域插槽`

- 默认插槽：又称匿名插槽，特点是没有`name`，并且一个组件只能有一个匿名插槽（很好理解：如果有两个插槽都不带`name`，那么父组件怎么知道萝卜插哪个坑呢）

  ```
  // 子组件 ： (假设名为：Son)
  <template>
    <div>
         <slot>默认值</slot>//父组件传了值就显示父组件的值，没有传就显示默认值
    </div>
  </template>
  
  // 父组件：（引用子组件 Son）
  <template>
    <div class= 'app'>
       <Son>萝卜</Son>
    </div>
  </template>
  ```

  

- 具名插槽：标签内带有`name`属性的插槽。特点是父组件可以根据`name`属性找到对应插槽（使用`v-slot`）

  ```vue
  // 子组件 ： (假设名为：Son)
  <template>
    <div>
         <slot name='a'>默认值a</slot>
         <slot name='b'>默认值b</slot>//父组件传了值就显示父组件的值，没有传就显示默认值
         <slot name='c'>默认值c</slot>
    </div>
  </template>
  
  // 父组件：（引用子组件 Son）
  <template>
    <div class= 'app'>
       <Son>
       	<template v-slot:a>萝卜a</template>
       	<template v-slot:b></template>
       	<template #c>萝卜c</template> // v-slot: 简写 #
       </Son>
    </div>
  </template>
  ```

  

- 作用域插槽：其实就是传参的默认插槽和具名插槽。子组件可以通过插槽传递数据给父组件，父组件接收后可以访问子组件数据（子组件->父组件）

  ```
  // 子组件 ： (假设名为：Son)
  <template>
    <div id='aaa'>
         <slot name='a' :value1='name1'>默认值a</slot> // 传递name1数据
         <slot name='b'>默认值b</slot>
         <slot name='c'>默认值c</slot>
    </div>
  </template>
  
  new Vue({
    el:'#aaa',
    data:{
      name1:'张三',
      name2:'李四'
    }
  })
  
  // 父组件：（引用子组件 Son）
  <template>
    <div class= 'app'>
       <Son>
       	<template v-slot:a = 'result1'>{{result1.value1}}</template> // 获取a插槽值，并赋值给result1。
       	<template v-slot:default='result2'>{{result2.value2}}</template> // 获取默认插槽值，并赋值给result2。
       </Son>
    </div>
  </template>
  ```

  总结来说就是：

  - 首先在子组件的`slot`上动态绑定一个值( `:key='value'`)
  - 然后在父组件通过`v-slot : name = ‘values ’`的方式将这个值赋值给 `values`
  - 最后通过`{{ values.key }}`的方式获取数据

还有一个注意点：`slot`插槽-新旧版本用法(`vue2.6.0`前后)

- 2.6之前：

  > 区别主要是：2.6.0为具名插槽和作用域插槽引入了一个新的统一的语法 (即 `v-slot` 指令)。它取代了 `slot` 和 `slot-scope`。
  >
  > 注意点：在`vue3` 中，被废弃的这两个，不会被支持即无效。

  - 默认插槽：

  - 具名插槽：父组件用`slot='name'`传递数据，子组件用`<slot name='name'>`接收

  - 作用域插槽：父组件用 `slot-scope='slot'`接收，子组件用v-bind`:data='value'`传递

    

实现原理：

首先要清除`vue`中父组件与子组件的关系（我认为父组件是相对而言的，只有在组件内使用到了其他组件，才有父子组件的概念。）

```js
// 注册一个全局组件
Vue.component('child',{
	props:['msgFather'],
	template:'<h1>{{msgFather}}</h1>
				<h1>{{msgChild}}</h1>',
	data(){return {msgChild:child}}
});
```

```js
// 此时，<Father>就是父组件，<child>就是子组件
<Father>
	<child msgFather='father'></child>
</Father>
```



原理：当子组件`vm`实例化后，获得到父组件传入的`slot`标签的内容，存放在`vm.$slot`中，默认插槽为`vm.$slot.default`，具名插槽为`vm.$slot.xxx`，`xxx`是插槽名。组件进行渲染时，遇到`slot`标签，使用`$slot`中内容进行替换，此时可以为插槽传递数据，若存在数据，则可称该插槽为作用域插槽