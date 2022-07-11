### 什么是token

token是一段我们看不懂的字符串，它是由服务器生成的，在用户进行登录操作时，服务器会生成唯一的token令牌，在后续访问时，我们就不需要进行登录，只用token令牌即可访问。

### 如何判断token是否过期

token相当于用户的账号密码，由于token的重要性，所以需要为token设置过期时间，那么我们前端如何判断token是否过期呢？两种方式：

- token + cookie ：前端判断是否过期

  > 通过if判断，如果过期那么就跳转到登录页面

- token + localStorage：后端判断是否过期

  > 通过后端返回的code码，例如：code为0即为token过期