1.**能否模拟实现JS的new操作符**

~~~js
/**
 * 模拟实现 new 操作符
 * @param  {Function} ctor [构造函数]
 * @return {Object|Function|Regex|Date|Error}      [返回结果]
 */
function newOperator(ctor){
    if(typeof ctor !== 'function'){
      throw 'newOperator function the first param must be a function';
    }
    // ES6 new.target 是指向构造函数
    newOperator.target = ctor;
    // 1.创建一个全新的对象，
    // 2.并且执行[[Prototype]]链接
    // 4.通过`new`创建的每个对象将最终被`[[Prototype]]`链接到这个函数的`prototype`对象上。
    var newObj = Object.create(ctor.prototype);
    // ES5 arguments转成数组 当然也可以用ES6 [...arguments], Array.from(arguments);
    // 除去ctor构造函数的其余参数
    var argsArr = [].slice.call(arguments, 1);
    // 3.生成的新对象会绑定到函数调用的`this`。
    // 获取到ctor函数返回结果
    var ctorReturnResult = ctor.apply(newObj, argsArr);
    // 小结4 中这些类型中合并起来只有Object和Function两种类型 typeof null 也是'object'所以要不等于null，排除null
    var isObject = typeof ctorReturnResult === 'object' && ctorReturnResult !== null;
    var isFunction = typeof ctorReturnResult === 'function';
    if(isObject || isFunction){
        return ctorReturnResult;
    }
    // 5.如果函数没有返回对象类型`Object`(包含`Functoin`, `Array`, `Date`, `RegExg`, `Error`)，那么`new`表达式中的函数调用会自动返回这个新的对象。
    return newObj;
}
~~~

2.**手写call函数**

 ~~~js
Function.prototype.myCall = function(context){
    if(typeof this !== 'function'){
        throw new Error('Not Function')
    }
    // 如果没有传入this，就默认是window
    context = context || ''
    // 这里的this就是我们的函数
    context.fn = this; 
    // 获得除this之外的参数
    const args = [...arguments].slice(1);
    const result = context.fn(...args)
    delete context.fn;
    return result;
}
 ~~~

3.**手写apply函数**

~~~js
Function.prototype.myApply = function(context){
    if(typeof this !== 'function'){
        throw new Error('Not Function')
    }
    // 如果没有传入this，就默认是window
    context = context || ''
    // 这里的this就是我们的函数
    context.fn = this; 
    let result;
    if(arguments[1])
        result = context.fn(...arguments[1])
    else
        result = context.fn();
    delete context.fn;
    return result;
}
~~~

4.**手写bind函数**

~~~js
Function.prototype.myBind = function(context){
    console.log(typeof this)
    // if(typeof this !== 'function'){
    //     throw new Error('Not Function')
    // }
    const _this = this;
    const args = [...arguments].slice(1);
     return function F(){
         // 返回一个函数的时候进行判断
         if(this instanceof F){
             return new _this(...args,...arguments);
         }
         console.log(args.concat(...arguments))
         return _this.apply(context,args.concat(...arguments))
     }
}
~~~

5.**手动实现一个map函数**

~~~js
Array.prototype.myMap = function myMap(fn, context){
    if(typeof fn !== "function"){
        throw new Error("arguments is not a function")
    }
    let arr = this;
    for(let i = 0;i < arr.length;i++){
        let result = fn.call(context,arr[i],i,arr)
        // 执行map函数的时候会传入三个参数：当前值，index和arr数组
        if(result)
            arr[i] = result;
    }
}
~~~

6.**实现一个reduce函数**

~~~js
Array.prototype.fakeReduce = function fakeReduce(fn, base) {
    if (typeof fn !== "function") {
      throw new TypeError("arguments[0] is not a function");
    }
    let initialArr = this;
   // arr为数组的一个副本
    let arr = initialArr.concat();
  
    if (base) arr.unshift(base);
    let index, newValue;
  
    while (arr.length > 1) {
      index = initialArr.length - arr.length + 1;
      newValue = fn.call(null, arr[0], arr[1], index, initialArr);
      arr.splice(0, 2, newValue); // 直接用 splice 实现替换
    }
  
    return newValue;
  };
