### 前言

我们知道，webpack是不认识除了js以外的文件，除了js以外的文件，我们都需要借助loader来处理。所以我们通过loader将图片进行打包

打包图片的核心原理就两种：将图片打包到本地、将图片转为base64存储到js中，具体的使用及区别我们下面来介绍一下。

### loader

webpack打包图片主要有两个loader：`file-loader`、`url-loader`

#### file-loader

file-loader属于地址引入，你需要为图片设置输出的文件夹、输出的文件名，然后打包后图片就会在对应的文件夹

##### 使用

1. 安装`npm install file-loader --save-dev`

2. 配置

   ```
   // 引入node的path模块
   const path = require('path');
   
   module.exports = {
     entry: './src/index.js', // 入口文件
     output: {
       filename: 'main.js', // 输出的文件名
       path: path.resolve(__dirname, 'dist') // 输出文件的路径（__dirname指该配置文件所在的目录）
     },
     module: {
       rules: [
         {
           test: /\.(jpg|png|gif)$/, // 针对这三种格式的文件使用file-loader处理
           use: {
             loader: 'file-loader',
             options: {
               // 定义打包后文件的名称；
               // [name]:原文件名，[hash]:hash字符串（如果不定义名称，默认就以hash命名，[ext]:原文件的后缀名）
               name: '[name]_[hash].[ext]', 
               outputPath: 'images/' //  定义图片输出的文件夹名（在output.path目录下）
             }
           }
         }
       ]
     }
   }
   ```

3. 引入

   ```
   <div id="root">
     <img src="images/xxx.jpg">
   </div>
   ```

#### url-loader

url-loader则功能会更强一点，它会将图片转为base64存储到打包的js中。我们也可以限制图片大小limit，大于这个限制就转为base64，而是和file-loader一样，存储到文件夹中。你可以将url-loader理解为增强版的file-loader

>tip:Base64 是一种基于 64 个可打印字符来表示二进制数据的表示方法。我们可以将base64转为图片，也可以将图片转为base64

##### 使用

1. 安装`npm install url-loader --save-dev`

2. 配置

   ```
   // 引入node的path模块
   const path = require('path');
   
   module.exports = {
     entry: './src/index.js', // 入口文件
     output: {
       filename: 'main.js', // 输出的文件名
       path: path.resolve(__dirname, 'dist') // 输出文件的路径（__dirname指该配置文件所在的目录）
     },
     module: {
       rules: [
         {
           test: /\.(jpg|png|gif)$/, // 针对这三种格式的文件使用file-loader处理
           use: {
             loader: 'file-loader',
             options: {
               // 定义打包后文件的名称；
               // [name]:原文件名，[hash]:hash字符串（如果不定义名称，默认就以hash命名，[ext]:原文件的后缀名）
               name: '[name]_[hash].[ext]', 
               outputPath: 'images/', //  定义图片输出的文件夹名（在output.path目录下）
               limit: 204800 // 大于200kb的图片会被打包在images文件夹里面，小于这个值的会被以base64的格式写在js文件中
             }
           }
         }
       ]
     }
   }
   ```

   

### file-loader 和 url-loader的比较

file-loader: 将文件打包到输出路径，并将打包后的路径返回。 url-loader：文件大于limit设置的值（单位为字节）时，和file-loader打包的方式一样。但如果文件小于limit设置的值时，不会将文件打包到输出路径，而是将文件内容转成base64的字符串放在js文件中。

### url-loader的优缺点

- 优点：
  - 将图片转为base64，存储到js中，减少请求次数
  - 有两种打包形式，小图片转为base64，大图片打包到指定文件夹
- 缺点：
  - 如果图片过大，并且转换成base64存储到js中，js文件会很大，加载速度会变慢

