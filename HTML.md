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

# 浏览器渲染过程

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

什么时候发生重绘、重排  https://juejin.cn/post/6844904083212468238#heading-9

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

### css加载会造成阻塞吗？

https://zhuanlan.zhihu.com/p/43282197

1. css加载不会阻塞DOM树的解析
2. css加载会阻塞DOM树的渲染
3. css加载会阻塞后面js语句的执行

因此，为了避免让用户看到长时间的白屏时间，我们应该尽可能的提高css加载速度，比如可以使用以下几种方法:

1. 使用CDN(因为CDN会根据你的网络状况，替你挑选最近的一个具有缓存内容的节点为你提供资源，因此可以减少加载时间)
2. 对css进行压缩(可以用很多打包工具，比如webpack,gulp等，也可以通过开启gzip压缩)
3. 合理的使用缓存(设置cache-control,expires,以及E-tag都是不错的，不过要注意一个问题，就是文件更新后，你要避免缓存而带来的影响。其中一个解决防范是在文件名字后面加一个版本号)
4. 减少http请求数，将多个css文件合并，或者是干脆直接写成内联样式(内联样式的一个缺点就是不能缓存)

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

# 两个静态html页面传值方法的总结

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

# SEO

**请用html知识解决seo(搜索引擎优化)优化问题**

答：1.设置 TDK

TDK是SEO术语，是三个单词的缩写。 即：title/description/keywords。

2. html语义化标签，要简洁合理，这样css和js加载不全的时候，使html文档尽量清晰的展示，而不会特别乱

**简述一下你对HTML语义化的理解？**

丢失样式时能让页面呈现清晰结构。

有利于SEO和搜索引擎建立良好沟通，有助于爬虫抓取更多的信息，爬虫依赖于标签来确定上下文和各个关键字的权重。

方便其它设备解析。 便于团队开发和维护，语义化根据可读性。

关于HTML语义化，你知道的都有哪些标签？header、article、address

# 小知识点

## 块状，行内，内联块状元素

1.**块状元素**

```xml
<div>、<p>、<h1>-<h6>、<ol>、<ul>、<dl>、<table>、<address>、<blockquote> 、<form>
```

2.**行内元素**(内联元素)

一个行内元素只占据它**对应标签的边框所包含的空间**。

```xml
<a>、<span>、<br>、<i>、<em>、<strong>、<label>、<q>、<var>、<cite>、<code>
```

（1）设置宽度 **width 无效**。

（2）设置高度 **height 无效**，但可以通过 **line-height** 来设置。

（3）设置 margin 只有 **左右有效，上下无效。**

（4）设置 padding 只有 **左右有效，上下无效**。注意元素范围是增大了，但是对元素周围的内容是没影响的。

**行内元素和块级元素对比**

- 行内元素只能包含**数据和其他行内元素**。而块级元素可以包含**行内元素和其他块级元素**。这种结构上的包含继承区别可以使块级元素创建比行内元素更”大型“的结构。

- 默认情况下，行内元素**不会以新行开始**，而块级元素会**新起一行**。

- 行内元素**不可以**设置宽高，块级元素**可以**设置宽高

- 行内元素**水平方向**的 margin 和 padding 可以生效。但**竖直方向**的 margin 和 padding 不能生效。块级元素可以设置margin，padding

3.内联-块状元素

inline-block 元素特点：
1.和其他元素都在一行上；

2.元素的高度、宽度、行高以及**顶和底边距**都可设置。

3.它也会有元素间出现空白区域的问题

常用的内联块状元素有：

```xml
<img>、<input>
```

补充：

若设置行内元素 **float:left/right**，则该行内元素转换为**块级元素** ，且具有浮动特性。

若为行内元素进行定位，**position:absolute** 或者 **position:fixed** 都会把行内元素转换为**块级元素**。



## *Inline-Block的坑 

空白间隙问题以及解决

http://layout.imweb.io/article/inline-block.html

## 自闭合标签

在HTML中，标签分为两种：一般标签和自闭合标签。

**只有开始符号而没有结束符号，因此不可以在内部插入标签或文字**

| 标签      | 说明                             |
| :-------- | :------------------------------- |
| <meta />  | 定义网页的信息（供搜索引擎查看） |
| <link />  | 引入“外部CSS文件”                |
| <br />    | 换行标签                         |
| <hr />    | 水平线标签                       |
| <img />   | 图片标签                         |
| <input /> | 表单标签                         |