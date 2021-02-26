# React插槽高阶组件和生命周期



### 一 受控组件

React通过受控组件实现双向数据绑定，即类似Vue的v-model指令的作用。

受控组件只适用于表单元素。

受控组件需要借助state数据来更新组件状态。

受控组件都必须通过onChange事件更新state，无论什么表单元素。

**受控组件的视图状态，只能通过state的更新而更新，不能通过操作更新。**

```javascript
class App extends Component {
    state = {msg:''}
	render(){
		const {msg,fn} = this.state;
		return (
			<div>
				<input type='text' value={msg} onChange={fn.bind(this)} />
				<p>{msg}</p>
			<div>
		)
	}
	fn(ev){
		this.setState({
			msg:ev.target.value
		})
	}
}
```



### 二 React插槽

React插槽的作用和Vue插槽的一致。

1：复用组件

2：多层传值

```JavaScript
// Box中可以通过 this.props.children获取插槽内容.
// 如果插槽只有一个节点,children就表示这个节点
// 如果插槽有多个节点,则children就是一个数组,数组内的元素表示各个对应的插槽.
class Box extends Component{
    render(){
        return (
            <div>
            	{this.props.children}
            	<p>子组件内容</p>
            </div>
        )
    }
}

// 下面的模板中,<h3>子组件</h3>就是Box的插槽.
class App extends Component{
	render(){
		return {
			<div>
				<h3>App组件</h3>
				<Box>
					<h3>子组件</h3>
				</Box>
			</div>
		}
	}
}
```

插槽还可以写成以下的形式

```JavaScript
// 通过父组件传递的render函数,可以获取到render函数中返回的插槽内容.
class Box extends Component{
    render(){
        return (
            <div>
            	{this.props.render()}
            	<p>子组件内容</p>
            </div>
        )
    }
}

// 下面的模板中,<h3>子组件</h3>就是Box的插槽.
// 这个写法,通过render函数把插槽return 出来.
// 这样子组件中获取了render函数,可以通过render函数的调用获取到这个插槽内容.
class App extends Component{
	render(){
		return {
			<div>
				<h3>App组件</h3>
				<Box render={ () => <h3>子组件</h3> }/>
			</div>
		}
	}
}
```

这种形式的插槽,可以实现类似Vue的作用域插槽的效果。

```javascript
// 通过render函数的调用,可以方便的把Box的数据传递到插槽中.
class Box extends Component{
    msg = '子组件'
    render(){
        return (
            <div>
            	{this.props.render(this.msg)}
            	<p>子组件内容</p>
            </div>
        )
    }
}
// render的形参msg,其实就是子组件中传入render函数的实参.
class App extends Component{
	render(){
		return {
			<div>
				<h3>App组件</h3>
				<Box render={ (msg) => <h3>{msg}</h3> }/>
			</div>
		}
	}
}
```



### 三 高阶组件

高阶组件实际上就是一个自定义函数。

高阶组件就是把一些基础组件加工成功能更丰富的组件。有点类似于Vue的mixins。

如果多个组件拥有相同的组件逻辑，可以把这些相同的组件逻辑封装到一个高阶组件中。

**高阶组件写法：**

**1：基础组件作为形参**

**2：经过加工的组件作为出参**

**3：相同的组件逻辑通过props传递给基础组件。**



以下两个组件拥有相同的组件逻辑，就是点击按钮把状态num+1、不同点只是模板不太一样。

```JavaScript
class Box1 extends Component {
	state = {num:10}
	render(){
		return (
			<div>
				<p>{this.state.num}</p>
				<button onClick={this.plus.bind(this)}>++</button>
			</div>
		)
	},
	plus(){
		this.setState({num:this.state.num+1})
	}
}

class Box2 extends Component {
	state = {num:10}
	render(){
		return (
			<div>
				<h3>{this.data.num}</h3>
				<button onClick={this.plus.bind(this)}>++</button>
			</div>
		)
	},
	plus(){
		this.setState({num:this.state.num+1})
	}
}
```

我们可以通过高阶组件加工一下这两个组件。

```JavaScript
class Box1 extends Component {
	render(){
		return (
			<div>
				<p>{this.props.num}</p>
				<button onClick={this.props.plus}>++</button>
			</div>
		)
	}
}

class Box2 extends Component {
	render(){
		return (
			<div>
				<h3>{this.props.num}</h3>
				<button onClick={this.props.plus}>++</button>
			</div>
		)
	}
}

// 这就是一个高阶组件,我们把相同的state和plus方法封装到了高阶组件return 的类中.
// 把不同的组件模板通过参数传入.
function HOC(MyComponent){
	return class extends Component{
        state = {num:10}
		render:() => <MyComponent num={this.state.num} plus={this.plus.bind(this)}  />
        plus(){
			this.setState({num:this.state.num+1})
		}
	}
}
// 把组件Box1和Box2传入高阶组件HOC中进行加工,得到新的,具有plus逻辑的新组件.
const NewBox1 = HOC(Box1);
const NewBox2 = HOC(Box2);

```



### 四 生命周期

为了方便学习，我们把React的生命周期划分为3个阶段。

#### 1：创建阶段触发4个钩子。

（1）constructor （组件初始化）

（2）componentWillMount （挂载前）

（3）render （编译模板）

（4）componentDidMount （挂载后）

#### 2：运行阶段触发5个钩子

（1）componentWillReceiverProps （父组件视图更新触发）

（2）shouldComponentUpdate （视图更新前拦截，用于性能优化）

（3）componentWillUpdate （更新前）

（4）render （更新虚拟节点）

（5）componentDidUpdate （更新后）

#### 3：销毁节点触发1个钩子

（1）componentWillUnmount  （即将销毁组件）



**注意点：**

**render钩子在创建阶段和更新阶段都会触发，这意味着render函数内不能修改state数据，否则无限更新报错。**

**shouldComponentUpdate钩子在每次更新之前都会触发，这个钩子return true就继续更新，return false会停止更新。**

**shouldComponentUpdate钩子中也不能修改state数据，否则无限更新报错。**

**shouldComponentUpdate和componentWillReceiverProps钩子的参数都是当前更新后的props和state。**

**在这两个钩子中，上一次更新的props和state可以通过this .props和this.state来获取。**

**父子组件的生命周期钩子顺序和Vue的一致。**





