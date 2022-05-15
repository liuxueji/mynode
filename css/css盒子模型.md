首先，css盒子模型有两种，分别为：标准盒子模型、IE盒子模型

区别：

- 标准盒子模型：margin、padding、border、content
- IE盒子模型：margin、content（padding、border、content）

当然，我们也可以将它们进行转换，通过css的box-sizing

- box-sizing: content-box; // 转为标准盒子模型
- box-sizing: border-box; // 转为IE盒子模型