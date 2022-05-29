首先说说position有哪些值

- static：默认，没有定位
- fixed：固定定位
- relative：相对定位
- absolute：绝对定位



static是默认值，使用场景是：如果有已经存在的position值，想要去除，那么久可以使用static

fixed是相对于浏览器窗口的，它不是相对于文档的，鼠标滚动它不动

relative与absolute区别：

- relative不脱离文档流，absolute脱离文档流（原来的位置不在了）
- relative相对于自身，absolute相对于第一个有relative的父元素