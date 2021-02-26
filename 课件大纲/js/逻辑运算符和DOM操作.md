

# 逻辑运算符和DOM操作

### 一：逻辑操作符

逻辑运算符用于实现：并且，或者和取反操作。

#### 1.1：! 逻辑非

逻辑非操作符是单目运算符。！操作数

计算逻辑：操作数隐式转换为布尔值是true，返回false，否则返回true。

```javascript
if(!条件){
	条件为假做的事情
}

// 利用两个逻辑非可以是隐式转换任何值为布尔值
let x = !!y;
```



#### 1.2：&& 逻辑与

逻辑与操作符是双目运算符。操作数1 && 操作数2

计算逻辑：第一个操作数隐式转换为布尔值是true，返回第二个操作数，否则返回第一个操作数。

用法：

```JavaScript
//用于判断
if(条件1 && 条件2){
	条件1和条件2成立需要做的事情
}

// 用于赋值
let x = y && z;
// 此用法相当于：
let x;
if(y){
   x = z;
}else{
   x = y;
}

// 用于代替简单的if判断
条件 && 条件为真做的事情
// 类似于：
if(条件){
    条件为真做的事情
}
```



#### 1.3：|| 逻辑或

逻辑或操作符是双目运算符。操作数1 || 操作数2

计算逻辑：第一个操作数隐式转换为布尔值是true，返回第一个操作数，否则返回第二个操作数。

用法：

```JavaScript
// 用于判断
if(条件1 || 条件2){
	条件1或者条件2成立需要做的事情
}

// 用于赋值
let x = y || z;
// 此用法相当于:
let x;
if(y){
   x = y;
}else{
   x = z;
}

// 用于代替简单if判断
!条件 || 条件为假做的事情
// 相对于:
if(!条件){
   条件为假做的事情
}
```



### 二 条件运算符

唯一的一个三目运算符。也叫三元运算符。

语法：操作数1 ？操作数2 ：操作数3

计算逻辑：第一个操作数隐式转换为布尔值是true，返回第二个操作数，否则返回第三个操作数。

用法：

```JavaScript
// 用于赋值
let x = 条件 ? y : z;
// 类似于：
let x;
if(条件){
	x = y
}else{
	x = z
}

// 用于代替简单的if else
条件? 条件为真做的事情 ：条件为假做的事情
if(条件){
    条件为真做的事情
}else{
    条件为假做的事情
}

// 用于代替if else if else语句（嵌套三目）
条件1 ? 
	条件1为真做的事情 :
		条件2 ? 条件2为真做的事情 ：条件2为假做的事情
// 类似于：
if(条件1){
    条件1为真做的事情
}else if(条件2){
    条件2为真做的事情     
}else{
    条件都不成立做的事情
}
```



### 三 赋值运算符

 = 也是运算符。类似 x = 100属于赋值表达式。

计算逻辑：返回赋值后变量的值。

计算顺序，赋值运算符是先计算 = 右边表达式的值。

```JavaScript
// 把100赋值给x
let x = 100;
// 把y = 100的返回值赋值给x，这个操作就是连续赋值
x = y = 100
```

其他拓展形式的赋值运算符

```javascript
x = x + 3;
// 可简写成：
x += 3;

x = x * 2;
// 可简写成：
x *= 2;

// 类似的还有：
x /= 2;
x -= 2;
x %= 2;
```



### 四 运算符优先级

表达式都有返回值，因此计算表达式的操作数可以由任意的表达式来提供。

作为操作数的表达式的返回值就是实际进行运算的操作数。

因此计算表达式可以写得很复杂

```JavaScript
// 这是用常量作为操作数
2 + 3

// 这是用变量做操作数
x + y

// 2*3的返回值与4进行算术加运算，这里运算符 + 的操作数是 6和4，相当于6*4
2*3 + 4;

// x*y的返回值与z*4的返回值进行算术加运算。
x*y + z*4

// 2*3的返回值与typeof 100的返回值进行比较运算。
2*3 < typeof 100

// 1*2的返回值与5-3的返回值进行逻辑与计算
1*2 && 5-3

//1<2的返回值和4+3的返回值和2<3的返回值进行三目运算
1<2 ? 4+3 : 2<3

// (2*3 + 4)的返回值和(1*2 && 5-3)的返回值和(x*y + delete oYm.name)的返回值进行三目运算
(2*3 + 4) ? (1*2 && 5-3) : (x*y + delete oYm.name)

```

当表达式内有很多操作数时，需要考虑操作符的优先级。

按操作数数目划分优先级

单目 > 双目 > 三目

按种类细分:

（）> 自增自减 > 逻辑非 > 算术 > 比较 > 逻辑与逻辑或 > 三目 > 赋值 > ,



### 五 DOM操作入门

DOM操作包含两部分基本操作:

1：获取标签（元素）（节点）

2：操作数标签（元素）（节点）

#### 5.1 获取元素

html标签在DOM中用 js 标签对象来表示。（标签都是js对象）

js 标签对象有很多默认的属性，不同属性有不同作用。

获取方法：