~~~

7、**谈谈你对let、const和var的理解**

​	首先var会存在变量的提升，也就是虽然var的变量还没有声明，但是却可以使用。

~~~js
var a
var a
a = 10
console.log(a)		//输出是10
~~~

var声明的变量会发生提升，声明的函数也会发生提升。并且优先于变量提升。

~~~js
console.log(a) // ƒ a() {}
function a() {}
var a = 1
~~~

对const和let声明的变量，变量并不会挂载到window上，这一点就和var声明有区别。



8、**什么是ajax，简单谈谈ajax的优缺点**

AJAX即“Asynchronous JavaScript and XML”（异步的JavaScript与XML技术），是一种用来创建`交互式网页应用`的网页开发技术，本质是在HTTP的基础上以异步的方式与服务器进行通信，包含的技术有：

- 基于HTML和CSS进行表示；
- 使用 DOM进行动态显示及交互；
- 使用 XML 和 JSON 进行数据交换及相关操作；
- 使用 XMLHttpRequest 进行异步数据查询、检索；
- 使用 JavaScript 将所有的东西绑定在一起。

传统Web应用请求存在的问题：

1.传统的Web应用提交表单的时候，服务器接收到表单信息之后会返回一个新的页面，但这个做法实际上浪费了很大的带宽，因为基本上我们前后的HTML代码u武器qin是相同的。

2.利用同步请求需要等待服务器的供求处理，导致了浏览器的阻塞。

ajax对问题的解决：

1.ajax可以指向请求仅需的数据，在客户端采用javascript进行数据的处理，由于之间交换的数据量已经大大降低，所以响应会更快。

2.非阻塞式的请求提高了用户体验。

ajax建立的过程：

（1）创建一个XMLHttpRequest对象，也就是一个异步调用的对象。

（2）创建一个新的HTTP请求，指定请求的方式、URL以及验证。

（3）设置HTTP请求的状态变化函数

（4）获取异步调用返回的数据。

（5）使用JavaScript和DOM实现局部刷新。

~~~html
<script>
    var xmlHttp = null;
    if(window.XMLHttpRequest){
        xmlHttp = new XMLHttpRequest();
    }

    if(xmlHttp != null){
        xmlHttp.onreadystatechange = state_change;
        // GET请求的方式
        // xmlHttp.open('GET',url,true);       //指定请求，这里要访问的url路径，true代表允许异步
        // xmlHttp.send(null);
        
        // POST请求的方式
        xmlHttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
        xmlHttp.send('name=jack&age=998');
    }

    function state_change(){
        if(xmlHttp.readyState == 4 && xmlHttp.status == 200){
            // console.log(xmlHttp.responseText)
        }
    }
</script>
~~~

ajax的优缺点：

优点：

1.能够在整个页面都不更新的前提下发送维护数据，避免发送没有改变的信息。

2.通过异步的模式，不阻塞用户，提升了用户的体验。

3.ajax不需要任何浏览器插件，需要用户允许JavaScript在浏览器上运行。

3.ajax引擎在客户端执行，承担了一部分服务器的工作，减少了大量用户下服务器的负担。

缺点：

1.破坏用户的后退和添加收藏夹功能，因为页面是 动态生成的，浏览器仅能记住静态的页面。

2.GET会暴露和服务器交互细节。

3.对搜索引擎支持比较弱。通过ajax更新的页面可能无法被搜索到。

9.**手写实现一个jsonp**

### JSONP原理：

