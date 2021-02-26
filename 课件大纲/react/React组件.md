# React组件

react的组件分为3类。

1：函数组件。

2：class组件。

3：createClass组件。

React的组件无需注册。被复用的组件被挂载到哪个组件的视图中，它自动变成该组件的子组件。

React的组件名必须都是首字母大写，否则报错。



### 一 组件分类

#### 1.1 函数组件.

**函数组件用于没有状态的静态组件。例如反向数据流中的子组件就应该是函数组件。**

函数组件的模板需要通过函数return 出来。

```JavaScript
function MyComponent(){
    let msg = '组件数据';
	return (
		<div>
			<h3>标题</h3>
			<p>{msg}</p>
		</div>
	)
}
```

函数组件挂载的方式和Vue一致，通过一个和组件名相同自定义标签挂载。

```javascript
ReactDOM.render(<MyComponent />,document.getElementById('root'))
```

函数组件的数据直接就是函数的局部变量。

**函数组件没有this，也没有生命周期钩子函数。**



#### 1.2 class类组件

React还可以通过ES6新增的class语法来书写组件。

class类组件的模板需要通过render返回。**render方法中的this指向当前组件。**

```javascript
class MyComponent {
	render(){
		return (
			<div>
                <h3>标题</h3>
                <p>{msg}</p>
            </div>
		)
	}
}
```

class类组件的组件数据就是class类的私有属性。因此有两种写法。

```JavaScript
class MyComponent {
	constructor(){
		this.msg = '组件数据msg'
	}
	str = '组件数据str'
}
```

class 类组件的组件方法class类的公有方法。

```JavaScript
class MyComponent {
	constructor(){
		this.msg = '组件数据msg'
	}
	// 公有方法fn就是组件MyComponent的方法
	fn(){
		this.msg = '修改后的msg'
	}
}
```

**注意：class组件自定义方法中的this默认不指向当前组件**

因此需要在事件绑定时通过bind处理this的指向。

```JavaScript
class MyComponent {
	msg = 'msg数据'
	render(){
		return <button onClick={this.fn.bind(this)} >按钮utton>
	}
	fn(){
		this.msg = '修改后的msg'
	}
}
```



#### 1.3 createClass组件

这类组件需要引入额外的插件。它的写法有点类似于Vue组件的写法。

```javascript
const Root = createReactClass({
	// 类似于constructor的作用.
    getDefaultProps(){
        return {
            msg:'默认的msg'
        }
    },
    // 提供模板
    render(){
        return (
            <Fragment>
                <input type='button' onClick={this.fn.bind(this,200)} value='按钮' />
                {this.state.msg}
			</Fragment>
		)
	},
    // 组件方法
    fn(val){
        this.msg = '修改后的msg'
    }
});
```



### 二 组件通信

#### 2.1 父传子

React父传子和Vue的父传子几乎一致。前子后父。子组件不需要额外声明这个数据是props数据。

父传子数据可以通过props获取，**注意，这里也是单向数据流，意味着你不能在子组件中直接修改props数据。**

```javascript
// 子组件是类组件,可以通过props属性获取父传子数据.
class Son {
    render(){
		return <div>{this.props.msg}</div>
	}
}
// 如果子组件是函数组件,直接通过参数就可以获取父组件传递的数据.
function Son({msg}){
    return <div>{msg}</div>
}

// 父组件
class Father {
    msg = '父组件数据'
	render(){
		return (
			<div>
                <h3>标题</h3>
                <Son :msg={this.msg} />
            </div>
		)
	}
}
```



#### 2.2 子传父

Vue子传父的逻辑依然适用于React。

1：父组件声明一个方法接受子组件数据。

2：子组件调用这个方法传递子组件数据。

子组件如何触发父组件的方法？

Vue可以通过自定义事件或者props触发父组件方法。

React没有添加自定义事件的api，因此只能通过props触发父组件的方法。

```javascript
// 子组件通过props获取到父组件的方法。
class Son {
    msg = '子组件的msg'
    render(){
		return <button onClick={()=>{this.props.fn(this.msg)}>按钮</button>
	}
}

// 父组件，注意，这里fn传递给子组件时，需要通过bind让fn内的this绑定为父组件。
class Father {
    msg = ''
	render(){
		return (
			<div>
                <h3>标题</h3>
                <Son fn={this.fn.bind(this)} />
            </div>
		)
	}
    fn(msg){
        this.msg = msg;
        this.forceUpdate();
    }
}
```



#### 2.3 反向数据流

知道了如何在子组件触发父组件的方法，就可以实现反向数据流了。

反向数据流可以实现简单的状态管理。

