# JS面试题

### 1：你如何理解闭包。（存储和保护）

### 2：事件循环（异步对事件循环影响）

### 3：ES6的新特性 

##### （箭头函数，let，const，promise，class，async，await，解构赋值，Set，Map，Proxy）

### 4：promise和then的参数 

### 5：如何理解JS的异步

### 6：跨域

### 7：arguments

### 8：原型链。（js如何实现继承）

### 9：proxy原理

### 10：Set去重原理 （不懂原理，就回答用法）

### 11：常用数组方法 

### 12：数组去重

### 13：promise有啥缺点

### 14：cookie，localstorage，sessionStorage有啥区别

### 15：let，const，var的区别。

### 16：箭头函数的理解

### 17：怎么用原生js解决滚动条的出线

### 18：函数声明和函数表达式的区别，它们在变量提升的时候有什么不一样

### 19：变量提升和预解析的理解

### 20：setTimeout的时间是不是精准的，如何实现精准定时；

### 21：html5新特性有哪些

### 22：DOM事件的3个阶段 

### 23：防抖截流解决的问题和实现

### 24：深浅拷贝的区别

### 25：new一个Object发生了什么（New做了什么）

### 26：事件委托原理

### 27：如何解决跨域（不要再说jsonp了）

cors和代理（proxy）

### 28：ajax的原理。

实际就是在问ajax的原生写法。然后凸出这是一个异步的监听等待过程就可以了。

### 29：axios的原理。（promise原理）

实际是在问promise 

### 30：promise解决了啥问题，3个状态是啥。

promise是一种异步编程解决方案。解决了回调地狱的问题，解决了信任问题。

3个状态分别是pendding，resolve，reject。

### 31：get和post的区别

GET把参数包含在URL中，POST通过request的body传递参数.（request 是一个原生的请求类。还有响应类）

GET 有长度限制，2000字节左右，因为受到url长度限制。POST没有长度限制。

GET请求只能进行url编码，而POST支持多种编码方式。

GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。

GET在浏览器回退时是无害的，而POST会再次提交请求。

GET产生的URL地址可以被Bookmark，而POST不可以。

### 32：http状态码的403，303表示什么意思