# DOM和BOM

### 一 获取元素尺寸

#### 1.1 标签“视口”宽高 ，只能获取，不能设置

元素.clientWidth

元素.clientHeight

#### 1.2 标签所在页面宽高，只能获取，不能设置。

元素.offsetWidth

元素.offsetHeight

#### 1.3 标签的内容宽高，只能获取，不能设置。

元素.scrollWidth

元素.scrollHeight

#### 1.4 网页视口尺寸，不能设置

document.documentElement.clientWidth

document.documentElement.clientHeight

window.innerWidth (BOM)

window.outerHeight (BOM)



### 二 获取元素偏移

#### 2.1 获取元素距离最近的具有定位样式的父元素的左偏移，不能设置

元素.offsetLeft

#### 2.2 获取元素距离最近的具有定位样式的父元素的上偏移，不能设置

元素.offsetTop

#### 2.3 获取元素左边框的宽度，不能设置

元素.clientLeft

#### 2.4 获取元素上边框的宽度，不能设置

元素.clientTop



### 三 获取元素滚动偏移

#### 3.1 获取页面滚动条的偏移量。(被藏起来的网页的宽或者高) (可以设置)

document.documentElement.scrollTop (纵向滚动条)

document.documentElement.scrollLeft (横向滚动条)

还可以通过设置来改变滚动条的位置.

document.documentElement.scrollTop = 0;回到顶部

页面滚动事件:

document.onscroll = 事件句柄

window.onscroll = 事件句柄

#### 3.2 获取元素滚动条的偏移量.

元素.scrollTop (纵向滚动条)

元素.scrollLeft (横向滚动条)

元素滚动事件:

元素.onscroll = 事件句柄

#### 3.3 页面载入事件

window.onload = 事件句柄

#### 3.4 页面尺寸变化事件

window.resize = 事件句柄



### 四 location对象 (网页地址对象)

| 属性     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| href     | 声明了当前显示文档的完整 URL，与其他 location 属性只声明部分 URL 不同，把该属性设置为新的 URL 会使浏览器读取并显示新 URL 的内容 |
| protocol | 声明了 URL 的协议部分，包括后缀的冒号。例如：“http:”         |
| host     | 声明了当前 URL 中的主机名和端口部分。例如：“www.123.cn:80”   |
| hostname | 声明了当前 URL 中的主机名。例如：“www.123.cn”                |
| port     | 声明了当前 URL 的端口部分。例如：“80”                        |
| pathname | 声明了当前 URL的路径部分。例如：“news/index.asp”             |
| search   | 声明了当前 URL 的查询部分，包括前导问号。例如：“?id=123&name=location” |
| hash     | 声明了当前 URL 中锚部分，包括前导符（#）。例如：“#top”,指定在文档中锚记的名称 |

网页的网址还可以这样获取:document.URL

监听hash修改事件: 元素/window.onhashchange



#### 五 navigator 对象 (浏览器对象)

| 属性            | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| appCodeName     | 返回浏览器的代码名                                           |
| appMinorVersion | 返回浏览器的次级版本                                         |
| appName         | 返回浏览器的名称                                             |
| appVersion      | 返回浏览器的平台和版本信息                                   |
| browserLanguage | 返回当前浏览器的语言                                         |
| cookieEnabled   | 返回指明浏览器中是否启用 cookie，如果启用则返回 true,否则返回 false |
| platform        | 返回运行浏览器的操作系统平台                                 |
| systemLanguage  | 返回 OS 使用的默认语言                                       |
| userAgent       | 返回由客户炳发送服务器的 use.agent 头部的值                  |



### 六 history 对象

| 方法                            | 描述                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| back()                          | 加载 history 列表中的前一个 URL                              |
| forward()                       | 加载 history 列表中的下一个 URL                              |
| go(number)                      | 加载 history 列表中的某个具体页面。参数 number 是要访问的 URL 在 history 的 URL 列表中的相对位置，可取正数或负数。在当前页面前面的 URL 的位置为负数（如在前一个页面的位置为 -1 ）,反之则为正数 |
| pushState(state, title, url)    | 添加指定的 url 到历史记录中，并且刷新将地址栏中的网址更新为 url |
| replaceState(state, title, url) | 使用指定的 url 替换当前历史记录，并口无需刷新浏览器就会将地址栏中的网址更新为 url |

监听前进后退事件: window.onpopstate = 事件句柄







