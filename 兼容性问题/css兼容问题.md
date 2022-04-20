1、不同浏览器的标签默认的内外边距不同

**解决方案：`\*{margin: 0; padding: 0;}`**

其实清除浏览器自带的默认样式，每个前端开发者所用的方法大同小异，这里给出我选用的清除默认样式的重置样式代码：

```css
/**
 * 该文件用于清除浏览器样式
 */
html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed,
figure, figcaption, footer, header, hgroup,
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
    padding:0;
    margin:0;
    border:0;
    outline: 0;
    font-family: "Helvetica Neue For Number", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", "Helvetica Neue", Helvetica, Arial, sans-serif;
    word-wrap:break-word;
}
html, body {
    width: 100%;
    height: 100%;
}
a {
    text-decoration: none;
    -webkit-tap-highlight-color:rgba(255,255,255,0);
}
ul, ol {
    list-style-type: none;
}
textarea {
    resize: none;
}
/*去除input button默认样式*/
input, button, textarea {
    -webkit-appearance: none;
    -webkit-tap-highlight-color: rgba(255, 225, 225, 0);
    padding: 0;
    border: 0;
    outline: 0;
}
// 修改placeholder属性默认文字颜色
input::-webkit-input-placeholder, textarea::-webkit-input-placeholder {
    /* WebKit browsers */
    color: #999;
}
input:-moz-placeholder, textarea:-moz-placeholder {
    /* Mozilla Firefox 4 to 18 */
    color: #999;
}
input::-moz-placeholder, textarea::-moz-placeholder {
    /* Mozilla Firefox 19+ */
    color: #999;
}
input:-ms-input-placeholder, textarea:-ms-input-placeholder {
    /* Internet Explorer 10+ */
    color: #999;
}
```

除了自己定义清除默认样式的代码，也可选择引用别人写好的成熟插件[Normalize.css](https://link.juejin.cn?target=http%3A%2F%2Fnecolas.github.io%2Fnormalize.css%2F)来清除默认样式；

2、图片加a标签在IE9中会有边框

**解决方案：`img{border: none;}`**

3、IE6及更低版本浮动元素，浮动边双倍边距

**解决方案：不使用`margin`，使用`padding`**

4、IE6及更低版本中，部分块元素拥有默认高度

**解决方案：给元素设置`font-size: 0;`**

5、a标签蓝色边框

**解决方案：`a{outline: none;}`**

6、IE6不支持min-height属性

**解决方案：{min-height: 200px; _height: 350px;}**

7、IE9以下浏览器不能使用opacity

**解决方案：Firefox/Chrome/Safari/Opera浏览器使用opacity；IE浏览器使用filter**

```css
opacity: 0.7; /*FF chrome safari opera*/
filter: alpha(opacity:70); /*用了ie滤镜,可以兼容ie*/
```

8、IE6/7不支持display:inline-block

**解决方案：`{display: inline-block; \*display: inline;}`**

9、cursor兼容问题

**解决方案：统一使用`{cursor: pointer;}`**

10、IE6/7中img标签与文字放一起时，line-height失效的问题

**解决方案：文字和`<img>`都设置`float`**

11、table宽度固定，td自动换行

**解决方案：table设置 `{table-layout: fixed}`，td设置 `{word-wrap: break-word}`**

12、相邻元素设置margin边距时，margin将取最大值，舍弃小值

**解决方案：不让边距重叠，可以给子元素加一个父元素，并设置该父元素设置：`{overflow: hidden}`**

13、a标签css状态的顺序

**解决方案：按照`link--visited--hover--active`的顺序编写**

14、IE6/7图片下面有空隙的问题

**解决方案：`img{display: block;}`**

15、ul标签在Firefox中默认是有padding值的，而在IE中只有margin有值

**解决方案：`ul{margin: 0;padding: 0;}`**

16、IE中li指定高度后，出现排版错误

**解决方案：设置`line-height`**

17、ul或li浮动后，显示在div外

**解决方案：清除浮动；须在ul标签后加`<div style="clear:both"></div>`来闭合外层div**

18、ul设置float后，在IE中margin将变大

**解决方案：`ul{display: inline;}`，`li{list-style-position: outside;}`**

19、li中内容超过长度时，用省略号显示

```css
li{
    width: 200px;
    white-space: nowrap;
    text-overflow: ellipsis;
    -o-text-overflow: ellipsis;
    overflow: hidden;
}
```

20、div嵌套p时，出现空白行

**解决方案：`li{display: inline;}`**

21、IE6默认div高度为一个字体显示的高度

**解决方案：`{line-height: 1px;}`或`{overflow: hidden;}`**

22、在Chrome中字体不能小于10px

**解决方案：`p{font-size: 12px; transform: scale(0.8);}`**

23、谷歌浏览器上记住密码后输入框背景色为黄色

```css
input{
    background-color: transparent !important;
}
input:-webkit-autofill, textarea:-webkit-autofill, select:-webkit-autofill{
    -webkit-text-fill-color: #333 !important;
    -webkit-box-shadow: 0 0 0 1000px transparent inset !important;
    background-color: transparent !important;
    background-image: none !important;
    transition: background-color 5000s ease-in-out 0s;
}
```

24、CSS3兼容前缀表示

| 写法     | 内核            | 浏览器        |
| -------- | --------------- | ------------- |
| -webkit- | webkit渲染引擎  | chrome/safari |
| -moz-    | gecko渲染引擎   | Firefox       |
| -ms-     | trident渲染引擎 | IE            |
| -o-      | opeck渲染引擎   | Opera         |

如：

```css
  .box{
       height: 40px;
       background-color: red;
       color: #fff;
       -webkit-border-radius: 5px; // chrome/safari
       -moz-border-radius: 5px; // Firefox
       -ms-border-radius: 5px; // IE
       -o-border-radius: 5px; // Opera
       border-radius: 5px;
   }
```


