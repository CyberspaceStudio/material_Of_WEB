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





