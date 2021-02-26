# Vuex和插槽



### 一 Vuex

Vuex用于实现状态管理。

什么是状态？状态简单来说就是视图上的数据。视图状态，指的大概就是试图上的数据。

状态管理至少要包含两个方面的基本功能：

1：多个组件共享同一个数据。

2：共享数据变化，可以驱动所有相关组件更新视图。



状态管理可行的方式有很多种，常见的有：

1：反向数据流。（范围较小的状态）

2：使用Vuex插件。（管理全局状态）



Vuex做 状态管理比反向数据流更简单，但是缺点是引入了一个额外的插件。

Vuex的实现逻辑几乎和反向数据流一致，就是把数据和修改数据的逻辑都放入同一个地方，方便维护。



Vuex学习的5个问题:

1：如何实例化和挂载

2：组件如何获取共享数据

3：组件如何修改共享数据

4：如何快速获取和修改（辅助方法）

5：面试题



#### 1.1 实例化和挂载

```javascript
// 实例化store用于状态管理.
const store = new Vuex.Store({
	strict:true,
    // 共享的数据写在state中
	state:{
		msg:''
	},
    // 修改数据的逻辑写在mutations中.
	mutations:{
		setMsg(state,val){
			state.msg = val
		}
	},
    // 异步操作写在actions中.
    actions:{
        syncSetMsg(store,val){
			setTimeout(()=>{
                store.commit('setMsg',val)
            },0)
		}
    },
    // vuex的计算属性.
    getters:{
        someValue(state){
            return state.msg + '计算属性'
        }
    }
})
```

store实例挂载到哪里都可以。如果想让A组件以及A组件的所有子孙组件共享数据，就挂载到A组件的config上。

```JavaScript
// 挂载到Vue实例上。
new Vue({
	el:'#app',
	render:h=>h(App),
	components:{App},
	store
})
```



#### 1.2 如何获取共享数据.

可以通过this.$store.state获取到共享的数据.一般写在计算属性内.

```JavaScript
const App = {
	template:`
		<div>
			<p>{{$store.state.msg}}</p>
			<p>{{msg}}</p>
			<p>{{someValue}}</p>
		</div>
	`,
    computed:{
        // 获取state数据
        msg(){
            return this.$store.state.msg
        },
        // 获取计算属性
        someValue(){
            return this.$store.getters.someValue
        }
    }
}
```



#### 1.3 修改共享数据

修改数据分为直接修改和异步修改。

直接修改数据需要在mutation中写逻辑。

如果修改涉及异步操作，需要写在actions中。

触发的顺序：组件 => actions => mutations => state

mutations 内部的方法使用 commit 触发.

actions 内部的方法使用 dispatch 触发.

```JavaScript
const App = {
	template:`
		<div>
			<button @click='setMsg'>按钮</button>
			<p>{{msg}}</p>
		</div>
	`,
    computed:{
        msg(){
            return this.$store.state.msg
        }
    },
    methods:{
    	setMsg(){
    		this.$store.dispatch('syncSetMsg','来自App的数据')
    	}
    }
}
```



#### 1.4 如何快速获取和修改

```JavaScript
// 获取辅助方法,快速的获取对应的数据或者修改方法
const {mapState,mapMutations,mapActions,mapGetters} = Vuex;

const App = {
	template:`
		<div>
			<button @click='syncSetMsg("来自App的礼物")'>按钮</button>
			<p>{{msg}}</p>
		</div>
	`,
    computed:{
        // 快速创建和store中和state和getter中名字相同的计算属性数据
        ...mapState(['msg']),
        ...mapGetters(['someValue'])
    },
    methods:{
        // 快速创建和mutations和actions中名字相同的方法名
    	...mapMutations(['setMsg'])，
        ...mapActions(['syncSetMsg'])
    }
}

```



#### 1.5 常见Vuex面试题：

1：Vuex的常见选项是哪些？

strict，state，mutations，actions，getters，module。

2：Vuex的数据流是如何的？

视图 => actions (dispatch) => mutations (commit) => state => 视图



### 二 插槽

插槽就是在组件挂载处，被组件包裹的组件内容（节点）。

