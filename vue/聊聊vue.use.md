### 介绍

先看看官方如何介绍vue.use：

通过全局方法 Vue.use() 使用插件，Vue.use 会自动阻止多次注册相同插件，它需要在你调用 new Vue() 启动应用之前完成，Vue.use() 方法至少传入一个参数，该参数类型必须是 Object 或 Function，如果是 Object 那么这个 Object 需要定义一个 install 方法，如果是 Function 那么这个函数就被当做 install 方法。在 Vue.use() 执行时 install 会默认执行，当 install 执行时第一个参数就是 Vue，其他参数是 Vue.use() 执行时传入的其他参数。就是说使用它之后调用的是该组件的install 方法。

简而言之，use会调用install方法，install方法中包含component()

use的内部运作，分为三步：

- 第一步。检查传入的是否被use过，被use过就不会重复执行（不做无用功）
- 第二步。检查是否有install方法，如果有，那么调用该方法
- 第三步。如果没有insall方法，检查传入的是否为函数，如果是函数，那么调用该方法

若以上三步都不符合，那么就不调用，即use不执行



再简单说一下vue中，如何全局注册组件，也是分为三步：

- 创建组件
- 引入组件
- 调用Vue.componen('组件名',组件)
