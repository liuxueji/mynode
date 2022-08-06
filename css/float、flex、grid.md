### 前言

1. 什么是布局？

   布局就是把页面分成一块一块的，然后按照左中右、上中下进行排列

2. 布局分类

   1. 固定宽度布局：常见的宽度有960px、1000px、1024px
   2. 不固定宽度布局：靠文档流的原理进行
   3. 响应式布局：根据窗口大小改变宽度

### float布局

> 可以让块级元素在一行上显示

1. 给子元素添加

   ```
   float:left;
   width:xxpx;
   ```

2. 给父元素添加

   ```
   .clearfix::after{
     content:'';
     clear:both;
     overflow:hidden;
   }
   ```

### flex布局

> 分为container和item

#### container属性

> ```
> display:flex;
> 
> flex-direction:row/row-reverse/column/column-reverse;
> 
> flex-wrap:wrap/no-wrap/wrap-reverse;
> 
> justify-content:center/flex-start/flex-end/space-between/space-around;
> 
> align-items:center/flex-start/flex-end;
> ```

1. 将容器改为flex容器

   ```
   .container{
     display:flex;
   }
   ```

2. 改变主轴方向

   > 常用属性是**column**

   ```
   .container{
     flex-direction:row/row-reverse/column/column-reverse;
   }
   ```

   ![image-20220806181429298](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/img/image-20220806181429298.png)

3. 换行

   > 常用属性是**wrap**

   ```
   .container{
     flex-wrap:wrap/no-wrap/wrap-reverse;
   }
   ```

   ![image-20220806181545458](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/img/image-20220806181545458.png)

4. 主轴对齐方式

   > 常用属性**center**、**space-between**

   ```
   .container{
     justify-content:center/flex-start/flex-end/space-between/space-around;
   }
   ```

   ![](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/img/image-20220806181801711.png)

5. 次轴对齐方式

   > 常用属性**center**
   >
   > 当然，如果侧轴有多个的话要用**align-content**

   ```
   .container{
     align-items:center/flex-start/flex-end;
   }
   ```

   ![image-20220806182019323](https://liuxueji.oss-cn-guangzhou.aliyuncs.com/img/image-20220806182019323.png)

#### items属性

>```
>order
>flex-grow
>flex-shrink
>```

### grid布局

#### 1

container：五行三列

```
 .container {  
            display: grid;
            grid-template-columns: 40px 50px auto 50px 40px;
            grid-template-rows: 60px 100px 50px;
 }
```

items：这个就是header

```
.a {
            grid-column-start: 1;
            grid-column-end: 6;
            grid-row-start: 1;
            grid-row-end: 2;

 }
```

#### 2.header、aside、main、ad、footer经典布局

container

```
    .container {
            display: grid;
            grid-template-rows: 60px auto 60px;
            grid-template-columns: 300px auto 200px;
            border: 1px solid red;
            grid-template-areas:
             "header header header "
             "aside main ad" 
            ". footer footer "; 
           min-height: 100vh; 
           /* grid-gap: 10px; */ 
           grid-row-gap: 10px; 
           grid-column-gap: 10px; 
       }
```

items

```
.a {
       grid-area: header;
 }                
.b {
       grid-area: aside;
 }                
.c {    
       grid-area: main;
}               
 .d {            
       grid-area: ad; 
}               
 .e {           
       grid-area: footer
}
```

