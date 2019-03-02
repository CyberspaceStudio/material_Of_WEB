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