1. 首先在客户端注册一个callback, 然后把callback的名字传给服务器。
2. 此时，服务器先生成 json 数据。
3. 然后以 javascript 语法的方式，生成一个function , function 名字就是传递上来的callback参数值 .
4. 最后将 json 数据直接以入参的方式，放置到 function 中，这样就生成了一段 js 语法的文档，返回给客户端。
5. 客户端浏览器，解析script标签，并执行返回的 javascript 文档，此时数据作为参数，传入到了客户端预先定义好的 callback 函数里.

JSONP由两部分组成：回调函数和数据。回调函数是当响应到来时应该在页面中调用的函数。回调函数的名字一般是在请求中指定的。而数据就是传入回调函数中的JSON数据。

~~~html
  <body>
    <div id="result"></div>
    <script>
      (function(window, document, undefined) {
        var jsonp = function(url, data, callback) {
          // 回调函数+时间戳
          var cbName = "callback_" + new Date().getTime();
          // 暴露全局函数给window
          // 判读查询字符串最后一位是否为?或者是&
          var queryString = url.indexOf("?") == -1 ? "?" : "&";
          // 遍历传进来的data实参赋值给查询字符串
          for (var k in data) {
            queryString += k + "=" + data[k] + "&";
          }
          // 查询字符串加上回调函数
          queryString += "callback=" + cbName;
          // 创建script标签
          var ele = document[0].createElement("script");
          // 给script标签添加src属性值
          ele.src = url + queryString;
          window[cbName] = function(data) {
            callback(data);
            document[0].body.removeChild(ele);
          };
          // 添加到body尾部
          document[0].body.appendChild(ele);
        };
        //jsonp函数暴露给window
        window.$jsonp = jsonp;
      })(window, document, undefined);
    </script>
    <script>
      $jsonp(
        "http://api.douban.com/v2/movie/in_theaters",
        {
          count: 1
        },
        function(data) {
          document.getElementsByTagName("body")[0].innerHTML = JSON.stringify(
            data
          );
        }
      );
    </script>
  </body>
~~~

~~~js
 const jsonp = function (url, data) {
      return new Promise((resolve, reject) => {
        // 初始化url
        let dataString = url.indexOf('?') === -1 ? '?' : '&'
        let callbackName = `jsonpCB_${Date.now()}`
        url += `${dataString}callback=${callbackName}`
        if (data) {
         // 有请求参数，依次添加到url
          for (let k in data) {
            url += `&${k}=${data[k]}`
          }
        }
        let jsNode = document.createElement('script')
        jsNode.src = url
        // 触发callback，触发后删除js标签和绑定在window上的callback
        window[callbackName] = result => {
          delete window[callbackName]
          document.body.removeChild(jsNode)
          if (result) {
            resolve(result)
          } else {
            reject('没有返回数据')
          }
        }
        // js加载异常的情况
        jsNode.addEventListener('error', () => {
          delete window[callbackName]
          document.body.removeChild(jsNode)
          reject('JavaScript资源加载失败')
        }, false)
        // 添加js节点到document上时，开始请求
        document.body.appendChild(jsNode)
      })
    }
    jsonp('http://192.168.0.103:8081/jsonp', {a: 1, b: 'heiheihei'})
      .then(result => { console.log(result) })
      .catch(err => { console.error(err) })
~~~

有几点需要注意：

1.callback函数要绑定在window对象上
2.服务端返回数据有特定格式要求：callback函数名+'('+JSON.stringify(返回数据) +')'
3.不支持post，因为js标签本身就是一个get请求

番外：

关于HTML几种标签能否获取数据的说明：

~~~html
<!-- 早期用于统计链接，支持跨域但是无法实现获取服务端返回的数据 -->
<img src="http://www.baidu.com?id=xxx">
<!-- 支持，可以接受服务端数据，但过程复杂 -->
<iframe src="http://www.baidu.com?id=xxx" frameborder="0"></iframe>
<!-- 会在css处理阶段报错 -->
<link rel="stylesheet" href="http://www.baidu.com?id=xxx">
<!-- script可以接受数据并处理 -->
<script src="http://www.baidu.com?id=xxx"></script>
<!-- callback({}) -->
~~~