1：数据写在共同的祖先组件上。

2：修改数据的逻辑也写在祖先组件上。

3：其他子组件触发在父组件上的修改数据逻辑。

```javascript
// 子组件通过props获取到父组件的方法。
class Son {
    render(){
		return <button onClick={this.props.fn}>{this.props.num}Son按钮</button>
	}
}
// 反向数据流中，子组件用函数组件，书写会更简单。
function Box({fn,num}){
    return <button onClick={fn}>{num}Box按钮</button>
}

// 父组件，注意，这里fn传递给子组件时，需要通过bind让fn内的this绑定为父组件。
class Father {
    num = 1000
	render(){
		return (
			<div>
                <h3>标题</h3>
                <Son :num={this.num} fn={this.fn.bind(this)} />
                <Box :num={this.num} fn={this.fn.bind(this)} />
            </div>
		)
	}
    fn(){
        this.num = Math.random();
        this.forceUpdate();
    }
}
```



#### 2.4 任意两个组件间共享数据

React没有Vue的bus共享数据的api。如果要实现，需要下载一个events插件实现类似bus的语法。



### 三 state数据

React的组件的数据分为三类：

1：一般数据。

2：props数据。

3：state数据。

其中，一般数据默认没有响应式。props数据依赖父组件数据的变化而变化。

state数据才是真正的响应式数据，才算是组件的“状态”。



state数据声明不同于一般数据。所有的响应式数据，都需要写在state对象中。

获取state数据，可以通过 this.state.数据名 获取。

```JavaScript
class MyComponent {
	// 在constructor中声明state响应式数据
	constructor(){
		this.state = {
			msg:'响应式的msg'
		}
	}
	// 或者这样声明
	state = {msg:'响应式的msg'}
	// 通过this.state.数据名获取响应式数据.
    render(){
		return <div>{this.state.msg}</div>
	}
}
```

如何修改state数据，用以实现数驱动？

**state数据必须通过setState方法来修改！**

```javascript
class MyComponent {
	state = {msg:'响应式的msg'}
    render(){
		return <button onClick={this.fn.bind(this)}>{this.state.msg}</button>
	}
    fn(){
        // 通过重新设置state对象中的数据来修改state数据。
        this.setState({msg:Math.random()})
    }
}
```



如果state数据是引用类型，例如是一个数组，修改这个数组时，应该遵循 **数据不可变原则**。

**数据不可变原则：**

**1：修改后的数据和修改前的数据不是同一个引用类型。**

**2：不应该通过修改源引用类型来生成新的引用类型。**

ps ：很多上班的同学都无视这个规则中的第二条。其实问题不大。但是第一条规则不遵守，很多时候会出乱子。

```javascript
class MyComponent {
	state = {arr:[111,222,333]}
    render(){
		return <button onClick={this.fn.bind(this)}>{this.state.arr}</button>
	}
    fn(){
        // 如果要删除333元素，下面的写法没有遵循数据不可变原则的两条规则。
        this.state.arr.pop();
        this.setState({arr:this.state.arr})
        // 这样写遵循了第一条规则但是没有遵循第二条规则
        this.state.arr.pop();
        this.setState({arr:[...this.state.arr]})
        // 这样写两条规则都得到了遵循。
        this.setState({arr:[...this.state.arr.slice(0,-1)]})
    }
}
```



**注意：setState修改数据的过程是异步的！**

如果你想要马上知道新修改的state的数据值，你需要给setState设置第二个参数。第二个参数是一个回调函数。

这个回调函数会在state数据修改完成之后自动触发。

```JavaScript
class MyComponent {
	state = {msg:'响应式的msg'}
    render(){
		return <button onClick={this.fn.bind(this)}>{this.state.msg}</button>
	}
    fn(){
        // setState的第二个参数，在数据修改完成之后会自动触发。
        this.setState({msg:Math.random()},()=>{
        	console.log(this.state.msg)
        })
    }
}
```



修改state数据时，如果后面的状态依赖之前的状态，setState的第一个参数还可以写成函数形式。

因为setState数据修改是异步的，因此，后修改的状态应该在前面的状态修改完成之后再修改。

```javascript
class MyComponent {
	state = {num:1}
    render(){
		return <button onClick={this.fn.bind(this)}>{this.state.msg}</button>
	}
    fn(){
        // 如果这样做，有可能效果不是依次递增。因为第二次点击时可能第一次点击的修改还没有完成
        this.setState({num:this.state.num+1});
        // 这样的需求，setState的参数应该写成函数形式。这个函数的形参就是上一次的state状态。
        this.setState((prevState)=>{
        	return {num:prevState.num+1}
        })
    }
}
```





