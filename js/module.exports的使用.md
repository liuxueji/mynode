## 前言

> 学习过程中经常会遇到`export`、`export default`、`module.exports`，因为没有去对比总结，所以经常记混，有时间需要花时间来整理对比它们的区别。这篇文章主要是对`module.exports`用法的介绍。

## 介绍

`module.exports` 对象是由模块系统创建的。在我们自己写模块的时候，需要在模块最后写好模块接口，声明这个模块对外暴露什么内容，`module.exports` 提供了暴露接口的方法。

## 使用

1. #### 返回一个`JSON Object`

   ```js
   var obj = {
   	name: 'Lxj',
   	age: 20,
   	sayName: function(name){
   		console.log(this.name);
   	}
   }
   module.exports = obj;
   ```

   这种方法可以返回全局共享的变量或者方法。
   调用方法：

   ```js
   var obj = require('./obj.js');//导入文件，该文件就是一个对象，将该对象赋值为 obj，这样更容易理解
   obj.sayName('hello');//hello
   ```

2. #### 返回一个方法

   ```js
   var func1 = function() {
      console.log("func1");
   };
   exports.function1 = func1;
   ```

   调用方法：

   ```js
   var functions = require("./functions.js");
   functions.function1();
   ```

3. #### 返回一个构造函数

   ```js
   var CLASS = function(args){
   	 this.args = args;
   }
   module.exports = CLASS;
   ```

   调用：

   ```js
   var CLASS = require('./CLASS.js');//导入文件，该文件暴露了构造函数，所以用CLASS接收后也为构造函数
   varc = new CLASS('args');
   ```

4. #### 返回一个实例对象

   ```js
   //CLASS.js
   var CLASS = function(){
   	this.name = "class";
   }
   CLASS .prototype.func = function(){
   	alert(this.name);
   }
   module.exports = new CLASS();
   
   ```

   调用

   ```js
   var c = require('./CLASS.js');
   c.func();//"class"
   ```

   

