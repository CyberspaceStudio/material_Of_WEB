一、**有一个高度自适应的div，里面有两个div，一个高度100px，希望另一个填满剩下的高度**

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>  
      html,
      body {
        height: 100%;
        padding: 0;
        margin: 0;
      }
        .outer{
            position: relative;
            background: red;
            height: 100%;
        }
        .iner1{
            height: 100px;	
            background:blue
        }
        .iner2{
            position: absolute;
            background-color: aqua;
            top: 50px;
            bottom: 0;
            width: 100%;
            left: 0;
        }
    </style>
</head>
<body>
    <div class="outer">
        <div class="iner1"></div>
        <div class="iner2"></div>
    </div>
</body>
</html>
~~~

二、**阐述一下CSS Sprites**

将一个页面涉及到的所有图片都包含到一张大图中去，然后利用CSS的 background-image，background- repeat，background-position 的组合进行背景定位。利用CSS Sprites能很好地减少网页的http请求，从而大大的提高页面的性能；CSS Sprites能减少图片的字节。

三、**style标签写在body后与body前有什么区别？**

页面加载自上而下 当然是先加载样式。
写在body标签后由于浏览器以逐行方式对HTML文档进行解析，当解析到写在尾部的样式表（外联或写在style标签）会导致浏览器停止之前的渲染，等待加载且解析样式表完成之后重新渲染，在windows的IE下可能会出现FOUC现象（即样式失效导致的页面闪烁问题）

四、**怎么让Chrome支持小于12px 的文字？**

p{font-size:10px;-webkit-transform:scale(0.8);} //0.8是缩放比例

五、**多行文字的垂直居中**

~~~html
<style type="text/css">
      .box {
        margin: 40px auto;
        border: 1px solid #ccc;
        width: 300px;
        height: 300px;
        line-height: 300px;
      }
      .box p {
        display: inline-block;
        line-height: 30px; /*覆盖父级元素的行高*/
        vertical-align: middle; /*基线居中对齐*/
      }
</style>
  <body>
    <div class="box">
      <p class="words_p">
        方法二：对子元素设置display:inline-block属性，使其转化成行内块元素，模拟成单行文本。。
      </p>
    </div>
  </body>
~~~

六、**元素竖向的百分比设定是相对于容器的高度吗？**

当按百分比设定一个元素的宽度时，它是相对于父容器的宽度计算的，但是，对于一些表示竖向距离的属性，例如 padding-top , padding-bottom , margin-top , margin-bottom 等，当按百分比设定它们时，依据的也是父容器的宽度，而不是高度。

七、**全屏滚动的原理是什么？用到了CSS的哪些属性？**

1. 原理：有点类似于轮播，整体的元素一直排列下去，假设有5个需要展示的全屏页面，那么高度是500%，只是展示100%，剩下的可以通过transform进行y轴定位，也可以通过margin-top实现
2. overflow：hidden；transition：all 1000ms ease；

八、**浮动造成父类元素塌陷的解决方案**
1.构建BFC

~~~
<style>
    .father{
        width: 90px;
        display:inline-block;
        background: red;
    }
    .brother{
        float: left;
        height: 40px;
        width: 40px;
        background: blue;

    }
    </style>
    <body>
    <div class="father">
        <div class="brother"></div>
    </div>
</body>
~~~

2.伪元素法；

~~~html
<style>
    .father{
        width: 90px;
        background: red;
    }
    .father:after{
        content: "";
        clear: both;
        display: block;
    }
    .brother{
        float: left;
        height: 40px;
        width: 40px;
        background: blue;
    }	
    </style>
    <body>
    <div class="father">
        <div class="brother"></div>
    </div>
</body>
~~~

这里有几个问题：

首先在设置伪元素的时候，clear:both必须要加上，display必须设置成block，如果设置成inline-block依然存在高度塌陷

构建BFC：

1.根元素

2.float的值不为none（缺点：宽度丢失，并且会使下方的元素上移）

​	position造成的宽度不是100%：直接设置width：100%；

​	设置left：0；right:0

3.display的值为inline-block，table-cell（会导致宽度丢失）

4.position的值为absolute或fixed

5.overflow不为visible

display：table也认为可以生成BFC，其实这里的主要原因在于Table会默认生成一个匿名的table-cell，正是这个匿名的table-ccell生成了BFC

BFC的作用：

1.margin重叠