10、**谈谈js的垃圾回收和内存泄漏**

JavaScript垃圾回收机制通常有下面的两种机制：

1.**引用计数**

老板浏览器采用的方式，判断一个对象是否要被回收就是要判断是否有引用指向他，如果是那么就回收。这种方式有一个非常糟糕的情况就是循环引用的时候会出现问题。

2.**标记清除**

这个算法就是“对象是否不在需要”定义为“对象是否可以获得”。算法会嘉定有一个根（root）的存在，在JavaScript里面，根是全局对象。垃圾回收期将定期从根开始，找到从根开始引用的对象，然后找到这些对象引用的对象。从根开始，垃圾回收器将找到所有可以获得的对象和不能够获得的对象。这个算法相对于引用计数的优势在于，“有零引用的对象”总是不可获得的，但是相反却不一定。

现在基本上所有的现代浏览器都采用了标记—清除的垃圾回收算法。限制就是哪些无法从根对象查询的对象都将被清除。当然在实际开发的过程中这种情况也很少见。

常见的内存泄漏的场景举例：

1.忘记声明局部变量：

~~~js
function a(){
    b=2
    console.log('b没有被声明!')
}
~~~

这个例子中，b没有用var声明，会被认为是一个全局的变量。在页面关闭之前都不会被清除。在严格模式下可以比卖你这种情况。

2.闭包带来的内存泄漏问题：

~~~js
var leaks = (function(){
    var leak = 'xxxxxx';// 闭包中引用，不会被回收
    return function(){
        console.log(leak);
    }
})()
~~~

leak在闭包中被引用，这时候就不会被回收，当然这里我们是故意不让其被回收的。

3.移除DOM节点的时候忘记暂存的值

有时候出于优化的目的，我们会利用一个变量来暂存节点，使用的时候就不用从DOM中去获取。但是我们在移除节点的时候却忘记了接触暂存的变量对DOM节点的引用，也会造成内存的泄漏。

~~~js
var element = {
  image: document.getElementById('image'),
  button: document.getElementById('button')
};

document.body.removeChild(document.getElementById('image'));
// 如果element没有被回收,这里移除了 image 节点也是没用的,image 节点依然留存在内存中.
~~~

这种情况还有DOM节点绑定了事件，但是在移除的时候没有解除事件的绑定，这时候仅仅移除DOM节点也是没有用的。

4.定时器中的内存泄漏问题：

~~~js
var someResource = getData();
setInterval(function() {
    var node = document.getElementById('Node');
    if(node) {
        node.innerHTML = JSON.stringify(someResource));
    }
}, 1000);

~~~

由于someResource被定时器引用，因此这里如果不清除定时器的话，someResource也就不会被释放，如果其刚好占用了很大的内存的话，就会引发性能问题。

11.**谈谈js函数的防抖和节流**

函数防抖：

在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时。

~~~js
function debounce(fun, delay) {
    return function (args) {
        let that = this
        let _args = args
        clearTimeout(fun.id)
        fun.id = setTimeout(function () {
            fun.call(that, _args)
        }, delay)
    }
}
~~~

比如现在我们需要做一个input的表单验证，现在我们定义一个keyup事件，如果我们每按下一个键，系统就会为我们做一次验证，很明显这里如果用户没有输入完就不需要做验证，这时候就可以用到防抖处理下，如果用户在设置的delay时间内触发了相同的动作，那么我们就会清除上次的定时器重新开始计时。如果用户在delay时间之外进行这个动作，则可以进行。

函数节流：

规定在一个时间单位内，只触发一次函数，如果在这个单位时间内多次触发了函数，那么也只有一次生效。

~~~js
function throttle (fn, delay) {
  let prev = Date.now()
  return function () {
    let now = Date.now()
    let args = arguments
    if (now - prev >= delay) {
      fn.apply(this, args)
      prev = Date.now()
    }
  }
}
~~~

