# vuex.$store中的dispatch

## 理解

**this.$store.dispatch 分发 actions -> 调用 mutations -> 改变states**

## 思考

- 为什么不直接分发mutation

  > mutation必须是同步的，而actions可以在内部执行异步操作，调用接口都是异步操作，所以要使用dispatch分发actions调用mutations改变states