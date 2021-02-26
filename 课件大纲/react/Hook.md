# Hook

React的组件可以分为class组件和函数组件。函数组件书写上比class 组件更简单，但是函数组件缺失很多功能。

函数组件：

1：没有this

2：没有state状态

3：没有生命周期钩子函数



class组件具备函数组件所不具备的state和生命钩子等功能，那如何使函数组件也可以使用state和生命钩子呢？

React16.8版本及以上就可以通过Hook 特性来给函数组件添加class 组件的一些功能。

Hook唯一的作用，就是可以在函数组件中使用各种 '钩子'，使函数组件具备class 组件的功能。

使用了Hook 特性后，理论上，就可以不再需要书写class 组件了。（工作中常用Hook）



使用Hook的准则：

**hook(无论什么hook),都必须在函数组件的作用域里调用.**



### Hook的Api

Hook常见的Api是：

1：useState

2：useEffect

3：useContext



#### useState 用于让函数组件可以拥有state数据。

使用该Hook后，我们也可以让组件具备响应式的功能。

使用该Hook，我们可以自定义state数据的名字和修改state数据的方法名。（class组件中固定叫setState）

修改state数据的方法参数也可以是一个函数。但是不能有第二个参数。

```javascript

// 解构出useState方法.
const {useState} = React;

function App(){
    // useState的参数是state数据的默认值.
    // num是state数据的数据名
    // setNum是修改num的逻辑.相当于setState
    const [num,setNum] = useState(100);
    
    // arr是共享的数据名.
    // setArr是修改arr的逻辑.相当于setState.
    const [arr,setArr] = useState([111,222,333]);
    
    // 点击按钮触发的句柄
    // 在内部修改共享数据num.
    // 然后通过setNum更新函数组件.
    function fn(){
        num++;
        setNum(num);
        
        // 如果共享的数据是一个引用类型.
        // 通过setArr修改时,应该使之变成一个新的引用类型.
        arr.pop();
        setArr([...arr])
    }
    
    return (
        <div>
        	<h3>{num}</h3>
        	<div>{arr}</div>
        	<button onClick={fn}>++</button>
        </div>
    )
}

```



类似的useReducer也可以实现响应式，只是写法更类似Redux的写法。

```javascript
// 解构出useReducer方法.
const {useState,useReducer} = React;

// 管理状态count的逻辑。
function Reducer(count,action){
    switch(action.type){
        case 'PLUS':
            return ++count
        default:
            return count
    }
}

function App(){
    // 状态数据叫count.dispatch就是触发修改count的方法.
    const [count,dispatch] = useReducer(reducer,100);
    return (
        <div>
        	<h3>{count}</h3>
        	<button onClick={()=>{dispatch({type:"PLUS"})}>++</button>
        </div>
    )
}

```



#### useEffect 使函数组件使用生命周期钩子。

该Hook可以让函数组件具备以下钩子：

componentDidMount

componentDidUpdate

componentWillUnmount

```javascript
// 解构出useEffect方法.
const {useEffect,useState} = React;

function App(){
    // 组件状态count。
    
    // useEffect的参数是一个回调函数
    // 这个回调函数在首次挂载和后续视图更新都会触发
    // 相当于 componentDidMount,componentDidUpdate    
    useEffect(()=>{
        console.log('组件挂载啦');
    })
    
    // 第二个参数是设置为空数组,则这个回调函数在默认挂载时触发一次,后续的更新就不触发了. 
    // 这样写相当于componentDidMount
	useEffect(()=>{
        console.log('组件挂载啦');
    },[])
    
   	// 数组中指定了数据count.则只有在这个count变化时才触发useEffect的回调函数.
    useEffect(()=>{
        console.log('count变化啦');
    },[count])
    
    // useEffect的回调函数还可以return 另一个回调函数.
    // 这个回调函数在组件被销毁时触发,相当于componentWillUnmount
    useEffect(()=>{
        let timer = setInterval(()=>{
            console.log(1000)
        },1000);
        // 监听App组件的销毁操作.
        return ()=>{
            // 销毁组件时需要把组件的定时器清除.
            clearInterval(timer);
            console.log('home组件被销毁了')
        }
    });
    
    return (
        <div>
        	<h3>{count}</h3>
        	<button onClick={()=>{dispatch({type:"PLUS"})}>++</button>
        </div>
    )
}
```



#### useContextHook，可以在函数组件中便捷的接收某个祖先组件传递的数据。

```javascript
		
	let { Component,useContext } = React;
     	
	// 实例化一个context，用于实现多层传值。
	// 默认值是200,如果没有任何的Provider.则默认值生效.
    let MyContext = React.createContext(200);

    function Item(){
       // 通过useContext获取最近的Provider挂载的数据.
       let count = useContext(MyContext);
       return <div>Item:{count}</div>
    }

    function List (){
       return <div><Item /></div>
    }

    function Root (){
        count = 12313200;
        // 把count挂载到Provider上.则Root的所有子孙组件都可以通过useContext获取到这个count
        return (
            <MyContext.Provider value={count}>
               Root:{count}
               <List />
            </MyContext.Provider>
        )
     }
```



#### 自定义Hook

有些时候，我们可以把重复的Hook逻辑封装到另一个自定义Hook中。方便复用。

自定义Hook就是一个自定义函数。函数名也必须以use开头。

```javascript
// 解构出useState方法.
const {useState} = React;

// 自定义Hook。
function useMyHook(val){
    const [num,setNum] = useState(val);
    return [num,setNum]
}

function App(){
    // 使用自定义hook.
    const [num,setNum] = useMyHook(100);
    
    function fn(){
        num++;
        setNum(num);
    }
    
    return (
        <div>
        	<h3>{num}</h3>
        	<div>{arr}</div>
        	<button onClick={fn}>++</button>
        </div>
    )
}

```









