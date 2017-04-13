# ES6与commonjs

ES6是ecmascript6.0新标准，在目前的前端开发中，会使用ES6然后用编译工具编译为ES5

* #### ES6允许为函数的参数设置默认值，即直接写在参数定义的后面。

```javascript
function log(x, y = 'World') {
  console.log(x, y);
}

log('Hello') // Hello World
log('Hello', 'China') // Hello China
log('Hello', '') // Hello
```

注意：参数变量是默认声明的，所以不能用`let`或`const`再次声明

* #### ES6允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）

```javascript
var [a, b, c] = [1, 2, 3];
```

如果解构不成功，变量的值就等于`undefined`。

```javascript
var [foo] = [];
var [bar, foo] = [1];
```

以上两种情况都属于解构不成功，`foo`的值都会等于`undefined`。

注意：解构左右需类型相同，左边为数组右边也需为数组

* #### 函数的length属性

指定了默认值以后，函数的`length`属性，将返回没有指定默认值的参数个数。也就是说，指定了默认值后，`length`属性将失真。

```javascript
(function (a) {}).length // 1
(function (a = 5) {}).length // 0
(function (a, b, c = 5) {}).length // 2
```

如果传入`undefined`，将触发该参数等于默认值，`null`则没有这个效果。

```javascript
function foo(x = 5, y = 6) {
  console.log(x, y);
}

foo(undefined, null)
// 5 null
```

* #### 函数的`name`属性，返回该函数的函数名。

```
function foo() {}
foo.name // "foo"

```

这个属性早就被浏览器广泛支持，但是直到ES6，才将其写入了标准。

* #### ES6允许使用“箭头”（`=>`）定义函数。

  ```javascript
  var f = () => 5;
  // 等同于
  var f = function () { return 5 };

  var sum = (num1, num2) => num1 + num2;
  // 等同于
  var sum = function(num1, num2) {
    return num1 + num2;
  };
  ```

如果箭头函数直接返回一个对象，必须在对象外面加上括号。

```javascript
var getTempItem = id => ({ id: id, name: "Temp" });
```

### 使用注意点

箭头函数有几个使用注意点。

（1）函数体内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象。

（2）不可以当作构造函数，也就是说，不可以使用`new`命令，否则会抛出一个错误。

（3）不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用Rest参数代替。

（4）不可以使用`yield`命令，因此箭头函数不能用作Generator函数。

上面四点中，第一点尤其值得注意。`this`对象的指向是可变的，但是在箭头函数中，它是固定的。

```javascript
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}

var id = 21;

foo.call({ id: 42 });
// id: 42
```

上面代码中，`setTimeout`的参数是一个箭头函数，这个箭头函数的定义生效是在`foo`函数生成时，而它的真正执行要等到100毫秒后。如果是普通函数，执行时`this`应该指向全局对象`window`，这时应该输出`21`。但是，箭头函数导致`this`总是指向函数定义生效时所在的对象（本例是`{id: 42}`），所以输出的是`42`。

箭头函数可以让`setTimeout`里面的`this`，绑定定义时所在的作用域，而不是指向运行时所在的作用域。下面是另一个例子。

```javascript
function Timer() {
  this.s1 = 0;
  this.s2 = 0;
  // 箭头函数
  setInterval(() => this.s1++, 1000);
  // 普通函数
  setInterval(function () {
    this.s2++;
  }, 1000);
}

var timer = new Timer();

setTimeout(() => console.log('s1: ', timer.s1), 3100);
setTimeout(() => console.log('s2: ', timer.s2), 3100);
// s1: 3
// s2: 0
```

上面代码中，`Timer`函数内部设置了两个定时器，分别使用了箭头函数和普通函数。前者的`this`绑定定义时所在的作用域（即`Timer`函数），后者的`this`指向运行时所在的作用域（即全局对象）。所以，3100毫秒之后，`timer.s1`被更新了3次，而`timer.s2`一次都没更新。

**总结：使用箭头函数，其中的this指向的是声明时函数的作用域（包括其中的变量）不会指向函数外的全局变量和作用域！且箭头函数无this，它的this是继承外部函数的this！**



**箭头函数的this是定义时绑定，普通函数是运行时绑定。**

 **[其实也就是说，this总是指向定义函数是所在的上一级]** 



#### JS中的this详解

### 1 函数调用

函数也可以直接被调用，这个时候this被绑定到了全局对象。

