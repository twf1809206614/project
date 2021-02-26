# css3新特性

css3 新特性

1：新的选择器

2：新的样式

3：新的ui方案

4：弹性盒模型

5：过渡，2d变换，3d变换

### 一 弹性盒模型

弹性盒模型用于布局。移动端首选布局。取代浮动布局。

弹性盒模型样式：display：flex。（添加到父元素上）

两个概念：1：容器（父元素）2：项目（子元素）

两个方向：1：主轴（水平方向）2：交叉轴（垂直方向）

容器有6个样式，项目有6个样式。弹性盒模型总共12个样式。

弹性盒模型的所有样式都具备相对意义。

#### 1.1 容器样式

**flex-direction **=> 确定弹性盒模型的方向，确定后，其他样式相对于当前方向生效。

样式值：row（默认值），column等

**justify-content** => 确定项目在当前方向上的排列方式.

样式值：flex-start（默认值），flex-end，center，space-between，space-around

**align-items** => 确定项目在另一个方向上的排列方式.

样式值：flex-start（默认值），flex-end，center

**flex-wrap** => 确定当前容器尺寸不足时,项目是否换行(列)

样式值：no-wrap（默认值），wrap

**align-content** => 设置换行或者换列之后元素的排列方式。

样式值：flex-start（默认值），flex-end，center，space-between，space-around

**flex-flow** => 复合样式。其实就是flex-direction和flex-wrap的复合写法



#### 1.2 项目样式

**order** => 自定义项目的排列方法. 值越大越靠后.

样式值：数字

**flex-grow** => 给项目设置额外的宽或者高，设置后项目会默认填满父元素的当前方向尺寸。

这个额外的尺寸计算 ：父元素额外的宽或者高 * grow占比。

如果希望项目的最终尺寸比和grow数值比一致，需要给项目设置原始尺寸为0.

样式值：数字。0为默认值。

**flex-shrink** => 确定项目在当前方向的尺寸是否可以小于默认值.

样式值：0表示不可以小于。非0表示可以小于。

**flex-basis** => 设置项目在当前方向上从尺寸。优先级高于width和height

**align-self** => 设置项目自己的align-items，不受容器样式影响。

**flex** => flex-basis，flex-grow，flex-shrink的复合写法



### 二 过渡和变换

#### 2.1 过渡 => 就是让样式的切换过程产生过渡，视觉上产生动画

transition-duration => 过渡持续时间

transition-delay => 过渡延迟时间

transition-property => 可以让过渡生效的样式

transition-timing-function => 过渡效果 (是否匀速等)

复合写法：transition



#### 2.2 变换 => 平移，旋转，缩放，倾斜

**transform => 变换样式**

样式值：

**旋转：rotate(角度)**。 角度写法：数字deg。

旋转还分3个方向，rotateX(角度)，rotateY(角度)，rotateZ(角度)。2d变换不考虑X和Y

**平移：translate(距离)**。

平移还分3个方向，translateX(距离)，translateY(距离)，translateZ(距离)。2d变换不考虑Z

**缩放：scale(倍数)**

缩放分两个方向。scaleX(倍数)，scaleY(倍数)。3d变换没有缩放。

**倾斜：skew(角度)**

缩放分两个方向。skewX(角度)，skewY(角度)。3d变换没有倾斜。



**transform-origin** => 变换的原点。例如旋转的原点在标签中心，也可以自由设置

**perspective** => 景深距离. 给2d变换的父元素加上这个样式,既可以实现3d变换.

**perspective-origin** => 景深基点. 设置透视点的位置

