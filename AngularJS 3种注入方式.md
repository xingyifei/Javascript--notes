###                            AngularJS

###  3种控制器依赖注入方式

1.内联注入（推荐使用）

```javascript
app.controller("test",[
  '$scope',function($scope){ 
  }
]);
```

2.隐式注入（不推荐使用）

```javascript
app.controller("test",function($scope){
  
});
```

#### 推荐使用方法1的理由是

* 比方法3更可靠（由于JS可以被压缩），Angualarjs又是通过解析服务名称找到对应Service的，因此JS压缩后Angularjs将无法找到指定的Service，但字符串不会被压缩，因此单独以字符串制定Service的名称可以避免这个问题

  使用1的注意点

  由于上述原因，Angularjs在编译是，由￥inject将数组中的Service的名称与方法体中的Service进行一一映射这种映射必须遵守Angularjs规定**所以要在 ngapp后加 ng-strict-di** 



### ng-table

ng-table是一个插件，从json中拉去数据进行表格展示并可以排序分页等。

* 首先需要引入脚本文件

  ```javascript
  ng-table.min.js 
  ```

* HTML代码如下

  ```html
  <body ng-controller="testController as vm">
  	     <table ng-table="vm.tabletest">
  			<tr ng-repeat="one in $data">
  				<td data-title="'Name'" filter="{name: 'text'}" sortable="'name'" ng-bind="one.name"></td>
  				<td data-title="'Age'" filter="{money: 'number'}" sortable="'age'" ng-bind="one.age"></td>
  			</tr>
  		</table>
  <script src="js/angular.min.js" type="text/javascript" charset="utf-8"></script>
  <script src="js/ng-table.min.js" type="text/javascript" charset="utf-8"></script>
  <script src="js/table.js" type="text/javascript" charset="utf-8"></script>
  </body>
  ```

* JS中需要在angular模块中注入ngtable

  ```javascript
  var $myapp = angular.module("myapp", ["ngTable"]);
  //注入ngtable依赖
  $myapp.controller("testController", ['NgTableParams',testController]);
  //注入NgTableParams，用来将数据绑定到前台
  function testController(NgTableParams) {
  	var data = [{name: 'aa',age: 21}, {name: 'bb',age: 254},{name:'cc',age:66},
  	{name: 'aa',age: 21}, {name: 'bb',age: 254},{name:'cc',age:66},
  	{name: 'aa',age: 21}, {name: 'bb',age: 254},];
  	var vm=this;
  	vm.tabletest = new NgTableParams({}, {
  		dataset: data//将data数组数据绑到￥data中
  	});
  };
  ```



### ng-repeat中的track by

当ng-repeat中的数组值有重复，Angular就会报错不允许值重复，原因是angular需要一个唯一值可以与生成的dom绑定，以便追踪。

所以，在这种情况下，track by 就出现了：

```html
<div ng-repeat="(key,value) in items track by key">
</div>
```

 因为key不会重复。





#### Angular中的三种注入类型，用来注入控制器层

依赖注入(DI)：就是在实例化一个类的时候，它会主动去实例化它里面所用到的其他实例，在Angular里面，service作为单例对象只有需要时被创建，关闭游览器时清楚，而controller在不需要时就会被销毁了，angular中有三种依赖注入，service，factory，provider

Angular提供了很多内置服务，都以 $开头，例如$$http(Ajax)

* service（推荐使用）

  只实例化一次，无论我们在什么地方注入service，将永远使用同一个实例，所以对很多controller层中的操作就可以放到service层中

  可以直接通过构造器参数注入类中使用

* Factory

  Factory一般就是创建一个对象，然后在对这个对象添加方法和数据，最后将这些对象返回即可，然后注入到控制器层

* provider

  provider服务就是负责告诉angular如何创建一个新的可注入的东西



### Angular中的几种service

* Constant

  ```javascript
  app.constant('fooConfig',{
      config1: true,
      config2: "Default config2"
  });   
  ```

  Constant是一个非常有用的service，它经常被用来在指令中提供默认配置。因此如果你正在创建一个指令，并且你想要在给指令传递可选参数的同时进行一个默认配置，一个Constant就是一个好办法。

* value

  ```javascript
  app.value('fooConfig',{
      config1: true,
      config2: "Default config2 but it can change"
  });   
  ```

  一个value service有点像是一个constant但是它是可以被改变的。它也经常被用在一个指令上面，来进行配置。一个value service有点像是一个factory service的缩小版，它经常用来保存值但是我们不能在其中对值进行计算。

  **我们可以使用angular对象的extend方法来改变一个value service**

  ```javascript
  app = angular.module("app", []);

  app.controller('MainCtrl', function($scope, fooConfig) {
    $scope.fooConfig = fooConfig;
    angular.extend(fooConfig, {config3: "I have been extended"});
  });

  app.value('fooConfig', {
    config1: true,
    config2: "Default config2 but it can changes"
  });   
  ```



#### AngularJS 1.5+中的.component

Component 可以替换 Controller和 Directive了

主要作用是使HTML能够识别自定义的标签 将代码组件化

例如把 controller 写成类

component可以使一个父组件对应多个子组件

子组件将模块注入到父模块中，在父模块页面HTML中写入子模块的标签将子模块插入进来 同时可以在标签属性上和子组件的compoment的bindings中设置哪些变量或方法从父控件中取得



【命名规范是驼峰命名风格  例：<xing-yi-fei>----xingYiFei】

```html
<html>
<myComponent xingyife="boy"></myComponent>
</html>
```

