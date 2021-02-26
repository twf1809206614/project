# HTML5新特性

html5是2014年发布的标准，距今6年。html5的发布和移动端的兴起，是前端火热的根本原因。

html5新特性：

1：新增了众多语义化标签，音视频新支持，删除了一些过时标签。

2：新增了一些DOM方法，API。拖放。

3：canvas绘图。内联SVG。

4：本地存储，websoket，本地数据库

5：地理位置获取。

6：worker多线程。

以上新特性，最重要的只有一个：**本地存储**。偶尔可能会遇到websoket。

### 一 新增的标签类别

新增语义化标签：有很多。例如布局标签，header，footer，nav，article，aside，section等

新增了很多表单type类型。例如color，range，time等。

新增了很多属性，例如：contenteditable，placeholder，mutiple等。

#### 1.1 音视频。

html5新增了音视频标签。

音频标签：audio。

视频标签：video。

音频标签和视频标签属性见菜鸟教程api。

可以通过js操作音视频标签。

```JavaScript
// 播放
oV.play();
// 暂停
oV.pause();
// 静音
oV.muted = true;
// 设置音量大小.取值0-1
oV.volume = 0.5;
// 重播.设置为非0表示设置进度
oV.currentTime = 0;
// 获取视频时长.
oV.duration
// 设置视频封面
oV.poster = '2.png'
// 获取音频标签宽
oV.width
// 获取音频标签高
oV.height
// 获取视频实际分辨率。
oV.videoWidth
oV.videoHeight

```

### 1.2 canvas和svg

html5新增了canvas和svg标签。

它们都可以用于实现网页动画效果。

canvas是画布动画，动画不存在于DOM树中，一旦绘制成功就无法修改。适合做游戏。

svg的所有细节都存储在DOM树中，可以通过DOM操作进行控制。不适合做游戏。可以导出svg格式的矢量图。



### 二 新增一些API

**新增获取元素的新方法：**

元素.querySelector(选择器)

元素.querySelectorAll(选择器)

元素.getElementsByClassName(类名)



**新增操作类的方法:**

classList => 类列表.伪数组.

元素.classList.add(新类)

元素.classList.remove(旧类)

元素.classList.toggle(类) (有就删,没有就添加)



**获取自定义属性data-的新方法**

元素.dataset => data-自定义属性所在的数组.



**拖放api**

html5提供了常见的桌面拖放文件功能.例如把一张图片拖入浏览器的某个页面元素中.

拖放事件中涉及两个对象：被拖的元素和目标元素。

要元素可以被拖，需要添加dragable属性。

这两个对象共可以添加7个事件。（被拖元素3个）（目标元素4个）

被拖元素.ondragstart = 事件句柄 （开始拖放时触发）

被拖元素.ondrag = 事件句柄 （拖放过程中触发）

被拖元素.ondragend = 事件句柄 （拖放结束时触发）

目标元素.ondragenter = 事件句柄 （被拖元素进入目标元素触发）

目标元素.ondragover = 事件句柄 （被拖元素在目标元素中移动触发）

目标元素.ondragleave = 事件句柄 （被拖元素离开目标元素触发）

目标元素.ondrop = 事件句柄 （被拖元素在目标元素中松开鼠标时触发）

**注意：要想drop事件触发，必须在dragover事件内阻止默认事件！**

如果需要在拖和放的事件句柄内共享数据，可以通过ev.dataTransfer对象提供



### 三 本地存储

这是html5中最有意义的也是最常用的知识。

本地存储用于在页面关闭后保存页面的数据状态，在再次打开页面后获取上一次页面的数据。

可以实现类似功能的技术是cookie。（cookie不是html5的新特性）

本地存储分为两种：

localStorage （永久存储）（不手动删除，永久存储在浏览器中）

sessionStorage （临时存储）（页面关闭后就会丢失，但是刷新不会）

**本地存储方法：**

把某些数据存储到本地存储中，并起一个数据名。

localStorage.setItem(数据名，数据值)

通过数据名获取本地数据

localStorage.getItem(数据名)

通过数据名删除数据

localStorage.removeItem(数据名)

删除所有本地存储数据

localStorage.clear()

通过下标得到数据的数据名.

localStorage.key(下标)



**本地存储数据变化事件**

window.onstorage = 事件句柄



**cookie存储数据**

可以通过：document.cookie = 'name=mimi' 方式存储数据到cookie中。但这是一种临时存储。

cookie还可以设置过期时间，时间到期后，数据会自动删除

document.cookie = 'name=mimi；expires = ' + new Date().toUTCString();



cookie和本地存储的区别:

1：cookie只能存大概4k左右的数据。本次存储可以存储5M的数据

2：cookie可以设置过期时间，本地存储默认无法设置。

3：cookie每次数据请求都会通过请求头发送到服务器，本地存储不会。





