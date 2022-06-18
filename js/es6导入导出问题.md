做项目时发现一个问题：a.js导出一个方法，b.js导入a导出的方法，输出后不是想要的结果

a.js

```
export const demo=()=>{
    let text = 'helloWorld';
    return text
}
```

b.js

```
import { a } from './a.js';
console.log(a)
```

原因：demo是一个方法，a.js只是将demo方法导出，没有执行；b.js中调用后，直接输出的只是demo方法，不是执行后的值。

若要获得a.js中的demo方法，需要执行后再输出，可以在导入后执行，也可以导出前执行！