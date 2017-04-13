### HTML5

* 语义化

  具体来说，就是在书写html时，尽量使用具有语义信息的标签，例如header,nav,aside,section等代替那些没有语义信息的标签

下面是HTML新增的语义化标签

### <header>[块模型]

header 元素代表“网页”或“section”的页眉。
通常包含`h1-h6`元素或`hgroup`，作为整个页面或者一个内容块的标题。也可以包裹一节的目录部分，一个搜索框，一个`nav`，或者任何相关logo。

整个页面没有限制header元素的个数，可以拥有多个，可以为每个内容块增加一个header元素

```html
<header>
    <hgroup>
        <h1>网站标题</h1>
        <h1>网站副标题</h1>
    </hgroup>
</header>
```

### <footer>[块模型]

`footer`元素代表“网页”或“section”的页脚，通常含有该节的一些基本信息，譬如：作者，相关文档链接，版权资料。如果`footer`元素包含了整个节，那么它们就代表附录，索引，提拔，许可协议，标签，类别等一些其他类似信息。

```html
<footer>
    COPYRIGHT@小北
</footer>
```

### <hgroup>[块模型]

`hgroup`元素代表“网页”或“section”的标题，当元素有多个层级时，该元素可以将`h1`到`h6`元素放在其内，譬如文章的主标题和副标题的组合

```html
<hgroup>
    <h1>这是一篇介绍HTML 5语义化标签和更简洁的结构</h1>
    <h2>HTML 5</h2>
</hgroup>
```

## <nav>[块模型]

`nav`元素代表页面的导航链接区域。用于定义页面的**主要导航部分**。

```html
<nav>
    <ul>
        <li>HTML 5</li>
        <li>CSS3</li>
        <li>JavaScript</li>
    </ul>
</nav>
```

## <aside>[块模型]

`aside`元素被包含在article元素中作为主要内容的附属信息部分，其中的内容可以是与当前文章有关的相关资料、标签、名次解释等。（特殊的section）

在article元素之外使用作为页面或站点全局的附属信息部分。最典型的是侧边栏，其中的内容可以是日志串连，其他组的导航，甚至广告，这些内容相关的页面。

```
<article>
    <p>内容</p>
    <aside>
        <h1>作者简介</h1>
        <p>小北，前端一枚</p>
    </aside>
</article>

```

`aside`实例

aside使用总结：

- aside在article内表示主要内容的附属信息，
- 在article之外则可做侧边栏，没有article与之对应，最好不用。
- 如果是广告，其他日志链接或者其他分类导航也可以用

## <section>[块模型]

`section`元素代表文档中的“节”或“段”，“段”可以是指一篇文章里按照主题的分段；“节”可以是指一个页面里的分组。

section通常还带标题，虽然html5中section会自动给标题h1-h6降级，但是最好手动给他们降级。如下：

```
<section>
    <h1>section是啥？</h1>
    <article>
        <h2>关于section</h1>
        <p>section的介绍</p>
        <section>
            <h3>关于其他</h3>
            <p>关于其他section的介绍</p>
        </section>
    </article>
</section>
```

`section`示例代码

section使用注意：

一张页面可以用section划分为简介、文章条目和联系信息。不过在文章内页，最好用article。section不是一般意义上的容器元素，如果想作为样式展示和脚本的便利，可以用div。

- 表示文档中的节或者段；
- article、nav、aside可以理解为特殊的section，所以如果可以用article、nav、aside就不要用section，没实际意义的就用div

## <article>[块模型]

`article`元素最容易跟`section`和`div`容易混淆，其实`article`代表一个在文档，页面或者网站中自成一体的内容，其目的是为了让开发者独立开发或重用。譬如论坛的帖子，博客上的文章，一篇用户的评论，一个互动的widget小工具。（特殊的section）

除了它的内容，`article`会有一个标题（通常会在`header`里），会有一个`footer`页脚。我们举几个例子介绍一下article，好更好区分article、section、div

```
<article>
    <h1>一篇文章</h1>
    <p>文章内容..</p>
    <footer>
        <p><small>版权：html5jscss网所属，作者：小北</small></p>
    </footer>
</article>
```

一篇简单文章的article示例代码

上例是最好简单的article标签使用情况，如果在article内部再嵌套article，那就代表内嵌的article是与它外部的内容有关联的，如博客文章下面的评论，如下：

