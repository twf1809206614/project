# ES6常用知识

### 一  ES6常用知识

#### 1.1 对象扩展

对象的属性和方法简写：

```javascript
let name = '幂幂';
let oYm = {
	name:name,	//当属性名和属性值相等时可以简写
	fn:function(){	//对象方法可以省略function()
		console.log(this.name)
	}
}
// 以上写法可以简写成
let oYm = {
    name,
    fn(){
        console.log(this.name)
    }
}
```

属性名表达式

```javascript
let str = 'name';
// 通过变量来充当key
let oYm = {			//[str] = "name" 可以通过变量名来代替变量值
	[str]:'幂幂'
}

console.log(oYm.name);
console.log(oYm[str]);

```

对象遍历

1: ES5的for in用于遍历对象

```JavaScript
let oCy = {name:"超越",age:22,sex:"女"}

// 对象有多少个属性,这个forin就循环多少次
// prop就是每次循环的键名

for(let key in oCy){
    console.log(key);		//key打印属性名
    console.log(oCy[key]); //oCy[key]打印属性值
}

```

2: ES6提供3个方法可以把对象转换成数组,然后再遍历数组. (其实不止3个,还有一些操作跟原型链相关)

```JavaScript
let oCy = {name:"超越",age:22,sex:"女"}
let arr = Object.keys(oCy);     //打印对象属性名组成的数组
    arr = Object.values(oCy);   //打印对象属性值组成的数组
    arr = Object.entries(oCy);  //打印对象属性值跟对象属性名组成的二维数组
console.log(arr);           
```



#### 1.2 数组扩展

##### 1.2.1 展开运算符: ... ,可用于展开具有iterator接口的数据和对象.

展开后的值,可用于传入函数作为实参,或者传入数组作为元素,或者传入对象作为属性.

```JavaScript
// 语法
let arr = [...[1,2,3]];	//把数组展开成三个元素
let obj = {...{a:1,b:2,c:3}};
let arr = [...'abcd'];	//把字符串展开成"a","b","c","d"
let arr = [...aDiv];	//展开节点	
let args = [...arguments];	//展开不定参
console.log(...arr);
```

##### 1.2.2 js具备iterator接口的数据结构如下:

- Array
- Map
- Set
- String
- TypedArray
- 函数的 arguments 对象
- NodeList 对象

这些数据结构都可以通过展开运算符展开.

##### 1.2.3 把iterator接口的数据类型转换成数组：Array.from( iterator接口的数据 )；

```javascript
let aDiv = document.getElementsByTagName("div");
console.log(Array.from(aDiv))；     // 得到一个元素是标签的真正数组。
Array.from(arguments) // 得到元素是实参的真正数组。
```

##### 1.2.4 for of 循环

具备iterator接口的数据结构还可以通过 for of循环来遍历。

```JavaScript
// item 就是 数组遍历过程中的元素
let arr = document.getElementsByTagName("div");
for(let el of arr){
    console.log(el);	//<div>1</div>  <div>2</div>
}   
for(let item of [1,2,3]){
	console.log(item); // 1,2,3
}
```

##### 1.2.4 数组的遍历器对象

```JavaScript
[1,2,3].keys(); //得到数组的下标数组。
[1,2,3].values(); // 得到数组的元素数组。
[1,2,3].entries(); // 得到下标和数组元素组成的二维数组。
```

##### 1.2.4 简单的数组去重

ES6提供了Set数据结构，简单理解它就是没有重复元素的 “数组”。

```JavaScript
let arr = [1,1,2,2,3,4,4,5];
arr = [...new Set(arr)];// [1,2,3,4,5]
```



#### 1.3 函数扩展

##### 1.3.1 箭头函数

ES6 提供了一种更简单的函数书写，箭头函数。用于替代匿名函数。

无法用箭头函数来书写函数声明，只能用箭头函数来书写函数表达式。

```JavaScript
let show = () => {
	console.log(100)
}

let arr = [1,2,3,2].filter(function(item,i){
    return item == 2;
});
//简写成以下写法
let arr = [1,2,3,2].filter((item,i)=>item == 2);


// 如果形参只有一个，可以省略形参的()
let show = x => {
	console.log(100)
}

// 如果函数体只有一个return 语句.可以省略{}和return,这样写的好处就是可以一眼看出函数的形参和出参.
let show = x => x+1;

// 相当于:
let show = function(x){
    return x+1
}

```

箭头函数和普通函数的区别:

**箭头函数内的this指向箭头函数所在作用域内的this.(以后学)**

##### 1.3.2 rest参数

可以通过形参写入...来代替arguments的实参列表获取.

```javascript
function fn(...args){
	console.log(...args) // 这里的args代替了arguments。
}
```

##### 1.3.3 函数默认值

```JavaScript
// 当第二个实参没有传入，就使用1来做y的值。
// 具有默认值的形参应该写在最后面
function fn(x,y,z=0){	//为了不然z为undefined，给它附一个默认值，打印它的时候为默认值
    console.log(z);		//这里z为0
}
fn(1,2)
```



#### 1.4 解构赋值

ES6提供了一种更简便的解构赋值操作，在取值和传参时更方便。

1.4.1 数组解构赋值

```JavaScript
// 数组解构赋值,按位置匹配
 let [x,y,z=10] = [2,3];
    console.log(x);	
    console.log(y);
    console.log(z);	//给它附赋一个默认值，使它不为undefined
// 相当于
let x = arr[0],
	y = arr[1],
	z = arr[2];
// -------------------------------------------------------
let [x,y,[z]] = [1,2,[3]];
// 相当于
let x = arr[0],
    y = arr[1],
    z = arr[2][0]
// -------------------------------------------------------
let [x,y,z] = [1,2];
// 相当于
let x = arr[0],
	y = arr[1],
	z = undefined;

```

1.4.2 对象解构赋值

```javascript
// 对象解构赋值,按键名匹配
let {name:x,age:y,height:z="170cm"} = {age:20,name:"峰哥哥",height:"180cm"};
    console.log(x);   //峰哥哥
    console.log(y);   //20
    console.log(z);   //180cm 设了一个默认值在没有值可以赋的情况下使用默认值
// ----------------------------------------------------------
let {a:a,b:b} = {a:1,b:2};
// 可简写成
let {a,b} = {a:1,b:2};
// ---------------------------------------------------------
let {a,b:{c}} = {a:1,b:{c:3}};
// 相当于
let a = obj.a,
    c = obj.b.c;
// ----------------------------------------------------------
let {a,b:{c},b} = {a:1,b:{c:3}};
// 相当于
let a = obj.a,
    b = obj.b,
    c = obj.b.c;
```

**注意，在任何赋值的地方都可以使用解构赋值，包括系统的自动赋值。例如实参给形参赋值时使用解构赋值。**

```javascript
// 实参是{a:1,b:2}
fn({a:1,b:2});
// 直接通过解构赋值使用实参的a和b属性。
function fn({a,b}){
    console.log(a);
    console.log(b);
}

let arr = [{a:1,b:2},{a:3,b:4}];

// 直接通过解构赋值获取数组元素的a和b属性。
for(let {a,b} of arr){
    console.log(a);
    console.log(b);
}

```

