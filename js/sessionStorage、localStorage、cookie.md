公共点：在客户端存放

区别：

1. 数据存放有效期：
   1. sessionStorage：浏览器关闭数据就被清除
   2. localStorage：除非主动删除，否则不会被清除
   3. cookie：在设置的过期时间之前，不会被清除。必须在线上设置cookie。存储大小4k左右，有较多限制

用法：

- sessionStorage、localStorage： `setItem()`、`getItem()` 和 `removeItem()`
- cookie：
  - 存储：`window.document.cookie = 'key=val';`
  - 取出：`document.cookie`
  - 清除：`this.setCookie('','',-1)`

