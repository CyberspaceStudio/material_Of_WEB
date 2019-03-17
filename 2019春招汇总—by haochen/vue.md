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

