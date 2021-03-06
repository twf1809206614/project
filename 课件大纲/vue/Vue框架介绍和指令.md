# Vue框架介绍和指令



### 一 什么是框架



#### 1.1 什么是框架

Vue框架本质上和jq一样，都是一种大型插件。

它们都是一种 js 工具，目的都是帮助开发人员更高效的开发项目。

工具不同，理念不同，提供的方法和方法的使用都不尽相同。

jq 是一个函数库，不能成之为框架，你可以理解为 jq 是一个工具箱，工具箱内很多功能独立的工具。

框架也提供了众多的功能方法，但是所有在功能方法组合成了一个有机的整体。

如果 jq 是一把铲子，框架就就是一台挖掘机。



#### 1.2 什么是视图框架？

我们学习的Vue，React，小程序都是一种“视图框架”。

这里先说什么是视图，简单来说，视图就是网页上显示网页内容的html布局单元。

那么视图框架就是用来写html布局的框架。视图框架把功能重心都放在了视图的渲染上。

**重点：视图框架都没有DOM操作！视图框架都没有DOM操作！视图框架都没有DOM操作！**

视图框架只关注视图的渲染，因此视图框架都把DOM操作封装在了源码内。

**我们使用框架的过程中不需要通过DOM操作更新视图。**



#### 1.3 框架的两个核心概念：

**1：视图**

**2：数据**

框架只关心视图渲染，那用什么来渲染视图呢？或者说用什么内填充视图内容呢？

答案就是用数据来渲染视图。

那框架如何把对应的数据更新到对应的视图上？如何让视图和数据对号入座？

框架采用直接在html模板内直接插入数据的方式来使对应的数据渲染对应的视图。



#### 1.4 框架的响应式功能

上面我们知道了，框架会在视图内直接插入数据来渲染视图，那数据改变后视图的内容会跟着改变吗？

答案是肯定的。框架能够自动检测视图内的数据，当对应的数据变化时，会自动更新对应的视图。



#### 1.5 MVC模式和MVVM模式

视图框架这种通过绑定数据来渲染视图和通过更新数据来更新视图的模式，属于软件设计模式中的MVVM模式。

要知道什么是MVVM模式，得先知道什么是MVC模式。

MVC模式是最早最传统的软件设计模式。m => model(数据层)，v => view(视图)，c=> controler (控制器)

MVC最经典的生活场景就是家里的电视系统。

要看电视，需要3个东西，1：电视机，2：电视节目，3：遥控器。

我们想更换电视机上的电视节目可以通过遥控器来操作。

这里，电视机就视图v，电视节目就是数据m，遥控器就是控制器c。

MVC最大的缺点就是C太复杂太臃肿了。（联想一下按钮如满天星辰的遥控器）

你想换个电视台，首先你得先学会使用遥控器。



MVVM模式蜕变于MVC，它把C用一个VM层代替了。VM => view-model (视图数据层)

VM极大的弱化了C的作用。让我们在开发中不需要把注意力放在复杂的C的操作上。

这样可以让我们把工作重心放在注视图和数据之间的关系上。

框架的两个核心概念中，数据就是MVVM中的M，视图就是MVVM中的V，VM就是框架的实例化对象。



### 二 Vue框架基本的知识大纲

1：实例化常用选项和绑定数据

2：指令的使用

3：watch和computed选项。

4：组件

​	（1）：组件通信

​	（2）：组件生命周期

5：路由



### 三 实例化Vue

#### 3.1 实例化和常用选项

如何通过Vue来创建一个网页呢？首先你得创建一个主视图。

```JavaScript
new Vue({
	el:'#app',
	data:{msg:'hello Vue'},
	methods:{fn:function(){
		console.log(this.msg)
	}}
})
```

以上就是一个非常简单的Vue程序.

el => 用于表示框架的主视图.

data => 框架的数据.

methods => 框架的方法. 注意，方法内的this默认指向当前的Vue实例，因此可以通过this访问到框架的数据。



#### 3.2 插值表达式：

实例化了Vue后，如何把Vue数据插入视图内呢? 如果要插入到视图标签的内容中，使用插值表达式。

```html
<div id='app'>{{msg}}</div>
```

插值表达式通过双{{}}来书写，{{}}内部可以写任意的 js 表达式。

要在{{}}内绑定Vue数据，只需要填写Vue的数据名即可。



### 四：指令

指令用于封装DOM操作。

常用指令：

#### 4.1：v-bind

v-bind用于帮数据绑定到视图的属性上，简写是：

```javascript
<div v-bind:class='cn'>{{msg}}</div>
<div :class='cn'>{{msg}}</div>
```

v-bind绑定style和绑定class时有特殊写法。

绑定class时可以写数组或者对象。

```javascript
<div :class='["active","box"]'>{{msg}}</div>
<div :class='{active:true,box:true}'>{{msg}}</div>
```

绑定style时，可以写对象

```html
<div :style='{background:"red",width:"200px"}'>{{msg}}</div>
```



#### 4.2 : v-on

v-on 用于给视图添加对应事件，简写是@

```html
<button v-on:click='fn'>按钮<button>
<button @click='fn'>按钮<button>
```

事件传参：vue支持直接写调用来传参。

```html
<button @click='fn(2000)'>按钮<button>
```

事件对象：如果需要传参，同时又需要使用事件对象，可以手动传入$event

```html
<button @click='fn(2000,$event)'>按钮<button>
```

阻止默认事件，阻止冒泡

```html
<button @click.stop='fn'>按钮<button>
<button @click.prevent='fn'>按钮<button>
```



#### 4.3：v-show和v-if

这两个指令统称为条件渲染。用于控制某个视图部分的显示与否。

```html
<div v-show='flag'>{{msg}}</div>
<div v-if='flag'>{{msg}}</div>
```

v-if还可以配合v-else-if和v-else指令使用实现多条件的条件渲染

```html
<div v-if='flag1'>11111</div>
<div v-else-if='flag2'>22222</div>
<div v-else'>33333</div>
```



#### 4.4 ：v-for

这个指令称之为列表渲染。

用于把数组或者对象中的数据渲染到视图中。

```html
<ul>
	<li v-for='(item,i) in [111,222,333]'>{{item}}</li>
</ul>
<ul>
	<li v-for='(value,key) in {a:1,b:2}]'>{{item}}</li>
</ul>
```

嵌套v-for

```html
<ul>
	<li v-for='(item,i) in [{a:1,b:2},{a:1,b:2}]'>
		<span v-for='value in item'>{{value}}</span>
	</li>
</ul>
```



#### 4.5：v-model

用于双向数据绑定。

**双向数据绑定：数据变化导致视图变化，视图变化反过来更新数据。**

**v-model只能用在表单元素上。目的是通过表单元素的操作来更新Vue数据。**

不同的表单使用v-model，绑定的的数据不太一样。

```html
<input type='text' v-model='msg'/>
<!--复选框单选框可以绑定布尔值-->
<input type='checkbox' v-model='flag'/>
<input type='radio' v-model='flag'/>
<!--复选框还可以绑定数组,这样如果复选框被勾选,其value值会被放入这个数组中,反之删除-->
<input type='checkbox' v-model='[111,222,333]'/>
```

