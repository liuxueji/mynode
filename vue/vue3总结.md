# 一、setup语法糖

> setup之前，变量必须return后才能在模板中使用，这样会导致代码冗余；
>
> 由于上面的原因，就有了setuo语法糖，只需要在script中添加setup，它会自动将变量、函数暴露给模板
>
> setup是比beforeCreate还要靠前的钩子函数

```
<template>
	<div>{{name}}</div>
	<div>{{person.name}}</div>
	<div>{{person.age}}</div>
</template>
<script setup>
// 从vue中引入 ref、reactive函数，作用：实现数据响应式
import {ref,reactive} from 'vue'

const name = ref('老王')

const person = reactive({
	name = '老张',
	age = 25
})
</script>
```

模板中可以直接使用setup中定义的变量
