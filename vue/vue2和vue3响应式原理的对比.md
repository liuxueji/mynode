## vue2与vue3的响应式原理

### vue2

- `vue2`响应式原理
  - 对象类型：通过`Object.definedProperty()`进行数据劫持，
    - `set`：新增数据调用
    - `get`：获取数据调用
  - 数组类型：通过重写更新数组的方法实现拦截
- 缺点：
  - 新增属性、删除属性，界面不会更新
  - 直接通过下标修改数组，界面不会自动更新

代码

```vue
<template>
  <div>
    <h1>姓名：{{person.name}}</h1>
    <h1 v-show="person.sex">性别：{{person.sex}}</h1>
    <h1 v-show="person.hobby">爱好：{{person.hobby}}</h1>
    <button @click="updataSex">新增性别</button>
    <button @click="deleteSex">删除性别</button>
    <button @click="updataHobby">修改数组</button>
  </div>
</template>

<script>

export default {
  data () {
    return {
      person: {
        name: '张三',
        hobby: ['吃饭', '睡觉']
      }
    }
  },
  methods: {
    updataSex () {
      // this.person.sex = '男' // 实际上改了，只是没有更新到DOM
      // 两种方法实现 响应式 新增数据
      // vue.set(this.person, 'sex', '男')
      this.$set(this.person, 'sex', '男')

    },
    deleteSex () {
      // delete this.person.sex // 实际上改了，只是没有更新到DOM
      this.$delete(this.person, 'sex')
      // vue.delete(this.person, 'sex')
    },
    updataHobby () {
      // this.person.hobby[0] = '学习'// 实际上改了，只是没有更新到DOM
      // 两种方法实现 响应式 更新数据
      // this.$set(this.person.hobby[0], 0, '学习')
      this.person.hobby.splice(0, 1) // 重写数组方法
    }
  },
}
</script>


```

![GIF4](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/GIF4.gif)

注意点：在`vue3`的项目中，不能使用`this.$set`，因为`vue3`导入的是`vue`的工厂函数`createApp`，里面没有包含`this.$set`，需要在`vue2`中执行

### vue3

- 实现原理
  - 通过`Proxy`：拦截对象属性的变化（属性值的：增删改查）
  - 通过`Reflect`：对被代理对象的属性进行操作
- 解决的问题
  - 解决了`vue2`中存在的问题（新增数据界面不更新）

### 实现原理

- 简单实现`vue2`的响应式

```
const p1 = {}
Object.defineProperty(p1,'name',{
	get(){
		return person.name
	},
	set(value){
		person.name = value
	}
})
```



- 简单实现`vue3`的响应式

```
const p1 = new Proxy(person,{
  // 数据被获取时，调用
  get(target,propName){
    return target[propName]
  },
  // 数据被修改或新增时，调用
  set(target,propName,value){
    target[propName] = value
  },
  // 数据被删除时，调用
  deleteProperty(target,propName){
    return delete target[propName]
  }
})
```

通过 `target[propName]`，可以拦截任意属性的变化，不需要像`vue2`中需要提前知道哪个属性被操作，到这里，数据的响应式已经实现了

但是vue底层并不仅仅是，找到`target[propName]`，然后操作数据，而是通过`Reflect`来操作数据

> `Reflect` 是一个内置的对象，它提供拦截 `JavaScript` 操作的方法
>
> 它存在的意义就是，如果操作对象出现报错，会返回`false`

改进`vue3`的响应式

```
const p1 = new Proxy(person,{
  // 数据被获取时，调用
  get(target,propName){
    return Reflect.get(target,propName)
  },
  // 数据被修改或新增时，调用
  set(target,propName,value){
    Reflect.set(target,propName,value)
  },
  // 数据被删除时，调用
  deleteProperty(target,propName){
    return Reflect.deleteProperty(target,propName)
  }
})
```

这才是`vue3`中实现响应式的原理

### 总结

Object.defineProperty

- 不能检测数组长度的变化
- 不能检测对象的添加
- 只能劫持对象属性，因此需要对对象进行遍历

Proxy

- 可以检测数组长度变化
- 可以检测对象的添加
- 可以代理整个对象，不需要对对象进行遍历