```
<article>

    <header>
        <h1>一篇文章</h1>
        <p><time pubdate datetime="2012-10-03">2012/10/03</time></p>
    </header>

    <p>文章内容..</p>

    <article>
        <h2>评论</h2>

        <article>
            <header>
                <h3>评论者: XXX</h3>
                <p><time pubdate datetime="2012-10-03T19:10-08:00">~1 hour ago</time></p>
            </header>
            <p>哈哈哈</p>
        </article>

        <article>
            <header>
                <h3>评论者: XXX</h3>
                <p><time pubdate datetime="2012-10-03T19:10-08:00">~1 hour ago</time></p>
            </header>
            <p>哈？哈？哈？</p>
        </article>

    </article>

</article>

```

文章里的评论，一个article嵌套article来表示的实例

article内部嵌套article，有可能是评论或其他跟文章有关联的内容。那article内部嵌套section一般是什么情况呢。如下：

```
<article>

    <h1>前端技术</h1>
    <p>前端技术有那些</p>

    <section>
        <h2>CSS</h2>
        <p>样式..</p>
    </section>

    <section>
        <h2>JS</h2>
        <p>脚本</p>
    </section>

</article>
```

文章里的章节，一个article里的section实例

因为文章内section部分虽然也是独立的部分，但是它门只能算是*组成整体的一部分*，从属关系，article是大主体，section是构成这个大主体的一部分。本网站的全部文章都是article嵌套一个个section章节，这样能让浏览器更容易区分各个章节所包括的内容。

那section内部嵌套article又有哪些情况呢，如下

```
<section>
    
    <h1>介绍: 网站制作成员配备</h1>

    <article>
        <h2>设计师</h2>
        <p>设计网页的...</p>
    </article>

    <article>
        <h2>程序员</h2>
        <p>后台写程序的..</p>
    </article>

    <article>
        <h2>前端工程师</h2>
        <p>给楼上两位打杂的..</p>
    </article>

</section>
```


#### HTML5的本地存储

众所周知Html4时代Cookie的大小、格式、存储数据格式等限制，网站应用如果想在浏览器端存储用户的部分信息，那么只能借助于Cookie。但是Cookie的这些限制，也就导致了Cookie只能存储一些ID之类的标识符等简单的数据，复杂的数据就更别扯了

下面是Cookie的限制：

1. 1, 大多数浏览器支持最大为 4096 字节的 Cookie。
2. 2, 浏览器还限制站点可以在用户计算机上存储的 Cookie 的数量。大多数浏览器只允许每个站点存储 20 个 Cookie；如果试图存储更多 Cookie，则最旧的 Cookie 便会被丢弃。
3. 3, 有些浏览器还会对它们将接受的来自所有站点的 Cookie 总数作出绝对限制，通常为 300 个。
4. 4, Cookie默认情况都会随着Http请求发送到后台服务器，但并不是所有请求都需要Cookie的，比如：js、css、图片等请求则不需要cookie。





#### 会话级别的本地存储 sessionStorage

sessionStorage提供了四个方法来辅助我们进行对本地存储做相关操作。

（1）setItem(key,value)：添加本地存储数据。两个参数，非常简单就不说了。

（2）getItem(key):通过key获取相应的Value。

（3）removeItem(key):通过key删除本地数据。

（4）clear():清空数据。

```javascript
<script type="text/javascript">
        //添加key-value 数据到 sessionStorage
        sessionStorage.setItem("demokey", "http://blog.itjeek.com");
        //通过key来获取value
        var dt = sessionStorage.getItem("demokey");
        alert(dt);
        //清空所有的key-value数据。
        //sessionStorage.clear();
        alert(sessionStorage.length);
    </script>            
```

#### 永久本地存储localStorage

localStorage提供了四个方法来辅助我们进行对本地存储做相关操作。

（1）setItem(key,value)：添加本地存储数据。两个参数，非常简单就不说了。

（2）getItem(key):通过key获取相应的Value。

（3）removeItem(key):通过key删除本地数据。

（4）clear():清空数据。

```javascript
<script type="text/javascript">
        //添加key-value 数据到 sessionStorage
        localStorage.setItem("demokey", "http://blog.itjeek.com");
        //通过key来获取value
        var dt = localStorage.getItem("demokey");
        alert(dt);
        //清空所有的key-value数据。
        //localStorage.clear();
        alert(localStorage.length);
    </script>
```

