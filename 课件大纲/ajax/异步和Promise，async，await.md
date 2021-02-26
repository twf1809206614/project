# 异步和Promise，async，await



### 一 异步操作

js 的代码执行，分为两个队列。一个是同步队列，另一个是异步队列。

同步操作：代码逐行执行，前面的操作完成了才可以进行后面的操作。(代码阻塞).

异步操作：异步代码执行不会阻塞代码，必定落后于同步操作。

js 中有很多异步操作，例如定时器。

```javascript
// 定时器的回调函数执行是异步操作，必定落后于同步操作。
// 因此先打印200，再打印100
setTimeout(()=>{
	console.log(100)
},0)
console.log(200);
```



*现实生活中：*

*睡觉是同步操作，因为没睡醒你无法做其他任何事情。*

*等公车是异步操作，因为在等公车的过程中，你还可以看手机。*



js 中只要是需要等待一段时间的操作，基本都是异步操作。

例如：定时器，ajax请求数据，FileReader读取文件等操作，都需要花一小段时间，他们都是异步操作。

为何异步操作要落后于同步操作？（为何异步操作的同时还可以进行同步操作？）

如果异步操作不落后于同步操作，则在这个等待的时间段内，页面会出现假死状态，影响用户体验。

ajax默认就是异步请求数据，请求数据需要消耗一段时间，这个时间有可能长有可能短。

如果ajax请求是同步的，则会导致在等待的时间中页面出现假死状态，无法做任何操作。这明显体验很不好。

因此ajax在请求数据的过程中，其实还可以进行其他操作。

数据请求成功之后，会自动触发onreadystatechange事件中的逻辑渲染页面。

js 是单线程的，异步操作有点类似于多线程.但是还是有本质的区别.



### 二 Promise

Promise是ES6的新增的一个新特性。它用于封装异步操作。

Promise之前，js 的异步操作全都是使用回调函数或者事件监听来实现的，会显得杂乱不统一。

Promise的出现，提供了可行的标准的异步编程方案。

Promise解决的问题：

1：解决了异步编程过程的回调地狱问题。

2：解决了异步编程中信任的问题。（因为Promise是标准化的）

Promise是一个构造函数，是一个异步类。通过它可以进行实例化，一个Promise实例表示一个异步操作。

Promise构造函数,接受一个回调函数作为参赛.这个回调函数用于封装异步操作.

这个回调函数的参数，又是两个函数，分别在异步操作成功时触发和在异步操作失败时触发.



```JavaScript
// oP实例表示一个定时器异步操作.
let op = new Promise((resolve,reject)=>{
    // 随机取异步时间.
    let t = Math.random()*10000;
    
    // 1秒后异步操作成功,调用resolve方法.
	setTimeout(()=>{
        if(t<5000){
           resolve('112路公车来了')
        }else{
           reject('车不会来了')
        }
	},t)
})

// 异步执行结束,then的回调函数会自动触发.
// 这里then的回调函数其实就是 resolve 函数.
oP.then((msg)=>{
    console.log(msg + '我就上车回家')
})
// 这里catch的回调函数就是 reject 函数.
oP.catch((msg)=>{
    console.log(msg + '回不了家了')
})

// then内的回调函数是异步操作,会落后于同步操作.
// 因此这里先打印 "我在看手机"
console.log('我在看手机');
console.log('我在看手机');
console.log('我在看手机');

```

每个Promise实例都会有三种状态，分别是pending（等待中），resolve（成功），reject（失败）。

Promise实例的 resolve 或者 reject 状态一旦确定，就无法再改变，而且状态只会确认一次。



Promise的then函数默认返回当前的Promise实例，还可以指定返回某个Promise实例。

这题通过链式调用，就可以解决回调地狱问题。

```JavaScript
// 如果希望异步操作1结束之后里面开始执行异步操作2
// 这里第一个then是第一个Promise实例调用的
// 第二个then是第二个Promise实例调用的.
new Promise((resolve,reject)=>{
	resolve()
}).then(res => {
	return new Promise((resolve,reject)=>{
		resolve()
	})
}).then(res => {
	console.log('第二个异步完成')
})
```



### 三 async 和 await

async和await是终极的异步编程解决方案。但是它们是语法糖。

使用async和await书写异步代码，就好像是在写同步代码一样。

async 需要使用在函数声明前。这表示函数内的代码有可能包含异步操作。

await需要写在包含Promise的操作前。这样await后面的代码会自动等待前面的异步操作完成之后再指向。

```JavaScript
setTimeout(()=>{
	console.log(1000)
},1000)

// 以上的定时器可以改写成下面的形式。
async function fn(){
	// 异步操作
	await new Promise((resolve,reject)=>{
		setTimeout(()=>{resolve()},1000)
	})
	// 异步操作完成之后才执行.
	console.log(1000);
}

fn();
```

通过这种方式，处理多个异步操作顺序执行的写法就会变得非常的直观。

```JavaScript
function dosync(t){
	return new Promise((resolve,reject)=>{
		setTimeout(()=>{
			resolve(t+1000)
		},t)
	})
}

async function fn(){
	// 这里res1是2000
	let res1 = await dosync(1000);
	// 这里res2是3000
	let res2 = await dosync(res1);
	// 这里res3是4000
	let res3 = await dosync(res2);
    // 进行3次顺序异步操作之后得到的数据res3.
    // 3个顺序执行的异步操作耗时6秒.
	console.log(res3);
}
```



### 四 事件循环 (event loop)

异步操作分两种：微任务和宏任务。

微任务只有Promise的then回调函数。

微任务之外的异步操作就是宏任务，例如定时器。

当整个程序包含同步任务，微任务和宏任务时，按照下面的顺序执行：

**1：先执行所有的同步任务。**

**2：再指向所有的微任务。**

**3：最后执行所有的宏任务。**

**如果执行微任务的过程中产生了宏任务，则宏认为记入宏任务队列，等到执行。**

**如果执行宏任务的过程中产生了微任务，则当前宏任务执行完毕会转而立即去执行新增的微任务。**



