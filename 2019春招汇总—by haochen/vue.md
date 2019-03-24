一、**谈谈vue生命周期和使用场景**

​	**beforeCreate**：实例初始化之后，组件被创建时期，data和$el都没有初始化，可以加入loading时间

​	**created**：实例创建完成后，data、methods被初始化。这时候适合结束loading事件，推荐这时候发送请求，尤其是返回的数据与绑定事件有关。

​	**beforeMount**：挂载初始化之前，完成el初始化，render初次调用，此时可以发送数据请求。

​	**mounted**：完成挂载。获取el中DOM元素，进行DOM操作

​	**beforeUpdate**：渲染完成，并监测到data发生变化，在变化的数据重新渲染视图之前会触发，这也是重新渲染之前最后修改数据的机会

​	**update**：检测到数据变化，重新渲染界面时调用。

​	**beforeDestroy**：实例销毁之前调用。适合发出删除确认信息，清除定时器。

​	**Destroy**：实例销毁后调用，所有事件监听器会被删除，子实例也会被删除。

二、**vuex是如何注入到组件中的**

​	Vuex 利用了 Vue 的 mixin 机制，混合 beforeCreate 钩子，将 store 注入至 Vue 组件实例上，并注册了 Vuex store 的引用属性 $store。

三、**什么是Vuex，简单接收下Vuex相关的一些属性**

​	Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。简单的来说，就是数据共用，对数据集中起来进行统一的管理。

相关的核心概念如下所示：

- State Vuex 使用单一状态树——是的，用一个对象就包含了全部的应用层级状态，将所需要的数据写放这里，类似于data。
- Getter 有时候我们需要从 store 中的 state 中派生出一些状态，使用Getter，类似于computed。
- Mutation 更改 Vuex 的 store 中的状态的唯一方法，类似methods。
- Action Action 提交的是 mutation，而不是直接变更状态，可以包含任意异步操作，这里主要是操作异步操作的，使用起来几乎和mutations方法一模一样,类似methods。
- Module 当应用变得非常复杂时，store 对象就有可能变得相当臃肿。Vuex 允许我们将 store 分割成模块（module）。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块。

四、**Vue的diff算法**

首先会将vnode和之前的vnode进行patch操作。patch将新老VNode节点进行比对，然后将根据两者的比较结果进行最小单位地修改视图，而不是将整个视图根据新的VNode重绘。patch的核心在于diff算法，这套算法可以高效地比较virtual DOM的变更，得出变化以修改视图。他们只是在同级进行比较。从代码中不难发现，当oldVnode与vnode在sameVnode的时候才会进行patchVnode，也就是新旧VNode节点判定为同一节点的时候才会进行patchVnode这个过程，否则就是创建新的DOM，移除旧的DOM。

sameVnode：当两个VNode的tag、key、isComment都相同，并且同时定义或未定义data的时候，且如果标签为input则type必须相同。这时候这两个VNode则算sameVnode，可以直接进行patchVnode操作。

patchVnode的规则是这样的：

1.如果新旧VNode都是静态的，同时它们的key相同（代表同一节点），并且新的VNode是clone或者是标记了once（标记v-once属性，只渲染一次），那么只需要替换elm以及componentInstance即可。

2.新老节点均有children子节点，则对子节点进行diff操作，调用updateChildren，这个updateChildren也是diff的核心。

3.如果老节点没有子节点而新节点存在子节点，先清空老节点DOM的文本内容，然后为当前DOM节点加入子节点。

4.当新节点没有子节点而老节点有子节点的时候，则移除该DOM节点的所有子节点。

5.当新老节点都无子节点的时候，只是文本的替换。

updateChildren：

不设key，newCh和oldCh只会进行头尾两端的相互比较，设key后，除了头尾两端的比较外，还会从用key生成的对象oldKeyToIdx中查找匹配的节点，所以为节点设置key可以更高效的利用dom。

五、**简述HashHistory**

hash虽然出现在url中，但不会被包括在http请求中，它是用来指导浏览器动作的，对服务器端没影响，因此，改变hash不会重新加载页面。

可以为hash的改变添加监听事件：

```js
window.addEventListener("hashchange",funcRef,false)
```

每一次改变hash(window.location.hash)，都会在浏览器访问历史中增加一个记录。通过修改window.location.hash进行路由跳转。replace()方法与push()方法不同之处在于，它并不是将新路由添加到浏览器访问历史栈顶，而是替换掉当前的路由。它与push()的实现结构基本相似，不同点它不是直接对window.location.hash进行赋值，而是调用window.location.replace方法将路由进行替换。

六、**简述HTML5History**

History interface是浏览器历史记录栈提供的接口，通过back(),forward(),go()等方法，我们可以读取浏览器历史记录栈的信息，进行各种跳转操作。

HTML5引入了history.pushState()和history.replaceState()方法，他们分别可以添加和修改历史记录条目。**pushState是压入浏览器的会话历史栈中，会使得History.length加1，而replaceState是替换当前的这条会话历史，因此不会增加History.length.**

pushState和replaceState两种方法的共同特点：当调用他们修改浏览器历史栈后，虽然当前url改变了，但浏览器不会立即发送请求该url，这就为单页应用前端路由，更新视图但不重新请求页面提供了基础。

### history模式的问题

hash模式仅改变hash部分的内容，而hash部分是不会包含在http请求中的(hash带#)：

```
http://oursite.com/#/user/id //如请求，只会发送http://oursite.com/
```

所以hash模式下遇到根据url请求页面不会有问题

而history模式则将url修改的就和正常请求后端的url一样(history不带#)

```
http://oursite.com/user/id
```

如果这种向后端发送请求的话，后端没有配置对应/user/id的get路由处理,会返回404错误。

##单页应用原理

单页应用不仅仅是在页面交互是无刷新的，连页面跳转都是无刷新的，为了实现单页应用，所以就有了前端路由。 类似于服务端路由，前端路由实现起来其实也很简单，就是匹配不同的 url 路径，进行解析，然后动态的渲染出区域 html 内容。但是这样存在一个问题，就是 url 每次变化的时候，都会造成页面的刷新。那解决问题的思路便是在改变 url 的情况下，保证页面的不刷新。

