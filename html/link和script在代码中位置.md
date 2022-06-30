在页面中link标签和script标签都是有固定的放置位置的

- link标签的位置

  link标签放在head标签后面，这样可以让页面逐步渲染，防止页面出现白屏或者没有样式的情况，提高用户体验

- script标签的位置

  我们知道script在解析js时，会阻塞HTML解析，所以把script放在/body标签之前，可以让HTML优先呈现给用户，防止HTML阻塞，影响用户体验。如果要将script放在head标签中，可以使用defer属性。

  >defer 属性规定当页面已完成加载后，才会执行脚本。

