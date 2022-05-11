## 分析new

我们来看看`new`操作符具体做了什么

首先我们创建一个方法，输出方法并执行

```
    function fun() {

    }
    console.log(fun())
```

> 此时，输出的是`undefined`

那我们通过给它添加`new`关键字，看看会如何变化

```
    function fun() {

    }
    console.log(new fun())
```

> 此时，输出`fun{}`，多了个空对象。
>
> 由此，可以得出，`new`关键字会创建一个空对象

接下来，判断一下`fun()`与`new fun()`原型的关系

```
    function fun() {

    }
    console.log(new fun().__proto__ === fun.prototype)
```

> 返回的结果是`true`
>
> 由此，我们可以得出 `new` 关键字会将 空对象的原型指向构造函数的原型

接下来，判断我们看看`this`的指向。

首先，方法内的this肯定是指向`window`的。如果给方法添加了`new`关键字后，`this`会改变吗？

```
    function fun() {
      this.name = 'zs'
      console.log(this)
    }
    fun()
```

> 此时，`this`指向`window`

```
    function fun() {
      this.name = 'zs'
      console.log(this)
    }
    new fun()
```

> 此时，`this`指向`fun`
>
> 由此，我们可以得出 `new` 关键字会改变`this`指向，将空对象的`this`指向构造函数

最后，我们看看如果方法内有`return`，那么`new`会做什么

当我们`return`一个基本类型时

```
    function fun() {
      // return 123
      // return 'a'
      return true
    }
    console.log(new fun())
```

> 输出的还是空对象，说明`new`没有做任何操作

当我们`return`一个引用类型时

```
    function fun() {
      return []
      // return {}
    }
    console.log(new fun())
```

> 此时，输出的是`[]`，说明`new`给我们返回的是我们`return` 的引用类型
>
> 由此，我们可以得出new关键字对构造函数有返回值时，会判断是引用类型还是基本类型，如果是引用类型，那就直接用你`return`的

所有，`new`大致做了四个操作：

1. 创建了一个空对象
2. 将空对象原型指向构造函数原型
3. 将空对象this指向构造函数
4. 对构造函数有返回值的处理进行判断

## 自定义new

### myNew()

说明：

> 模拟构造函数创建实例对象的过程

- 语法：`myNew(Fn,...args)`
- 功能：创建Fn构造函数的实例对象

实现：

> 我们实现的结构：`let obj = myNew(Person,'zs',20)`，`Person`是构造函数

- 结构：函数接收的参数，第一为构造函数，后面的参数为实例化需要的参数

- 功能实现：

  1. 创建新对象

  2. 修改新对象的原型对象。函数的显示原型给对象隐式原型

  3. 修改函数内部`this`指向新对象，并执行。通过`call()`执行函数并修改`this`

  4. 返回新对象

     > 判断返回结果，通过`instanceof`判断是否为`Object`对象，如果是对象类型的就返回`result`，如果不是对象类型的就返回新对象。


代码：

`myNew.js`

```js
function myNew(Fn, ...args) {
  // 创建对象
  let obj = {}
   // 修改新对象的原型
  obj.__proto__ = Fn.prototype
  // 修改this指向，并执行
  let result = Fn.call(obj, ...args)
  // 返回新对象
  return result instanceof Object ? result : obj
}
```

`myNew.html`

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script src="./myNew.js"></script>
</head>

<body>
  <script>
    function Person(name) {
      this.name = name
      return {
        age: 18
      }
    }

    let obj = myNew(Person, 'zs')
    console.log(obj)
  </script>
</body>

</html>
```

## 