~~~
.container {
            overflow: hidden;
            width: 100px;
            height: 100px;
            background-color: red;
        }
        
        .box1 {
            width: 20px;
            height: 20px;
            margin: 10px 0;
            background-color: green;
        }
        
        .box2 {
            width: 20px;
            height: 20px;
            margin: 20px 0;
            background-color: green;
        }
    </style>
        <div class="container">
                <div class="box1"></div>
                <div class="box2"></div>
        </div>
~~~

这里，相邻两个块儿的margin发生了重叠，并且这里当两个margin不相同的时候，会采用最大的那个margin作为最终结果。

2.防止高度塌陷

3.float文字环绕的问题

~~~html
<style>
      .div1 {
        float: left;
        height: 100px;
        width: 100px;
        background: red;
      }
      .div2 {
        overflow: hidden;
      }
</style>

    <div style="width:150px;">
      <div class="div1"></div>
      <div class="div2">
        你好啊啊啊啊啊啊你好啊啊啊啊啊啊你好啊啊啊啊啊啊你好啊啊啊啊啊啊你好啊啊啊啊啊啊你好啊啊啊啊啊啊你好啊啊啊啊啊啊
      </div>
    </div>
~~~

这种情况会导致div2中的文字会沿着浮动的元素环绕，如果不想环绕可以给div2设置overflow: hidden;触发一个

BFC，从而可以去掉文字环绕的效果

九.**常见的两栏布局和三栏布局**

两栏布局：

~~~html
    <style>
        .column:nth-child(1){
            float: left;
            width: 200px;
            height: 300px;
            margin-right: 10px;
            background-color: red;
        }
        .column:nth-child(2){
            overflow: hidden;/*创建bfc */
            height: 300px;
            background-color: purple;
        }
    </style>
    <div>
      <div class="column"></div>
      <div class="column"></div>
    </div>
~~~

三栏布局：

~~~html
    <style>
      .column:nth-of-type(1),
      .column:nth-of-type(2) {
        float: left;
        width: 100px;
        height: 300px;
        background-color: green;
      }

      .column:nth-of-type(2) {
        float: right;
      }

      .column:nth-of-type(3) {
        overflow: hidden; /*创建bfc*/
        height: 300px;
        background-color: red;
      }
    </style>
    
    <div>
      <div class="column"></div>
      <div class="column"></div>
      <div class="column"></div>
    </div>
~~~

十、**未知宽高的元素实现水平垂直居中**

1.通过display:table实现垂直居中

优势：父元素可以动态改变高度。
劣势：table属性容易造成多次reflow,IE8以下不支持

~~~html
  <style>
    .parent1 {
      display: table;
      height: 300px;
      width: 300px;
      background-color: #fd0c70;
    }
    .parent1 .child {
      display: table-cell;
      vertical-align: middle;
      text-align: center;
      color: #fff;
      font-size: 16px;
    }
  </style>
    <body>
    <div class="parent1">
      <div class="child">hello world-1</div>
    </div>
  </body>
~~~

2.利用伪元素添加一个新的元素实现：

优点：兼容性好
缺点：多出来个空元素、麻烦

~~~html
  <style>
    .wrap {
      position: absolute;
      width: 100%;
      height: 100%;
      text-align: center;
      background: #92b922;
    }
    .test {
      background: #de3168;
      display: inline-block;
      color: #fff;
      padding: 20px;
    }
    .wrap:after {
      display: inline-block;
      content: "";
      width: 0px;
      height: 100%;
      vertical-align: middle;
    }
  </style>
    <div class="wrap">
    <div class="test">
      水平垂直居中了吧<br />
      两行文字哦
    </div>
  </div>
~~~

3.绝对定位+tranform

优点：方便，支持webkit内核
缺点：transform兼容性差，IE9以下不支持

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>未知宽高元素水平垂直居中</title>
</head>
<style>
.parent3{
    position: relative;
    height:300px;
    width: 300px;
    background: #FD0C70;
}
.parent3 .child{
    position: absolute;
    top: 50%;
    left: 50%;
    color: #fff;
    transform: translate(-50%, -50%);
}
</style>
<body>
<div class="parent3">
        <div class="child">hello world</div>
    </div>
</body>
</html>

~~~

十一、**列举HTML5特性**

语意化标签(nav、aside、dialog、header、footer等)

canvas

拖放相关api

Audio、Video

获取地理位置

更好的input校验

web存储(localStorage、sessionStorage)

webWorkers(类似于多线程并发)

webSocket

十二、**列举CSS3特性**

选择器

边框(border-image、border-radius、box-shadow)

背景(background-clip、background-origin、background-size)

