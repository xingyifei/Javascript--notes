## Webpack+gulp+Babel+ES6

#### Wabpack >>>>模块打包器

将一些相互依赖的模块(文件)，打包成一个或多个js文件，减少http请求次数，提升性能。这些相互依赖的模块可以是图片、字体、coffee文件、样式文件、less文件等,解决了静态文件和模块化开发两大问题

#### gulp>>>>>

在文件有改动的时候自动完成这些编译工作. 使用 npm init 创建一个 package.json 存储依赖关系等配置信息

#### Babel>>>>>

[Babel](https://babeljs.io/)是一个广泛使用的ES6转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。这意味着，你可以用ES6的方式编写程序，又不用担心现有环境是否支持。下面是一个例子。

```javascript
// 转码前
input.map(item => item + 1);

// 转码后
input.map(function (item) {
  return item + 1;
});
```

上面的原始代码用了箭头函数，这个特性还没有得到广泛支持，Babel将其转为普通函数，就能在现有的JavaScript环境执行了





### 配置Webpack

安装Webpack之前建议先安装Node.js，然后采用npm的方式来安装Webpack：

npm install webpack -g

因为要用到angular，所以要安装angular：

npm install angular

还要安装一系列加载器(loader)：

npm install style-loader css-loader url-loader sass-loader raw-loader

注意：除了webpack是全局安装之外，其他组件都是安装在app文件夹下面，会自动生成node_modules文件夹

* ## 配置文件webpack.config.js

  ```javascript
  1    module.exports = {
   2   context: __dirname + '/app',//上下文
   3   entry: './index.js',//入口文件
   4   output: {//输出文件
   5     path: __dirname + '/app',
   6     filename: './bundle.js'
   7   },
   8   module: {
   9     loaders: [//加载器
  10       {test: /\.html$/, loader: 'raw'},
  11       {test: /\.css$/, loader: 'style!css'},
  12       {test: /\.scss$/, loader: 'style!css!sass'},
  13       {test: /\.(png|jpg|ttf)$/, loader: 'url?limit=8192'}
  14     ]
  15   }
  16 };
  ```

  就是告诉webpack需要做什么

### [实际项目中写法]

```javascript
var path=require('path');
var webpack=require('webpack');
var HtmlWebpackPlugin=require('html-webpack-plugin')
```

这种风格用同步require 的方法去加载一个依赖并用暴露一个接口。 一个模块可以通过给`export`对象添加属性或给`module.exports`设置值 来指定导出。

路径寻航 path.resolve([from …], to)
特点：相当于不断的调用系统的cd命令

示例：

```javascript
path.resolve('foo/bar', '/tmp/file/', '..', 'a/../subfile')

//相当于：
cd foo/bar
cd /tmp/file/
cd ..
cd a/../subfile
```

html-webpack-plugin 插件作用

- 为html文件中引入的外部资源如`script`、`link`动态添加每次compile后的hash，防止引用缓存的外部文件问题
- 可以生成创建html入口文件，比如单页面可以生成一个html文件入口，配置**N**个`html-webpack-plugin`可以生成**N**个页面入口


#### 那为什么要动态生成HTML，我自己写不行吗？答案当然是可以的。

之所以要动态生成，主要是希望**webpack在完成前端资源打包以后，自动将打包后的资源路径和版本号写入HTML中，达到自动化的效果**






















* ### 需要一个入口文件index.js

  ```javascript
  var angular = require('angular');//引入angular
  var ngModule = angular.module('app',[]);//定义一个angular模块
  require('./directives/hello-world/hello-world.js')(ngModule);//引入指令(directive)文件
  require('./css/style.css');//引入样式文件
  ```

  ​

  ​

