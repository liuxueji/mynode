函数执行与不执行区别很大，例如：

```
function fun(){
	console.log(111)
	return '222'
}
console.log(fun) // 方法没有执行，打印函数体
console.log(fun()) // 111 222，此时方法执行，方法体与返回值都要执行
console.log(new fun())// 111 fun{}，方法体执行，并且返回的一定是一个对象，因为new fun()就是一个对象，相当于obj = new fun()  console.log(obj)，如果方法内有 this.a = '333' 那么方法体内就有fun{a:333}

```

如果方法内有`this.a=333`，那么这里的this如何指向呢

```
function fun(){
	this.a = '333'
	console.log(111)
	return '222'
}
console.log(fun) // 
console.log(fun()) // 111 222，此时this是指向window，因为是fun()在全局作用域执行
console.log(new fun())// 111 fun{a:333}，此时this指向的是对象

```

综合题：

```
function Foo(){
	getName = function(){console.log(1)}
	return this
}
Foo.getName = function(){console.log(2)}
Foo.prototype.getName = function(){console.log(3)}
var getName = function(){console.log(4)}
function getName(){
	console.log(5)
}
```

我们讨论一下以下输出

`Foo.getName()`没有执行的情况，直接对号入座

```
Foo.getName() // 2，因为Foo()没有执行，所以直接找Foo.getName()，执行了就是1
getName() // 4，还是一样，Foo()没有执行，直接找getName()。那么为什么是4不是5呢？因为虽然变量和方法都会提升，但是变量提升优先级会更高一些，所以属性和方法重名和时，永远都是执行属性的值。
```

`Foo.getName()`执行的情况

```
Foo().getName() // 1，因为Foo执行了，所以在Foo()内找getName
getName() // 1，同样是因为上面执行了Foo()，所以它就会在函数体内找
new Foo().getName() // 3，首先我们要知道对象执行顺序：对象本身 => 构造函数本身 => 对象原型 => 构造函数原型 => Object。首先对象本身没有，再看构造函数本身，有getName但是它是指向window的，再看对象原型也没有，再看构造函数原型，有getName为3，所以就打印3
```

```
Foo.getName() // 2，会执行1，因为下面方法执行了，会将方法提升
getName() // 4
Foo().getName() // 1
getName() // 1
new Foo().getName() // 3
```





再看一题

```
var o = {
	a:1,
	b:{
		fn:function(){
			console.log(this.a) // undefined
			console.log(this) // b{}	
		}
	}
}
o.b.fn()
```

> 我们需要知道谁指向`fn`，那么this就指向谁。这里指向`fn`的是`b`，所以`this`是指向`fn`的。
>
> 那么就能得出，`this`指向`b`，`b`中没有`a`值，所以`this.a`为`undefined`