作用域很容易理解，顾名思义就是属性的作用范围，在`js`中还有作用域链的问题。

### 知识点：

- 作用域只存在函数内

- 作用域的存在，使得作用域内部可以调用作用域外部的属性（单向）

更细节的部分

- 提升机制。方法与`va`r定义的变量名会提升，方法是整体提升，变量是变量名提升
- 默认全局。如果变量没有设置var，那么该变量就是全局`window`的，当然如果设置`var a`，那么a就属于作用域的
- 优先级。声明变量>声明普通函数>参数>变量提升



通过代码理解

```js
function fun(){
	console.log(a) // undefined
	var a = 10
	console.log(a) // 10
}
```

> 因为a会变量提升，所以会变成这样，第一次输出时，`a`只声明未赋值，所以时`undefined`
>
> ```js
> function fun(){
> 	// var a
> 	console.log(a) // undefined
> 	var a = 10
> 	console.log(a) // 10
> }
> ```



```js
var a = 100
function fun(){
	console.log(a) // 100
	var a = 10
	console.log(a) // 10
}
```

> 第一个`a`输出的是全局变量`a`的值



```js
function fun(){
	var a = b = 10
	console.log(b)
}
console.log(a) // 报错，作用域外不能获取作用域内部的值
console.log(b) // 10
```

> `a`报错应该很好理解，为什么`b`能输出10呢？因为`b`没有`var`定义，所以它不属于作用域，属于全局变量`window`，所以能正常输出，上面的代码等于：
>
> ```js
> var b
> function fun(){
> 	var a = b = 10
> 	console.log(b)
> }
> console.log(a) // 报错，作用域外不能获取作用域内部的值
> console.log(b) // 10
> ```



```js
function fun(){
	var a = 100
	a = 10
	console.log(a)//10，此时a是局部变量，属于变量的再次赋值
}
==================================
function fun(){
	a = 10
	console.log(a)//10，此时a是局部变量，因为var a会变量提升
	var a = 100
	console.log(a)//100，此时a是局部变量
	
}
===================================
function fun(){
	a = 10
	console.log(a)//10，此时a是全局变量
}
```

> 特别注意变量提升。看代码首先要看是否有变量提升！

```js
function fun(){
	var a = 1
	function fun1(){
		console.log(a) // undefined
		var a = 2
		console.log(a) // 2
	}
	fun1()
	console.log(a) // 1
}
fun()
```

>还是注意变量提升。首先，`fun1()`中的`var a` 会提到作用域最上面，然后因为局部变量优先级是大于全局变量的，第一个打印`a`时，`a`还没赋值，所以是`undefined`。第二次打印时，`a`已经赋值了，所以是`2`。第三次打印时，找到作用域中变量，打印`1`。



```js
var a = 'a'
function fun(){
	if(typeof a == 'undefined'){
		var a = 'b'
		console.log('nnn'+a)
	}else{
		console.log('mmm'+a)
	}
}
fun()
```

> 结果为：`nnnb`。我们还是先来找变量提升，在`if`内定义了`var a`，所以存在变量提升（注意if代码块中不属于作用域），所以会将`var a`提升到作用域最上面，然后进行`if`判断，发现`a`为`undefined`，所以输出`nnnb`。
>
> 提升后代码：
>
> ```js
> var a = 'a'
> function fun(){
> 	var a
> 	if(typeof a == 'undefined'){
> 		a = 'b'
> 		console.log('nnn'+a)
> 	}else{
> 		console.log('mmm'+a)
> 	}
> }
> fun()
> ```
>
> 



