1.**简述express和koa的区别**

Express主要基于Connect中间件框架，功能丰富，随取随用，并且框架自身封装了大量便利的功能，比如路由、视图处理等等。而koa主要基于co中间件框架，框架自身并没集成太多功能，大部分功能需要用户自行require中间件去解决，但是由于其基于ES6 generator特性的中间件机制，解决了长期诟病的“callback hell”和麻烦的错误处理的问题，大受开发者欢迎。

路由处理Express是自身集成的，而koa需要引入中间件。

Express框架自身集成了视图功能，提供了consolidate.js功能，可以是有几乎所有Javascript模板引擎，并提供了视图设置的便利方法。Koa需要引入co-views中间件，co-views也是基于consolidate.js，支持能力一样强大。

Express由于是在ES6特性之前的，中间件的基础原理还是callback方式的；而koa得益于generator特性和co框架（co会把所有generator的返回封装成为Promise对象），使得中间件的编写更加优雅。

异常处理：https://hijiangtao.github.io/2017/11/10/Mastering-Koa-Middleware/