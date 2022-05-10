对象是引用数据类型，属性是基本数据类型

例子：

```
console.log([1,2]===[1,2])// false
```

上述例子中，虽然两个数组是一模一样的，但是它们不是同一个数组，就比如：双胞胎长得很像，但是不是同一个人。它们的存放地址是在堆中，并且有两个数组的堆内存地址不一样，所以两个数组是不相同的。

如果是这样：

```
var obj1={
	a:1
}
var obj2=obj1
console.log(obj1===obj2) // true
```

这种情况是`true`，此时两个对象是指向同一个堆内存（此时可以认为是同一个人）。

因为是同一个对象，所以当其中一个对象的值修改时，另一个对象的值也会被修改，例如：

```
var obj1={
	a:1
}
var obj2 = obj1
obj2.a=2
console.log(obj1) // {a=2}

// 补充
(function(){
	console.log(a) // undefined，此时是局部变量的提升
	var a = 1
})
```

对象中，`key`值都是`string`，如果某一属性是对象，也会被转为`string`，例如：

```
var obj1={
	a:'111'
}
var obj2={
	// '{}':'333'  //下面的obj2[obj1]='333'，这里的对象属性会被转为string，打印[object Object]
}
obj2[obj1]='333'
for(var k in obj2){
	console.log(k) // [object Object]，此时它也是string，可以用typeof判断
	console.log(typeof k)// string
}
```

所以说，不管你传入的属性是指还是对象，最终输出键值都是`string`类型

那么如果有两个对象重复赋值，就会出现赋值覆盖的情况。如下：

```
var a = {}
var b = {
	k:'a'
}
var c = {
	k:'c'
}
a[b] = '1'
a[c] = '2'
console.log(a[b]) // 2
```

> 因为将对象以属性的形式存入对象中，会以`[object Object]`的形式存储，是`string`类型，那么再次存储一个`[object Object]`就会覆盖前面的。
>
> 上述代码会转为
>
> ```
> var a = {
> 	[object Object]:1 // 赋值
> 	[object Object]:2 // 赋值，覆盖上一个赋值
> }
> var b = {
> 	k:'a'
> }
> var c = {
> 	k:'c'
> }
> a[b] = '1'
> a[c] = '2'
> console.log(a[b]) // 2
> ```



原型的问题

1. 每一个函数都自带一个`prototype`，每一个new出来的对象都自带`__proto__`，且`prototype`与`__proto__`等价
2. `new fun` 该`fun`构造函数的原型指向对象（`new fun`）的原型

每一个对象都有方法，并且是通过`new`出来的一个对象。

```
function Array(){

}
new Array
// 虽然我们不会创建数组方法，和new数组方法，但是它是存在的，它帮我们new了。
console.log([1,2].constructor) // Array

function Fun(){

}
let obj = new Fun()
console.log(Fun.prototype === obj.__proto__) // true
```

图示：

![](C:\Users\Shinelon\Desktop\随堂笔记\image\Snipaste_2022-05-10_09-21-16.png)

执行的优先级是，优先执行自身的，自身没有，再去找原型上的

```
function Fun(){
	this.a = 'Fun函数自身'
}
Fun.prototype.a = 'Fun原型'
let obj = new Fun()
obj.a = '对象本身'
obj.__proto__.a = '对象原型'

console.log(obj.a) 
```

> 它们的执行顺序是：对象本身 => 对象原型 => Fun函数自身 => Fun原型 => 沿着原型链往上直到undefined

图示：

![](C:\Users\Shinelon\Desktop\随堂笔记\image\Snipaste_2022-05-10_09-26-04.png)



总结

1. 对象是通过`new`操作符构建出来的，所以对象之间不相等
2. 对象注意是引用类型，需要区别于基本类型
3. 对象的key都是字符串
4. 对象找属性或方法的顺序是：对象本身 => 对象原型 => `Fun`函数自身 => `Fun`原型 => 沿着原型链往上直到`undefined`