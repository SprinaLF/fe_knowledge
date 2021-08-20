# 高阶组件

https://juejin.cn/post/6844903576997724167 (入门)

https://juejin.cn/post/6844904050236850184  (进阶)

组件->组件

### 函数式编程

## 对React的理解

https://zhuanlan.zhihu.com/p/24833815

\1.    React的第一要点就是组件。所有React代码基本上就是一个包含许多小组件在内的大组件。

\2.    HTML和JS代码写在一起。通过JSX的语法扩展（X代表XML）实现。

\3.    State用于组件保存、控制、修改自己的可变状态，组件之间通过Props进行交互。

\4.    组件API。render、setState、constructor……React还提供了一些按照特定次序触发的[生命周期函数](https://link.zhihu.com/?target=https%3A//facebook.github.io/react/docs/state-and-lifecycle.html)

\5.    组件类型。你也完全可以用函数声明只有render方法的无状态组件。

区分处理UI和数据逻辑的组件是一种很好的开发实践。

高阶组件函数可以传入一个组件并为其赋予更多功能。



### JSX

浏览器只能处理 JavaScript 对象，而不能读取常规 JavaScript 对象中的 JSX。所以为了使浏览器能够读取 JSX，首先，**需要用像 Babel 这样的 JSX 转换器将 JSX 文件转换为 JavaScript 对象**，然后再将其传给浏览器.

SX是React的核心组成部分，它使用XML标记的方式去直接声明界面，界面组件之间可以互相嵌套。可以理解为在JS中编写与XML类似的语言,一种定义带属性树结构（DOM结构）的语法，它的目的不是要在浏览器或者引擎中实现，它的目的是通过各种编译器将这些标记编译成标准的JS语言。

 

JSX是一种 JavaScript 的语法扩展。 推荐在 React 中使用 JSX 来描述用户界面。JSX 乍看起来可能比较像是模版语言，但事实上它完全是在 JavaScript 内部实现的。

简单来说，React 认为组件才是王道，而组件是和模板紧密关联的， JSX 这种语法，就是为了把HTML模板直接嵌入到JS代码里面，这样就做到了模板和组件关联，但是 JS 不支持这种包含 HTML 的语法，所以需要通过工具将 JSX 编译输出成 JS 代码才能使用。

在编译之后呢，JSX 其实会被转化为普通的 JavaScript 对象。这也就意味着，你其实可以在 if 或者 for 语句里使用 JSX，将它赋值给变量，当作参数传入，作为返回值都可以：



## react生命周期(最新)

https://juejin.im/post/5b6f1800f265da282d45a79a#heading-0

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/12/19/16f1e95830a01935?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 挂载阶段

挂载阶段，也可以理解为组件的初始化阶段，就是将我们的组件插入到DOM中，只会发生一次

这个阶段的生命周期函数调用如下：

- constructor

- **getDerivedStateFromProps**

  取代之前的component**Will**Mount、component**Will**ReceiveProps和component**Will**Update

- ~~componentWillMount~~

- render

- componentDidMount

### 更新阶段

更新阶段，当组件的props改变了，或组件内部调用了setState或者forceUpdate发生，会发生多次

这个阶段的生命周期函数调用如下：

- ~~componentWillReceiveProps/UNSAFE_componentWillReceiveProps~~

- getDerivedStateFromProps

- shouldComponentUpdate

  当组件接收到新属性，或者组件的状态发生改变时触发。组件首次渲染时并不会触发。一般我们通过该函数来优化性能：<u>一个React项目需要更新一个小组件时，很可能需要父组件更新自己的状态。而一个父组件的重新更新会造成它旗下所有的子组件重新执行render()方法，形成新的虚拟DOM，再用diff算法对新旧虚拟DOM进行结构和属性的比较，决定组件是否需要重新渲染</u>

  无疑这样的操作会造成很多的性能浪费，所以我们开发者可以根据项目的业务逻辑，在shouldComponentUpdate()中加入条件判断，从而优化性能(手动判断组件是否需要更新)

  例如React中的就提供了一个PureComponent的类，当我们的组件继承于它时，组件更新时就会默认先比较新旧属性和状态，从而决定组件是否更新。值得注意的是，PureComponent进行的是浅比较，所以组件状态或属性改变时，都需要返回一个新的对象或数组

- ~~componentWillUpdate/UNSAFE_componentWillUpdate~~

- render

- **getSnapshotBeforeUpdate**

  这个方法在render之后，componentDidUpdate之前调用。有两个参数prevProps和prevState，表示之前的属性和之前的state，这个函数有一个返回值，会作为第三个参数传给componentDidUpdate

  **代替componentWillUpdate**

- componentDidUpdate

  组件被更新完成后触发。页面中产生了新的DOM的元素，可以进行DOM操作

### 卸载阶段

- componentWillUnmount

**componentWillUnmount**

当我们的组件被卸载或者销毁了就会调用，我们可以在这个函数里去清除一些定时器，取消网络请求，清理无效的DOM元素等垃圾清理工作

注意不要在这个函数里去调用setState，因为组件不会重新渲染了



## Redux

问题来了，React 中一个组件里面维护数据只需要 state 和 setState 就可以轻松搞定。假如多个组件都需要维护这一份数据怎么办呢？

没有redux时，1. 都传给公共父组件，再由父组件一层层传给需要的组件。   2. 消息订阅与发布的方式，需要该数据的组件去订阅

Redux 核心的部分 Store，Store 中管理的数据独立于 React 组件之外，如果 React 某个组件中的某个数据在某个时刻改变了（可以称之为状态改变了），就可以直接更改这个 Store 中管理的数据，这样其他组件想要拿到此时的数据直接拿就行了，不需要传来传去。

Store 通常要和 Reducer 来配合使用，Store 存数据，Reducer 是个纯函数，它接收并更新数据。

提供可预测化状态管理。State以对象树形式存在store中。通过触发action来改变state.用到函数reducer

![img](http://www.ruanyifeng.com/blogimg/asset/2016/bg2016091802.jpg)

## react高频面试题

https://zhuanlan.zhihu.com/p/82840768

https://www.bbsmax.com/A/nAJvyxMmzr/

项目：https://juejin.im/post/5bca74cfe51d450e9163351b

# [React Hooks](https://segmentfault.com/a/1190000016950339)

https://www.ruanyifeng.com/blog/2019/09/react-hooks.html

https://zh-hans.reactjs.org/docs/hooks-intro.html

https://ahooks.js.org/zh-CN/hooks/dom/use-size/   ahooks

在不编写 class 的情况下(函数组件)使用 state 以及其他的 React 特性。

你可以使用 Hook 从组件中提取状态逻辑，使得这些逻辑可以单独测试并复用。Hook 使你在无需修改组件结构的情况下复用状态逻辑。

hooks出现的原因 https://zh-hans.reactjs.org/docs/hooks-intro.html

在目前的react开发中，很多新项目都采用函数组件，因此，我们免不了会接触到hooks。

此外，Hooks也是前端面试中react方面的一个高频考点，需要掌握常用的几种hooks。

常用的有

​	基本：useState, useEffect, useContext

​	额外：useCallback, useMemo, useRef

刚接触公司的react项目代码时，发现组件都是用的函数组件，不得不去学习hooks，之前只会类组件和react基础

其中useState不用说了，很容易理解，使我们在函数组件中也能像类组件那样获取、改变state

 项目中很多地方都有useEffect, useCallback,  useMemo,初看时感觉这三个都是包着一个东西，有它们跟没有它们感觉也没什么区别，很难分清这三个什么时候要用

所以这里就略微总结一下，附上一点个人在开发过程中的理解吧，
其实这三个区别还是挺明显的，
useEffect是在函数组件中实现像类组件中的生命周期那样某个阶段做某件事情(主要是`componentDidMount`，`componentDidUpdate` 和 `componentWillUnmount`)



```
useEffect(() => { getStuInfo({ id: stuId }); }, [getStuInfo, stuId]); //依赖项改变时调用getStuInfo函数
```

而useCallback和useMemo都是性能优化的手段，区别是
useCallback返回一个函数，当把它返回的这个函数作为子组件使用时，可以避免每次都去重新渲染这个子组件，

```
const renderButton = useCallback(
    () => (
      <Button type="link">
        {buttonText}
      </Button>
    ),
    [buttonText]    // 当buttonText改变时才重新渲染
);
```

useMemo返回的的是一个值，用于避免在每次渲染时都进行高开销的计算

```
const text = useMemo(() => (type === '1' ? '成功' : '失败'), [type]);

```

所以你这个可以用useCallback包裹或者都不用

## 虚拟DOM和diff

https://juejin.im/post/5a3200fe51882554bd5111a0

- 用JS对象模拟DOM树
-  **当状态变更的时候，重新构造一棵新的对象树**。比较两棵虚拟DOM树的差异
- 把差异应用到真正的DOM树上

这样就可以做到：视图的结构确实是整个全新渲染了，但是最后操作DOM的时候确实只变更有不同的地方。

## 虚拟dom

https://juejin.im/entry/5aedcfa351882506a36c664c

相对于 DOM 对象，原生的 JavaScript 对象处理起来更快，而且更简单。DOM 树上的结构、属性信息我们都可以很容易地用 JavaScript 对象表示出来：

用 JavaScript 对象表示 DOM 信息和结构，当状态变更的时候，重新渲染这个 JavaScript 的对象结构。(当然这样做其实没什么卵用，因为真正的页面其实没有改变) 用新渲染的对象树去和旧的树进行对比，记录这两棵树差异。记录下来的不同就是我们需要对页面真正的 DOM 操作，然后把它们应用在真正的 DOM 树上，页面就变更了。这样就可以做到：视图的结构确实是整个全新渲染了，但是最后操作DOM的时候确实只变更有不同的地方。(**避免了整棵 DOM 树变更**)

步骤：

1. 用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中
2. 当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异
3. 把2所记录的差异应用到步骤1所构建的真正的DOM树上，视图就更新了

Virtual DOM 本质上就是在 JS 和 DOM 之间做了一个缓存。CPU（JS）只操作内存（Virtual DOM），最后的时候再把变更写入硬盘（DOM）。

### 新技术与虚拟DOM

react虚拟DOM深入https://juejin.im/post/5cb66fdaf265da0384128445#heading-10

他们的思想是**每次更新 dom 都尽量避免刷新整个页面，而是有针对性的去刷新那被更改的一部分，来释放掉被无效渲染占用的 gpu，cup 性能。**

#### angular

脏值检测查机制 所有使用了 ng 指令的 scope data 和 {{}} 语法的 scope data 都会被加入脏检测的队列

#### vue

vue 采用的是虚拟 dom 通过重写 setter ， getter 实现观察者监听 data 属性的变化生成新的虚拟 dom 通过 h 函数创建真实 dom 替换掉dom树上对应的旧 dom。

#### react**

react 也是通过虚拟 dom 和 setState 更改 data 生成新的虚拟 dom 以及 **diff 算法来计算和生成需要替换的 dom 做到局部更新的。**

**无虚拟 DOM**

1. state（数据）
2. JSX（模板）
3. state + JSX 通过 JS 生成真实的 DOM
4. state 改变
5. state + JSX 通过 JS 生成真实的 DOM
6. 新生成的 DOM 节点替换旧 DOM 节点

**有虚拟 DOM**

1. state（数据）
2. JSX（模板）
3. state + JSX 通过 react 内部机制生成虚拟 DOM（虚拟 DOM 就是一个 JS 对象）
4. state + JSX 通过 JS 生成真实的 DOM
5. state 改变
6. state + JSX 通过 react 内部机制生成虚拟 DOM
7. 新生成的虚拟 DOM 与旧虚拟 DOM 做比较（JS 对象之间的比较）
8. 对有差异的 DOM 节点进行操作

**性能分析：**生成虚拟 DOM 所消耗的性能远小于生成真实 DOM，虚拟 DOM 的比较远小于真实 DOM 的比较，对指定 DOM 的内容进行替换做消耗的行能小于对 DOM 进行替换。

#### React Diff 算法

**传统diff算法的问题**

传统的diff算法是使用循环递归对节点进行依次对比，复杂度为O(n^3),效率低下。

**React diff算法策略**

- 针对树结构(tree diff)：对UI层的DOM节点跨层级的操作进行忽略。（数量少）
- 针对组件结构(component diff)：拥有相同*类*的两个组件生成相似的树形结构，拥有不同*类*的两个组件会生成不同的属性结构。
- 针对元素结构(element-diff): 对于同一层级的一组节点，使用具有*唯一性*的id区分 (key属性)

**tree diff（树形结构） 的特点**

- React 通过使用 updateDepth 对 虚拟DOM树进行层次遍历
- 两棵树只对同一层级节点进行比较，只要该节点不存在了，那么该节点与其所有子节点会被*完全删除*,不在进行进一步比较。
- 只需要遍历一次，便完成对整个DOM树的比较。

**component diff的特点**

- 同一类型的组件，按照原策略(tree diff)比较 virtual DOM tree
- 同类型组件，组件A转化为了组件B，如果virtual DOM 无变化，可以通过`shouldComponentUpdate()`方法来判断是否

**element diff特点**

对于处于同一层级的节点，React diff 提供了三种节点操作: 插入，移动，删除

- 插入： 新的组件不在原来的集合中，而是全新的节点，则对集合进行插入操作。
- 删除：组件已经在集合中，但集合已经更新，此时节点就需要删除。
- 移动：组件*已经存在于*集合中，并且集合更新时，组件并没有发生更新，只是位置发生改变，例如：(A,B,C,D) → (A,D,B,C), 如果为***传统 diff***  则会在检测到旧集合中第二位为B，新集合第二位为D时删除B，插入D，并且后面的所有节点都要重新加载，而 **React diff** 则是通过向同一层的节点添加 *唯一key* 进行区分，并且移动。

### React 渲染优化：diff 与 shouldComponentUpdate

https://ssshooter.com/2019-03-15-react-render/

## react传值方式

https://juejin.im/post/5cbea2535188250a52246717

https://www.jianshu.com/p/77467c15a0ce

## 受控组件和非受控组件*

https://juejin.cn/post/6858276396968951822

在`React`中定义了一个`input`输入框的话，它并没有类似于`Vue`里`v-model`的这种双向绑定功能。<u>并没有一个指令能够将数据和输入框结合起来，用户在输入框中输入内容，然后数据同步更新。</u>

在HTML的表单元素中，它们通常自己维护一套`state`，并随着用户的输入自己进行`UI`上的更新，<u>这种行为是不被我们程序所管控的</u>。

如果将`React`里的<u>`state`属性和表单元素的值建立依赖关系</u>，再通过`onChange`事件与`setState()`结合更新`state`属性，就能达到控制用户输入过程中表单发生的操作。被`React`以这种方式控制取值的表单输入元素就叫做**受控组件**。

## react-router

如何工作？如果输入 '/a/b/c/index.html'应该怎么处理

## 项目中的难点

### 1.async和await在循环中使用方式的问题

https://juejin.im/post/5cf7042df265da1ba647d9d1

当使用`await`时，希望JavaScript暂停执行，直到等待 promise 返回处理结果。for`循环中的`await` 应该按顺序执行。

但是它不能处理需要回调的循环，如`forEach`、`map`、`filter`和`reduce`    不能在 `forEach` 使用 `await`。

如果在`map`中使用`await`, `map` 始终返回`promise`数组，这是因为异步函数总是返回`promise`必须等待`promises` 数组得到处理。 或者通过`await Promise.all(arrayOfPromises)`来完成此操作。

 const numFruits = await Promise.all(promises);

**总结：**

如果你想连续执行`await`调用，请使用`for`循环(或任何没有回调的循环)。

永远不要和`forEach`一起使用`await`，而是使用`for`循环(或任何没有回调的循环)。

不要在 `filter` 和 `reduce` 中使用 `await`，如果需要，先用 `map` 进一步骤处理，然后在使用 `filter` 和 `reduce` 进行处理。

## **2.state与不可变对象的问题

**数组不能用push (需要能返回新数组的方法)**

**setState**

设置状态的子集。始终使用它来改变状态。您应该将此this.state视为不可变的。

无法保证this.state将立即更新，因此在调用此方法后访问this.state可能会返回旧值。

无法保证对setState的调用将同步运行，因为它们最终可能会一起批处理。您可以提供一个可选的回调，该回调将在setState的调用实际完成时执行。

https://juejin.im/post/599b8f066fb9a0247637d61b#heading-3

https://www.jianshu.com/p/1e7e956ec1ee

**问题：**

数组不能用push (需要能返回新数组的方法)

state https://zhuanlan.zhihu.com/p/128455114

setState 做的事情不仅仅只是修改了 `this.state` 的值，另外最重要的是它会触发 React 的更新机制，会进行 diff ，然后将 patch 部分更新到真实 dom 里。

如果你直接 `this.state.xx == oo` 的话，state 的值确实会改，但是改了不会触发 UI 的更新，那就不是数据驱动了。

在 React 的 setState 函数实现中，会根据 isBatchingUpdates(默认是 false) 变量判断是否直接更新 this.state 还是放到队列中稍后更新。然后有一个 batchedUpdate 函数，可以修改 isBatchingUpdates 为 true，当 React 调用事件处理函数之前，或者生命周期函数之前就会调用 batchedUpdate 函数，这样的话，setState 就不会同步更新 this.state，而是放到更新队列里面后续更新。

这样你就可以理解为什么原生事件和 setTimeout/setinterval 里面调用 this.state 会同步更新了吧，因为通过这些函数调用的 React 没办法去调用 batchedUpdate 函数将 isBatchingUpdates 设置为 true，那么这个时候 setState 的时候默认就是 false，那么就会同步更新。

### **3.项目优化

性能优化

​	分类、缓存

1. JS合并数组对象中重复数据
   在项目中，将订单按照sellerId分类，再调用接口请求信息。来减少http请求数，优化性能

```js
// 转换格式->{sellerId,orders}
    const oldOrders =filterOrders.map((e,index)=> {
      return{
       sellerId:e.sellerId,
        orders:[{
          ...e,
        }],
      }
    });

    // 合并相同sellerId的orders,以达到分类目的
    const orderData=[];
    const newOrderInfo={};
    oldOrders.map((e,index)=>{
      if (!newOrderInfo[e.sellerId]) {
        orderData.push(e);
        newOrderInfo[e.sellerId] = true;
      }else{
        orderData.forEach(e => {
          if (e.sellerId === oldOrders[index].sellerId) {
            e.orders = e.orders.concat(oldOrders[index].orders);
            // e.orders = [...el.orders, ...oldOrders[i].orders]; 
          }
       })}
  });
```

另一个合并数组对象中重复数据的示例： https://blog.csdn.net/qq_36242361/article/details/83655364

```js
var oldData = [
  {
    city_id: 1,
    city_name: '北京',
    city_img: "http://dfknbdjknvkjsfnvlkjdn.png",
    city_country: "中国"
  },
  {
    city_id: 2,
    city_name: '上海',
    city_img: "http://wergerbe.png",
    city_country: "中国"
  },
  {
    city_id: 3,
    city_name: '广州',
    city_img: "http://hrthhr.png",
    city_country: "中国"
  },
  {
    city_id: 4,
    city_name: '西雅图',
    city_img: "http://frevfd.png",
    city_country: "美国"
  },
  {
    city_id: 5,
    city_name: '纽约',
    city_img: "http://反而个.png",
    city_country: "美国"
  }
]
 
// 把源数据先变成目标数据的规则
var oldDataRule = []
oldData.forEach(el => {
  var oldObj = {
    name: el.city_country,
    citys:[]
  }
  var cityObj = {
    city_name: el.city_name,
    city_img: el.city_img,
    city_id: el.city_id
  }
  oldObj.citys.push(cityObj)
  oldDataRule.push(oldObj)
})
 
/**
 * 先去重，后合并
 * 1、源数据去重
 * 2、把去重后的数据和源数据中相同name的数据合并citys
*/0
var newData = []
var newObj = {}
oldDataRule.forEach((el, i) => {
  if (!newObj[el.name]) {
    newData.push(el);
    newObj[el.name] = true;
  } else {
    newData.forEach(el => {
      if (el.name === oldDataRule[i].name) {
        el.citys = el.citys.concat(oldDataRule[i].citys);
        // el.citys = [...el.citys, ...oldDataRule[i].citys]; // es6语法
      }
    })
  }
})
console.log(newData); // 目标数据
```

2. 从缓存中查找，没有的话再去请求
3. **图片压缩**

ReactJS antd 环境中项目上传图片后压缩（lrz的使用）[localResizeIMG](https://github.com/think2011/localResizeIMG) 

**用于**：在客户端压缩好要上传的图片可以节省带宽更快的发送给后端，特别适合在移动设备上使用。

- 解决了很多问题：
  - 图片扭曲、某些设备不自动旋转图片方向，没有jpeg压缩算法..
  - 不支持new Blob,formData构造的文件size为0..
  - 还有某些机型和浏览器（例如QQX5浏览器）莫名其妙的BUG..
- 按需加载（会根据对应设备自动异步载入JS文件，节省不必要带宽）
- 原生JS编写，不依赖例如jquery等第三方库，支持AMD or CMD规范。

**基本格式**：

```
lrz(file, [options]);
```

解释：

```
file： 通过 input:file 得到的文件，或者直接传入图片路径。

[options] ：这个参数允许忽略。
    width {Number} 图片最大不超过的宽度，默认为原图宽度，高度不设时会适应宽度；
    height {Number} 同上；
    quality {Number} 图片压缩质量，取值 0 - 1，默认为0.7；
    fieldName {String} 后端接收的字段名，默认：file；

返回结果是一个promise对象，有then()、catch()、always三个方法。
```

**用法**：（在react中，配合antd-mobile的ImagePicker 图片选择器使用lrz压缩图片，压缩后的图片是base64格式）

1、在项目中安装lrz

```
npm install lrz 
```

2、在js文件中import lrz

```
import lrz from 'lrz';
```

3、项目中具体使用部分代码

```js
onImageChange01 = (files01, type, index) => {
        console.log(files01, type, index);
        if(type==='add'){
            lrz(files01[0].url, {quality:0.1})
                .then((rst)=>{
                    // 处理成功会执行
                    console.log('压缩成功')
                    console.log(rst.base64);
                    this.setState({
                        imagesrc01:rst.base64.split(',')[1],
                    })
                })
        }else{
            this.setState({imagesrc01:''})
        }
        this.setState({
            files01,
        });
    }
 <div className="ImageFlex">
     <div className="ImageTitle"> 身份证正面照片：</div>
     <p className="ImageTip"> 支持jpg,png,gif,bmp,psd,tiff等图片格式</p>
     <ImagePicker
        files={files01}
        onChange={this.onImageChange01}
        onImageClick={(index, fs) => console.log(index, fs)}
        selectable={files01.length < 1}
        multiple={this.state.multiple}
    />
 </div>
```

4.组件，图片懒加载

https://juejin.im/post/5c32d40951882524b333994d

**组件懒加载：**

```
// lazy 接受一个函数作为参数
const MyComponent = React.lazy(() => import('path/MyComponent'));

// 展示时才加载（可以自定义一些条件）
const App = () => (
  <div>
    <MyComponent />
  </div>
)
```

5.**函数防抖**

如果在滚动过程中不断触发遍历并判断图片是否在可视区的监听事件，会耗费很大的性能，这里采用函数防抖：当用户停止滚动时统一遍历判断图片位置

```
debounce(fn) {
    // 函数防抖：用户停止操作之后触发
    clearTimeout(this.timer);
    this.timer = setTimeout(() => {
        fn();
    }, 1000);
}
```

我们可以将加载图片的方法放在debounce中

```
lazyLoad() {
    this.debounce(this.loadImg);
}
```

这样当用户滚动页面时，松开手才会执行`loadImg`来遍历判断图片位置。

又出现了一个问题：如果用户在滚动时从页面底部上拉到顶部一直没有松手，那么在这期间都不会执行`loadImg`，这意味着页面的图片都不会显示，非常影响用户体验

**防抖优化**

我们规定，若用户上拉高度大于500px那么就自动加载一次可视区内图片，这里我们用oldScrollTop记录上次上拉高度

```
lazyLoad() {
    // 如果上拉距离大于500px则自动加载
    if(this.$refs.lazy.scrollTop - this.oldScrollTop > 500) {
        this.loadImg();
        this.oldScrollTop = this.$refs.lazy.scrollTop; // 更新oldScrollTop
    } else {  // 如果向下拉但小于500px则防抖加载
        this.debounce(this.loadImg);
    }
}
```

**下拉优化**

当用户下拉的时候我们并不需要执行`lazyLoad`，因为我们之前的图片已经加载过了，所以可以修改一下`lazyLoad`

```
lazyLoad() {
    // 如果上拉距离大于500px则自动加载
    if(this.$refs.lazy.scrollTop - this.oldScrollTop > 500) {
        this.loadImg();
        this.oldScrollTop = this.$refs.lazy.scrollTop;
    } else if(this.$refs.lazy.scrollTop - this.oldScrollTop < 0) {  // 如果向下拉则不做操作
        return ;
    } else {  // 如果向下拉但小于500px则防抖加载
        this.debounce(this.loadImg);
    }
}
```

**减少遍历个数**

最重要的优化已经做完了，但是还可以从一些小细节更加优化一下，我们的`loadImg`方法中每次都是从0号下标开始遍历检查图片，但是在用户上拉操作之后一部分图片已经被加载了，就不需要再次去检查了。

我们可以用一个变量`len`记录上一次被加载后的最后一个图片，然后修改一下`loadImg`

```js
 loadImg() {
    var img = this.getImages(); 
    var top = this.$refs.lazy.scrollTop + window.screen.height;
    
    // 从len开始检查
    for(var i = this.len; i < img.length; i++) {
        if(img[i].offsetTop <= top) {
            img[i].src = img[i].getAttribute("datasrc");
            this.len = i;  // 更新len
        }
    }
}
```

### 

## VUE

快速入门https://www.cnblogs.com/keepfool/p/5619070.html

使用Vue的过程就是定义MVVM各个组成部分的过程的过程。

1. **定义View**
2. **定义Model**
3. **创建一个Vue实例或"ViewModel"，它用于连接View和Model**

MVVM模式本身是实现了双向绑定的，在Vue.js中可以使用`v-model`指令在表单元素上创建双向数据绑定。

```
<!--这是我们的View-->
<div id="app">
	<p>{{ message }}</p>
	<input type="text" v-model="message"/>
</div>
```

将message绑定到文本框，当更改文本框的值时，`<p>{{ message }}</p>` 中的内容也会被更新。

### vue特点

**1) 轻量级的框架**

Vue.js 能够自动追踪依赖的模板表达式和计算属性，提供 MVVM 数据绑定和一个可组合的组件系统，具有简单、灵活的 API，更易理解，能够更快上手。

**2) 双向数据绑定**

声明式渲染是数据双向绑定的主要体现，同样也是 Vue.js 的核心，它允许采用简洁的模板语法将数据声明式渲染整合进 DOM。

**3) 指令**

Vue.js 与页面进行交互，主要就是通过内置指令来完成的，指令的作用是当其表达式的值改变时相应地将某些行为应用到 DOM 上。

**4) 组件化**

Vue.js 最强大的功能之一。组件可以扩展 HTML 元素，封装可重用的代码。

在 Vue 中，父子组件通过 props 传递通信，从父向子单向传递。子组件与父组件通信，通过触发事件通知父组件改变数据。这样就形成了一个基本的父子通信模式。

在开发中组件和 HTML、JavaScript 等有非常紧密的关系时，可以根据实际的需要自定义组件，使开发变得更加便利，可大量减少代码编写量。

组件还支持热重载（hotreload）。当我们做了修改时，不会刷新页面，只是对组件本身进行立刻重载，不会影响整个应用当前的状态。CSS 也支持热重载。

**5) 客户端路由**

Vue-router 是 Vue.js 官方的路由插件，与 Vue.js 深度集成，用于构建单页面应用。Vue 单页面应用是基于路由和组件的，路由用于设定访问路径，并将路径和组件映射起来，传统的页面是通过超链接实现页面的切换和跳转的。

**6) 状态管理**

状态管理实际就是一个单向的数据流，State 驱动 View 的渲染，而用户对 View 进行操作产生 Action，使 State 产生变化，从而使 View 重新渲染，形成一个单独的组件。



## react与vue比较

https://juejin.cn/post/6844903974437388295

http://caibaojian.com/vue-vs-react.html

React与Vue最大的不同是模板的编写。Vue鼓励你去写近似常规HTML的模板。写起来很接近标准HTML元素，只是多了一些属性。

Vue鼓励你去使用HTML模板去进行渲染

React推荐你所有的模板通用JavaScript的语法扩展——JSX书写



在React中你需要使用`setState()`方法去更新状态。在Vue中，state对象并不是必须的，数据由data属性在Vue对象中进行管理。

## 前端新动态

https://cloud.tencent.com/developer/article/1589825

### vue3.0新特性

https://juejin.im/post/5e71d5f751882549003d3900#heading-13

1，压缩包体积更小

当前最小化并被压缩的 Vue 运行时大小约为 20kB（2.6.10 版为 22.8kB）。Vue 3.0捆绑包的大小大约会减少一半，即只有10kB！

2.使用typescript重写Virtual DOM

我们知道，vue是使用Vitrual DOM内部渲染机制，在vue3.0中，Vitrual DOM进行了一次完全的重构，结合了模板编译等小技巧来提高性能，使得初始渲染的提速最高可以达到翻倍。

### es2016,17,18,19,20

### taro3  2020.7

https://aotu.io/notes/2020/06/30/taro-3-0-0/index.html

1. 在 Taro 3 中内置了 React、Nerv、Vue 2、Vue 3 四种框架的支持，开发者可以通过 `taro init` 命令创建对应的模板，并编写与框架本身生命周期、调用方法合乎一致的代码（idiomatic code），在语法上也没有任何的限制。

2. 早在 [Taro 1.2 发布](https://aotu.io/notes/2018/12/17/taro-1-2/) 时，我们就提供微信小程序转 Taro 的功能，转换后的微信小程序应用会变成一个多端应用。现在这个功能也完全兼容 Taro Next 的新架构，并且转换后的代码提供 React 和 Vue 两种选项。和以前一样，只需要在微信小程序项目根目录执行命令 `taro convert` ：

```
	$ taro convert
```

​		选择想要转换后的框架即可：

3. 预渲染

   在 Taro CLI 将页面初始化的状态直接渲染为无状态的 wxml，在框架和业务逻辑运行之前执行渲染流程。我们将这一技术称之为预渲染（Prerender），经过 Prerender 的页面初始渲染速度通常会和原生小程序一致甚至更快。