我们知道Chrome文字大小默认是16px，最小是12px

如果想要让文字小于12px，应该如何实现？

我们可以让字体离我们远一点，使用缩放的形式

```
display:inline-block;

-webkit-transform:scale(1.6)
```

需要注意的是：此时是以中心进行缩放，注意位置