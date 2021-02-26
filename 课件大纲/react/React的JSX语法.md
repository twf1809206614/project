# React框架的JSX语法



React框架和Vue框架一样，是一种视图框架，这意味着Vue的很多概念在React中也都适用。

例如，组件，组件通信，生命周期，路由等。



带着下面的问题学习一个视图框架，可以快速入门。

**1：如何绑定数据**

​	1.1：如何利用插值表达式绑定数据到内容中

​	1.2：如何绑定数据到属性中。class属性和style属性有没有特殊写法。

​	1.3：如何给视图标签添加事件。

​			 1.3.1：如何传参。1.3.2：如何使用事件源。1.3.3：如何使用this。

​	1.4：如何条件渲染。

​	1.5：如何列表渲染。

​	1.6：如何双向数据绑定。

​	1.7：如何进行数据驱动视图更新。

**2：如何使用组件**

​	2.1：如何写组件template，如何挂载组件，如何注册组件。

​	2.2：如何组件通信。

​			2.2.1：如何子传父。2.2.2：如何父传子。2.2.3：如何实现反向数据流。

​	2.3：如何使用生命周期钩子函数。

​			2.3.1：虚拟节点是如何的。2.3.2：key如何实现。2.3.3：父子组件生命周期钩子顺序。

​	2.4：如何使用插槽，如何复用组件。

​			2.4.1：插槽如何实现多层父传子。2.4.2：作用域插槽如何实现。

**3：如何实现状态管理**

​	3.1：如何创建数据state，如何挂载store。

​	3.2：如何获取state数据，如何修改state数据。

​	3.3：如何把state和修改state的方法映射成组件的属性和方法。

**4：如何实现路由**

​	4.1：如何写基本路由

​	4.2：如何写嵌套路由

​	4.3：如何调整路由

​	4.4：如何调整路由并传递参数。

​	4.5：如何实现动态路由，如何获取路由参数。

​	4.6：如何实现路由守卫。

​	4.7：如何实现异步路由。

**5：有没有其他特有的API，该如何使用**



**React和Vue在以上问题的最终目的上都是一致的，就是写法不一样！**

React也有一些特有的API，例如：state，高阶组件，context，hooks等。

**React的编程风格更趋向于原生js编程，没有Vue那么多的API。**



### 一：如何绑定数据



#### 1.1：什么是JSX，JSX和字符串模板的区别。

React的视图模板采用JSX的语法来书写。JSX语法需要通过一个babel插件来转换成React的虚拟节点。

什么是JSX语法？实际上它就是不用写引号的字符串模板。

```JavaScript

// Vue的模板可以写成字符串形式
template:`<div>11111</div>`

// react的模板直接不写引号.
let vNode = <div>11111</div>

```

#### 1.2：如何挂载视图。

有了视图，就可以挂载到html中。React通过ReactDOM.render(vnode)挂载视图。

```javascript
// React的JSX视图.
const vNode = <div>11111</div>;

// 把视图vNode挂载到一个id叫root的元素所在的页面位置。
ReactDOM.render(vNode,document.getElementById('root'))
```



#### 1.3：如何绑定数据到内容

JSX通过单{}把数据插入到视图中。可以写任意表达式。

**注意：JSX的{}内不能是对象，不能是函数，但可以是数组。**

```JavaScript
// 数据。
let msg = 'hello React';

// React的JSX视图.通过单{}插入数据到视图中。
const vNode = <div>{msg}</div>;

// 把视图vNode挂载到一个id叫root的元素所在的页面位置。
ReactDOM.render(vNode,document.getElementById('root'))
```



#### 1.4：如何绑定数据到属性

注意：

**绑定数据到内容和属性中的写法是一样的！（Vue不一样）。**

**绑定属性时，不要写引号！**

其中，绑定class没有特殊写法,绑定class要写成className，**style绑定时，必须写成对象！**

```javascript
// 数据。
let cn = 'active';

// 通过单{}插入数据到class属性中。
const vNode = <div className={cn} style={{width:'900px',height:'100px'}}>11111</div>;

// 把视图vNode挂载到一个id叫root的元素所在的页面位置。
ReactDOM.render(vNode,document.getElementById('root'))
```



#### 1.5：如何给视图标签添加事件

JSX通过：on+事件名 = {事件句柄} 的方式来绑定事件。注意，事件名首字母大写。

如何传参：跟原生js的事件传参完全一致。

如何使用事件对象：跟原生js的事件对象使用方式完全一致。

如何使用this：事件内的this默认不指向触发事件的元素。如果在组件中，默认指向当前的组件。

```javascript
function handler(){
    console.log('事件触发了')
}

// 通过单{}插入数据到class属性中。
const vNode = <button onClick={handler}>按钮</button>;

// 把视图vNode挂载到一个id叫root的元素所在的页面位置。
ReactDOM.render(vNode,document.getElementById('root'))

```



#### 1.6：如何条件渲染

JSX 可以在 {} 中通过三目运算符来实现条件渲染。如果有多个条件，采用嵌套三目实现。

```javascript
// 条件渲染的条件
let flag = true;
// 三目实现条件渲染.根据flag的值,视图最终显示 <span>11111</span>
const vNode = (
	<div>
		{
			flag ? <span>11111</span> : <span>22222</span>
		}
	</div>
);

// 把视图vNode挂载到一个id叫root的元素所在的页面位置。
ReactDOM.render(vNode,document.getElementById('root'))
```



#### 1.7：如何列表渲染

**JSX列表渲染的两个条件：**

**1：{} 内必须返回数组。**

**2：数组内的元素是JSX模板。**



习惯通过原生js的数组遍历方法map来进行列表渲染。（不是map自然也可以）

```JavaScript
// 列表渲染的数组。
let arr = [1111,2222,3333];
// 列表渲染的对象.
let obj = {name:'幂幂',age:32,sex:'女'}

const vNode = (
	<div>
		{
			arr.map((item,i)=> <span>{item}</span>)
		}{
			Object.values(obj).map((item,i)=> <span>{item}</span>)
		}
	</div>
);

// 把视图vNode挂载到一个id叫root的元素所在的页面位置。
ReactDOM.render(vNode,document.getElementById('root'))
```



#### 1.8 如何实现数据驱动视图更新.

React除了state数据，默认是不会有响应式效果的。所以可以通过手动更新视图来查看最新的视图状态。







