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

4.常见的两栏布局和三栏布局

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

