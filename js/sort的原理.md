定义：

​	sort() 方法用于对数组的元素进行排序，并返回数组。默认排序顺序是根据字符串UniCode码。

语法：

​	arrayObject.sort(sortby)

参数：

​	sortby 可选，用来规定排序的顺序，但必须是**函数**。

代码示例：

例一：

> 如果我们直接对数组元素进行sort排序，会出现如下情况：

```
var arr =[0,1,56,23,34,3]
arr.sort()
console.log(arr)
//打印[0, 1, 23, 3, 34, 56]
```

> 为什么呢？因为sort排序，它会拿ASCLL值的第一个字符来比较，所以出现了这个结果。
>
> 需要如何修改呢？参数必须时函数

例二：

```
//升序
var arr =[0,1,56,23,34,3]
function sortNumber(a,b){
   return a-b;
}
arr.sort(sortNumber)
console.log(arr)//打印[0, 1, 3, 23, 34, 56]

//降序
var arr =[0,1,56,23,34,3]
function sortNumber(a,b){
   return b-a;
}
arr.sort(sortNumber)
console.log(arr)//打印[56, 34, 23, 3, 1, 0]
```

> 传入参数是方法即可

例三：

> 还可以比较数组对象的值

```
const arr2 = [
            { id: 10, flag: true },
            { id: 5, flag: false },
            { id: 6, flag: true },
            { id: 9, flag: false }
];
const r = arr2.sort((a, b) => b.flag - a.flag);//如果这里想根据id进行排序就改成.id
console.log(r);
// [
//     { id: 10, flag: true },
//     { id: 6, flag: true },
//     { id: 5, flag: false },
//     { id: 9, flag: false }
// ]

```

> 按照数组对象中某个属性值进行排序



原理：

V8引擎sort函数

之前给的排序方案是：InsertionSort和QuickSort，数量小于10，用插入排序，大于十用快排

现在给的排序方案是：冒泡和快速排序

最近好像由于快速排序不稳定，换成了混合排序（归并+...），总之不是十大排序中的

