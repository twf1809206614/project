# this的指向

### 一 this的指向

#### 1.1 this的作用

this的作用就是找到调用函数的对象。

通过这个this，还可以访问该对象身上的其他属性。减少了很多传值的操作。



#### 1.2 如何确定this的指向

记住一句话：谁调用它，它就指向谁。

这句话的完整版应该是：**哪个对象调用this所在的函数，this就指向这个对象。**



#### 1.3 如何调用一个函数？

调用函数有好几种形式。

```javascript
// 形式1.
show();

// 形式2.
obj.show();

// 形式3.
show.call(obj);

// 形式4.
show.apply(obj)

// 形式5.
new show();
```



#### 1.4 如何知道函数是被哪个对象调用的？

在面向对象的编程语言里，函数都应该属于某个对象，是某个对象的方法。

例如，alert就是window对象的一个方法。parseInt也是window的一个方法。

正常情况下，只有函数的所有者可以调用函数。

例如，只有window可以调用alert。Math对象调用不了alert方法。因为alert不属于Math。

不同的调用形式，调用函数的对象是不同的。

```JavaScript
// 形式1. 非严格模式，show会变成window的方法，这种调用事件上省略了window.因此show是被window调用的
// 如果是严格模式，因为没有显式的把show变成window的方法，因此show不属于window，也就是没有对象调用show
show();

// 形式2.show是通过obj来调用的。
obj.show();

// 形式3.show是通过obj调用的
show.call(obj);

// 形式4.show是通过obj调用的
show.apply(obj);

// 形式5.通过new 操作符来修饰。这里show没有对象在调用。
new show();
```

确定了哪个对象在调用show函数，则show函数内的this就指向这个对象。



#### 1.5 正常情况下的this指向分析步骤：

1：先确定this写在哪个函数声明内。

2：确定这个函数调用在哪里。

3：函数前面的对象是哪个，this就指向这个对象。如果没有对象，this指向window或者undefined



#### 1.6 call和apply

理论上，函数是哪个对象的方法，就只能由哪个对象来调用。

但是，有些时候，一个对象没有对应的方法，可以去跟另一个对象临时 "借用"一次.

call和apply可以临时调用不属于自己的方法一次.

```javascript

let oYm = {
	name:'幂幂'
	fn(){console.log(this.name)}
};
let oCy = {name:'超越'};

// 理论上,oCy没有fn方法,是不能调用fn的.但是通过call和apply可以临时调用一次.
oYm.fn.call(oCy);
// 这时，fn是被oCy对象调用的。因此fn内的this在这里指向oCy。

```

call和apply的区别：传参的方式不一样。apply需要传递数组作为参数。

```JavaScript
function show(x,y){
	console.log(x,y)
}
// 正常调用
show(100,200);
// call调用
show.call(null,100,200);
// apply调用
show.apply(null,[100,200])
```



#### 1.7 箭头函数内的this

箭头函数内的this：**指向箭头函数所在作用域内的this。**（为了面向对象方便获取this）

```JavaScript
let fn = null;
let oYm = {
	name:'幂幂',
	show(){
		console.log(this.name);
		fn = ()=>{
			console.log(this.name);
		}
	}
};
// show被oYm调用，show内的this指向oYm。
oYm.show(); // 幂幂
let oCy = {name:'超越',fn};
// fn被oCy调用。但是由于fn是一个箭头函数，因此fn内的this指向show内的this，也就是oYm。
oCy.fn(); // 幂幂
```



#### 1.8 bind内的this。

bind和call和apply类似，可以改变函数内的this指向。bind可以让某个函数内的this永远指向某个对象。

但是，**bind不会马上调用函数，call和apply会**！

另外：call和apply返回的值和源函数相同，**bind返回的是一个和源函数一模一样的新函数**

```JavaScript
function show(){
	return 100
}
// 利用call或者apply调用函数show。num得到的值都是100.
// 这种情况下,show都会被立即调用执行.
let num  = show.call(null);
let num  = show.apply(null);
// 这里,show不会被立马调用.num是一个函数,长得跟show一模一样.
let num = show.bind(null);
console.log(num); // function(){return 100}
console.log(num == show); // false

```

call和apply调用函数后，函数内的this指向call和apply的第一个参数。

注意：bind返回的函数在调用时，无论以何种方式调用，这个函数内的this永远指向bind的第一个参数。



#### 1.9 总结

this指向，分普通情况和特殊情况



普通情况的分析步骤：

**1：先确定this写在哪个函数声明内。**

**2：确定这个函数调用在哪里。**

**3：函数前面的对象是哪个，this就指向这个对象。如果没有对象，this指向window或者undefined**



特殊情况的分析步骤：

**1：有call和apply，this指向call和apply的第一个参数。**

**2：有监听函数，this指向箭头函数所在作用域内的this。**

**3：有bind，bind返回的函数的this永远指向bind的第一个参数。**

**4：有new 的情况。this指向实例。（后面学）**



### 二 事件内的this

标签事件内的this，默认都指向触发事件的标签本身。任何事件都适用。

```JavaScript
oBtn.onclick = function(){console.log(this)}; // oBtn
oBtn.onmouseover = function(){console.log(this)}; // oBtn
oBtn.onmouseout = function(){console.log(this)}; // oBtn
oBtn.onchange = function(){console.log(this)}; // oBtn
```

这个匿名函数是如何被oBtn调用的？这个匿名函数是oBtn的方法吗？

实际上事件发生时，系统会自动调用这个匿名函数。系统是通过以下方式调用这个匿名函数的。

```javascript
// 通过分析下面的调用方式，可以确定，匿名函数内的this就是这些方法调用时前面的对象：oBtn
oBtn.click();
oBtn.mouseover();
oBtn.mouseout();
oBtn.change();
```



### 三 自执行函数

自执行函数：声明和调用写在一起的特殊函数。

自执行函数的作用：方便创建一个局部作用域。(插件中都使用自执行函数来创建一个局部作用域)

语法:

```javascript
// 自执行函数形式1
(function (){
	console.log(100)
})();

// 自执行函数形式2
(function (){
	console.log(100)
}());

// 自执行函数传参
(function (x){
	console.log(x)
})(100);
```

自执行函数本质上是一个函数调用表达式.

```JavaScript
// num的值就是200
let num = (function(){
	return 200;
})()
```

自执行函数内的this指向window或者undefined.

```JavaScript
// 非严格模式下，this指向window。严格模式下，this指向undefined。
(function(){
	console.log(this)
})()
```

