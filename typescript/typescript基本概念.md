## typescript介绍

### 什么是typescript？

typescript是javascript的扩展语言，有点类似Less和css之间的关系，使用时需要将typescript转化为javascript。

### 为什么需要typescript？

原因有很多：

- 因为typescript有静态检查，提高了高效开发
- 提供类型检查，避免javascript后期出现的bug
- 有强大的类型系统
- 支持js新特性

### typescript特点？

一门语言在编译时报错，那么就是静态类型语言，如果在运行时报错，那么就是动态语言。

所以javascript属于动态语言，typescript属于静态语言，但是typescript要兼容javascript，所以它会容忍隐式转换，不完全是强类型语言，这点区别于其他后端语言。

# 搭建typescript

## 安装最新typescript

`npm i -g typescript`

## 安装te-node

`npm i -g ts-node`

## 创建一个tsconfg.json文件

`tsc --init`

## 执行index.ts

新建index.ts，通过 `ts-node index.ts` 执行

# 基础数据类型

## 内置八种类型

```js
let str:string = 'zhangsan';
let num:number = 18;
let bool:boolean = false;
let u:undefined = undefined;
let n:null = null;
let obj:object = {a:1};
let big:bigint = 100n;
let sym:symbol = Symbol('me');
```

# 其他类型

## Array

### 对数组的定义有两种方式

```
let arr:string[] = ["1","2","3"];
let arr2:Array<string> = ["1","2","3"];
```

### 定义联合类型数组

```
// 定义了arr数组，可以存储数值类型，也可以用来存储字符串类型
let arr:(number|string)[];
```

### 定义对象类型数组

```
// interface是接口
interface Arr{
	name:string,
	age:number
}
let arr:Arr[]=[{name:"zhangsan",age:18}]
```

## 函数

### 函数声明

```
function sum(x:number,y:number):number{
	return x+y
}
```

### 函数表达式

## Tuple元组

### 元组定义

数组由同种类型的值组成，如果我们需要在数组中存放不同类型的值，这时就需要用到元组。元组时typescript中特有的值，工作方式类似js中的数组

## 使用

```
let a:[number,number,string]
// 类型必须匹配，个数必须是3个
a = [1,2,'a'] //正确
a = [1,2,1] // 不对
a = [1,'a'] // 不对
```

## void

void表示没有任何类型，和其他类型是平等关系，不能直接赋值：

```
let a:void
let b:number = a // 错误
```

只能赋值 null和undefined。

一般不会给变量赋值void，没什么意义。

一般给没有返回值的函数去声明

## never

never类型表示的是那些用不存在的值的类型

值会用不存在的两种情况：

- 函数执行时抛出异常
- 死循环，使得程序永远无法运行到函数返回值，就不存在返回值了。

never类型和null、undefined一样，也是任何类型的子类型，可以赋值给任何类型。

## any

any类型顾名思义任何类型。所有的类型都可以归为any类型。所以any类型是所有类型的父类。

在typescript中使用了any类型，就相当于在用javascript了，因为any允许被赋值为任意类型

如果变量声明时，未指定其类型，那么它会被识别为任意类型。

`let a` <==> `let a:any`

总之，使用了any，typescript就没什么意义了，所以，尽量不要用any。

## unknown

> unknown的出现是为了解决any带来的问题。在typescript 3.0 引入的

unknown与any一样，都可以被赋值为任意类型

unknown与any的区别：

- unknown：只能赋值为unknown和any
- any：可以赋值为任意类型

如果我们需要使用unknown，就要缩小类型，不然无法操作unknown类型。

## Number、String、Boolean、Symbol

这里有个概念和java中很像：基本类型和包装类型

如果是小写的：number,string,boolean,symbol那就是原始类型

如果是大写的：Number,String,Boolean,Symbol那就是包装类型

需要注意：包装类型是包含原始类型的，所以可以将原始类型赋值给包装类型，反之不行。

## object、Object、{}

object代表所有非原始类型（非number、string、boolean、symbol），所以原始类型不能赋值给object，可以赋值 空对象 {}

> 在严格模式下，`null` 和 `undefined` 类型也不能赋给 object

Object代表所有拥有 toString、hasOwnProperty 方法的类型，所以所有原始类型、非原始类型都可以赋给 Object

> 在严格模式下，null 和 undefined 类型也不能赋给 Object

Object是包含object，Object包含原始类型，object不包含原始类型

{}和Object有点类似，也是表示原始类型和非原始类型的集合

> 在严格模式下，null 和 undefined 类型也不能赋给 {}

