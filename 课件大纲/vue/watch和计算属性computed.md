# watch和计算属性computed



### 一 响应式原理

Vue的数据变化会自动更新对应的视图，这个功能得首先能捕捉到数据的变化。

如何知道一个数据发生了变化呢？

这里Vue源码内通过Object.defineProperty()来进行数据劫持（数据监听）。

Object.defineProperty()的setter触发，能够监听到数据的变化。

```javascript

let data = {msg:'幂幂'};
let val = data.msg;
// 监听data的msg属性，当msg变化时，会自动触发set方法。
Object.defineProperty(data,'msg',{
	get(){
		return val
	},
	set(newVal){
		val = newVal;
		console.log('msg被修改了');
	}
})

```

经过Object.defineProperty设置之后的对象属性有以下特征：

1：属性值必须通过getter函数返回。

2：数据变化会触发setter函数。



我们已经知道了Vue如何监听数据变化，那Vue是如何在对应数据变化时通知对应的视图更新的呢？

这个问题太难，需要对源码有一定了解才能理解。但是这个问题又是面试的必问问题。

如果能手写类似的响应式源码，则面试能够获得巨大优势。



### 二 watch选项

Vue在数据变化的时候默认会更新对应的视图，这是默认行为。

那我们能不能在数据变化时做一些额外的事情呢？

通过设置watch选项就可以在数据变化后做一些额外的操作。

watch选项内可以监听任意data内的数据。监听的方法名就是数据名。不过这是简写。

```JavaScript
new Vue({
	el:'#app',
	data:{
		msg:'幂幂'
	}，
	watch:{
		msg(){
			console.log('msg数据被修改啦')
		}
	}
})
```

完整的写法，是写对象，对象的名字是监听的数据名.

deep => 深度监听. (监听数据的所有子字段)

immediate => 默认执行一次

handler => 监听触发的函数.

```JavaScript
new Vue({
	el:'#app',
	data:{
		msg:'幂幂'
	}，
	watch:{
		msg:{
    		deep:true,
    		immediate:true,
			handler(){
				console.log('msg数据被修改啦')
			}
		}
	}
})
```

实际上还有其他写法细节,详见官网api.



### 三 computed计算属性.

有些时候，某个Vue数据需要通过别的Vue数据计算得出最后的结果。

这个计算的逻辑如果比较简单，可以直接写表达式来取值。

如果计算逻辑很复杂，只能通过计算属性来运算求值。

以下通过商品总价的例子说明：

```javascript
// 商品数量
<input type='text' v-model='count' />
// 商品单价
<input type='text' v-model='price' />
// 数量 * 单价 就是总价。这里直接写表达式即可。
<div>总价:{{count * price}}</div>
```

以上例子也可以换成计算属性来实现。

这里的total就是计算属性，他的值需要通过函数返回。虽然它写成函数，但是它的返回值是一个数据。这是简写

这个函数什么时候触发？

函数内的数据发生变化时都可以触发这个方法，以返回新的计算结果。

这个例子里，count变化或者price变化都会触发total方法，最终返回新的计算结果更新到视图上。

```JavaScript
// 商品数量
<input type='text' v-model='count' />
// 商品单价
<input type='text' v-model='price' />
// 这里total是一个计算属性，它的值依赖于数量和单价的乘积。
<div>总价:{{total}}</div>

new Vue({
    el:'#app',
    data:{
        count:1,
        price:10
    }
    computed:{
        total(){
            return this.count * this.price
        }
    }
})

```

以上的计算属性是简写，全写和watch一样，需要写成对象的形式。

对象内可以写get或者set方法。简写相当于没有set方法而只有get方法。

set方法可以用于手动修改计算属性。

```javascript
new Vue({
    el:'#app',
    data:{
        count:1,
        price:10
    }
    computed:{
        total:{
            get(){
    			return this.count * this.price
			},
            set(){
                console.log('count或者price变化了')
            }
        }
    }
})
```



**computed和watch的区别**

严格上来说，计算属性能够实现的效果，watch都可以实现。只是有时候watch写起来比较麻烦。

但是watch能够实现的效果computed不一定能够实现。

1：watch内部可以包含异步操作，computed不行。

2：计算属性默认会自动运行一次，watch需要设置immediate属性才可以。



### 四 过滤器和自定义指令 （不重要）



#### 4.1 过滤器

某些情况下，可以通过过滤器来代替watch。

例如：数据的原有格式和渲染到视图上的格式不一致时，可以使用过滤器。

```javascript
<input type='text' v-model='msg' />
    
// 原始数据是msg,通过过滤规则toUpperCase进行转换后显示到视图上是全大写的msg.
<div>{{msg | toUpperCase}}</div>

new Vue({
	el:'#app'，
	data:{
		msg:''
	},
	filters:{
		toUpperCase(val){
			return val.toUpperCase()
		}
	}
})
```



#### 4.2 自定义指令

Vue默认有14种指令，我们还可以自己封装属于自己的指令。指令的功能自行设计。

自定义指令可以封装某些特殊的DOM操作逻辑到某个指令内，例如拖拽指令。

```JavaScript
// v-color是需要封装的自定义指令，它的作用是让当前视图标签的背景色变成后面绑定的color数据。
// v-是固定写法,color才是自定义指令的名字.
<div v-color='color'>11111</div>

new Vue({
	el:'#app'，
	data:{
		msg:''
	},
	directives:{
        // el参数表示绑定了自定义指令的视图.
        // data表示自定义指令绑定的Vue数据.
        color(el,data){
    		el.style.backgroundColor = data.value
		}
    }
})
```





















