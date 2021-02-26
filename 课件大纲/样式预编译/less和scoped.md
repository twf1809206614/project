### 1：组件style的scoped

scoped -> 让当前样式只对当前组件生效。

原理：

通过给当前的组件标签添加一个随机生成的属性选择器。

不同的组件这个随机的属性选择器是不一样的。

再把style内的所有选择器都加上这个属性选择器。就可以实现样式独享。



### 2：样式预编译

样式预编译：less，sass，stylus。

less的样式可以根据标签的嵌套关系去嵌套样式。

less还可以声明样式变量。

less还可以声明样式函数。

less还可以封装样式。

#### 1：变量

声明变量用@声明，使用变量也需要使用@

赋值是：，声明结束需要写；

声明的同时还可以进行运算；

```less
@color : green;
@width : 100px + 120px;
```

#### 2：混合

可以把一些通用样式，写成一个类样式。

然后在需要使用这个通用样式的地方，通过类选择器混合。

```less
.left{
	float: left;
	margin:10px;
}

.home{
	background: red;
	width:@width;
	.left;
}
```

混合样式，还可以写成函数的形式。可以声明形参接受实参，形参还可以有默认值。

```less
.left(@direction:left,@width:10px){
    float: @direction;
    margin: @width;
}
.home{
    .left(right,20px)
}
```

混合样式可以使用rest参数。也可以使用arguments变量。

```less
.border(...){
	// border:@w @style @color
	border:@arguments
}
.home{
    .border(5px,solid,#000);
} 
```

### 3：嵌套

子孙标签的样式可以直接嵌套写进父标签的样式内。

默认的嵌套关系，实际上会被解析为后代选择器。

如果要表达子选择器的关系，可以直接在子类选择器前面添加一个 > 

可以通过 & 号直接把嵌套的子标签选择器拼接到父标签上进行解析。

可以很方便的书写类似 :hover,交集选择器以及过长的类名书写.

### 4:  插件

a: easy less -> 在保存时自动生成一个同名的css文件

b: css tree -> 选中多个html标签, ctrl + p 调出命令窗, 写 > 筛选命令选项, 选中命令 Generate css tree 命令,

会自动根据当前选中的html标签生成对应的嵌套的less结构.







