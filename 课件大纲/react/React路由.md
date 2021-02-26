# React路由

React路由无需实例化，无需挂载，只是通过引入一系列的路由组件进行布局即可。



### 一 基本路由

路由需要引入的路由组件有：Router，Route，NavLink，Redirect，Switch等。

Router：引入的所有路由组件都需要包裹在这个Router组件中，内部只能有一个根元素。

这个Router也分为两个种类，表示哈希路由和历史记录路由。

哈希路由对应HashRouter，历史记录模式对应BrowserRouter。

Route类似于Vue的路由选项，它用于配置路由路径和路由视图的对应关系。

NavLink就是用于渲染a标签。

Redirect用户实现重定向。

Switch用于实现路由选项的匹配顺序。

```javascript

const {HashRouter:Router,Route,NavLink,Redirect,Switch} = ReactRouterDOM

// Switch可以让视图渲染时，根据当前的路径从上往下匹配对应的Route。
// 最终渲染时,只会渲染多个Route中的其中一个Route所指定的component组件.
function App (){
    return (
        <Router>
        	<NavLink to='/home'>首页</NavLink>
        	<NavLink to='/news'>新闻</NavLink>
        	<NavLink to='/sport'>体育</NavLink>
        	<Switch>
        		<Route path='/home' exact component={Home} />
				<Route path='/news' component={News} />
                <Route path='/sport' component={Sport} />
                <Redirect path='/' to='/home' />
        	</Switch>
        </Router>
    )
}

```

注意：Route中挂载路由视图组件可以使用component指定，还可以通过以下方式指定。总共4种。

1：component指定。值是组件名。

2：children指定。值是组件标签

3：render指定。值是返回组件标签的函数。

4：插槽指定。直接把路由组件视图作为Route的插槽。

```javascript
<Switch>
    <Route path='/home' component={Home} />
    <Route path='/news' children={<News />} />
    <Route path='/sport' render={()=><Sport />}/>                         
    <Route path='/concat'><Concat /></Route>                         
</Switch>     
```

React路由的路径匹配默认是包含匹配。（Vue是完全匹配）。

**如果想要让React的路径匹配也变成完全匹配，需要添加exact属性。**



**路由的配置，也可以新建一个config，进行列表渲染Route。**

```JavaScript
const routes = [
	{
		path:'/home',
		component:Home
	},{
		path:'/news',
		component:News
	},{
		path:'/sport',
		component:Sport
	}
]

function App (){
    return (
        <Router>
        	<NavLink to='/home'>首页</NavLink>
        	<NavLink to='/news'>新闻</NavLink>
        	<NavLink to='/sport'>体育</NavLink>
        	<Switch>
       			{routes.map({path,component},i) => (
        			<Route to={path} component={component} />
    			)}
                <Redirect path='/' to='/home' />
        	</Switch>
        </Router>
    )
}
```



### 二 嵌套路由

嵌套路由：在某个路由的路由视图内，又包含另一个路由。

因为React的路由配置都是通过内置的组件进行布局的写法，因此嵌套路由非常好实现。

```javascript
const {HashRouter:Router,Route,NavLink,Redirect,Switch} = ReactRouterDOM

// 假设在 News 路由中还包含另一个路由.
function News(){
    return (
        <Fragment>
        	<h3>新闻组件</h3>
        	<Router>
        		<NavLink to='/news/phone'>手机</NavLink>
                <NavLink to='/news/pad'>平板</NavLink>
                <NavLink to='/news/computer'>电脑</NavLink>
                <Switch>
                    <Route path='/news/phone' component={Phone} />
                    <Route path='/news/pad' component={Pad} />
                    <Route path='/news/computer' component={Computer} />
                    <Redirect path='/' to='/news/home' />
                </Switch>
        	</Router>
        </Fragment>
    )
}

function App (){
    return (
        <Router>
        	<NavLink to='/home'>首页</NavLink>
        	<NavLink to='/news'>新闻</NavLink>
        	<NavLink to='/sport'>体育</NavLink>
        	<Switch>
        		<Route path='/home' component={Home} />
				<Route path='/news' component={News} />
                <Route path='/sport' component={Sport} />
                <Redirect path='/' to='/home' />
        	</Switch>
        </Router>
    )
}
```



### 三 跳转路由

类似Vue的$route对象，React路由可以通过props进行路由path获取，路由参数获取，路由跳转对应方法获取。

```javascript
// props.history -> 用来做跳转
// props.location -> 获取路径(路由),以及路由跳转的传递的参数
// props.match -> 获取路由参数
```



#### **3.1：路由跳转：**

```JavaScript

function Box({history}){
	function goNews(){
		// 跳转,能后退
		history.push({pathname:'/news'});
		// 跳转,不能后退
		history.replace({pathname:'/news'});
		// 前进
		history.goForward();
		// 后退
		history.goBack();
		// 指定跳转
		history.go(-1);
	}
	return <button onClick={goNews}>到新闻页</button>
}

```