渐变(linear-gradients、radial-gradents)

字体(@font-face)

转换、形变(transform)

过度(transition)

动画(animation)

弹性盒模型(flex-box)

媒体查询(@media)

十三、**transition和animation的区别是什么？**

过渡属性transition可以在一定的事件内实现元素的状态过渡为最终状态，用于模拟一种过渡动画效果，但是功能有限，只能用于制作简单的动画效果；

动画属性animation可以制作类似Flash动画，通过关键帧控制动画的每一步，控制更为精确，从而可以制作更为复杂的动画。

十四、**link和@import区别**

**1.从属关系区别**
`@import`是 CSS 提供的语法规则，只有导入样式表的作用；`link`是HTML提供的标签，不仅可以加载 CSS 文件，还可以定义 RSS、rel 连接属性等。

**2.加载顺序区别**
加载页面时，`link`标签引入的 CSS 被同时加载；`@import`引入的 CSS 将在页面加载完毕后被加载。

**3.兼容性区别**
`@import`是 CSS2.1 才有的语法，故只可在 IE5+ 才能识别；`link`标签作为 HTML 元素，不存在兼容性问题。

**4.DOM可控性区别**
可以通过 JS 操作 DOM ，插入`link`标签来改变样式；由于 DOM 方法是基于文档的，无法使用`@import`的方式插入样式。

**5.权重区别(该项有争议，下文将详解)**
`link`引入的样式权重大于`@import`引入的样式。

~~~html
    <link rel="stylesheet" href="test34.css">
    <style type="text/css">
        @import url("test33.css");
    </style>
<body>
        <div style="width: 50px; height: 50px;"></div>
</body>
~~~

这里会发现div的背景颜色是黄色，说明@import起到了作用（打脸）

~~~html
    <style type="text/css">
        @import url("test34.css");
    </style>
    <link rel="stylesheet" href="test34.css">
~~~

这里的背景颜色是绿色

~~~
    <style type="text/css">
        div{
            background:yellow
        }
        @import url("test33.css");
    </style>
~~~

这里的背景颜色是黄色，且无边框

原因：https://www.cnblogs.com/KilerMino/p/6115803.html

这样做会导致css无法并行下载，因为使用@import引用的文件只有在引用它的那个css文件被下载、解析之后，浏览器才会知道还有另外一个css需要下载，这时才去下载，然后下载后开始解析、构建render tree等一系列操作。

十四、**如何实现0.5px的边框**



十五、**CSS动画和js动画比较**

chrome中，渲染线程分为main thread和compositor thread。如果CSS动画改变的只是`transforms`和`opacity`，那么整个动画是在compositor thread中完成的。而js动画是在main thread中完成的，然后会触发compositor thread进行接下来的操作。但是js执行的过程中，main thread处于繁忙，CSS动画使用compositor thread会相对流畅一点。

CSS动画比JS流畅的前提：

- 在Chromium基础上的浏览器中
- JS在执行一些昂贵的任务
- 同时CSS动画不触发layout或paint
  在CSS动画或JS动画触发了paint或layout时，需要main thread进行Layer树的重计算，这时CSS动画或JS动画都会阻塞后续操作。

两者的对比：

- 实现/重构难度不一，CSS3比JS更简单，性能调优方向固定
- 对于帧速表现不好的低版本浏览器，CSS3可以做到自然降级，而JS则需要撰写额外代码
- CSS动画有天然事件支持（`TransitionEnd`、`AnimationEnd`，但是它们都需要针对浏览器加前缀），JS则需要自己写事件
- JavaScript在浏览器的主线程中运行，而主线程中还有其它需要运行的JavaScript脚本、样式计算、布局、绘制任务等,对其干扰导致线程可能出现阻塞，从而造成丢帧的情况。

- CSS3有兼容性问题，而JS大多时候没有兼容性问题
- 功能涵盖面，JS比CSS3大
  - 定义动画过程的`@keyframes`不支持递归定义，如果有多种类似的动画过程，需要调节多个参数来生成的话，将会有很大的冗余（比如jQuery Mobile的动画方案），而JS则天然可以以一套函数实现多个不同的动画过程
  - 时间尺度上，`@keyframes`的动画粒度粗，而JS的动画粒度控制可以很细
  - CSS3动画里被支持的时间函数非常少，不够灵活
  - 以现有的接口，CSS3动画无法做到支持两个以上的状态转化，JavaScript动画控制能力很强, 可以在动画播放过程中对动画进行控制：开始、暂停、回放、终止、取消都是可以做到的。