一般情况，挂载处被组件包裹的内容在组件挂载后将不可显示。插槽可以让其显示。

```JavaScript
// 以下的h3就是myComponent的一个插槽.
const App = {
	template:`
		<div>
			<my-component>
				<h3>我是一个插槽</h3>
			</my-component>
		</div>
	`，
	components:{myComponent}
}
```

插槽用于将两个template模板类似的组件封装成一个组件.

插槽也可以用于复用组件。

如果某个组件在视图中很多地方都用到，则这个组件就需要复用，如何复用？

1：把这个组件注册成为对应组件的子组件。

2：把这个组件作为对应组件的插槽插入。

第一种方式直观，但是不利于多层传值。

第二种插槽的方式，利于多层传值。



#### 2.1 默认插槽

可以在子组件模板中通过内置组件slot获取到组件挂载时插入的插槽节点。

默认插槽可以将 **所有** 的插槽节点插入到指定的位置。

```javascript
// 子组件
const myComponent = {
    template:`
		<div>
			<slot></slot>
			<p>段落内容</p>
		</div>
	`
}

// 父组件
const App = {
	template:`
		<div>
			<my-component>
				<h3>我是一个插槽</h3>
				<button>我是一个插槽按钮</button>
			</my-component>
		</div>
	`，
	components:{myComponent}
}
```



#### 2.2 具名插槽

具名插槽，可以将指定的插槽节点插入到子组件的对应位置。

```JavaScript
// 子组件
const myComponent = {
    template:`
		<div>
			<slot name='a'></slot>
			<p>段落内容</p>
			<slot name='b'></slot>
		</div>
	`
}

// 父组件
const App = {
	template:`
		<div>
			<my-component>
				<h3 slot='a'>我是一个插槽</h3>
				<button slot='b'>我是一个插槽按钮</button>
			</my-component>
		</div>
	`，
	components:{myComponent}
}
```



#### 2.3 利用插槽进行多层传值。

利用插槽，可以将孙组件之间挂载到父组件视图中，这样在父组件template中暴露了孙组件，就可以直接传值。

不过这个做法会导致son1不再是son2的父组件，也就是son1不能直接通过props给son2传值了。

```JavaScript
// 孙组件
const son2 = {
    template:`
		<div>
			<p>{{msg}}</p>
		</div>
	`,
    props:['msg']
}

// 子组件
const son1 = {
    template:`
		<div>
			<slot></slot>
		</div>
	`
}

// 父组件
const App = {
	template:`
		<div>
			<son1>
				<son2 :msg='msg' />
			</son2>
		</div>
	`，
	data(){
		return {msg:"来自App的数据礼物"}
	},
	components:{son1,son2}
}
```



#### 2.4 作用域插槽

A组件通过插槽插入B，这个时候A和B不是父子关系。

例如下面的son2不是son1的子组件。因此son1无法通过props把str传递给son2.

**这里 son1 可以通过作用域插槽将 str 先传递给App，再通过App传递给son2.**

son1 如何通过把自身的 str 先 “传递”给App？

在son1的组件挂载处，通过指令 v-slot 来获取 son1 的str。这个str会存到一个对象内，这个对象可以任意起名。

如果要指定获取具名插槽的 作用域值，需要在 v-slot后指定具名插槽的名字。v-slot:a='slotProps'.

并且v-slot需要写在template组件上.

```javascript
// 孙组件
const son2 = {
    template:`
		<div>
			<p>{{msg}}</p>
			<p>{{str}}</p>
		</div>
	`,
    props:['msg','str']
}

// 子组件
const son1 = {
    template:`
		<div>
			<slot :str='str'></slot>
		</div>
	`,
    data(){
        return {str:'来自son1的礼物'}
    }
}

// 父组件，v-slot='slotProps'.这个slotProps就是自定义的名字.
const App = {
	template:`
		<div>
			<son1 v-slot:default='slotProps'>
				<son2 :msg='msg' :str='slotProps.str' />
			</son2>
		</div>
	`，
	data(){
		return {msg:"来自App的数据礼物"}
	},
	components:{son1,son2}
}
```





