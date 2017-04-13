# Nodejs

安装nodejs并安装npm

* **Node.js REPL（交互式解释器）**

  Node.js REPL(Read Eval Print Loop:交互式解释器) 表示一个电脑的环境，类似 Window 系统的终端或 Unix/Linux shell，我们可以在终端中输入命令，并接收系统的响应。

  Node 自带了交互式解释器，可以执行以下任务：

  - **读取** - 读取用户输入，解析输入了Javascript 数据结构并存储在内存中。

  - **执行** - 执行输入的数据结构

  - **打印** - 输出结果

  - **循环** - 循环操作以上步骤直到用户两次按下 **ctrl-c** 按钮退出

    在控制台使用交互式解释器↓

  ```javascript
  $ node
  > 1 +4
  5
  ```

  停止 REPL

  前面我们已经提到按下两次 **ctrl + c** 建就能退出 REPL:

* #### Node js 回调函数

Nodejs特点就是使用大量的回调函数，就是在一个程序执行的同事并发执行另一个程序，从而大大提高了程序的性能。

下面就是传统的阻塞代码与Nodejs中非阻塞代码的区别

【传统阻塞代码】需要在一个程序运行完成后再执行另一个程序

```javascript
var fs = require("fs");

var data = fs.readFileSync('input.txt');

console.log(data.toString());
console.log("程序执行结束!");
-----------------------------------------------------
$ node main.js
菜鸟教程官网地址：www.runoob.com

程序执行结束!
```

【非阻塞代码】利用回调函数在一个程序执行的同事执行另一个程序

```javascript
var fs = require("fs");

fs.readFile('input.txt', function (err, data) {
    if (err) return console.error(err);
    console.log(data.toString());
});
//以上程序中 fs.readFile() 是异步函数用于读取文件。 如果在读取文件过程中发生错误，错误 err 对象就会输出错误信息。
//如果没发生错误，readFile 跳过 err 对象的输出，文件内容就通过回调函数输出。
console.log("程序执行结束!");
------------------------------------------------
 $ node main.js
程序执行结束!
菜鸟教程官网地址：www.runoob.com
```

**因此，阻塞是按顺序执行的，而非阻塞是不需要按顺序的，所以如果需要处理回调函数的参数，我们就需要写在回调函数内。异步模式是高性能并发的前提，在nodejs中也有很大的体现，nodejs与js都是单线程编程+异步操作，但底层C++代码是多线程+异步操作，所以nodejs的性能大大提高。**

### express

是一个基于 [Node.js](http://nodejs.org/) 平台，快速、开放、极简的 web 开发框架

* 使用命令行输入npm install express body-parser，进行安装
* 然后使用npm init  初始化packagejson文件进行依赖的安装
* 创建appjs编写express基础代码

```javascript
var express = require('express');//引入express依赖
var bodyparser = require('body-parser');//引入bodyparser依赖,用来操作中间件
var app = express();//初始化应用级中间件
var router = express.Router();//初始化路由级中间件
var port = process.env.PORT || 8080;
app.use(bodyparser.urlencoded({extended:true}));
```

路由写法一：应用级中间间app.use

```javascript
app.use(function(req,res,next){
	console.log(new Date().toLocaleDateString());
	next();
});//这是应用级中间件，所有请求都会经过，next（）代表进行下一个中间件，如果是next（‘route’），将跳过剩余中间件进入路由
app.route('/index')//链式声明路由
.post(function(req,res){
	var name = req.query.name;
	var age = req.query.age;
	res.json({
		name:name,
		age:age
	})
})
.get(function(req,res){
	res.send({
		name:"qiunan",
		age:22
	})
});
```

路由写法二：