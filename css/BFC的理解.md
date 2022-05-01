`BFC`（Block Formatting Context），即块级格式化上下文，它是页面中的一块渲染区域，并且有一套属于自己的渲染规则

`BFC`的目的是形成一个相对于外界完全独立的空间，让内部子元素不会影响到外部的元素

我们可以通过一些条件触发`BFC`，例如：

1. 根元素`html`
2. `float`为`left`、`right`
3. `overflow`不为`visible`，为`auto`、`scroll`、`hidden`
4. `display`的值为`inline-block`、`inltable-cell`、`table-caption`、`table`、`inline-table`、`flex`、`inline-flex`、`grid`、`inline-grid`
5. `position`值为`absolute`、`fixed`

`BFC`特性：

1. 属于同一个`bfc`的两个相邻容器的上下`margin`会重叠，解析两者的较大值
2. 计算`bfc`高度时，浮动元素也要参与计算
3. `bfc`的区域不会与浮动容器发生重叠
4. `bfc`内的容器在垂直方向上依次排列
5. 元素的`margin-left`与其包含块的`border-left`接触
6. `BFC`是独立容器，容器内部元素不会影响外部元素

`BFC`功能总结：

1. 解决两个相邻元素的上下`margin`重叠问题
2. 解决高度塌陷问题
3. 实现多栏布局