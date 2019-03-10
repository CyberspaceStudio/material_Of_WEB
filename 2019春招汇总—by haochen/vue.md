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