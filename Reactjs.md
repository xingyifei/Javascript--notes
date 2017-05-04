### Reactjs

### 基本语法

* ReactDOM.render 是 React 的最基本方法，用于将模板转为 HTML 语言，并插入指定的 DOM 节点

```javascript
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('example')
);
```

* HTML 语言直接写在 JavaScript 语言之中，不加任何引号，这就是 [JSX 的语法](http://facebook.github.io/react/docs/displaying-data.html#jsx-syntax)，它允许 HTML 与 JavaScript 的混写JSX 的基本语法规则：遇到 HTML 标签（以 `<` 开头），就用 HTML 规则解析；遇到代码块（以 `{` 开头），就用 JavaScript 规则解析

```javascript
var names = ['Alice', 'Emily', 'Kate'];

ReactDOM.render(
  <div>
  {
    names.map(function (name) {
      return <div>Hello, {name}!</div>
    })
  }
  </div>,
  document.getElementById('example')
);
```

* JSX 允许直接在模板插入 JavaScript 变量。如果这个变量是一个数组，则会展开这个数组的所有成员

```javascript
var arr = [
  <h1>Hello world!</h1>,
  <h2>React is awesome</h2>,
];
ReactDOM.render(
  <div>{arr}</div>,
  document.getElementById('example')
);
```



## 组件

React 允许将代码封装成组件（component），然后像插入普通 HTML 标签一样，在网页中插入这个组件。React.createClass 方法就用于生成一个组件类

```javascript
var HelloMessage = React.createClass({
  render: function() {
    return <h1>Hello {this.props.name}</h1>;
  }
});

ReactDOM.render(
  <HelloMessage name="John" />,
  document.getElementById('example')
);
```

**注意：组件类的第一个字母必须大写，且只能包含一个顶层标签（一个最高标签）**

添加组件属性，有一个地方需要注意，就是 `class` 属性需要写成 `className` ，`for` 属性需要写成 `htmlFor` ，这是因为 `class` 和 `for` 是 JavaScript 的保留字

    	.gg{
    		font-size:30px;
    	}
    </style>
```javascript
<div id="example"></div>
<script type="text/babel">
 var Xingyifei= React.createClass({
       render:function(){
	     return <p className="gg">{this.props.name}11{this.props.temp}</p>
   }
 });
 ReactDOM.render(
       <Xingyifei name="xingyifei" temp="thisistemp"></Xingyifei>,
       document.getElementById('example')
 )
   
</script>
```


this.props.children比较特殊它代表了组件的所有子节点

```javascript
var Xingyifei=React.createClass({
	render:function(){
		return(
			<h1>{this.props.children}</h1>
		)//此处必须带括号 因为含有多个标签
	}
});
ReactDOM.render(
	<Xingyifei>
	<span>879</span>
	<span>9879</span>
	</Xingyifei>,
	document.getElementById('example')
);
```
这里需要注意， `this.props.children` 的值有三种可能：如果当前组件没有子节点，它就是 `undefined` ;如果有一个子节点，数据类型是 `object` ；如果有多个子节点，数据类型就是 `array` 。所以，处理 `this.props.children` 的时候要小心。

React 提供一个工具方法 [`React.Children`](https://facebook.github.io/react/docs/top-level-api.html#react.children) 来处理 `this.props.children` 。我们可以用 `React.Children.map` 来遍历子节点，而不用担心 `this.props.children` 的数据类型是 `undefined` 还是 `object`



####	PropTypes属性

组件类的`PropTypes`属性，就是用来验证组件实例的属性是否符合要求

```javascript
var MyTitle = React.createClass({
  propTypes: {
    title: React.PropTypes.string.isRequired,
  },

  render: function() {
     return <h1> {this.props.title} </h1>;
   }
});
```

`getDefaultProps` 方法可以用来设置组件属性的默认值。

```javascript
var MyTitle = React.createClass({
  getDefaultProps : function () {
    return {
      title : 'Hello World'
    };
  },
  render: function() {
     return <h1> {this.props.title} </h1>;
   }
});
ReactDOM.render(
  <MyTitle />,
  document.body
);
```





#### 组件并不是真实的 DOM 节点，而是存在于内存之中的一种数据结构，叫做虚拟 DOM （virtual DOM）。只有当它插入文档以后，才会变成真实的 DOM 。根据 React 的设计，所有的 DOM 变动，都先在虚拟 DOM 上发生，然后再将实际发生变动的部分，反映在真实 DOM上，这种算法叫做 [DOM diff](http://calendar.perfplanet.com/2013/diff/) ，它可以极大提高网页的性能表现。

但是，有时需要从组件获取真实 DOM 的节点，这时就要用到 `ref` 属性

```javascript
var MyComponent = React.createClass({
  handleClick: function() {
    this.refs.myTextInput.focus();
  },
  render: function() {
    return (
      <div>   //注意return只能有一个外层节点
        <input type="text" ref="myTextInput" />
        <input type="button" value="Focus the text input" onClick={this.handleClick} />
      </div>
    );
  }
});

ReactDOM.render(
  <MyComponent />,
  document.getElementById('example')
);
```

#### 组件免不了要与用户互动，React 的一大创新，就是将组件看成是一个状态机，一开始有一个初始状态，然后用户互动，导致状态变化，从而触发重新渲染 UI

```javascript
var LikeButton = React.createClass({
  getInitialState: function() {
    return {liked: false};
  },
  handleClick: function(event) {
    this.setState({liked: !this.state.liked});
  },
  render: function() {
    var text = this.state.liked ? 'like' : 'haven\'t liked';
    return (
      <p onClick={this.handleClick}>
        You {text} this. Click to toggle.
      </p>
    );
  }
});

ReactDOM.render(
  <LikeButton />,
  document.getElementById('example')
);
```

**总结：【获取用户输入的值需要利用ref获取真实DOM或者event.target.value通过原生方法传入来获取】 对控件进行赋值或修改操作需要进行this.state状态机间接操作**

```javascript
var Input = React.createClass({
  getInitialState: function() {
    return {value: 'Hello!'};
  },
  handleChange: function(event) {
    this.setState({value: event.target.value});
  },
  render: function () {
    var value = this.state.value;
    return (
      <div>
        <input type="text" value={value} onChange={this.handleChange} />
        <p>{value}</p>
      </div>
    );
  }
});

ReactDOM.render(<Input/>, document.body);
```





#### 组件的声明周期

- Mounting：已插入真实 DOM
- Updating：正在被重新渲染
- Unmounting：已移出真实 DOM

React 为每个状态都提供了两种处理函数，`will` 函数在进入状态之前调用，`did` 函数在进入状态之后调用，三种状态共计五种处理函数。

> - componentWillMount()
> - componentDidMount()
> - componentWillUpdate(object nextProps, object nextState)
> - componentDidUpdate(object prevProps, object prevState)
> - componentWillUnmount()

此外，React 还提供两种特殊状态的处理函数

- componentWillReceiveProps(object nextProps)：已加载组件收到新的参数时调用
- shouldComponentUpdate(object nextProps, object nextState)：组件判断是否重新渲染时调用

组件的`style`属性的设置方式也值得注意，不能写成

> ```
> style="opacity:{this.state.opacity};"
>
> ```

而要写成

> ```
> style={{opacity: this.state.opacity}}
> ```

这是因为 [React 组件样式](https://facebook.github.io/react/tips/inline-styles.html)是一个对象，所以第一重大括号表示这是 JavaScript 语法，第二重大括号表示样式对象







### React-router

#### 基本语法

Router本身就是一个React组件	

`Router`组件本身只是一个容器，真正的路由要通过`Route`组件定义

```javascript
import { Router, Route, hashHistory } from 'react-router';

render((
  <Router history={hashHistory}>
    <Route path="/" component={App}/>
  </Router>
), document.getElementById('app'));

```

`Router`组件有一个参数`history`，它的值`hashHistory`表示，路由的切换由URL的hash变化决定，即URL的`#`部分发生变化。举例来说，用户访问`http://www.example.com/`，实际会看到的是`http://www.example.com/#/`



#### 嵌套路由

```javascript
-------------写法一---------------------------------
<Router history={hashHistory}>
  <Route path="/" component={App}>//父路由
    <Route path="/repos" component={Repos}/> //子路由
    <Route path="/about" component={About}/>
  </Route>
</Router>
 ---------------------写法二------------------
   const routsons=[
     {
       path:'/',
       component:App,
       childrenRoutes:[{path:'first',components:first}]
     },
      {
        path:'*',
        components:Notfound  //注意此对象为进入错误路由时的重定向
      }
   ]
   <Router routes={routsons}></Router>
页面渲染为
<App>
  <Repos/>
</App>
//父路由的组件需要写this。props。children 代表子组件
export default React.createClass({
  render() {
    return <div>
      {this.props.children}
    </div>
  }
})
```


子路由也可以不写在`Router`组件里面，单独传入`Router`组件的`routes`属性。

> ```javascript
> let routes = <Route path="/" component={App}>
>   <Route path="/repos" component={Repos}/>
>   <Route path="/about" component={About}/>
> </Route>;
>
> <Router routes={routes} history={browserHistory}/>
> ```

path属性的匹配

```javascript
<Route path="/hello/:name">
// 匹配 /hello/michael
// 匹配 /hello/ryan

<Route path="/hello(/:name)">
// 匹配 /hello
// 匹配 /hello/michael
// 匹配 /hello/ryan

<Route path="/files/*.*">
// 匹配 /files/hello.jpg
// 匹配 /files/hello.html

<Route path="/files/*">
// 匹配 /files/ 
// 匹配 /files/a
// 匹配 /files/a/b

<Route path="/**/*.jpg">
// 匹配 /files/hello.jpg
// 匹配 /files/path/to/file.jpg
  
（1）:paramName
:paramName匹配URL的一个部分，直到遇到下一个/、?、#为止。这个路径参数可以通过this.props.params.paramName取出。
（2）()
()表示URL的这个部分是可选的。
（3）*
*匹配任意字符，直到模式里面的下一个字符为止。匹配方式是非贪婪模式。
（4） **
** 匹配任意字符，直到下一个/、?、#为止。匹配方式是贪婪模式
```

path也可以使用相对路径（不带/） 嵌套路由中  子路由相对于父路由

路由匹配是从上倒下执行  遇到第一个匹配到的后就不会再继续执行

URL的查询字符串`/foo?bar=baz`，可以用`this.props.location.query.bar`获取



### IndexRoute组件

```javascript
<Router>
  <Route path="/" component={App}>
    <Route path="accounts" component={Accounts}/>
    <Route path="statements" component={Statements}/>
  </Route>
</Router>
 //此时访问/时 不会渲染子路由
```

```javascript
<Router>
  <Route path="/" component={App} onEnter={requireAuth}>
    <IndexRoute component={Home}/>
    <Route path="accounts" component={Accounts}/>
    <Route path="statements" component={Statements}/>
  </Route>
</Router>
function (nextState, replaceState) {
  replaceState(null, '/messages/' + nextState.params.id);
}//onEnter 方法表示进入这个路由前执行的方法
 //IndexRoute他在没有匹配子路由的时候，在 / 路由下渲染默认的子组件 home。 类似angular嵌套路由抽象路由的里的匿名视图
```

`IndexRoute`组件没有路径参数`path`

## IndexRedirect 组件

`IndexRedirect`组件用于访问根路由的时候，将用户重定向到某个**子组件**

```javascript
<Route path="/" component={App}>
  ＜IndexRedirect to="/welcome" />
  <Route path="welcome" component={Welcome} />
  <Route path="about" component={About} />
</Route>
```

## Redirect 组件

`<Redirect>`组件用于路由的跳转，即用户访问一个路由，会自动跳转到另一个路由。

> ```javascript
> <Route path="inbox" component={Inbox}>
>   {/* 从 /inbox/messages/:id 跳转到 /messages/:id */}
>   ＜Redirect from="messages/:id" to="/messages/:id" />
> </Route>
> ```

现在访问`/inbox/messages/5`，会自动跳转到`/messages/5`



## histroy 属性

如果设为`hashHistory`，路由将通过URL的hash部分（`#`）切换，URL的形式类似`example.com/#/some/path`

如果设为`browserHistory`，浏览器的路由就不再通过`Hash`完成了，而显示正常的路径`example.com/some/path`，背后调用的是浏览器的History API。(这种情况需要对服务器进行修改)

`createMemoryHistory`主要用于服务器渲染。它创建一个内存中的`history`对象，不与浏览器URL互动。



### 路由间的跳转

第一种方法是使用`browserHistory.push`

第二种方法是使用`context`对象。

> ```javascript
> export default React.createClass({
>
>   // ask for `router` from context
>   contextTypes: {
>     router: React.PropTypes.object
>   },
>
>   handleSubmit(event) {
>     // ...
>     this.context.router.push(path)
>   },
> })
> ```



## React-router的按需加载（类似Angular嵌套路由的lazyloding）

首先需要在Webpack输出路径output中配置一个属性

```javascript
output: {
    path: path.join(__dirname, '/../dist/assets'),
    filename: 'app.js',
    publicPath: defaultSettings.publicPath,
    // 添加 chunkFilename
    chunkFilename: '[name].[chunkhash:5].chunk.js',
},
```

`name` 是在代码里为创建的 chunk 指定的名字，如果代码中没指定则 webpack 默认分配 id 作为 name。

`chunkhash` 是文件的 hash 码，这里只使用前五位。

然后将路由配置中的component改为getcomponent

```javascript
const Routerconfig={
  path:'/',
  indexRoute:{
    getComponent(nextState,cb){
      require.ensure([], (require) => {
        cb(null, require('components/layer/HomePage'))
      }, 'HomePage')
    },
    }
  },
  getComponent(nextState,cb){
    require.ensure([], (require) => {
      cb(null, require('components/Main')【.default】)
    }, 'Main')//如果此处的模块是用ES6的import和export来进行导入导出需在函数后加.default
  },
  childRoutes:[
    require('./routes/baidu'),
    require('./routes/404'),
    require('./routes/redirect')
  ]  //三个子路由分别对应一个路由模块(如下)
}

ReactDOM.render(
   (
    <Router
      history={browserHistory}
      routes={rootRoute}
      />
  ), document.getElementById('app')
)
```

```
routes/
├── 404
│   └── index.js
├── baidu
│   ├── index.js
│   └── routes
│       ├── frequency
│       │   └── index.js
│       └── result
│           └── index.js
└── redirect
    └── index.js
```

和 rootRoute 类似，里面的每个 index.js 都是一个路由对象：

### /404/index.js

```javascript
module.exports = {
  path: '404',

  getComponent(nextState, cb) {
    require.ensure([], (require) => {
      cb(null, require('components/layer/NotFoundPage'))
    }, 'NotFoundPage')
  }
}
```

### /baidu/index.js

```javascript
module.exports = {
  path: 'baidu',

  getChildRoutes(partialNextState, cb) {
    require.ensure([], (require) => {
      cb(null, [
        require('./routes/result'),
        require('./routes/frequency')
      ])
    })
  },

  getComponent(nextState, cb) {
    require.ensure([], (require) => {
      cb(null, require('components/layer/BaiduPage'))
    }, 'BaiduPage')
  }
}
```

### /baidu/routes/frequency/index.js

```javascript
module.exports = {
  path: 'frequency',

  getComponent(nextState, cb) {
    require.ensure([], (require) => {
      cb(null, require('components/layer/BaiduFrequencyPage'))
    }, 'BaiduFrequencyPage')
  }
}
```

```javascript
module.exports = {
  path: '*',
  onEnter: (_, replaceState) => replaceState(null, "/404")
}
```





### React ES6写法

```javascript
import React form 'react';
class TodoItem extends React.Component{
    static propTypes = { // 筛选属性值条件(常量)
        name: React.PropTypes.string
    };
    static defaultProps = { // 默认属性值(常量)
        name: ''
    };
    constructor(props){
        super(props)
        this.state={//设置初始状态(变量)
          name:1
        }
    }
    render(){
        return <div></div>
    }
}
```
**在React ES6之前的写法中 React.creatClass中，会自动将组件的this(也就是组件的实例化)绑定到函数里的this中。**

**但是在ES6的写法中有4个方法将来改变指向**

```javascript
//第一种
class xingyifei extends Component(){
  constructor(){
    
  }
  getMoney(canshu,e){
    console.log(canshu)//我是参数
  }
  render(){
    <div onCick={this.getMoney.bind(this,"我是参数")}></div>
    //此处bind（）将函数里的this绑定到组件this
  }
}

//第二种写法
class xingyifei extends Component(){
  constructor(){
    this.getMoney=this.getMoney.bind(this)
  }
  getMoney(e){
    console.log(111)
  }
  render(){
    <div onClick={this.getMoney}></div>
  }
}
//第三种写法
class xingyifei extends Component(){
  constructor(){
  }
  getMoney(){
  }
  render(){
    <div onClick={()=>{this.getMoney()}}>
  }
}
//第四种写法
class xingyifei extends Component(){
  constructor(){
  }
  getMoney=(e)=>{
  }
  render(){
    <div onClick={this.getMoney}>
  }
}
```