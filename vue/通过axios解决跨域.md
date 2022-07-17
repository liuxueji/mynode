### 介绍

axios是一个基于promise的HTTP库

可以使用proxyTable配置解决跨域问题

例如要调用`http://172.xxx.xxx:8888/login`这个接口

- 首先需要配置baseURL

  ```
  const service = axios.create({
    baseURL:'/api'//标志
  })
  ```

  使用：`service.get(login,{params:data})`

- config配置

  ```
  dev:{
    proxyTable:{
      '/api':{
        target:'http://172.xxx.xxx:8888',//设置调用接口的域名和接口
        secure:false,
        changeOrigin:true,//跨域
        pathRewrite:{
          '^/api':''//去掉标志
        }
      }
    }
  }
  ```

  配置好后重新运行项目

我们进行请求接口时，请求的是`http://localhost:8080/api/login`，实际上会将请求转发成`http://172.16.13.205:9011/login`

这样就解决了跨域的问题，问：为什么不会出现跨域呢？

因为浏览器请求的是webpack开启的服务器，webpack开启的服务器协议、域名、端口号和浏览器一致，所以可以请求；然后服务器与服务器之间是没有跨域问题的，所以webpack服务器帮我们向`http://172.xxx.xxx:8888`发起请求，就不会出现跨域问题了。