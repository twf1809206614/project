# AJAX原理和应用



在写网页布局时，手动往标签内填写内容，这是非常不灵活的方式，这种把内容写死的网页，就是静态网页。

实际工作中，网页的内容都是通过请求服务器获得的。

**服务器请求得到数据后，再通过DOM操作渲染到页面上，或者通过框架的响应式机制渲染到页面上。**

前端html页面如何请求服务器的数据？ajax就是方法之一。（fetch也可以。）



### 一 AJAX原理

ajax实际上就是一个js对象，这个对象通过构造函数XMLHttpRequest实例化。

通过这个对象的open方法发起请求。

通过send方法发送请求正文。

通过onreadystatechange事件监听请求过程，请求成功后获取服务器数据。

因为通过事件监听的方式监听请求过程，因此实现了网页的异步更新。

```javascript
// 构造ajax对象
let xhr = new XMLHttpRequest();
// 发起请求
xhr.open('GET','url',true);
// 发送请求正文
xhr.send();
// 监听请求过程
xhr.onreadystatechange = function(){
	if(xhr.readyState == 4){
		if(xhr.status == 200){
			console.log(xhr.responesText)
		}
	}
}
```

**xhr.open方法**:

第一个参数表示请求类型，常见的有GET和POST请求。

第二个参数表示请求的服务器地址。

第三个参数表示是否使用异步请求。

**xhr.send方法**：

如果通过post请求发送数据到服务器，需要把数据填写在send中。

**xhr.readyState属性**:

值为0-4.不同的数值表示请求处于不同的阶段。例如4表示请求完成.

**xhr.status属性：**

表示服务器的响应类型，分为5中，用3位数字表示，每种类型的开头数字是固定的。1xx，2xx，3xx，4xx，5xx

1xx => 表示请求还在进行,需要进一步操作

2xx => 表示请求得到了理解和响应,返回了客户端需要的结果.

3xx => 重定向.需要进一步访问别的地址.

4xx => 客户端错误.

5xx => 服务端错误.

**xhr.responesText属性:**

服务器返回的文本内容.



*前端访问后端的 ajax请求过程，非常类似和陌生女子的对话过程。*

*这里做个比喻：你 (前端)，陌生女子 (后端)，你需要问这个陌生女子要他的电话号码。*

*你：美女你好！（open）请问能把你的电话号码给我吗？（send）*

*等待对方回答.... (onreadystatechange)，这里陌生女子有几种可能回答。*

*陌生女子：*

*1：稍等。我写给你。（1xx）*

*2：好的，我的电话号码是.... （2xx）*

*3：不好意思，我是男的。（3xx）*

*4：滚！你不够帅！（4xx）*

*5：滚！我喜欢女人！（5xx）*



**请求头：**

请求的过程中，除了send发送的请求正文外，还会通过请求通自动发送一些客户端的基本信息和到服务端。

**响应头：**

响应的正文通过xhr.responesText获取，其他服务器的基本信息通过响应头获取。



### 二 AJAX发送数据

前端请求后端的过程，可能并不一味是索要数据，有时候还需要把数据发送给后端。

例如登录功能中需要把用户填写的账号密码发送给后端。

**前端通过AJAX发送给后端的数据就叫参数**



#### 2.1 通过GET请求发送参数

get发送的参数需要拼接到url后面，并且用 ? 链接。多个数据间用&连接.

数据都有数据名和数据值.数据名和数据值直接用 = 链接.

**数据名需要前后端保持一致**

```JavaScript
// name=幂幂&age=32 => 序列化字符串
https://192.168.7.194:5500/01.ajax.html?name=幂幂&age=32
```



#### **2.2 通过POST请求发送参数**

post发送参数,需要把序列化字符串写入send方法中。并且设置请求头为对应的数据格式。

设置请求头的操作一定要在open和send之间。

```javascript
xhr.open("POST",url,true);
xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
xhr.send('name=幂幂&age=32');
```

ajax可以发送的数据，有4种编码方式，对应4种Content-Type

1：纯文本：对应text/plain

2：json字符串：对应application/json

3：序列化字符串：对应application/x-www-form-urlencoded

4：表单FormData对象：multipart/form-data

5：XML格式。



#### 2.3 发送的数据类型

ajax只能发送字符串数据都后端。

如果拥有一个非字符串数据，需要序列化操作或者JSON.stringfy操作转换为字符串。

自然请求的数据如果是一个JSON字符串，也需要经过JSON.parse转换之后才能使用.



### 三 AJAX应用

所有的ajax请求过程都大致相同，每次请求都书写重复的请求逻辑，会让代码变得冗余和难以维护。

因此经过封装的AJAX工具就有了非常大的用武之地。工作中都不会书写原生AJAX。

目前最常用的AJAX工具是axios。axios的使用可以查看axios的官网api。

http://www.axios-js.com/