12、**数组相关的原型方法**

数组：push、pop

concat：

~~~js
arrayObject.concat(arrayX,arrayX,......,arrayX)		//返回拼接后的数组
~~~

copyWithin：

~~~js
array.copyWithin(target, start, end)
~~~

entries：

~~~js
array.entries()  //返回键值对
~~~

slice：（不会改变数组）

```
arrayObject.slice(start,end)
```

keys、values、includes、indexOf、lastIndexOf、reverse、splice、unshift（改变数组，返回长度）

13、**string的原型方法**

slice（start,end）、substr（start，length）、substring（start，end）、split（字符串/正则,length）、charAt（pos）、indexOf（searchString，position）、concat（string…） 

14、**jquery事件绑定**

```
$(selector).bind(event,data,function)
```

event.stopPropagation() 防止事件冒泡

15、**谈一谈闭包的应用场景和闭包的缺点**

应用场景：

1.函数的防抖：

一般方法实现函数防抖

~~~js
window.onresize = function(){
	debounce(fn,1000)
}
 var fn = function(){    
	console.log('fn')
}
var time = ''
function debounce(fn,timeLong){
   if(time){
      clearTimeout(time)
      time = ''
   }

   time =setTimeout(function(){
      fn()
   },timeLong)
}
~~~

使用闭包的方式：

~~~js
window.onresize = debounce(fn,500)

function debounce(fn){
        var timer = null
        return function(){
        if(timer){     //timer第一次执行后会保存在内存里 永远都是执行器 直到最后被触发
           clearTimeout(timer)
            timer = null
        }
        timer = setTimeout(function(){

            fn()
        },1000)
        
        }
    
}
var fn = function(){

    console.log('fn')
}
~~~

2.使用闭包实现单例模式：

~~~js
class CreateUser {
    constructor(name) {
        this.name = name;
        this.getName();
    }
    getName() {
         return this.name;
    }
}
// 代理实现单例模式
var ProxyMode = (function() {
    var instance = null;
    return function(name) {
        if(!instance) {
            instance = new CreateUser(name);
        }
        return instance;
    }
})();
// 测试单体模式的实例
var a = ProxyMode("aaa");
var b = ProxyMode("bbb");
// 因为单体模式是只实例化一次，所以下面的实例是相等的
console.log(a === b);    //true
~~~

3.为多个组件设置不同的值

4.设置私有变量

5.异步问题正确执行

16、**CMD和AMD区别**

1.AMD推崇依赖前置，CMD推崇就近依赖

2.执行时机不同：AMD是加载完立即执行，CMD是延迟执行（二者的最大区别）	

3、实现按需加载的方法不同

https://juejin.im/post/5aaa37c8f265da23945f365c

## ES6 模块与 CommonJS 模块的差异

### 1. CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。

- CommonJS 模块输出的是值的拷贝，也就是说，一旦输出一个值，模块内部的变化就影响不到这个值。
- ES6 模块的运行机制与 CommonJS 不一样。JS 引擎对脚本静态分析的时候，遇到模块加载命令`import`，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。换句话说，ES6 的`import`有点像 Unix 系统的“符号连接”，原始值变了，`import`加载的值也会跟着变。因此，ES6 模块是动态引用，并且不会缓存值，模块里面的变量绑定其所在的模块。

### 2. CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。

- 运行时加载: CommonJS 模块就是对象；即在输入时是先加载整个模块，生成一个对象，然后再从这个对象上面读取方法，这种加载称为“运行时加载”。
- 编译时加载: ES6 模块不是对象，而是通过 `export` 命令显式指定输出的代码，`import`时采用静态命令的形式。即在`import`时可以指定加载某个输出值，而不是加载整个模块，这种加载称为“编译时加载”。

CommonJS 加载的是一个对象（即`module.exports`属性），该对象只有在脚本运行完才会生成。而 ES6 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。