## 从输入URL到页面展示

https://juejin.im/post/5b148a2ce51d4506965908d2 （详细）

区分：浏览器工作、服务器所做工作

- **URL输入**

- **DNS解析**

  查找顺序： 浏览器缓存--> 操作系统缓存--> 本地host文件 --> 路由器缓存--> ISP DNS缓存 --> 顶级DNS服务器/根DNS服务器

  ![img](https://user-gold-cdn.xitu.io/2018/6/4/163c83c423b021ca?imageslim)

- **TCP连接**

  1.三次握手

  第一次握手：建立连接时，客户端发送syn包（syn=j）到服务器，并进入SYN_SENT状态，等待服务器确认；SYN：同步序列编号（Synchronize Sequence Numbers）。

  第二次握手：服务器收到syn包，必须确认客户的SYN（ack=j+1），同时自己也发送一个SYN包（syn=k），即SYN+ACK包，此时服务器进入SYN_RECV状态；

  第三次握手：客户端收到服务器的SYN+ACK包，向服务器发送确认包ACK(ack=k+1），此包发送完毕，客户端和服务器进入ESTABLISHED（TCP连接成功）状态，完成三次握手。

  2.为什么需要三次握手？

   防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误

- **发送HTTP请求**

  建立了TCP连接之后，发起一个http请求。一个典型的 http request header 一般需要包括请求的方法，例如 GET 或者 POST 等，不常用的还有 PUT 和 DELETE 、HEAD、OPTION以及 TRACE 方法，一般的浏览器只能发起 GET 或者 POST 请求。  

  客户端向服务器发起http请求的时候，会有一些请求信息，请求信息包含三个部分：

  - 请求方法URI协议/版本
  - 请求头(Request Header)
  - 请求正文

   补充：服务器重定向   301，302区别https://juejin.im/post/5b148a2ce51d4506965908d2

- **服务器处理请求**

- **服务器响应请求**

  HTTP响应与HTTP请求相似，HTTP响应也由3个部分构成，分别是：

  - 状态行        ->状态码
  - 响应头(Response Header)
  - 空行
  - 响应正文

- **浏览器解析渲染页面**

- **连接结束**

  四次挥手

  第一步，当主机A的应用程序通知TCP数据已经发送完毕时，TCP向主机B发送一个带有FIN附加标记的报文段（FIN表示英文finish）。

  第二步，主机B收到这个FIN报文段之后，并不立即用FIN报文段回复主机A，而是先向主机A发送一个确认序号ACK，同时通知自己相应的应用程序：对方要求关闭连接（先发送ACK的目的是为了防止在这段时间内，对方重传FIN报文段）。

  第三步，主机B的应用程序告诉TCP：我要彻底的关闭连接，TCP向主机A送一个FIN报文段。

  第四步，主机A收到这个FIN报文段后，向主机B发送一个ACK表示连接彻底释放。

## 关于http，最全

https://www.cnblogs.com/ranyonsue/p/5984001.html

服务器传输超文本到本地浏览器的传送协议

基于TCP/IP通信协议来传递数据（HTML 文件, 图片文件, 查询结果等）。

属于应用层的面向对象的协议

HTTP使用统一资源标识符（Uniform Resource Identifiers, URI）来传输数据和建立连接



客户端发送一个HTTP请求到服务器的请求消息包括以下格式：

**请求行（request line）、请求头部（header）、空行和请求数据四个部分组成。**

![img](https://upload-images.jianshu.io/upload_images/2964446-fdfb1a8fce8de946.png?imageMogr2/auto-orient/strip%7CimageView2/2)

## 一个完整的URL包括以下几部分：

1.协议部分：该URL的协议部分为“http：”，这代表网页使用的是HTTP协议。在Internet中可以使用多种协议，如HTTP，FTP等等本例中使用的是HTTP协议。在"HTTP"后面的“//”为分隔符

2.域名部分：该URL的域名部分为“www.aspxfans.com”。一个URL中，也可以使用IP地址作为域名使用

3.端口部分：跟在域名后面的是端口，域名和端口之间使用“:”作为分隔符。端口不是一个URL必须的部分，如果省略端口部分，将采用默认端口

4.虚拟目录部分：从域名后的第一个“/”开始到最后一个“/”为止，是虚拟目录部分。虚拟目录也不是一个URL必须的部分。本例中的虚拟目录是“/news/”

5.文件名部分：从域名后的最后一个“/”开始到“？”为止，是文件名部分，如果没有“?”,则是从域名后的最后一个“/”开始到“#”为止，是文件部分，如果没有“？”和“#”，那么从域名后的最后一个“/”开始到结束，都是文件名部分。本例中的文件名是“index.asp”。文件名部分也不是一个URL必须的部分，如果省略该部分，则使用默认的文件名

6.锚部分：从“#”开始到最后，都是锚部分。本例中的锚部分是“name”。锚部分也不是一个URL必须的部分

7.参数部分：从“？”开始到“#”为止之间的部分为参数部分，又称搜索部分、查询部分。本例中的参数部分为“boardID=5&ID=24618&page=1”。参数可以允许有多个参数，参数与参数之间用“&”作为分隔符。

## 前端性能优化

**性能优化黄金法则**： 只有10%~20%的最终用户响应时间花在了下载HTML文档上，其余的80%~90%时间花在了下载页面中的所有组件上（脚本、CSS样式表、图片、Flash）。(改动前端的收益大)

https://segmentfault.com/a/1190000020867090

https://segmentfault.com/a/1190000021580224

### 1.加载优化

**减少HTTP请求**

![rule1.2.png](https://segmentfault.com/img/bVbCH94)

- 图片地图  若导航栏和超链接中使用多个图片，则使用图片地图是加速页面的最简单的方式
- 合并CSS和JS
- CSS精灵图

**缓存资源**

​	所有静态资源都要在服务器端设置缓存，并且尽量使用长缓存(**使用时间戳更新缓存**)

**压缩代码**

**无阻塞**：头部内联的样式和脚本会阻塞页面的渲染，样式放在头部并使用`link`方式引入，脚本放在尾部并使用异步方式加载。（关于CSS和JS放置位置https://juejin.im/post/5de5cd1951882573135415fd）

**首屏加载**：首屏快速显示

**按需加载**：将不影响首屏的资源和当前屏幕不用的资源放到用户需要时才加载(会导致大量重绘，影响渲染性能**)

- 懒加载（1.延时加载；2.条件加载；3.可视区加载）
- 滚屏加载 当拉动浏览器的滚动条时到页底时，页面会继续自动加载更多内容供用户浏览，AJAX应用

**预加载**：大型资源页面可使用`Loading`，资源加载完成后再显示页面，但加载时间过长，会造成用户流失

- 可感知Loading：进入页面时`Loading`
- 不可感知Loading：提前加载下一页

**压缩图像**

**避免重定向**

**异步加载第三方资源**：第三方资源不可控会影响页面的加载和显示，要异步加载

```
加载过程是最为耗时的过程，可能会占到总耗时的`80%时间(**优化重点**)
```

### 2.执行优化

- **CSS写在头部，JS写在尾部并异步**

- **避免img、iframe等的src为空**：空`src`会重新加载当前页面，影响速度和效率

- **尽量避免重置图像大小**：会引发图像的多次重绘，影响性能

- **图像尽量避免使用DataURL**：`DataURL`图像没有使用图像的压缩算法，文件会变大，并且要解码后再渲染，加载慢耗时长

- 频繁DOM操作合成一个
  	
  	[使用文档碎片（DocumentFragments）追加DOM元素](https://www.cnblogs.com/jehorn/p/8117100.html)
    	
    	因为DocumentFragment不是真实DOM树的一部分，它的变化不会引起DOM树的重新渲染的操作(reflow)，且不会导致性能等问题。
    	
    	```js
    	const listNode=document.getElementsByClassName('list')
    		//创建一个文档片段
    		const frag=document.createDocumentFragment()
    		//执行插入
    		for(let x=0;x<10;x++){
    			const li=document.createElement('li')
    			frag.appendChild(li)
    		}
    		//再插入到DOM树
    		listNode.appendChild(frag)
    	```
  
- DOM查询缓存
  
- 尽量使用DOMContentLoaded代替window.onLoad

```
执行处理不当会阻塞页面加载和渲染
```

### 3.渲染优化

- **设置viewport**：HTML的`viewport`可加速页面的渲染

  ```
  <meta name="viewport" content="wi
  dth=device-width, user-scalable=no, initial-scale=1, minimum-scale=1, maximum-scale=1">
  ```

- **减少DOM节点**：DOM节点太多影响页面的渲染，尽量减少DOM节点

- **优化动画**

  - 尽量使用CSS3动画
  - 合理使用requestAnimationFrame动画代替setTimeout
  - 适当使用Canvas动画：5个元素以内使用`CSS动画`，5个元素以上使用`Canvas动画`，`iOS8+`可使用`WebGL动画`

- **优化高频事件**：`scroll`、`touchmove`等事件可导致多次渲染

  - 函数节流 (每隔一定的时间去执行,应用拖拽)
  - 函数防抖 (避免第一次执行未结束再执行下一次，应用多次点击)
  - 使用requestAnimationFrame监听帧变化：使得在正确的时间进行渲染
  - 增加响应变化的时间间隔：减少重绘次数

- **GPU加速**：使用某些HTML5标签和CSS3属性会触发`GPU渲染`，请合理使用(**过渡使用会引发手机耗电量增加**)

  - HTML标签：`video`、`canvas`、`webgl`
  - CSS属性：`opacity`、`transform`、`transition`

### 4.样式优化

​      隐藏在屏幕外，或在页面滚动时，尽量停止动画；

- **避免在HTML中书写style**
- **避免CSS表达式**：CSS表达式的执行需跳出CSS树的渲染
- **移除CSS空规则**：CSS空规则增加了css文件的大小，影响CSS树的执行
- **正确使用display**：`display`会影响页面的渲染
  - `display:inline`后不应该再使用`float`、`margin`、`padding`、`width`和`height`
  - `display:inline-block`后不应该再使用`float`
  - `display:block`后不应该再使用`vertical-align`
  - `display:table-*`后不应该再使用`float`和`margin`
- **不滥用float**：`float`在渲染时计算量比较大，尽量减少使用
- **不滥用Web字体**：Web字体需要下载、解析、重绘当前页面
- **不声明过多的font-size**：影响CSS树的效率
- **值为0时不需要任何单位**
- **标准化各种浏览器前缀**
  - 无前缀属性应放在最后
  - CSS动画属性只用-webkit-、无前缀两种
  - 其它前缀为-webkit-、-moz-、-ms-、无前缀四种：`Opera`改用`blink`内核，`-o-`已淘汰
- **避免让选择符看起来像正则表达式**：高级选择符执行耗时长且不易读懂，避免使用

### 5.脚本优化

- **减少重绘和回流**
  - 避免不必要的DOM操作
  - 避免使用document.write
  - 减少drawImage
  - 尽量改变class而不是style，使用classList代替className
- **缓存DOM选择与计算**：每次DOM选择都要计算和缓存
- **缓存.length的值**：每次`.length`计算用一个变量保存值
- **尽量使用事件代理**：避免批量绑定事件
- **尽量使用id选择器**：`id`选择器选择元素是最快的
- **touch事件优化**：使用`tap`(`touchstart`和`touchend`)代替`click`(**注意`touch`响应过快，易引发误操作**)

## 

```
*加载更快
	减少资源体积 压缩代码
	减少访问次数  缓存304  SSR服务端渲染
	CSS精灵图（同一张图片不同位置）
	使用更快的CDN网络
*渲染更快
	渐进式渲染（只加载一个页面所需文件），预加载，
		渲染过程：HTMLDOMTree->CSSOMTree->RenderTree(JS代码终止)
```

## 浏览器渲染过程

### 浏览器组件

1. **用户界面**    除了浏览器主窗口显示的您请求的页面外，其他显示的各个部分都属于用户界面。
2. **浏览器引擎** - 在用户界面和呈现引擎之间传送指令。
3. **渲染引擎(浏览器内核)** - 负责显示请求的内容。如果请求的内容是 HTML，它就负责解析 HTML 和 CSS 内容，并将解析后的内容显示在屏幕上。
   - **Trident**：俗称 IE 内核，也被叫做 MSHTML 引擎，目前在使用的浏览器有 IE11 -
   - **Gecko**：俗称 Firefox 内核，开源
   - **Webkit**：Safari 内核，也是 Chrome 内核原型，也被大量使用在移动端浏览器上。
   - **Blink**： 由 Google 和 Opera Software 开发，在Chrome（28及往后版本）、Opera（15及往后版本）和Yandex浏览器中使用。Blink 其实是 Webkit 的一个分支，添加了一些优化的新特性，例如跨进程的 iframe，将 DOM 移入 JavaScript 中来提高 JavaScript 对 DOM 的访问速度等，目前较多的移动端应用内嵌的浏览器内核也渐渐开始采用 Blink。
4. **网络** - 用于网络调用，比如 HTTP 请求。
5. **用户界面后端** - 绘制基本的窗口小部件
6. **JavaScript 解释器**      用于解析和执行 JavaScript 代码。
7. **数据存储**。这是持久层。浏览器需要在硬盘上保存各种数据，例如 Cookie。

![img](https://user-gold-cdn.xitu.io/2018/2/22/161bb3c9aeaf8771?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 渲染过程



![img](https://user-gold-cdn.xitu.io/2018/2/22/161bb3c9b220f8cb?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



浏览器的**渲染过程**主要包括以下几步：

1. 解析HTML生成DOM树。  

2. 解析CSS生成CSSOM规则树。

   每个CSS文件都被分析成一个StyleSheet对象，每个对象都包含CSS规则。

3. 将DOM树与CSSOM规则树合并在一起生成渲染树。

4. 遍历渲染树开始布局，计算每个节点的位置大小信息。

5. 将渲染树每个节点绘制到屏幕。

### 渲染阻塞

https://juejin.im/entry/59e1d31f51882578c3411c77

存在阻塞的 CSS 资源时，浏览器会延迟 JavaScript 的执行和 DOM 构建。另外：

1. 当浏览器遇到一个 script 标记时，DOM 构建将暂停，直至脚本完成执行。
2. JavaScript 可以查询和修改 DOM 与 CSSOM。
3. CSSOM 构建时，JavaScript 执行将暂停，直至 CSSOM 就绪。

所以，script 标签的位置很重要。实际使用时，可以遵循下面两个原则：

1. CSS 优先：引入顺序上，CSS 资源先于 JavaScript  资源。

2. JavaScript 应尽量少影响 DOM 的构建。

   

 1. **生成DOM树过程，遇到  js 会停下来直到脚本完成解析。**

 2. 如果JavaScript脚本还操作了CSSOM，而正好这个CSSOM还没有下载和构建，会延迟脚本执行和构建DOM（除了async 和 await），直至完成CSSOM的下载和构建。

    <u>(因此开发时CSS放在前面，通常把JS代码放到页面底部，且JavaScript 应尽量少影响 DOM 的构建)</u>
    
    <link>放置在<head></head>是**因为浏览器要先渲染页面呈现给用户**，在渲染时需要构建dom树(html标签内容)和render渲染树(css样式)，才能完整呈现，所以**放在头部优先加载**。
    
    而JS脚本文件比较大，且一般是后期JS引擎运行，渲染引擎会将控制权交给JS引擎而停止渲染，如果JS文件较大，会导致长时间白屏，影响用户体验，所以才会有JS<script>放在</body>之前。或者上一个问题中提到的优化加载的方式。

**补充**：

#### **异步加载js的方法**

**1.**    **async加载（异步加载JS文件）**

<script src="script.js" async="async"></script>

   HTML 5新定义的<script>属性。当渲染引擎解析文档时，遇到script标签继续解析，同时并行加载<script>标签中请求的资源。当<script>中的资源加载完成后，此时渲染浏览器才会暂停解析文档，将控制权交给JS引擎，执行指定脚本文件，执行完成后再移交控制权给渲染引擎继续解析文档（解析未完成的情况下）。

  async属性通过异步加载文件的方式解决了阻塞的问题，也因为异步执行，导致文件加载执行顺序不同。

**2.**    **defer加载（仅指加载JS文件）**

  使用方法:<script src="script.js" defer></script>

  严格按照<script>脚本顺序加载，加载的同时解析文档，但会延迟执行JS脚本，**文档解析完成后再执行JS脚本。**

![wfL82.png](https://segmentfault.com/img/bVWhRl?w=801&h=814)

### 构建渲染树

通过DOM树和CSS规则树  构建渲染树。浏览器会先从DOM树的根节点开始遍历每个可见节点。对每个可见节点，找到其适配的CSS样式规则并应用。

渲染树构建完成后，每个节点都是可见节点并且都含有其内容和对应规则的样式。这也是渲染树与DOM树的最大区别所在。display等于none的也不会被显示在这棵树里头，但是visibility：hidden的元素会显示。

### 渲染树布局

从渲染树的根节点开始遍历，确定每个节点对象在页面上的确切大小与位置，输出是一个盒子模型。

### 渲染树绘制

在绘制阶段，遍历渲染树，调用渲染器的**paint()**方法在屏幕上显示其内容。渲染树的绘制工作是由浏览器的UI后端组件完成的。

#### reflow与repaint：

HTML默认是流式布局的，CSS和js会打破这种布局，改变DOM的外观样式以及大小和位置。这时就要提到：replaint和reflow。
repaint：屏幕的一部分重画，不影响整体布局，比如某个CSS的背景色变了，但元素的几何尺寸和位置不变。
reflow： 元件的几何尺寸变了，需要重新验证并计算渲染树（Layout）
所以我们应该尽量减少reflow和repaint，这也是为什么现在很少有用table布局的原因之一。

display:none 会触发 reflow，visibility: hidden属性并不算是不可见属性，它的语义是隐藏元素，但元素仍然占据着布局空间，它会被渲染成一个空框，所以visibility:hidden 只会触发 repaint，因为没有发生位置变化。

有些情况下，比如修改了元素的样式，浏览器并不会立刻 reflow 或 repaint 一次，而是会把这样的操作积攒一批，然后做一次 reflow，又叫异步 reflow。
有些情况下，比如 resize 窗口，改变了页面默认的字体等。会马上进行 reflow。

## 浏览器重绘(repaint)重排(reflow)与优化[浏览器机制]

https://juejin.im/post/5c15f797f265da61141c7f86#heading-13

## 前端安全性相关

https://juejin.im/post/5e16fc706fb9a02fe45805a6

### **XSS(跨站脚本攻击)**

向web页面或者网站的url添加恶意的script(脚本)代码，使用户访问该网站时，执行恶意代码对客户端网页进行篡改，从而在用户浏览网页时，对用户浏览器进行控制或者获取用户隐私数据。 

注入恶意脚本一般包括Javascript，有时也会包含HTML和Flash,共同点为：将一些隐私数据像cookie、session发送给攻击者，将受害者重定向到一个由攻击者控制的网站，

**发生XSS的场景：**

\1. 网站对用户的输入过滤不足，返回给用户的展示结果过滤不足。

\2. 网站的链接地址未经过过滤

为了减轻这些攻击，需要在HTTP头部配上，set-cookie： httponly-这个属性可以防止XSS,它会禁止javascript脚本来访问cookie。secure - 这个属性告诉浏览器仅在请求为https的时候发送cookie

**XSS攻击的防范：** 

​    1、输入检查    对用户的任何输入进行检查、过滤和转义。建立可信任的字符和HTML标签白名单，对于不在白名单的字符或者标签进行过滤或编码。 

​    2、输出检查    服务器的输出也会存在问题，在变量输出到HTML页面时，可以使用编码或者转义的方式来预防XSS攻击。

- 对 HTML 标签及一些特殊符号进行转义

  > 该种方法是一种非常简单的过滤方法，仅仅是通过转义的方式将一些 HTML 标签和属性转义，比如 < 转义成 &lt ;， 这样像<script>xxx</script>的脚本就运行不了。当然简单的过滤方式也就代表很容易就会被绕过。 
  > 另外如果需要使用含有富文本的功能时，使用这样的过滤就会使富文本失去作用。

- 使用白名单、黑名单的方式进行过滤

  > 白名单、黑名单顾名思义是要定义哪些东西是可通过的，哪些东西不可通过。比如常见 <b>、<p>; 、< 等等标签，不可通过的比如 javascript、<a>、<script>、<iframe>、onload 等等一些属性，将其进行转义。 
  > 当然使用该种方法也有自身的缺点，你并不可能穷举出所有元素，也可能会某些元素在黑名单内，但在某些情况它是需要使用的，这就需要我们在设计 XSS 过滤器的时候权衡好，取最合理最适合需求的设计。

### CSRF 跨站请求伪造

https://zhuanlan.zhihu.com/p/114750961

​    是一种劫持受信任用户向服务器发送非预期请求的攻击方式。 通常情况下，CSRF攻击时攻击者借助受害者的Cookie骗取服务器的信任，可以在受害者毫不知情的情况下**以受害者名义伪造请求发送给受攻击服务器，从而在未授权的情况下进行操作。**

你可以这样来理解：攻击者盗用了你的身份，以你的名义发送恶意请求，对服务器来说这个请求是完全合法的，但是却完成了攻击者所期望的一个操作，比如以你的名义发送邮件、发消息，盗取你的账号，添加系统管理员，甚至于购买商品、虚拟货币转账等。

**CSRF的防范** 

  验证 HTTP Referer 字段；在请求地址中添加 token 并验证；在 HTTP 头中自定义属性并验证。

将cookie设置为HttpOnly
	具体的设置方法因网站的语言不同，而有所差异，具体方法请自行谷歌
检测Referer
	HTTP头部的Referer用来记录HTTP请求的来源地址，一般情况下，来自本站的请求都是合法且安全的，而且对于比较敏感操作，必须设置来源本站，于是通过检测Referer信息，就可以避免此类攻击
设置token
	在请求中放入攻击者无法伪造的东西，从而避免此类攻击，如在http请求中加入随机的token，然后在数据提交时，先进行token验证，如果正确，则继续后续操作，否则阻止继续进行。

## 前端安全

https://juejin.im/entry/598d6eb46fb9a03c3a25d2c1

一、XSS攻击与防御

  跨站脚本攻击，是说攻击者通过注入恶意的脚本，在用户浏览网页的时候进行攻击，比如获取cookie，或者其他用户身份信息，可以分为存储型和反射型，存储型是攻击者输入一些数据并且存储到了数据库中，其他浏览者看到的时候进行攻击，反射型的话不存储在数据库中，往往表现为将攻击代码放在url地址的请求参数中，防御的话为cookie设置httpOnly属性，对用户的输入进行检查，进行特殊字符过滤

 

 

二、CSRF攻击

跨站请求伪造，可以理解为攻击者盗用了用户的身份，以用户的名义发送了恶意请求，比如用户登录了一个网站后，立刻在另一个ｔａｂ页面访问量攻击者用来制造攻击的网站，这个网站要求访问刚刚登陆的网站，并发送了一个恶意请求，这时候CSRF就产生了，比如这个制造攻击的网站使用一张图片，但是这种图片的链接却是可以修改数据库的，这时候攻击者就可以以用户的名义操作这个数据库，防御方式的话：使用验证码，检查https头部的refer，使用token

防御CSRF 攻击主要有三种策略：验证 HTTP Referer 字段；在请求地址中添加 token 并验证；在 HTTP 头中自定义属性并验证。

 

三、HTTP劫持与对策

当我们访问页面的时候，运营商在页面的HTML代码中，插入弹窗、广告等HTML代码，来获取相应的利益。

针对这种情况，最好的解决方式也就是使用HTTPS，加密过后，他们就没法插入广告代码了。

那么对于还没有升级的情况，我们可以努力让影响降到最低。

 

四、界面操作劫持

五、防御手段

**上面列举的例子都不具备实际攻击作用**，因为浏览器厂商，W3C等已经做了很多安全工作，让我们的页面可以安稳的运行起来。但道高一尺魔高一丈，我们要合理运用防护手段，才能让页面不被攻击。

1、HTTP响应头，在响应头可以通过这些字段来提高安全性

- X-Frame-Options 禁止页面被加载进iframe中
- X-XSS-Protection 对于反射型XSS进行一些防御
- X-Content-Security-Policy 这个就比较复杂了，可选项很多，用来设置允许的的资源来源以及对脚本执行环境的控制等。

2、使用HTTPS、使用HTTP ONLY的cookie。cookie的secure字段设置为true

3、GET请求与POST请求，要严格遵守规范，不要混用，不要将一些危险的提交使用JSONP完成。

## 大前端性能总结

https://juejin.im/post/5b025d856fb9a07aa0484e54

## **cookie**

Cookie是客户端保存用户信息的一种机制，用来记录用户的一些信息，实际上Cookie是服务器在**本地机器**上存储的一小段文本，并随着每次请求发送到服务器。

**Cookie技术通过请求和响应报文中写入Cookie信息来控制客户端的状态。**

1. 访问某个网站时，服务器首先根据浏览器的编号生成一个cookie 返回给客户端。客户端下次再访问时就会将自己本地的cookie 加上url访问地址一同给服务器。服务器读出来**以此来辨别用户的状态**。

   ![cookie是什么，如何使用cookie？](https://exp-picture.cdn.bcebos.com/a007a9b1eef97fbdb9e85a00b74133bad2413328.jpg?x-bce-process=image%2Fresize%2Cm_lfit%2Cw_500%2Climit_1)

   2    当你浏览某网站时，由Web服务器置于你硬盘上的一个非常小的文本文件，它可以记录你的用户ID、密码、浏览过的网页、停留的时间等信息。当你再次来到该网站时，网站通过读取Cookie，得知你的相关信息，就可以做出相应的动作，如在页面显示欢迎你的标语，或者让你不用输入ID、密码就直接登录等等。如果你清理了Cookie，那么你曾登录过的网站就没有你的修改过的相关信息。

网站通过Cookie信息来记忆以及辨认你的帐号，它可以记忆你的偏好设置。它还可以使网站提供个性化的内容，举个例子，如果你在淘宝上购物，淘宝可以记忆你所查看过的产品并据此来向你来推荐商品，即便你没有登陆个人帐号。

**session**

服务器会发放用以识别用户的Session ID。通过验证从客户端发送过来的信息进行验证，然后把用户的认证状态与Session ID 绑定后记录在服务器端。向客户端返回响应时，会在首部字段Set-Cookie 内写入Session ID（如PHPSESSID=l128ogl…）。你可以把Session ID 想象成一种用以区分不同用户的唯一Id。

步骤三：客户端接收到从服务器端发来的Session ID 后，会将其作为Cookie 保存在本地。下次向服务器发送请求时，浏览器会自动发送Cookie，所以Session ID 也随之发送到服务器。服务器端可通过验证接收到的Session ID 验证状态。

### **cookie与session的区别**

```
cookie和session都是用来跟踪浏览器用户身份的会话方式。
```

(1)cookie数据存放在客户的浏览器上，session数据放在服务器上
(2)cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗,如果主要考虑到安全应当使用session
(3)session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能，如果主要考虑到减轻服务器性能方面，应当使用COOKIE
(4)一个站点在客户端存放的COOKIE不能超过3K。
(5)所以：**将登陆信息等重要信息存放为SESSION**;其他信息如果需要保留，可以放在COOKIE中

### cookies，sessionStorage和localStorage的区别？

在之前都是使用cookie的，HTML5提供两种本地存储方案：localStorage：用于持久化的本地存储，数据永不过期，关闭浏览器不丢失。

 sessionStorage ：同一个会话的页面才能访问并且当会话结束后数据也会随之销毁，不是一种持久化的本地存储，仅仅是会话级别的存储

共同点：都保存在浏览器端，且是同源的。

区别：cookies是为了标识用户身份而存储在用户本地终端上的数据，始终在同源http请求中携带，**即cookies在浏览器和服务器间来回传递**，而sessionstorage和localstorage不自动把数据发给服务器，仅在本地保存。

**存储大小**不同。cookie保存的数据不能超过4k，sessionstorage和localstorage可达到5M。

**数据的有效期**不同。cookie在设置的cookie过期时间前一直有效，即使浏览器关闭。sessionstorage仅在浏览器窗口关闭之前有效。localstorage始终有效。

作用域不同。cookie在所有的同源窗口都是共享；sessionstorage不在不同的浏览器共享，即使同一页面，localstorage在所有同源窗口都是共享（所谓"同源"指的是"三个相同"。

协议、域名、端口相同）

### session

![img](https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=2169381799,1320776160&fm=173&app=49&f=JPEG?w=450&h=300&s=440A5532435E4DC80AD4B1DB0000C0B2)

session称为会话信息，位于web服务器上，主要负责访问者与网站之间的交互，当访问浏览器请求http地址时，将传递到web服务器上并与访问信息进行匹配， 当关闭网站时就表示会话已经结束，网站无法访问该信息了，所以它无法保存永久数据，我们无法访问以及禁用网站

Session与cookie功能效果相同。**Session与Cookie的区别在于Session是记录在服务端的，而Cookie是记录在客户端的。**

解释session：当访问服务器否个网页的时候，会在服务器端的内存里开辟一块内存，这块内存就叫做session，而这个内存是跟浏览器关联在一起的。这个浏览器指的是浏览器窗口，或者是浏览器的子窗口，意思就是，只允许当前这个session对应的浏览器访问，就算是在同一个机器上新启的浏览器也是无法访问的。而另外一个浏览器也需要记录session的话，就会再启一个属于自己的session

原理：HTTP协议是非连接性的，取完当前浏览器的内容，然后关闭浏览器后，链接就断开了，而没有任何机制去记录取出后的信息。而当需要访问同一个网站的另外一个页面时(就好比如在第一个页面选择购买的商品后，跳转到第二个页面去进行付款)这个时候取出来的信息，就读不出来了。所以必须要有一种机制让页面知道原理页面的session内容。

问题：如何知道浏览器和这个服务器中的session是一一对应的呢？又如何保证不会去访问其它的session呢？

原理解答：就是当访问一个页面的时候给浏览器创建一个独一无二的号码，也给同时创建的session赋予同样的号码。这样就可以在打开同一个网站的第二个页面时获取到第一个页面中session保留下来的对应信息（理解：当访问第二个页面时将号码同时传递到第二个页面。找到对应的session。）。这个号码也叫sessionID，session的ID号码，session的独一无二号码。

## cookie session token

保持登录状态https://blog.csdn.net/qq_39385118/article/details/86350049

## 前端多线程--Web Worker

**为什么引入多线程？**

Javascript 的确是单线程的，阻塞和其他异步的需求的确是通过实现循环来解决的，但是这套机制当线程需要处理大规模的计算的时候就不大适用了

为了利用多核 `CPU` 的计算能力，**`HTML5` 提出 `Web Worker` 标准（多线程解决方案），允许 `JavaScript` 脚本创建多个线程**。但子线程完全受主线程控制，不得操作 `DOM`。没有改变 `JavaScript` 单线程的本质。

**使用场景：**
1.复杂数据处理场景
	某些检索、排序、过滤、分析会非常耗费时间，这时可以使用Web Worker来进行，不占用主线程。
2预渲染
	在某些渲染场景下，比如渲染复杂的canvas的时候需要计算的效果比如反射、折射、光影、材料等，这些计算的逻辑可以使用Worker线程来执行，也可以使用多个Worker线程

3.页头消息状态更新，比如页头的消息个数通知

4.高频用户交互，拼写检查，譬如：根据用户的输入习惯、历史记录以及缓存等信息来协助用户完成输入的纠错、校正功能等

5.加密：加密有时候会非常地耗时，特别是如果当你需要经常加密很多数据的时候（比如，发往服务器前加密数据）。

​	加密是一个使用 `Web Worker` 的绝佳场景，因为它并不需要访问 `DOM` ，只是纯粹使用算法进行计算。随着大众对个人敏感数据的日益重视，信息安全和加密也成为重中之重。这可以从近期的 12306 用户数据泄露事件中体现出来。

​	在 Worker 进行计算，对于用户来说是无缝地且不会影响到用户体验。

6.**预取数据**：为了优化网站或者网络应用及提升数据加载时间，你可以使用 `Workers` 来提前加载部分数据以备不时之需。  预加载图片：有时候一个页面有很多图片，或者有几个很大的图片的时候，如果业务限制不考虑懒加载，也可以使用Web Worker来加载图片



**`workers` 和主线程间的数据传递**：

​	双方都使用 `postMessage()` 方法发送各自的消息，使用`onmessage` 事件处理函数来响应消息（消息被包含在 `Message` 事件的 `data` 属性中）。

​	这个过程中数据并不是被共享而是被复制。

**注意**

- 虽然使用worker线程不会占用主线程，但是启动worker会比较耗费资源
- 主线程中使用XMLHttpRequest在请求过程中浏览器另开了一个异步http请求线程，但是交互过程中还是要消耗主线程资源



## 跨域

https://segmentfault.com/a/1190000011145364

跨域是指一个域下的文档或脚本试图去请求另一个域下的资源，这里跨域是广义的。

广义的跨域：

1.) 资源跳转： A链接、重定向、表单提交

2.) 资源嵌入： <link>、<script>、<img>、<frame>等dom标签，还有样式中background:url()、@font-face()等文件外链

3.) 脚本请求： js发起的ajax请求、dom和js对象的跨域操作等

通常所说的跨域是狭义的，是由浏览器相似策略限制的一类请求场景

同源策略/ SOP（Same origin policy）是一种约定，是浏览器最核心最基本的安全功能，

​     “协议+域名+端口”三者相同

**同源策略限制内容有：**

- Cookie、LocalStorage、IndexedDB 等存储性内容
- DOM 节点
- AJAX 请求发送后，结果被浏览器拦截了

但是有三个标签是允许跨域加载资源：

- `<img src=XXX>`
- `<link href=XXX>`
- `<script src=XXX>`

**跨域如何解决**

1. JSONP：动态创建script，再请求一个带参网址实现跨域通信。

2. document.domain + iframe跨域：两个页面都通过js强制设置document.domain为基础主域，就实现了同域。

3. location.hash + iframe跨域：a欲与b跨域相互通信，通过中间页c来实现。 三个页面，不同域之间利用iframe的location.hash传值，相同域之间直接js访问来通信。

4. window.name + iframe跨域：通过iframe的src属性由外域转向本地域，跨域数据即由iframe的window.name从外域传递到本地域。

5. postMessage跨域：可以跨域操作的window属性之一。（HTML5）

6. CORS：服务端设置Access-Control-Allow-Origin即可，前端无须设置，若要带cookie请求，前后端都需要设置。

7. 代理跨域：启一个代理服务器，实现数据的转发

8. WebSocket协议跨域

### jsonp

https://www.jianshu.com/p/e1e2920dac95

面试相关：https://blog.csdn.net/weixin_43424101/article/details/84288991

1) JSONP原理

JSONP的跨域方式是<u>利用`<script>`标签的src属性可以跨域引用资源的特点</u>，有这些属性的标签还有`<img>`、`<iframe>`，但是JSONP只支持GET方式。JSONP请求一定需要对方的服务器做支持才可以。

2) JSONP和AJAX对比

JSONP和AJAX相同，都是客户端向服务器端发送请求，从服务器端获取数据的方式。但AJAX属于同源策略，JSONP属于非同源策略（跨域请求）

3) JSONP优缺点

优点是简单兼容性好，可用于解决主流浏览器的跨域数据访问的问题。**缺点是仅支持get方法具有局限性,不安全可能会遭受XSS攻击。**

下面我们以点击获取随机新闻列表的例子来演示一下JSONP的具体工作原理(test.com访问a.test.com)

HTML如下:

```html
<div class="container">
  <ul class="news">
    <li>第11日前瞻：中国冲击4金 博尔特再战</li>
    <li>男双力争会师决赛 </li>
    <li>女排将死磕巴西！</li>
  </ul>
  <button class="change">换一组</button>
</div>
```

<u>首先，我们在前端要在调用资源的时候动态创建script标签，并设置src属性指向资源的URL地址</u>，代码如下:

```javascript
document.querySelector('.change').addEventListener('click', function() {
  var script = document.createElement('script')
  script.setAttribute('src', '//a.test.com:8080/getNews?callback=appendHtml')   //callback=appendHtml是给后端资源打包数据用的参数，同时也是前端定义的回调函数
  document.head.appendChild(script)
  document.head.removeChild(script) //删除script标签是因为script标签插入页面的时候资源已经请求到了
})
```

定义获取资源后需要执行的回调函数:

```javascript
function appendHtml(news) {
    var html = ''
    for (var i = 0; i < news.length; i++) {
        html += '<li>' + news[i] + '</li>'
    }
    document.querySelector('.news').innerHTML = html
}
```

后端是把前端发送的URL地址拿到的数据以前端定义的回调函数（appendHtml）的<u>参数的形式返回</u>给前端，这样到了前端就可以调用执行了:

```javascript
var news = [
    "第11日前瞻：中国冲击4金 博尔特再战200米羽球",
    "正直播柴飚/洪炜出战 男双力争会师决赛",
    "女排将死磕巴西！郎平安排男陪练模仿对方核心",
    "没有中国选手和巨星的110米栏 我们还看吗？",
    "中英上演奥运金牌大战",
    "博彩赔率挺中国夺回第二纽约时报：中国因对手服禁药而丢失的奖牌最多",
    "最“出柜”奥运？同性之爱闪耀里约",
    "下跪拜谢与洪荒之力一样 都是真情流露"
]
var data = [];
for (var i = 0; i < 3; i++) {
  var index = Math.floor(Math.random() * news.length);

  data.push(news[index]);
}
var callback = req.query.callback;   //查询前端有没有传入回调函数
if (callback) {
    res.send(callback + '(' + JSON.stringify(data) + ')');    //数据以函数参数的方式传给前端
} else {
    res.send(data);
}
```

这样我们就从test.com访问到了a.test.com下的资源

### cors  跨域资源共享

1. iframe   2.打开目标窗口并发送  https://zhuanlan.zhihu.com/p/26777882

它允许浏览器向跨源服务器，发出[`XMLHttpRequest`](http://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html)请求，从而克服了AJAX只能[同源](http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html)使用的限制。

整个CORS通信过程，都是浏览器自动完成，不需要用户参与。对于开发者来说，CORS通信与同源的AJAX通信没有差别，代码完全一样。浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。

因此，实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信。

分为**简单请求和非简单请求**

一旦服务器通过了"预检"请求，以后每次**浏览器正常的`CORS`请求，就都跟简单请求一样，会有一个`Origin`头信息字段。服务器的回应，也都会有一个`Access-Control-Allow-Origin`头信息字段。**

比较：JSONP只支持`GET`请求，CORS支持所有类型的HTTP请求。JSONP的优势在于支持老式浏览器，以及可以向不支持CORS的网站请求数据。

### postMessage()

https://zhuanlan.zhihu.com/p/26777882 postMessage跨域

HTML5的新方法，可以使用它来向其它的window对象发送数据，无论这个window对象是属于同源或不同源

**发送页面使用postMessage()方法，接收监听window的message事件即可。**

## http2.0新特性

１.HTTP/2 采用二进制格式传输数据，而非 HTTP/1.x 的文本格式。二进制格式在协议的解析和优化扩展上带来更多的优势和可能。 
 2.HTTP/2 对消息头采用 HPACK 进行压缩传输，能够节省消息头占用的网络的流量。而 HTTP/1.x 每次请求，都会携带大量冗余头信息，浪费了很多带宽资源。头压缩能够很好的解决该问题。 
3.多路复用，所有的请求都是通过一个 TCP 连接并发完成。HTTP/1.x 虽然通过 pipeline 也能并发请求，但是多个请求之间的响应会被阻塞。HTTP/2 做到了真正的并发请求。同时，流还支持优先级和流量控制。 （不需要精灵图，一个TCP连接加载一整个页面）
4.Server Push：服务端能够更快的把资源推送给客户端。例如服务端可以主动把 JS 和 CSS 文件推送给客户端，而不需要客户端解析 HTML 再发送这些请求。当客户端需要的时候，它已经在客户端了。



## 找出一个网页中标签名以 h 开头的所有元素

```js
var eles = document.querySelectorAll('*');
function find(eles){
    var res = [];
    for(let i = 0; i < eles.length; i++) {
        if (eles[i].tagName.startsWith('H')){
            res.push(eles[i].tagName);
        }
    }
    return res;
}
var res = find(eles);
var _set = [...new Set(res)];
```

## 打印所有DOM

```js
var _body = document.getElementsByTagName("body")[0];
let j=0
for (var i = 0; i < _body.childNodes.length; i++) {
    if (_body.childNodes[i].nodeType == 1) {   //去除空格
        j++
        console.log("第" + j + "个标签：" + _body.childNodes[i].nodeName);
    }
    
}
```

## innerHTML,innerText,outerHTML

https://blog.csdn.net/shadow_zed/article/details/72846958

innerHTML,  包含内部标签

innerText,   除了内部标签的内容

outerHTML  包含自身标签，内部标签以及内容

## 两个静态html页面传值方法的总结

https://blog.csdn.net/csdn_ds/article/details/78393564