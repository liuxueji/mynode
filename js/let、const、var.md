let、const、var的区别

## 区别一：

​	var存在变量提升

​	let、const不存在变量提升

![](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/20220520091806.png)

​	

![](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/20220520091832.png)

> 可以看出，var a 进行了变量提升，但是仅对声明变量提升，赋值并没有提（函数也存在变量提升，不同的是，函数是整体提升）

![](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/20220520092121.png)

![](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/20220520092132.png)

![image-20220520092429574](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220520092429574.png)

![image-20220520092441970](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220520092441970.png)

> 可以看出，let和const没有进行变量提升，所以输出变量a，在定义会报错



## 区别二：

​	var、let值可以被修改

​	const值不能被修改

​	![image-20220520093216139](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220520093216139.png)

![image-20220520093239475](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220520093239475.png)

![image-20220520093255188](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220520093255188.png)

> 当用var和let赋值时，a的值可以被修改，但是使用const赋值，a值不能被修改。所以，使用var、let定义的我称之为变量，使用const定义的我称之为常量。但是，如果是引用类型的话，const可以修改值，例如：

![image-20220520093641340](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220520093641340.png)

> 可以理解为，arr是常量，但是里面的数据没有定义为const，所以还是变量。



## 区别三：

​	var没有自己的作用域

​	let、const有自己的作用域

举例：

![](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220520093916989.png)

![image-20220520093949174](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220520093949174.png)

![image-20220520094004636](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220520094004636.png)

> 可以看出，var没有自己的作用域，let和const有作用域，基于这个问题，有一道经典面试题，题目如下：回答它们的输出结果

![image-20220520094318224](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220520094318224.png)

![image-20220520094432220](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220520094432220.png)

![image-20220520094530092](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220520094530092.png)

> 因为var没有块级作用域，是全局存在的，并且function是复杂数据类型，会存在堆中，所以等i执行完了，才去输出i的值，所以此时 i 为 2。设置为let时，function也会在堆中存储，当for循环执行完了，才回去执行function里面的输出，不同的是，每当我们循环一次，let就会产生一个块级作用域，所以最终输出 0 1。



## 区别四：

​	var 可以多次声明同一个变量

​	let、const 不能多次声明同一个变量

![image-20220520095137849](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220520095137849.png)



![image-20220520095153139](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/image-20220520095153139.png)

> 可以看出，var多次声明变量，会出现覆盖的情况。

