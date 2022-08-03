### 属性

有四个属性 state、getters、mutations、actions、modules

- state：类似 data，存储数据

- getters：类似 computed

- mutations：类似 methods

- actions：用于提交mutations

  >也可以写方法，但是一般情况方法只写在 mutations中

- modules：细分上面四个属性，便于管理

  > 例如一个很大的项目，用上面四个属性还不够，需要再细分进行管理，就需要用到modules

### 区别

actions与mutations的区别

> actions 可以包含异步操作
>
> mutations 只能包含同步操作

### 数据流

vuex是单向数据流的

### 持久化存储

> vuex本身不是持久化存储的，但是可以通过一些方法实现持久化存储

1. 使用localStorage
2. 使用vuex-persist插件