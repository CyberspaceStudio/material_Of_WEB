一、**谈谈你对组件化的理解**

组件化：从具体到抽象再到抽象

作用：封装、复用

**封装要点**：视图、数据（外界没必要知道）、变化逻辑（数据驱动视图变化）

**复用**：props传递参数

二、**JSX本质是什么**

**JSX语法：**html形式、引入js变量和表达式，利用if和else语句，利用循环，绑定style和className和事件。

jsx语法根本没办法被浏览器解析，那么他如何在浏览器中执行的？

~~~jsx
var aa = <div>
	<img src="aa.png" className="test1"/>
	<h3>{[user.firstName, user.lastName].join(' ')}</h3>
</div>
~~~

被转化为

~~~js
var aa = React.vreateElement("div",null,
	React.createElement("img",{src:"aa.png",className:"test1"}),
	React.createElement("h3",null,[user.firstName, user.lastName].join(' ')),
)
~~~

![1552828812809](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\1552828812809.png)

jsx实际上是语法糖，开发环境会将jsx编译成JS代码，JSX的写法大大降低了学习的成本和编码的工作量，也增加了debug成本。jsx是react引入的，但不是react独有，React.createElement是可以自定义修改的。

三、**JSX和vdom的关系**

为何需要vdom？

- vdom就是react初次推广开来的，结合JSX
- JSX就是模板，最终要渲染成htnl
- 初次渲染+修改后state后的re-render
- 正好符合vdom的应用场景

h函数和patch函数（第一次渲染和渲染之后）

何时patch（）——1.初次渲染-reactDOM.render(<App/>,container),会触发patch(container,vnode)

2.re-render--setState，会触发patch(vnode,newVnode)

自定义组件的解析

![1552829766886](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\1552829766886.png)

会把自定义组件的类传进去，而不是一个字符串。但是Input和List是自定义组件，vdom不认识，因此Input和List定义的时候必须声明一个render函数，根据props初始化实例，然后执行实例的render函数，render函数返回的还是一个vnode对象。实际上等价于：

~~~
var input = new Input();
return todo.render()
~~~