```JavaScript
// 通过id获取元素（只获取一个）
let oDiv = document.getElementById(元素id);
// 通过标签名获取元素（获取一堆）
let aDiv = document.getElementsByTagName(元素标签名);
// 通过类名获取元素（获取一堆）
let aDiv = document.getElementsByClassName(元素类名);
// 通过选择器获取元素（只获取一个）
let oDiv = document.querySelector(选择器);
// 通过选择器获取元素（获取一堆）
let aDiv = document.querySelectorAll(选择器);
```

如果标签不在页面中，则返回null。



#### 5.2 操作元素

js 可以操作html标签的内容和属性。

js 不能操作html标签的样式，只能操作html标签的style属性。

**js 获取到的任何关于html标签的数据，都是字符串！**（html标签的内容，属性，样式都是字符串！）



##### 5.2.1 操作元素内容

两个方法：

```JavaScript
// 获取元素内容
console.log(oDiv.innerText);
console.log(oDiv.innerHTML);

// 设置元素内容
oDiv.innerText = 新的内容;
oDiv.innerHTML = 新的内容;
```

其中：

**innerHTML可以获取html标签内容，并且可以实现插入新标签。**

**innerText只能获取和操作文本内容。**



##### 5.2.2 操作元素行间样式

**html标签是一个 js 对象, html标签的style属性也是一个 js 对象。style对象是标签对象的一个属性。**

```JavaScript
// 获取行间width样式
console.log(oDiv.style.width)
// 设置width属性。
oDiv.style.width = '100px';

// 如果样式名带-，需要写成驼峰形式
oDiv.style.marginLeft = '100px';
```

js 只能获取行间样式和操作行间样式，不能获取和操作内部样式和外部样式。

js 只能获取最终样式。不能设置

```JavaScript
// 获取优先级最高的那个width样式 （最终的width样式）
getComputedStyle(oDiv).width
```



##### 5.2.3 操作元素属性

html标签的属性可以在对应的 js 对象中通过 js 对象属性获取。（html标签属性和 js 对象属性不要混为一谈）

因此，给 js 标签对象添加新属性，html标签并不会多出新的属性。

同样，删除 js 标签对象的属性，html标签并不会删除对应属性。

js 标签对象默认拥有的属性是html标签的全局属性，事件属性和可选属性。

因此通过 js 我们也可以操作html标签的全局属性，事件属性和可选属性。

```javascript
// 获取对应的html标签属性。class属性需要写成className
console.log(oDiv.id);
console.log(oText.value);
console.log(oText.className);

// 设置元素的id属性
oDiv.id = 'wrap';
oText.value = '你好吗';
oText.className = 'active';
```

标签的布尔属性，用 js 获取，得到的就是布尔属性。例如checked，mutiple，disabled属性

```JavaScript
// 获取复选框的checked属性，得到的值就是true或者false
console.log(oCheckbox.checked)// true或者false
// 通过设置checked属性可以控制复选框的勾选状态
oCheckbox.checked = true
// 禁用oBtn
oBtn.disabled = true
```

标签的自定义属性默认在对应  js 标签对象上没有，因此不能通过操作 js 标签对象来操作html标签的自定义属性。

可以通过标签对象的getAttribute方法来获取自定义属性。

可以通过标签对象的setAttribute方法来设置自定义属性。

```JavaScript
// 获取abc自定义属性。
oDiv.getAttribute('abc');

// 把abc自定义属性设置为wwww。
oDiv.setAttribute('abc','wwww');
```



##### 5.2.4 添加事件

什么是事件？

特定情况下，发送的事情，例如下雨了，下课了。

事件发生后会有对应的事件现象。例如，下雨了，大家都会撑伞，下课了，大家都去吃饭。

标签也有对应的事件，例如，按钮被点击了。这里点击就是一个事件。



如果点击按钮，需要产生某些效果，则需要通过事件属性来添加代码实现需要的效果。

通过操作 js 标签对象的**事件属性**，可以给标签添加事件。事件需要实现的效果需要需要写在**函数内**。

```JavaScript
// 给按钮添加点击事件，事件发生产生的效果写在函数内。
// 点击按钮，弹出100
oBtn.onclick = function(){
	alert(1000)
}

// 或者
oBtn.onclick = show;
function show(){
    alert(1000)
}
```

常见事件（注意事件没有驼峰）：

onclick。（点击事件）

onmouseover，onmouseout。（鼠标移入移出事件）

onmouseenter，onmouseleave。（鼠标移入移出事件）

onblur，onfocus （失去获得焦点事件）

onchange （value改变事件）

oninput （正在输入事件）

onmousedown，onmouseup （鼠标按下松开事件）

onkeydown，onkeyup （键盘按下松开事件）



**事件产生的效果，为什么要写在函数内？**

我们知道，js的运行是发生在html文档的解析阶段的，而事件需要实现的效果是发生在html文档渲染之后的，

如何让某些代码在html文档在渲染之后才执行呢？通过函数就可以。

**函数可以决定某些代码什么时候执行，这是函数的一个显著特征。**



