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
   3   entry: {        //多个入口文件，属性值为chunk值
          app: './src/app.js',
          vendors: './src/vendors.js'
       },                                     
   4   output: {      //输出文件（只能为一个）
   5     path: __dirname + '/app',  //绝对路径,
         chunkFilename:'./bundle.js',
   6     filename: './bundle.js'  //文件名；当多个入口文件或使用了提取  公共插件时 应用[name] 被 chunk 的 name 替换。
            //[hash] 被 compilation 生命周期的 hash 替换。
            //[chunkhash] 被 chunk 的 hash 替换
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

### 加载器

```javascript
module: {
		loaders: [{
			test: /\.(jpe?g|png|gif|svg)$/i,
			loader: "url-loader?limit=8192&name=./[name].[ext]?[hash]"
		}, //url-loader比file-loader优点是可以压缩文件
		{
			test: /\.css$/i,
			loader: "style-loader!css-loader"//先使用css-loader将css加载到js中，然后使用style-loader让js识别css
		},
		{
			test: /\.js$/,
			loaders: "babel-loader",
			exclude: path.join(__dirname, 'node_modules'),
			query: {
                   presets: ['es2015']//使用babel-loader来加载ES6语法
            }
		}]
	},
```

### 常用插件

**html-webpack-plugin**   将打包的JS文件自动按照模版index引入并生成新的index

```javascript
html-webpack-plugin

new htmlwebpackplugin({
      template: './index.html',//模版文件地址
      inject: 'body',//JS插入的位置
      hash: true
    })
```

#### 那为什么要动态生成HTML，我自己写不行吗？答案当然是可以的。

之所以要动态生成，主要是希望**webpack在完成前端资源打包以后，自动将打包后的资源路径和版本号写入HTML中，达到自动化的效果**



**webpack.optimize.CommonsChunkPlugin**

用来将文件公共部分提取

```javascript
 entry: {
    main1: './mian1.js',
    mian2: './mian2.js',
    vendor: [
      'vue',
      'vuex',
      'whatwg-fetch',
      'es6-promise'
    ],
  }, 
    --------------------------------------------------------------
new webpack.optimize.CommonsChunkPlugin({
    	name:'main',//提取的打包JS的chunk名
    	filename:'common.js'//文件名
        chunks:[]//限定提取哪个chunk里的公共部分，不写默认所有
 })
   ---------------------------------------------------------------
 //注意 name值如果不存在（也就是匹配不到已存在的chunk值）那么插件会从chunks里提取他们的公共部分，生成新的chunk，名子就是name【见下列】
  new webpack.optimize.CommonsChunkPlugin({
      name: 'common',
      chunks: ['main1', 'main2'],
      filename: 'js/common.bundle.js',
      minChunks: 2,
    }),//本例子会从main1和main2 chunk里提取他们的公共部分生成新的chunk名为commom
    
//如果name存在 会从chunks属性里提取共同的name（也就是chunk对应的公共部分）
    new webpack.optimize.CommonsChunkPlugin({
      name: 'vendor',
      chunks: ['common'],
      filename: 'js/vendor.bundle.js',
      minChunks: Infinity,
    }),//本例子中会从commom chunk中提取公共的vendor部分！也就是第三方插件!
```



**webpack.ProvidePlugin**   声明全局变量（一般用来声明第三方类库）

```javascript
new webpack.ProvidePlugin({
			$: "jquery",//声明变量$,对应resolve里的alias里的jquery
			jQuery: "jquery"//声明变量jquery，对应resolve里的alias的jquery
		                })
	--------------------------------------------------------	
resolve: {
		alias: {
			'jquery': path.resolve(path.join(__dirname, 'jquery-3.1.1.min.js'))//预加载JQ源文件并对应名字为jquery与全局变量对应
		}
	    },
```


## 使用gulp来启动webpack+服务器

webpack可以对文件进行打包编译压缩

但是在开发模式中我们需要本地搭建服务器来进行调试本例中使用gulp中的browser-sync来搭建服务器，gulp来启动相关服务。

#### gulp

gulp是一种工程流工具，可以自定义任务并执行

```javascript
gulp.task('webpack', (cb) => {
	config.entry.app =path.join(__dirname,'index.js')
	webpack(config, (err, stats) => {
		if(err) {
			throw new gutil.PluginError("webpack", err);
		}
		gutil.log("[webpack]", stats.toString({
			colors: colorsSupported,
			chunks: false,
			errorDetails: true
		}));
		cb();//执行下一个流任务
	})
});
gulp.task('default', ['serve']);//默认任务会先执行
```

#### 在gulp中使用Browser-sync

```javascript
gulp.task('serve', (cb) => {
	config.entry.app = [
		'webpack-hot-middleware/client?reload=true',//其中 ? 后的内容相当于为webpack-hot-middleware设置参数，这里 reload=true 的意思是，如果碰到不能hot reload的情况，就整页刷新。
		path.join(__dirname, 'index.js')
	]
	var temp=webpack(config)
	serve({
		port: process.env.PORT || 3000, //端口号
		open: true,   //是否启动服务器后打开游览器
		server: {
			baseDir: '/' //指定服务器启动根目录
		},
		middleware: [   //自定义中间件
			webpackDevMiddelware(temp, {
				stats: {
					colors: colorsSupported,
					chunks: false,
					modules: false
				},
				publicPath: config.output.publicPath //获取webpack打包后的文件进行操作（处理静态资源）
			}),
			webpachHotMiddelware(temp)//启动热重载
		]
	});
})
gulp.task('watch',['serve']);//监听serve流的变化刷新
```

**总计：实际开发中可以使用gulp中的browser-sync来搭建服务器并加入webpack-hot-middleware和webpack-Dev-middleware来增加热替换功能（缺一不可）**

**或者使用流行的express框架搭建本地服务器加入webpack-dev-middleware和webpack-hot-middleware来增加热重载（express例子如下↓）**

```javascript
var webpack = require('webpack'),
    webpackDevMiddleware = require('webpack-dev-middleware'),
    webpackHotMiddleware = require('webpack-hot-middleware'),
    webpackDevConfig = require('./webpack.config.js');

var compiler = webpack(webpackDevConfig);

// 给服务器增加webpackDevMiddleware来处理静态资源
app.use(webpackDevMiddleware(compiler, {

    // 此处的路径需要与webpack配置文件里的出口路径一致
    publicPath: webpackDevConfig.output.publicPath,
    noInfo: true,
    stats: {
        colors: true
    }
}));
app.use(webpackHotMiddleware(compiler));//服务器启动热重载
```