# 接口

在typescript中，我们使用接口（interfaces）来定义对象的类型

## 什么是接口

接口是面向对象的一个概念。接口（interfaces）对行为进行抽象，需要通过类（class）去实现（implement）

# 泛型

## 介绍

是一种把明确类型的工作推迟到创建对象或者调用方法的时候才去明确的特殊的类型

# tsconfig.json

## 介绍

tsconfig.json是typescript的配置项，如果一个目录存在tsconfig.json，那么该目录就是typescript的根目录

我们可以通过配置tsconfig.json来将typescript转为 ES6、ES5、node

## tsconfig.json重要字段

- files - 设置要编译的文件的名称
- include - 设置需要进行编译的文件，支持路径模式匹配
- exclude - 设置不需要进行编译的文件，支持路径模式匹配
- compilerOptions - 设置与编译流程相关的选项

## compilerOptions选项

```js
{
  "compilerOptions": {
  
    /* 基本选项 */
    "target": "es5",                       // 指定 ECMAScript 目标版本: 'ES3' (default), 'ES5', 'ES6'/'ES2015', 'ES2016', 'ES2017', or 'ESNEXT'
    "module": "commonjs",                  // 指定使用模块: 'commonjs', 'amd', 'system', 'umd' or 'es2015'
    "lib": [],                             // 指定要包含在编译中的库文件
    "allowJs": true,                       // 允许编译 javascript 文件
    "checkJs": true,                       // 报告 javascript 文件中的错误
    "jsx": "preserve",                     // 指定 jsx 代码的生成: 'preserve', 'react-native', or 'react'
    "declaration": true,                   // 生成相应的 '.d.ts' 文件
    "sourceMap": true,                     // 生成相应的 '.map' 文件
    "outFile": "./",                       // 将输出文件合并为一个文件
    "outDir": "./",                        // 指定输出目录
    "rootDir": "./",                       // 用来控制输出目录结构 --outDir.
    "removeComments": true,                // 删除编译后的所有的注释
    "noEmit": true,                        // 不生成输出文件
    "importHelpers": true,                 // 从 tslib 导入辅助工具函数
    "isolatedModules": true,               // 将每个文件做为单独的模块 （与 'ts.transpileModule' 类似）.

    /* 严格的类型检查选项 */
    "strict": true,                        // 启用所有严格类型检查选项
    "noImplicitAny": true,                 // 在表达式和声明上有隐含的 any类型时报错
    "strictNullChecks": true,              // 启用严格的 null 检查
    "noImplicitThis": true,                // 当 this 表达式值为 any 类型的时候，生成一个错误
    "alwaysStrict": true,                  // 以严格模式检查每个模块，并在每个文件里加入 'use strict'

    /* 额外的检查 */
    "noUnusedLocals": true,                // 有未使用的变量时，抛出错误
    "noUnusedParameters": true,            // 有未使用的参数时，抛出错误
    "noImplicitReturns": true,             // 并不是所有函数里的代码都有返回值时，抛出错误
    "noFallthroughCasesInSwitch": true,    // 报告 switch 语句的 fallthrough 错误。（即，不允许 switch 的 case 语句贯穿）

    /* 模块解析选项 */
    "moduleResolution": "node",            // 选择模块解析策略： 'node' (Node.js) or 'classic' (TypeScript pre-1.6)
    "baseUrl": "./",                       // 用于解析非相对模块名称的基目录
    "paths": {},                           // 模块名到基于 baseUrl 的路径映射的列表
    "rootDirs": [],                        // 根文件夹列表，其组合内容表示项目运行时的结构内容
    "typeRoots": [],                       // 包含类型声明的文件列表
    "types": [],                           // 需要包含的类型声明文件名列表
    "allowSyntheticDefaultImports": true,  // 允许从没有设置默认导出的模块中默认导入。

    /* Source Map Options */
    "sourceRoot": "./",                    // 指定调试器应该找到 TypeScript 文件而不是源文件的位置
    "mapRoot": "./",                       // 指定调试器应该找到映射文件而不是生成文件的位置
    "inlineSourceMap": true,               // 生成单个 soucemaps 文件，而不是将 sourcemaps 生成不同的文件
    "inlineSources": true,                 // 将代码与 sourcemaps 生成到一个文件中，要求同时设置了 --inlineSourceMap 或 --sourceMap 属性

    /* 其他选项 */
    "experimentalDecorators": true,        // 启用装饰器
    "emitDecoratorMetadata": true          // 为装饰器提供元数据的支持
  }
}


```