**注意，必须是Route指定的路由视图组件可以通过props获取到history，location，match3个属性。**

如果不是Router指定挂载的路由视图组件也希望使用这三个属性进行跳转，需要使用方法withRoute处理。

```javascript
const {withRouter} = ReactRouterDOM;
// withRouter是一个高阶组件，App经过这个高阶组件处理后，props就可以获取对应的3个属性
const NewApp = withRouter(App);
// NewApp必须是Router的子孙组件.
<Router>
    <NewApp />
</Router>

```



#### **3.2：跳转并传递参数**：

跳转时，可以通过params传递参数。目标路由组件通过 props.location.params获取传递的参数。

```JavaScript
function Box({history}){
	function goNews(){
		// 跳转,能后退
		history.push({pathname:'/news',params:{msg:'来自Box的msg'}});
	}
	return <button onClick={goNews}>到新闻页</button>
}

function News({location}){
    return <div>{location.params.msg}</div>
}
```



### 四 动态路由和路由参数

动态路由：多个path对应一个路由视图组件，路由path切换时更新对应的路由视图组件内容。

路由参数的写法和Vue一样。：后面的变量即是路由参数。可以通过props.match.params.参数名获取。

```JavaScript
function MyComponent({match}){
	return <div>{match.params.path}</div>
}

function App (){
    return (
        <Router>
        	<NavLink to='/home'>首页</NavLink>
        	<NavLink to='/news'>新闻</NavLink>
        	<NavLink to='/sport'>体育</NavLink>
        	<Switch>
        		<Route path='/:path' component={MyComponent} />
        		<Redirect path='/' to='/home' />
        	</Switch>
        </Router>
    )
}

```

**如何监听路由发生了切换？**

React的路由在切换的过程中，组件会重复的创建和销毁，因此：

class 组件可以在使用 生命周期钩子监听到路由变化。

函数组件本身就可以监听路由变化。

如果是动态路由，组件没有被销毁和创建，可以通过componentWillReceiveProps钩子监听动态路由跳转。



### 五 路由守卫

React没有组件守卫，没有全局守卫。只能通过别的方式进行设置。

#### 5.1 Prompt 阻止跳转

Prompt类似Vue组件守卫中的beforeRouteLeave守卫，在路由准备离开时触发。

Prompt组件有两个属性：

1：when。指定何时使用Prompt的拦截功能。

2：message。拦截逻辑。可以有两种值，一是字符串，另一个是回调函数。

如果message的值是一个函数，则这个函数return true会继续跳转，如果return false会终止跳转。

这个函数的形参是目标路由的location对象。相当于Vue路由守卫中的to对象。

当前组件的location对象相当于Vue路由守卫的from对象。

```javascript
function Box({history}){
	return (
        <Fragment>
        	<h3>主页</h3>
        	<Prompt when={true} message='你确定要离开首页吗' />
        	<Prompt when={false} message={(location)=>{
                if(location.pathname=='/news'){
                   return true
                }else{
                   return false
                }
            }} />
        </Fragment>
    )
}
```

全局的Prompt拦截可以通过一个高阶组件来处理路由组件。

```JavaScript

function HOCRoute(RouteComponent){
    return class extends Component{
        render(){                   
            return (
                <Fragment>
                   <RouteComponent />
                   <Prompt message={this.handler.bind(this)} />
                </Fragment>
       		)
        }
        handler(location){
            console.log(this.props.location.pathname)
            return true               
        }
    }
}

const NewHome = widthRouter(HOCRoute(Home));
const NewNews = widthRouter(HOCRoute(News));
const NewSport = widthRouter(HOCRoute(Sport));

```



#### **5.2 全局的进入拦截.**

React守卫如何实现类似Vue的beforeEach的功能？

可以通过高阶组件处理Route组件，适时的通过Redirect进行重定向。

需要拦截逻辑的路由组件就需要封装一个高阶组件进行处理，反之不需要高阶组件处理。



以下例子，需要进行登录权限判断的路由，就需要通过HOC高阶组件进行处理。

经过高阶组件处理的路由组件，统一会通过在进入前判断是重定向到login组件还是，正常跳转。

```javascript
 function HOC(){
     return class extends Component{
     render(){
         let {path,children} = this.props;
         return (
             <Route path={path} children={
                 isLogin 
                 ? <Redirect to={{pathname:'/login',params:{path}}} /> 
                 : children
             }/>
         )
     }
 }

const PrivateRoute = HOC();
 
<Fragment>
    <HashRouter>
        <NavLink to='/'>首页</NavLink>
        <NavLink to='/news'>新闻</NavLink>
        <NavLink to='/sport'>体育</NavLink>
        <Switch>
            <Route path='/' exact component={Home} />                        
            <PrivateRoute path='/news' children={<News />} />                        
            <PrivateRoute path='/sport' children={<Sport />} />
            <Route path='/Login' component={Login} />
        </Switch>                        
	</HashRouter>
</Fragment>
 
```

