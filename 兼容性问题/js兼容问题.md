### JS兼容问题

1、事件对象的兼容

```
e = ev || window.event
```

2、滚动事件的兼容

```
scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
```

3、阻止冒泡的兼容

```
  if (e.stopPropagation) {
      e.stopPropagation();
  } else {
      e.cancelBubble=true;
  }
```

4、阻止默认行为的兼容

```
  if (e.preventDefault) {
      e.preventDefault();
  } else {
      e.returnValue = false;
  }
```

5、添加事件监听器的兼容

```
  if (target.addEventListener) {
      target.addEventListener("click", fun, false);
  } else {
      target.attachEvent("onclick", fun);
  }
```

6、ajax创建对象的兼容

```
  var xhr = null；
  if (window.XMLHttpRequest) {
      xhr = new XMLHttpRequest();
  } else {
      xhr = new ActiveXObject("Microsoft XMLHTTP");
  } 
```

7、鼠标按键编码的兼容

```
  function mouse (ev) {
      var e = ev || window.event; 
      if (evt) {
          return e.button;
      } else if (window.event) {
          switch (e.button) {
              case 1: return 0;
              case 4: return 1;
              case 2: return 2;
          }
      }  
  }
```

8、在IE中，event对象有x，y属性，Firefox中与event.x等效的是event.pageX，而event.pageX在IE中又没有

```
x = event.x ? event.x : event.pageX;
```

9、在IE下，event对象有srcElement属性，但是没有target属性；Firefox下，event对象有target属性，但是没有srcElement属性

```
  var source = ev.target || ev.srcElement;
  var target = ev.relatedTarget || ev.toElement;
```

10、在Firefox下需要用CSS禁止选取网页内容，在IE用JS禁止

```
-moz-user-select: none; // Firefox 
obj.onselectstart = function() {return false;} // IE
```

11、innerText在IE中能正常工作，但在FireFox中却不行

```
if (navigator.appName.indexOf("Explorer") > -1) {
    document.getElementById('element').innerText = "IE";
} else {
    document.getElementById('element').textContent = "Firefox";
}
```

12、在Firefox下，可以使用const关键字或var关键字来定义常量；在IE下，只能使用var关键字来定义常量

**解决方案：统一使用`var`关键字来定义常量**

