其实我们前端程序员现在能见到的应该是MVVM，它很好的解决了model和view耦合的问题

我在学pring框架的时候，学过MVC，model：负责存储数据；View：负责展示视图；Contoller：负责业务逻辑。所以controller是核心

然后MVVM也差不多，model：存储数据；View：视图；ViewModel：DOM操作(核心)。vm是核心，通过实现一套数据相应机制自动响应数据变化，然后展示到视图中；也可以监听视图数据，然后实时修改数据