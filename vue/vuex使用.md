# vuex的使用步骤：

安装流程省略

## 一、状态state

### 1. 管理状态

```
// 初始化vuex对象
const store = new vuex.Store({
  state: {
    // 管理数据
    count: 0
  }
})
```

## 2.获取状态

#### 2.1原始用法插值表达式

```
<div>A组件 state的数据：{{$store.state.count}}</div>
```

#### 2.2使用计算属性

```
// 把state中数据，定义在组件内的计算属性中
computed: {
  // 1. 最完整的写法
  // count: function () {
  //   return this.$store.state.count
  // },
  // 2. 缩写
  count () {
    return this.$store.state.count
  }
}
// 不能使用剪头函数  this指向的不是vue实例

```

#### 2.3映射mapState

> 映射的目的是简化操作

步骤：

- 导入mapState

  ```
  import { mapState } from 'vuex'
  ```

- 对象形式使用：mapState(对象)

  ```
  // 使用mapState来生成计算属性  mapState函数返回值是对象
  // 使用mapState使用对象传参
  // computed: mapState({
  //   // 1. 基础写法 (state) 代表就是vuex申明的state 
  //   // count: function(state) {
  //   //   return state.count
  //   // }  
  //   // 2. 使用箭头函数
  //   // count: state => state.count
  //   // 3. vuex提供写法 (count是state中的字段名称)
  //   count: 'count',
  //   // 4. 当你的计算属性 需要依赖vuex中的数据 同时  依赖组件中data的数据
  //   count (state) {
  //     return state.count + this.num
  //   }
  // })
  
  ```

- 数组形式使用：mapState(数组)

  ```
  // 2、mapState参数是一个数组
  // computed: mapState(['count', 'total'])
  
  ```

- 如果组件自己有计算属性，state的字段映射成计算属性

  ```
  // 3、即在内部保留原有的计算属性，又要把store中的数据映射为计算属性
  computed: {
    // 组件自己的计算属性
    calcNum () {
      return this.num + 1
    },
    // 把mapState返回值那个对象进行展开操作（把对象的属性添加到该位置）
    ...mapState(['count'])
  }
  
  ```

  

## 二、状态修改mutations

```
目标：Vuex规定必须通过mutation修改数据，不可以直接通过store修改状态数据。

为什么要用mutation方式修改数据？Vuex的规定

为什么要有这样的规定？统一管理数据，便于监控数据变化
```

### 1.定义状态修改函数

```
// mutations是固定的，用于定义修改数据的动作（函数）
mutations: {
    // 定义一个mutation，用于累加count值
    // increment这个名字是自定义的
    increment (state, payload) {
        // state表示Store中所有数据
        // payload表示组件中传递过来的数据
        state.count = state.count + payload
    }
}

```

### 2.组件内调用

```
handleClick () {
  // 从js语法角度，是否可以这样修改对象的属性值？可以
  // 但是我们不应该这样修改数据（Vuex不建议这样修改数据）
  // this.$store.state.count++
  // 那么Vuex建议如何修改数据？通过mutation修改
  // 触发mutation
  this.$store.commit('increment', 5)
}

```

### 3.mapMutations

- 把vuex中的mutations的函数映射到组件的methods中

- 通俗：通过mapMutations函数可以生成methods中函数

  ```
  methods: {
      // 1、对象参数的写法
      // ...mapMutations({
      //   // 冒号右侧的increment是mutation的名称
      //   // 冒号左侧的increment是事件函数的名称，可以自定义
      //   increment: 'increment'
      // })
      // 2、数组参数的写法（事件函数名称和mutation名称一致）
      ...mapMutations(['increment'])
      // 3、这种写法和第2种等效
      // increment (param) {
      //   // 点击触发该函数后要再次触发mutation的
      //   this.$store.commit('increment', param)
      // }
  }
  
  ```

  



以上就是vuex的基本使用！