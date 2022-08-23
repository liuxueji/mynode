闪动的原因是因为Vue代码没有被解析，没有 控制页面中DOM的显示，所以会看到模板字符串等代码

#### 解决办法

在css中添加`v-cloak`，同时在待编译的标签上添加v-cloak属性：

```
[v-cloak] {display:none;}

<div>
  {{message}}
</dv>
```

