### Angular2

**第一点：Angular2 删掉了 $scope 的概念**

在 Angular 1.x 里面，$scope 是一个相当强大又相当可怕的东西，由于在很多需要回调的场景之下，脏值检测机制无法感知到 $scope 上发生的变化，因此经常需要开发者自己手动调用 $apply() 方法。典型的场景有：事件回调、setTimeout 回调、Ajax 回调等等。Angular2 响应社区的强烈呼吁，删除（或者说隐藏）了 $scope 的概念，开发者不再需要感知到它的存在。另外，Angular2 在底层引入了 zone.js，所以即使在各种回调函数中修改数据模型也不需要手动调用 $apply() 方法了。

 

**第二点：删掉了 ng-controller 指令**

这就意味着 Controller 不再是一个独立的组件，它合并到了 Component 内部。这是一个非常大的演进，因为从大量的实战经验来看，在复杂的业务逻辑中复用 Controller 几乎是不可能的。在其它同类的前端框架里面也有类似的处理手法，例如 Backbone 虽然也强调 MVC 的概念，但是它也没有定义单独的 Controller 类，Controller 也是合并在 View 里面编写的。

 

**第三点：大幅度演进了脏值检测机制**

众所周知，“双向数据绑定”之所以能运行，是因为 Angular 底层有“脏值检测”这么一个神奇的机制。而实际上 Angular 1.x 里面的脏值检测机制的运行效率非常差，这就是为什么大家一直在抱怨绑定的对象不能太多、太深的原因。Angular2 大幅度演进了这一机制，不仅引入了单向绑定，还增加了各种检测策略，例如：只检测一次、利用 JIT 动态生成脏值检测代码等等。毫无疑问，有了这些工具之后，数据绑定效率不再是问题。

 

**第四点：嵌套路由**

Angular 1.x 里面有一个非常讨厌的问题，框架内置的路由机制不支持嵌套使用，这就导致开发者在日常的开发过程中不得不依赖于第三方的 ui-router 库。Angular2 没有这个问题了，因为 Angular2 的路由是基于 Component 的，天然支持嵌套。

 

**第五点：依赖注入机制演进**

Angular2 中的依赖注入写法与 Java 中的注解（Annotation）非常类似，如果你熟悉 Spring 注解的用法，那么使用 Angular2 的依赖注入几乎没有学习成本。当然，概念上是有区别的，Angular2 中叫 Decorator（装饰器），更加贴近 Python 里面的 Decorator 的概念。

 

**第六点：框架整体上基于 TypeScript 开发**

这是最大的一个变更，有很多人担忧这样是否会带来比较大的学习成本，实际的情况并非如此。因为 TypeScript 的语法与 Java 或者 C# 非常类似，因此对于从后端转过来的开发者来说，学习这门语言几乎是没有难度的。

还有一个重要的方面需要大家注意：TypeScript 是 Microsoft 开发的一门语言，Google+Microsoft 这样的组合会产生多么强大的推动力，大家可以想象。Google 和 Microsoft 本身都是重要的浏览器厂商，Chrome 和 IE 加起来的市场份额占据了一大半的市场份额，未来如果两款浏览器内建 TypeScript 引擎，很显然 TypeScript 和 Angular 的前景将会一片光明。这一优势是大量的同类技术框架根本无法企及的，因此大家在做技术选型的过程中需要综合考虑这些情况作出理性的决策。

 

**第七点：Angular 1.x 和 Angular2 都自带 UI 控件库**

两个版本的 UI 控件库都实现了 Material Design 所提出来的设计风格，Material Design 是 Google 提出来的一种 UI 设计原则，终极的目标是：用一套 UI 设计规范来兼容各种各样的设备，例如桌面、平板、大屏幕的电视、车载系统、甚至 watch，从而保证用户体验的一致性。

* **项目的模块入口**

  ```javascript
  import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
  import { AppModule } from './app.module';
  //此处为主模块的入口
  platformBrowserDynamic().bootstrapModule(AppModule);
  ```


* **模块声明**

  ```javascript
  import './rxjs-extensions';

  import { NgModule }      from '@angular/core';
  import { BrowserModule } from '@angular/platform-browser';
  import { FormsModule }   from '@angular/forms';
  import { HttpModule }    from '@angular/http';

  import { InMemoryWebApiModule } from 'angular2-in-memory-web-api';
  import { InMemoryDataService }  from './in-memory-data.service';

  import { AppComponent }         from './app.component';
  import { DashboardComponent }   from './dashboard.component';
  import { HeroesComponent }      from './heroes.component';
  import { HeroDetailComponent }  from './hero-detail.component';
  import { HeroService }          from './hero.service';
  import { HeroSearchComponent }  from './hero-search.component';
  import { routing }              from './app.routing';
  //imports是模块引用其他模块  declarations是注入组件 providers是注册服务 bootstrap是启动主模块
  @NgModule({
    imports: [  
      BrowserModule,
      FormsModule,
      HttpModule,
      InMemoryWebApiModule.forRoot(InMemoryDataService),
      routing
    ],
    declarations: [
      AppComponent,
      DashboardComponent,
      HeroDetailComponent,
      HeroesComponent,
      HeroSearchComponent
    ],
    providers: [
      HeroService,
    ],
    bootstrap: [ AppComponent ]
  })
  //抛出模块
  export class AppModule {
  }

  ```



* **声明组件**

  ```javascript
  import { Component }          from '@angular/core';
  //声明一个组件   
  @Component({
    selector: 'my-app',

    template: `
      <h1>{{title}}</h1>
      <nav>
        <a routerLink="/dashboard" routerLinkActive="active">Dashboard</a>
        <a routerLink="/heroes" routerLinkActive="active">Heroes</a>
      </nav>
      <router-outlet></router-outlet>
    `,
    styleUrls: ['app/app.component.css']
  })
  export class AppComponent {
    private title = 'Tour of Heroes';
    constructor(){
    	this.title='haha'  
    }
  }
  //模版绑定数据会优先使用构造器里的数据,本例子中 前台title取的是构造器里的title即为haha
  ```