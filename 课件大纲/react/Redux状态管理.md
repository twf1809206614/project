# Redux状态管理



状态管理的两个基本功能：

1：多个组件共享一个数据

2：共享数据变化，多个组件同时更新视图



状态管理的实现方法：

1：反向数据流实现小范围状态管理。

2：插件实现全局的状态管理。



如何使用一个插件来实现状态管理？考虑以下问题.

1：如何实例化store

2：如何挂载store

3：组件中如何获取store数据

4：组件中如何修改store数据

5：如何把数据和修改数据的逻辑映射成组件的数据和方法。



React的状态管理采用的是第三方的状态管理工具。常见的有redux和mobx。



### 一 Redux如何实例化

Redux的数据存需要存储到一个函数中。（Vuex是存储到一个对象中）

因此我们需要自定义一个函数用于存储store数据，然后把这个函数传入createStore中进行实例化。

```javascript

const {createStore} = Redux;

// myState 共享的数据就是return 出去的值.
function myState(){
	return 'state共享的数据'
}

// 通过myState来实例化store.
const store = createStore(state)

```

共享的数据的默认值

```javascript
// myState 共享的数据可以使用Es6的 参数默认值的写法.
function myState(msg = 'state共享的数据'){
	return msg
}
```

如果需要共享多个数据，理论需要多个子定义函数。一个函数对应一个自定义函数。

如果有多个自定义函数，需要合并成一个之后再实例化store。合并使用combineReducers方法。

```JavaScript
const {createStore,combineReducers} = Redux;

// myMsg 共享msg
function myMsg(msg = '没事干'){
	return msg
}

// myStr 共享str
function myStr(str = '石头人'){
	return str
}

// 合并两个方法
const reducer = combineReducers({myMsg,myStr});

// 通过合并的reducer来实例化store.
const store = createStore(reducer)
```

养成习惯，就算只有一个数据，也应该使用合并的写法，这样好扩展。

```javascript
const {createStore,combineReducers} = Redux;

// myMsg 共享msg
function myMsg(msg = '没事干'){
	return msg
}

// 合并两个方法
const reducer = combineReducers({myMsg});

// 通过合并的reducer来实例化store.
const store = createStore(reducer)
```



### 二 如何获取共享数据

实例化后，默认就可以通过store对象来获取共享数据。

```JavaScript
const {createStore,combineReducers} = Redux;

// myMsg 共享msg
function myMsg(msg = '没事干'){
	return msg
}

// myStr 共享str
function myStr(str = '石头人'){
	return str
}

// 合并两个方法
const reducer = combineReducers({myMsg,myStr});

// 通过合并的reducer来实例化store.
const store = createStore(reducer)

// 获取msg数据
<div>{store.getState().msg}</div>
// 获取str数据
<div>{store.getState().str}</div>

```



### 三 如何修改共享数据

可以通过store.dispatch来修改数据.dispatch可以传递一个对象作为参数.

这个实参对象的type属性表示的是对共享数据的不同修改操作.

还可以通过自定义字段传递参数到reducer中.



**注意：如果修改的共享数据是一个引用类型，必须保证修改后是一个新的引用类型！（数据不可变原则的第一条）**

```JavaScript
const {createStore,combineReducers} = Redux;

// myMsg 共享msg
// 这里的action参数就是dispatch方法的实参对象.
// action.type指的就是修改数据的动作类型.(类似于于mutations内的方法名)
function myMsg(msg = '没事干',action){
    // 如果是ADD修改,返回action传递的参数val,否则返回msg的默认值.
	if(action.type == 'ADD'){
       return action.val
    }else{
       return msg
    }
}

// 合并两个方法
const reducer = combineReducers({myMsg});

// 通过合并的reducer来实例化store.
const store = createStore(reducer);

fn(){
    // 修改共享数据需要通过dispatch方法进行修改.
    // 修改数据,需要制定修改的操作类型是ADD.val表示传递的修改数据.
    store.dispatch({
        type:"ADD",
        val:"没事干就加点需求"
    })
}

```



### 四 如何挂载

引入redux是第三方的状态管理插件，因此当共享数据变化时，React的组件是不会自动更新的。

如果想要实现数据变化后自动更新对应的组件，需要引入react-redux插件。

有了react-redux插件，我们可以把对应的store挂载到对应的组件上。

挂载store需要高阶组件Provider。挂载到哪里，取决于你希望哪些组件共享store数据。

**注意：Provider内部只能有一个唯一的节点！**

```JavaSCript

const {Provider} = ReactRedux;

// 以下写法，把store实例，挂载到了根节点上。（类似于Vue的store挂载到了new Vue的选项中）

ReactDOM.render((
	<Provider store={store}>
		<App />
	</Provider>
),document.getElementById('root'))

```



### 五 如何把数据和修改数据的逻辑都映射到组件上

Vuex通过mapState和maoMutations等辅助方法把数据和修改数据的逻辑都映射到了组件上。

React会把数据和修改数据的逻辑都映射成组件的Props上。

组件需要对props进行加工，使之可以访问store的数据和方法。因此需要通过高阶组件connect处理。

conect高阶组件需要传递两个方法作为参数。

第一个方法用于把共享数据映射成组件的props。

第二个方法用于把store的修改数据逻辑映射成组件的props。

这两个映射方法都需要return 一个对象。这个对象实际上就是组件的Props对象。

```JavaScript
// 解构两个高阶组件.
const {Provider,conect} = ReactRedux;

// 映射数据到组件.形参state就是 store.getState()的值.
function mapStateToProps(state){
	return {msg:state.msg}
}
// 映射修改逻辑到组件.形参dispatch就是store.dispatch.
function mapDispatchToProps(dispatch){
	return {
        fn(){
            dispatch({
                type:'ADD',
                val:'新的msg'
            })
        }
    }
}
// 高阶组件处理.让App的props默认可以访问msg数据和修改数据的逻辑方法.
const NewApp = conect(mapStateToProps,mapDispatchToProps)(App);

// 组件使用映射的props获取共享数据msg
<div>{this.props.msg}</div>
// 组件通过映射的props触发修改数据的逻辑.
<button onClick={this.props.fn}></button>

// 挂载时需要使用经过connect处理的NewApp.
ReactDOM.render((
	<Provider store={store}>
		<NewApp />
	</Provider>
),document.getElementById('root'))


```



### 六 中间件

如果共享数据需要异步修改。需要使用中间件。

中间件是一个thunk插件，它唯一的作用就是让dispatch可以传入一个函数。

我们可以把所有的异步操作都写到这个函数中。

因为CDN和NPM下的写法有点区别。这里省略了导入部分。

```JavaScript

const thunk = 某种导入方式;

// applyMiddleware用于给store实例添加中间件。
const {createStore,combineReducers,applyMiddleware} = Redux;

// 实例化时传入异步的中间件.
const store = createStore(combineReducers({msg}),applyMiddleware(thunk));

// 传入dispatch的自定义函数。
// 内部属性异步修改共享数据的逻辑.
function setMsg(){
    return dispatch => {
        setTimeout(()=>{
            dispatch({type:'ADD',val:'新的msg'})
        },2000)
    }
}

function mapDispatchToProps(dispatch){
	return {
        fn(){
            // 传入dispatch的是一个函数。
            dispatch(setMsg())
        }
    }
}

```





