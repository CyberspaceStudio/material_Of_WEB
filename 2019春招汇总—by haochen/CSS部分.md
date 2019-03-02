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