```javascript
myModule.component('myComponent', JSON);
```

```javascript
 var JSON={
  restrict: 'E',//指令声明方式
   //E 代表替换标签 A代表 替换带有此属性的
   //C 代表替换带此样式的 M 代表替换带此注释的
  bindings: {
    'xingyifei:'='//等号代表双向数据绑定，一方修改数据后双方同时修改，内存中指向统一地址内存  也可以调用方法并传参数但不能带括号和调用其他方法
    'xingyifei':'<'//小于号代表单项，取得数据后，

    'xingyifei:'&'//此符号代表单向传递方法需要在html带括号 并可以通过方法调用别的方法
    //但不能传参数
  },
  template,
  controller,//此处为定义该指令的控制器
  controllerAs: 'vm'//控制其名称
 }
```

**此时在前端页面需要写vm.run(),来执行vm控制器中的run方法**

**注意：使用component将控制器注入angular模块也可以使用严格模式**



### Angularjs中的promise对象

Javascript是单线程，当某个任务耗时过长容易造成阻塞，为了解决阻塞问题，引入了promise对象也就是异步模式

`Promise`是一种异步方式处理值（或者非值）的方法，`promise`是对象，代表了一个函数最终可能的返回值或者抛出的异常。

我们可以先使用`$q`的`defer()`方法创建一个`deferred`对象，然后通过`deferred`对象的`promise`属性，将这个对象变成一个`promise`对象；这个`deferred`对象还提供了三个方法，分别是`resolve()`,`reject()`,`notify()`。

```javascript
var $myapp=angular.module("myapp",[]);
$myapp.controller("controller_1",['$scope','$q',function($scope,$q){
	$scope.flag=false;
	$scope.run=function(){
		var deferred=$q.defer();
		var promise=deferred.promise;
	
		promise.then(function(result){
			alert("SUCCESS"+result);
		},function(error){
			alert("fald"+error);
		});
		//不管promise成功还是失败都运行，如果执行就result
      //如果拒绝就error
		if($scope.flag){
			deferred.resolve("XX");
          //用来执行deferred promise
		}else{
			deferred.reject("YYYY");
          //用来拒绝deferred promise 
		}
	}
}]);
```



```javascript
var $myapp=angular.module("myapp",[]);
$myapp.controller("controller_1",['$scope','$q',function($scope,$q){
	$scope.flag=false;
	$scope.run=function(){
		var deferred=$q.defer();
		var promise=deferred.promise;
		
		promise.then(function(result){
			result=result+111;
			return result;
		},function(error){
			error=error+111;
			return error;
          // //然后根据deferred的结果执行此函数（3）↑
		}).then(function(result){
			alert("SSS"+result);
		},function(){
			alert("FFF"+error);
		});
       //然后根据deferred的结果（执行还是拒绝）执行此函数（2）并接受参数，此处的参数需要自己定义名字并传入then中↑
		if($scope.flag){
			deferred.resolve("XX");
		}else{
			deferred.reject("YYYY");
		}//先看deferred执行还是拒绝并传数据（1）↑
	}
}]);
```



**在实际开发中，一般见http网络参数和get，post请求写到服务类里并返回promise对象，然后用service注入angular模块中在控制器类中调用promise对象**



#### AngularJS中的$http对象（Ajax）

我们可以使用内置的$http服务直接同外部进行通信。$http服务只是简单的封装了[浏览器](http://www.2cto.com/os/liulanqi/)原生的XMLHttpRequest对象

$http服务是只能接受一个参数的函数，这个参数是一个对象，包含了用来生成HTTP请求的
配置内容。这个函数返回一个promise对象，具有success和error两个方法。

```javascript
$http({
url:'data.json',
method:'GET'
}).success(function(data,header,config,status){
//响应成功

}).error(function(data,header,config,status){
//处理响应失败
});
```



```javascript
$http({
method:'GET',
  //可以是get post JSONP PUT HEAD DELETE
url:'/api/users.json',
  //绝对的或相对的请求目标
params:{
'username':'tan'
  //字符串map或者对象，会被转换成查询字符串加在url后如果不时字符串，会被JSON序列化为 ?username=tan的格式
});
```

有可能有data，这个对象中包含了将会被当做消息体发送给服务器的数据，通常在post请求时使用





## AngularJS常用组件

* 加载框 -----cg-busy

  首先安装依赖（组件）-- npm  insatall angular-busy--save

  然后在app.js中引入注入依赖 ‘cgBusy’

  将后台的promise异步对象传入控制器

* HTML------<div cg-busy="vm.promise"></div>

  只有promise中的数据成功返回才会正常显示异步请求期间都会显示加载画面

* 其中的各个参数

  promise 要求。将导致忙碌指示器显示的承诺（或数组）。
  message -可选。默认为“请等待…”。指示器中显示的消息。此值可以更新，而该承诺是活跃的。该指标将反映更新的值，因为它们被改变。
  backdrop -可选。布尔，默认为真。如果真的褪色的背景后面会显示进度指标。
  templateurl可选。如果提供，给定的模板将在默认的模板显示进度指示。
  延迟-可选。等待显示指示器的时间量。默认为0。毫秒指定。
  minduration可选。保持指示器显示的时间的量，即使承诺得到更快的解决。默认为0。毫秒指定。
  wrapperclass可选。名称（S）的CSS类适用于繁忙的标志/动画的元素。默认为CG动画CG忙忙。通常只适用于如果你想应用不同的定位动画。






##### Angularjs内置指令 

ng-disabled只能用在以下标签中

<input>  <textarea><select><button>

