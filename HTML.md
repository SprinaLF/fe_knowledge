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

渲染过程中，**遇到script标签，如果是普通JS标签则同步加载并执行，阻塞页面渲染，如果标签上有defer / async属性则异步加载JS资源**

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

1. <u>JSONP：动态创建script，再请求一个带参网址实现跨域通信。</u>

2. document.domain + iframe跨域：两个页面都通过js强制设置document.domain为基础主域，就实现了同域。

3. location.hash + iframe跨域：a欲与b跨域相互通信，通过中间页c来实现。 三个页面，不同域之间利用iframe的location.hash传值，相同域之间直接js访问来通信。

4. window.name + iframe跨域：通过iframe的src属性由外域转向本地域，跨域数据即由iframe的window.name从外域传递到本地域。

5. postMessage跨域：可以跨域操作的window属性之一。（HTML5）

6. <u>CORS：需要服务器设置header：`Access-Control-Allow-Origin`</u>，浏览器发送请求是携带origin字段。

7. <u>Nginx反向代理 可以不需要目标服务器配合，不过需要Nginx中转服务器，用于转发请求（服务端之间的资源请求不会有跨域限制）</u>

8. WebSocket协议跨域

### jsonp

https://www.jianshu.com/p/e1e2920dac95

面试相关：https://blog.csdn.net/weixin_43424101/article/details/84288991

1) JSONP原理

<u>利用`<script>`标签的src属性可以跨域引用资源的特点</u>，有这些属性的标签还有`<img>`、`<iframe>`，但是JSONP只支持GET方式。需要对方的服务器做支持

2) JSONP和AJAX对比

JSONP和AJAX相同，都是客户端向服务器端发送请求，从服务器端获取数据的方式。但AJAX属于同源策略，JSONP属于非同源策略（跨域请求）

3) JSONP优缺点

优点是简单兼容性好，可用于解决主流浏览器的跨域数据访问的问题。**缺点是仅支持get方法具有局限性,不安全可能会遭受XSS攻击。**

下面我们以点击获取随机新闻列表的例子来演示一下JSONP的具体工作原理(test.com访问a.test.com)

例子：https://www.jianshu.com/p/447fe4d86dd5

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

<u>首先，我们在前端要在获取资源的时候动态创建script标签，并设置src属性指向资源的URL地址</u>，代码如下:

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

**CORS需要浏览器和服务器同时支持**

CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）。

它允许浏览器向跨源服务器，发出[`XMLHttpRequest`](https://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html)请求，从而克服了AJAX只能[同源](https://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html)使用的限制



1. iframe   2.打开目标窗口并发送  https://zhuanlan.zhihu.com/p/26777882

![img](https://upload-images.jianshu.io/upload_images/4023562-50e621b42aa7641d.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/922/format/webp)

整个CORS通信过程，都是浏览器自动完成，不需要用户参与。对于开发者来说，CORS通信与同源的AJAX通信没有差别，代码完全一样。浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。

因此，实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信。

分为**简单请求和非简单请求**

一旦服务器通过了"预检"请求，以后每次**浏览器正常的`CORS`请求，就都跟简单请求一样，会有一个`Origin`头信息字段。服务器的回应，也都会有一个`Access-Control-Allow-Origin`头信息字段。**

比较：JSONP只支持`GET`请求，CORS支持所有类型的HTTP请求。JSONP的优势在于支持老式浏览器，以及可以向不支持CORS的网站请求数据。



### Nginx反向代理

补充：反向代理：浏览器不知道真正的服务器。正向代理：服务器不知道浏览器 VPN 就是正向代理。

https://zhuanlan.zhihu.com/p/94197713

使用nginx反向代理实现跨域，只需要修改nginx的配置即可解决跨域问题。

A网站向B网站请求某个接口时，向B网站发送一个请求，nginx根据配置文件接收这个请求，代替A网站向B网站来请求。
nginx拿到这个资源后再返回给A网站，以此来解决了跨域问题。

例如nginx的端口号为 8090，需要请求的服务器端口号为 3000。（localhost:8090 请求 localhost:3000/say）

nginx配置如下:

```
server {
    listen       8090;

    server_name  localhost;

    location / {
        root   /Users/liuyan35/Test/Study/CORS/1-jsonp;
        index  index.html index.htm;
    }
    location /say {
        rewrite  ^/say/(.*)$ /$1 break;
        proxy_pass   http://localhost:3000;
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Credentials' 'true';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
    }
    # others
}
```

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

路由传参： 取值方便，可以跨域，利于页面分享，没有环境限制。缺点：url携带参数值的长度有限制。

放到cookie,优点：可以在同源内的的任意网页中访问，存储数据的周期可以自由设置。缺点：有长度限制

H5: localStorage：储存空间大，有5M存储空间。



# DOCTYPE有什么作用？标准模式与混杂模式如何区分？它们有何意义?

告诉浏览器用哪个版本的HTML规范来渲染文档。DOCTYPE不存在或形式不正确会导致HTML文档以混杂模式呈现。

 1）严格模式(标准模式)：排版和JS运作模式以该浏览器支持的最高标准运行。

  2）混杂模式(几乎标准模式)：页面以宽松的向后兼容的方式显示；模拟老浏览器的行为以防站点无法工作。

  3）怪异模式（Quirks mode）：使用浏览器自己的方式来解析执行代码

4.2 怪异模式quirk有哪些怪异的行为

1）最主要的比如IE与W3C的盒子模型不同（也是目前浏览器兼容的问题）；

2）可以设置行内元素的高宽

3）可设置百分比的高度，在standards模式下，一个元素的高度是由其包含的内容来决定的，如果父元素没有设置高度，子元素设置一个百分比的高度是无效的。

4）用margin:0 auto设置水平居中在IE下会失效 使用margin:0 auto在standards模式下可以使元素水平居中，但在quirks模式下却会失效,quirk模式下的解决办法，用text-align属性: body{text-align:center};#content{text-align:left}

5）设置图片的padding会失效

6）quirk模式下Table中的字体属性不能继承上层的设置 