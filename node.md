Node.js是基于谷歌浏览器的V8引擎打造，C++语言编写的一个js运行环境，Node.js就是使用前端的js语言（有别于传统的js语言）来实现后端的技术（服务器端）。我们以前对于前端开发人员来说，所编写的代码大部分都是运行在浏览器等前端运行环境上，对于后端服务器而言，接触的比较少，现在我们可以通过NodeJS来完成使用前端语言编写后端服务器的操作，使得我们前端的开发人员想要做后端服务方面的事情，不用再去学习一门后端语言，而是使用前端的语言技术就能够实现。本课程将一步一步地让大家快速地掌握NodeJS这样一个前端核心框架，以适应公司的开发需要。

![image-20210519155331084](/Users/wenxin/Library/Application Support/typora-user-images/image-20210519155331084.png)



原因：浏览器带v8引擎，能执行js代码。安装node也是同样道理，能解析执行js代码。

### 终端操作

$node   进入node环境(交互模型 repl模式)

$.exit  退出node环境

https://www.nodebeginner.org/index-zh-cn.html

### nvm

管理node版本

nvm list

nvm use 10.15.3

nvm install 版本号



es6不支持  es6-bable-es5（使所有浏览器支持）

箭头函数没有自己的this，指向外部this。函数有自己的作用域

![image-20210519171850874](/Users/wenxin/Library/Application Support/typora-user-images/image-20210519171850874.png)



### node全局变量

![image-20210519175248521](/Users/wenxin/Library/Application Support/typora-user-images/image-20210519175248521.png)

## 模块化

export以对象形式导出

![image-20210520142646993](/Users/wenxin/Library/Application Support/typora-user-images/image-20210520142646993.png)

require引入模块

./相对路径

模块扩展名可写可不写

![image-20210520175812678](https://tva1.sinaimg.cn/large/008i3skNly1gqp1o5w4czj31gm0lytva.jpg)



### node.js常用内置模块

![image-20210520175848370](https://tva1.sinaimg.cn/large/008i3skNly1gqp1ors0mkj31c80u0njv.jpg)

### this

交互模式中 this===global

文件中 this指向这个模块导出的对象module.exports

**`fs.readdir(path[, options], callback)`**  读取目录文件名数组

### 应用不同模块分析

我们来分解一下这个应用，为了实现上文的用例，我们需要实现哪些部分呢？

- 我们需要提供Web页面，因此需要一个*HTTP服务器*
- 对于不同的请求，根据请求的URL，我们的服务器需要给予不同的响应，因此我们需要一个*路由*，用于把请求对应到请求处理程序（request handler）
- 当请求被服务器接收并通过路由传递之后，需要可以对其进行处理，因此我们需要最终的*请求处理程序*
- 路由还应该能处理POST数据，并且把数据封装成更友好的格式传递给请求处理入程序，因此需要*请求数据处理功能*
- 我们不仅仅要处理URL对应的请求，还要把内容显示出来，这意味着我们需要一些*视图逻辑*供请求处理程序使用，以便将内容发送给用户的浏览器
- 最后，用户需要上传图片，所以我们需要*上传处理功能*来处理这方面的细节

Node.js 是单进程单线程应用程序，但是因为 V8 引擎提供的异步执行回调接口，通过这些接口可以处理大量的并发，所以性能非常高。

Node.js 基本上所有的事件机制都是用设计模式中观察者模式实现。

Node.js 单线程类似进入一个while(true)的事件循环，直到没有事件观察者退出，每个异步事件都生成一个事件观察者，如果有事件发生就调用该回调函数.

### 使用Node.js内部模块

Node.js中自带了一个叫做“http”的模块，我们在我们的代码中请求它并把返回值赋给一个本地变量。

这把我们的本地变量变成了一个拥有所有 *http* 模块所提供的公共方法的对象。

给这种本地变量起一个和模块名称一样的名字是一种惯例，但是你也可以按照自己的喜好来：

### 如何来进行请求的“路由”

**使用路由将请求以URL路径为基准映射到处理程序上。**

要为路由提供请求的URL和其他需要的GET及POST参数，

随后路由需要根据这些数据来执行相应的代码（这里“代码”对应整个应用的第三部分：一系列在接收到请求时真正工作的处理程序）。

因此，我们需要查看HTTP请求，从中提取出请求的URL以及GET/POST参数。这一功能应当属于路由还是服务器（甚至作为一个模块自身的功能）确实值得探讨，但这里暂定其为我们的HTTP服务器的功能。

我们需要的所有数据都会包含在request对象中，该对象作为*onRequest()*回调函数的第一个参数传递。但是为了解析这些数据，我们需要额外的Node.JS模块，它们分别是*url*和*querystring*模块。