```javascript
var x = 1;  
　function test(){  
　　　this.x = 0;  
　}  
　test();  
　alert(x); //0  
```

**注意：此时的this为全局对象！**



**但这样就会出现一些问题，就是在函数内部定义的函数，其this也会指向全局**

**内层函数并未从外层函数继承`this`的值。在内层函数里，`this`会是`window`（全局）或`undefined`**

**这时我们就需要把外层this传到底层函数中**

**利用 [var self=this]**

```javascript
var A=function(){
    var self=this;
       var B=function(){
       self.x=1;  // 此时的this就是局部对象
       }
}
```

### 2 构造函数调用

在javascript中自己创建构造函数时可以利用this来指向新创建的对象上。这样就可以避免函数中的this指向全局了。

所谓构造函数，就是通过这个函数生成一个新对象（object）。这时，this就指这个新对象。 

```javascript
var x = 2;  
　function test(){  
　　　this.x = 1;  //默认是指向全局对象，相当于windows.x
　}  
　var o = new test();  
　alert(x); //2  
```

##  总结：其实也就是说，this总是指向定义函数是所在的上一级的实例对象。 

​       ↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓

```javascript
var obj = {}; 

obj.x = 100; 

obj.y = function(){ alert( this.x ); }; 

obj.y(); //弹出 100 
//这段代码非常容易理解，当执行 obj.y() 时，函数是作为对象obj的方法调用的，因此函数体内的this指向的是obj对象，所以会弹出100。
```

```javascript
var checkThis = function(){ 

alert( this.x); 
}; 

var x = 'this is a property of window';

checkThis(); //弹出 'this is a property of window' 
//这里的checkThis中的this指向上一级也就是windows对象，所以引用的是全局变量。
```

```javascript
var xx=99;
var obj={
    xx:0,
    func:function(){
        this.xx；   //就是指obj.xx    0
        xx;       //就是指window.xx    99
        //所以一般不加this等等的都是直接调用的window对象的属性
    }
}
```



# Class与继承

在ES6中引入了class的概念  但也是一种语法糖，原理依旧是ES5中的原型链实现的。

```javascript
//定义类
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}

//其中的constructor为构造方法，是类的默认方法，通过new命令生成对象实例时，自动调用该方法。一个类必须有constructor方法，如果没有显式定义，一个空的constructor方法会被默认添加
//里面this声明的变量都为实例对象

```

构造函数的`prototype`属性，在ES6的“类”上面继续存在。事实上，类的所有【方法】都定义在类的`prototype`属性上面

#### 继承

```javascript
class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}

//子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象
```
#### super关键字

`super`这个关键字，既可以当作函数使用，也可以当作对象使用。在这两种情况下，它的用法完全不同。

第一种情况，`super`作为函数调用时，代表父类的构造函数。ES6 要求，子类的构造函数必须执行一次`super`函数。

所以子类super必须重写父类的构造器里的所有参数

第二种情况，`super`作为对象时，指向父类的原型对象。

也就是super只能调用父类prototype注入的属性或方法，不能调用constructor中的实例



ES6 规定，通过`super`调用父类的方法时，`super`会绑定子类的`this`。

如果通过`super`对某个属性赋值，这时`super`就是`this`

```javascript
class A {
  constructor() {
    this.x = 1;
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
    super.x = 3;//相当于this.x=3；
    console.log(super.x); // undefined 找的是父类的原型属性，所以为undefined
    console.log(this.x); // 3 
  }
}

let b = new B();
```

#### 静态方法

类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上`static`关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

```javascript
class Foo {
  static classMethod() {
    return 'hello';
  }
}

Foo.classMethod() // 'hello'

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```

#### 静态属性

```javascript
class Foo {
}
Foo.prop = 1;
Foo.prop // 1
```

上面的写法为`Foo`类定义了一个静态属性`prop`。

目前，只有这种写法可行，因为ES6明确规定，Class内部只有静态方法，没有静态属性。



### promise对象

Promise是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。

所谓`Promise`，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise是一个对象，从它可以获取异步操作的消息。Promise提供统一的API，各种异步操作都可以用同样的方法进行处理。

他有2种状态

* 对象的状态不受外界影响
* 一旦状态改变就不会再改变

和angualrjs中的promise对象用法想同

缺点就是then方法的链式调用会造成语义不清楚

## 在ES6中引入了Generator函数

Generator 函数是协程在 ES6 的实现，最大特点就是可以交出函数的执行权（即暂停执行）

