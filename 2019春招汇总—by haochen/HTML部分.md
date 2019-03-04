一、**`doctype`的意义是什么？**

- 让浏览器以标准模式渲染
- 让浏览器知道元素的合法性

二、**HTML、HTML5、XHTML的关系**

- `HTML`属于`SGML`
- `XHTML`属于`XML`，是`HTML`进行`XML`严格化的结果
- `HTML5`不属于`SGML`或者`XML`，比`XHTML`宽松

​	GML 是第一代置标语言，使文档能明确将标示和内容分开，所以文件使用同样的标示方法。

　　SGML 在 GML 的基础上进行整理，形成了一套非常严谨的文件描述方法。它的组成包括语法定义，DTD，文件实例三部分。SGML 因太严谨规范达500多页，故而不易学、不易用、难以实现，所以在它的基础上又发展出了其他的更易用的置标语言。

　　HTML 是人们抽取了 SGML 的一个微小子集而提取出来的。其早期规范比较松散，但比较易学。

　　XML 也是 SGML 的一个子集，但使用比较严格的模式。

　　XHTML 的出现是因为HTML扩充性不好，内容的表现跟不上时代的变化（如无法表示某些化学符号等），以及因为性能的问题，官方逐渐趋于严格的模式，所以使用 XML 的严格规则的 XHTML 成了 W3C 计划中 HTML 的替代者。

 　　HTML 经过一系列修订，到现在说的 HTML 一般指 HTML 4.01；而现在的 HTML 5 则是 HTML 的第五个修订版，其主要的目标是将互联网语义化，以便更好地被人类和机器阅读，并同时提供更好地支持各种媒体的嵌入。而HTML5本身并非技术，而是标准。

​	XML设计用来传送及携带数据信息，不用来表现或展示数据，HTML语言则用来表现数据

在 HTML 4.01 中，<!DOCTYPE> 声明引用 DTD，因为 HTML 4.01 基于 SGML。DTD 规定了标记语言的规则，这样浏览器才能正确地呈现内容。

HTML5 不基于 SGML，所以不需要引用 DTD。

三、**HTML5新变化**

新的语义化标签

表单增强(新的元素，表单验证)

新的API：(离线、音视频、图形、实时通信、本地存储、设备能力) 

- `Canvas`+`WEBGL`等技术，实现无插件的动画以及图像、图形处理能力；
- 本地存储，可实现`offline`应用；
- `websocket`，一改`http`的纯`pull`模型，实现数据推送的梦想；
- `MathML`，`SVG`等，支持更加丰富的`render`；

四、**em和i有什么区别**

- `em`是语义化的标签，表强调
- `i`是纯样式的标签，表斜体
- `HTML5`中`i`不推荐使用，一般用作图标

五、**语义化的意义是什么**

- 开发者容易理解
- 机器容易理解结构(搜索、读屏软件)
- 有助于`SEO`

六、**哪些元素可以实现自闭合**

- 表单元素`input`
- 图片`img`
- `br` `hr`
- `meta` `link`

七、**meta头部相关的总结**

meta标签有两个属性，一个是name另一个是http-equiv属性

1.name属性

name属性主要用于描述页面（关键词、叙述等）。与之对应的属性是content，是对name的具体描述

其中，name属性又有几种常用的参数

A、keywords（关键字）

用于告诉搜索引擎网页的关键字

~~~
<meta name="keywords" content="Lxxyx,博客，文科生，前端">
~~~

B、description（网页内容的描述）

用于告诉搜索引擎网站的主要内容

~~~
<meta name="description" content="文科生，热爱前端与编程。目前大二，这是我的前端博客"> 
~~~

C、viewport（移动端的窗口）

~~~
<meta name="viewport" content="width=device-width, initial-scale=1">
~~~

D、robots（定义搜索引擎爬虫的索引方式）

~~~~
<meta name="robots" content="none"> 
~~~~

E、作者

F、网页的制作软件

2.http-equiv属性

顾名思义，相当于http文件头作用，其中http-equiv属性主要有以下几种参数：

A、content-type

用于设定网页字符集，便于浏览器解析与渲染页面

~~~
<meta http-equiv="content-Type" content="text/html;charset=utf-8">  //旧的HTML，不推荐

<meta charset="utf-8"> //HTML5设定网页字符集的方式，推荐使用UTF-8
~~~

B、X-UA-Compatible

用于告诉浏览器以何种版本渲染渲染页面

~~~
 <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/> //指定IE和Chrome使用最新版本渲染当前页面
~~~

C、cache-control

知道浏览器如何缓存某个响应以及缓存多长时间

~~~
<meta http-equiv="cache-control" content="no-cache">
~~~

有下面几种用法：

1. no-cache: 先发送请求，与服务器确认该资源是否被更改，如果未被更改，则使用缓存。
2. no-store: 不允许缓存，每次都要去服务器上，下载完整的响应。（安全措施）
3. public : 缓存所有响应，但并非必须。因为max-age也可以做到相同效果
4. private : 只为单个用户缓存，因此不允许任何中继进行缓存。（比如说CDN就不允许缓存private的响应）
5. maxage : 表示当前请求开始，该响应在多久内能被缓存和重用，而不去服务器重新请求。例如：max-age=60表示响应可以再缓存和重用 60 秒。

本字段也可以用于禁止百度进行转码：

~~~~html
<meta http-equiv="Cache-Control" content="no-siteapp" />
~~~~

D、网页到期时间

用于设定网页的到期时间 ，过期之后网页必须到服务器上重新传输

~~~html
<meta http-equiv="expires" content="Sunday 26 October 2016 01:00 GMT" />
~~~

E、refresh（自动刷新并指向某个页面）

网页将在设定的时间内，自动刷新并调向指定的网站

~~~html
<meta http-equiv="refresh" content="2；URL=http://www.lxxyx.win/"> //意思是2秒后跳转向我的博客
~~~

F、Set-cookie

如果页面过期，那么这个页面存在的本地cookies也会被自动删除

~~~html
<meta http-equiv="Set-Cookie" content="name, date"> //格式

<meta http-equiv="Set-Cookie" content="User=Lxxyx; path=/; expires=Sunday, 10-Jan-16 10:00:00 GMT"> //具体范例
~~~



