# react与vue比较

https://juejin.cn/post/6844903974437388295 vue,react

https://segmentfault.com/a/1190000038518135

https://zhuanlan.zhihu.com/p/359540593  angular,vue,react

http://caibaojian.com/vue-vs-react.html

https://www.cnblogs.com/pengfei-nie/p/9087844.html ，https://blog.csdn.net/weixin_44369568/article/details/89816490   (工作中更接地气的回答)

### 1.相似之处

- 两者都是用于创建UI的JavaScript库；
- 两者都快速轻便(专注于创造前端的富应用。不同于早期的JavaScript框架“功能齐全”，)
- 都有基于组件的架构；
- 都是用虚拟DOM；
- 都可放入单个HTML文件中，或者成为更复杂webpack设置中的模块；
- 都有独立但常用的路由器和状态管理库(Reat与Vue只有框架的骨架，其他的功能如路由、状态管理等是框架分离的组件。)

它们之间的最大区别是Vue通常使用HTML模板文件，而React则完全是JavaScript。Vue有双向绑定语法糖。

### 2.**区别：**

最大的不同是模板的编写。Vue鼓励你去写近似常规HTML的模板。写起来很接近标准HTML元素，只是多了一些属性。React推荐你所有的模板通用JavaScript的语法扩展——JSX书写

React默认是通过比较引用的方式（diff）进行的，如果不优化可能导致大量不必要的VDOM的重新渲染。为什么React不精确监听数据变化呢？这是因为Vue和React设计理念上的区别，Vue使用的是可变数据，而React更强调数据的不可变，两者没有好坏之分，Vue更加简单，而React构建大型应用的时候更加鲁棒。

1. 监听数据变化的实现原理不同

   在React中你需要使用`setState()`方法去更新状态。在Vue中，state对象并不是必须的，数据由data属性在Vue对象中进行管理。

   Vue通过 getter/setter以及一些函数的劫持，能精确知道数据变化。

2. 数据流不同

   Vue1.0中可以实现两种双向绑定：父子组件之间，props可以双向绑定；组件与DOM之间可以通过v-model双向绑定。Vue2.x中去掉了第一种，也就是父子组件之间不能双向绑定了（但是提供了一个语法糖自动帮你通过事件的方式修改），并且Vue2.x已经不鼓励组件对自己的 props进行任何修改了。

   React一直不支持双向绑定，提倡的是单向数据流，称之为onChange/setState()模式。不过由于我们一般都会用Vuex以及Redux等单向数据流的状态管理框架。

3. 模板渲染方式不同

   在表层上，模板的语法不同，React是通过JSX渲染模板。而Vue是通过一种拓展的HTML语法进行渲染，但其实这只是表面现象，毕竟React并不必须依赖JSX。

   在深层上，模板的原理不同，这才是他们的本质区别：React是在组件JS代码中，通过原生JS实现模板中的常见语法，比如插值，条件，循环等，都是通过JS语法实现的，更加纯粹更加原生。而Vue是在和组件JS代码分离的单独的模板中，通过指令来实现的，比如条件语句就需要 v-if 来实现对这一点，这样的做法显得有些独特，会把HTML弄得很乱。

4. ##### 框架本质不同

   Vue本质是MVVM框架，由MVC发展而来；

   React是前端组件化框架，由后端组件化发展而来。

### 3.各自的优点

**React**

- 通过模块化的结构使其拥有灵活的代码，节省时间和成本。
- 助力复杂应用程序的高性能的实现。
- 使用 React 前端开发能够更容易去做代码维护。
- 支持适用于 Android 和 iOS 平台的移动端原生应用程序。

**Vue**

- 它的体积小巧，便于安装和下载。
- 倘若我们正确利用，我们就可以在多处重用 Vue。
- Vue.js 允许我们更新网页中的元素，而无需渲染整个 DOM，因为它是虚拟的 DOM。
- 需要较少的优化。
- 加速 Web 应用程序的开发，并允许大佬将模板到虚拟 DOM 与编译器分开。
- 经过验证的兼容性和灵活性。
- 不管应用程序的规模如何，代码库都不会变。

### 4.选型

- 如果你喜欢用模板搭建应用（或者有这个想法），请选择Vue。相比之下，React应用不使用模板，它要求开发者借助JSX在JavaScript中创建DOM。

- ##### 如果你喜欢简单和“能用就行”的东西，请选择Vue。典型的React代码是重度依赖于JSX和诸如class之类的ES6特性的。vue双向绑定

- ##### 如果你想要一个同时适用于Web端和原生APP的框架，请选择React

  React Native是一个使用Javascript构建移动端原生应用程序（iOS，Android）的库。 它与React.js相同，只是不使用Web组件，而是使用原生组件。 如果你学过React.js，很快就能上手React Native，反之亦然。

- ##### 如果你打算构建一个大型应用程序，请选择React。基于模板的应用程序第一眼看上去更加好理解，而且能很快跑起来。但是这些好处引入的技术债会阻碍应用扩展到更大的规模。模板容易出现很难注意到的运行时错误，同时也很难去测试，重构和分解。相比之下，Javascript模板可以组织成具有很好的分解性和干（DRY）代码的组件，干代码的可重用性和可测试性更好。Vue也有组件系统和渲染函数，但是React的渲染系统可配置性更强，还有诸如浅（shallow）渲染的特性，和React的测试工具结合起来使用，使代码的可测试性和可维护性更好。与此同时，React的immutable应用状态可能写起来不够简洁，但它在大型应用中意义非凡，因为透明度和可测试性在大型项目中变得至关重要。

- ##### 如果你想要最大的生态系统，请选择React

  毫无疑问，React在NPM远高于Vue。这意味着更多的文章，教程和更多Stack Overflow的解答，还意味有着更多的工具和插件可以在项目中使用，让开发者不再孤立无援。

**vue和react分别适合哪种项目？？

# 对React的理解

https://zhuanlan.zhihu.com/p/24833815

\1.    React的第一要点就是组件。所有React代码基本上就是一个包含许多小组件在内的大组件。

\2.    HTML和JS代码写在一起。通过JSX的语法扩展（X代表XML）实现。

\3.    State用于组件保存、控制、修改自己的可变状态，组件之间通过Props进行交互。

\4.    组件API。render、setState、constructor……React还提供了一些按照特定次序触发的[生命周期函数](https://link.zhihu.com/?target=https%3A//facebook.github.io/react/docs/state-and-lifecycle.html)

\5.    组件类型。你也完全可以用函数声明只有render方法的无状态组件。

区分处理UI和数据逻辑的组件是一种很好的开发实践。

高阶组件函数可以传入一个组件并为其赋予更多功能。



## JSX

浏览器只能处理 JavaScript 对象，而不能读取常规 JavaScript 对象中的 JSX。所以为了使浏览器能够读取 JSX，首先，**需要用像 Babel 这样的 JSX 转换器将 JSX 文件转换为 JavaScript 对象**，然后再将其传给浏览器.

SX是React的核心组成部分，它使用XML标记的方式去直接声明界面，界面组件之间可以互相嵌套。可以理解为在JS中编写与XML类似的语言,一种定义带属性树结构（DOM结构）的语法，它的目的不是要在浏览器或者引擎中实现，它的目的是通过各种编译器将这些标记编译成标准的JS语言。

 

JSX是一种 JavaScript 的语法扩展。 推荐在 React 中使用 JSX 来描述用户界面。JSX 乍看起来可能比较像是模版语言，但事实上它完全是在 JavaScript 内部实现的。

简单来说，React 认为组件才是王道，而组件是和模板紧密关联的， JSX 这种语法，就是为了把HTML模板直接嵌入到JS代码里面，这样就做到了模板和组件关联，但是 JS 不支持这种包含 HTML 的语法，所以需要通过工具将 JSX 编译输出成 JS 代码才能使用。

在编译之后呢，JSX 其实会被转化为普通的 JavaScript 对象。这也就意味着，你其实可以在 if 或者 for 语句里使用 JSX，将它赋值给变量，当作参数传入，作为返回值都可以：



# react生命周期

https://juejin.im/post/5b6f1800f265da282d45a79a#heading-0

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/12/19/16f1e95830a01935?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**挂载阶段**

组件初始化阶段，将组件插入到DOM中，只发生一次

这个阶段的生命周期函数调用如下：

- constructor

- **getDerivedStateFromProps**

  取代之前的component**Will**Mount、component**Will**ReceiveProps和component**Will**Update

- ~~componentWillMount~~

- render

- componentDidMount



**更新阶段**

当组件的props改变了，或组件内部调用了setState或者forceUpdate发生

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

**卸载阶段**

- componentWillUnmount

组件被卸载或者销毁了就会调用，我们可以在这个函数里去清除一些定时器，取消网络请求，清理无效的DOM元素等垃圾清理工作。<u>注意不要在这个函数里去调用setState，因为组件不会重新渲染了</u>



补充：

![image-20210513133817448](https://tva1.sinaimg.cn/large/008i3skNly1gtyx88p293j612s0dcacf02.jpg)

# [React Hooks](https://segmentfault.com/a/1190000016950339)

https://www.ruanyifeng.com/blog/2019/09/react-hooks.html

https://zh-hans.reactjs.org/docs/hooks-intro.html

https://ahooks.js.org/zh-CN/hooks/dom/use-size/   ahooks

在不编写 class 的情况下(函数组件)使用 state 以及其他的 React 特性。

**hooks出现的原因** 

https://zh-hans.reactjs.org/docs/hooks-intro.html

- 在组件之间复用状态逻辑很难。

  <u>可以使用 Hook 从组件中提取状态逻辑，使得这些逻辑可以单独测试并复用。</u>**Hook 使你在无需修改组件结构的情况下复用状态逻辑。** 

- 复杂组件难以理解，每个生命周期中常常会包含一些不相关的逻辑。

  例如，组件常在 `componentDidMount` 和 `componentDidUpdate` 中获取数据。但是，同一个 `componentDidMount` 中可能也包含很多其它的逻辑，如设置事件监听，而之后需在 `componentWillUnmount` 中清除。

  相互关联且需要对照修改的代码被进行了拆分，完全不相关的代码在同一个方法中组合在一起。很容易产生 bug。 hook**将组件中相互关联的部分拆分成更小的函数（比如设置订阅或请求数据）**，而并非强制按照生命周期划分。

-  不用再去考虑class组件中的 this 的指向问题。在类组件中，你必须去理解 JavaScript 中 this 的工作方式。

hooks缺点： https://www.cnblogs.com/ranyonsue/p/14700528.html

1. 写函数组件时，必须清楚代码中`useEffect`和`useCallback`的“依赖项数组”的改变时机。有时候，你的useEffect依赖某个函数的不可变性，这个函数的不可变性又依赖于另一个函数的不可变性，这样便形成了一条依赖链。一旦这条依赖链的某个节点意外地被改变了，你的useEffect就被意外地触发了，如果你的useEffect是幂等的操作，可能带来的是性能层次的问题，如果是非幂等，那就糟糕了。

   

**怎么避免react hooks的常见问题**

1. 不要在`useEffect`里面写太多的依赖项，划分这些依赖项成多个单一功能的`useEffect`。其实这点是遵循了软件设计的“单一职责模式”。
2. 如果你碰到状态不同步的问题，可以考虑下手动传递参数到函数。如：

```jsx
   // showCount的count来自父级作用域 
   const [count,setCount] = useState(xxx); 
   function showCount(){ console.log(count) } 

   // showCount的count来自参数 
   const [count,setCount] = useState(xxx); 
   function showCount(c){ console.log(c) }
```

但这个也只能解决一部分问题，很多时候你不得不使用上述的`useRef`方案。

\3. 重视`eslint-plugin-react-hooks`插件的警告。

\4. 复杂业务的时候，使用Component代替hooks。



hooks**模拟生命周期**

![image-20210830160043952](https://tva1.sinaimg.cn/large/008i3skNly1gtyvhbx84uj61d40hyq7f02.jpg)



**常用的hooks**

​	基本：useState, useEffect, useContext

​	额外：useCallback, useMemo, useRef

**useState**

使我们在函数组件中也能像类组件那样获取、改变state。你可以在事件处理函数中或其他一些地方调用这个函数。它类似 class 组件的 `this.setState`，但是它不会把新的 state 和旧的 state 进行合

**useEffect**

给函数组件增加了操作副作用的能力(数据获取、订阅或者手动修改过 DOM)。是在函数组件中实现像类组件中的生命周期那样某个阶段做某件事情(主要是`componentDidMount`，`componentDidUpdate` 和 `componentWillUnmount`)

默认情况下，React 会在每次渲染后调用副作用函数 —— **包括**第一次渲染的时候。

数组作为 `useEffect` 的第二个可选参数, 可以使React **跳过**对 effect 的调用， 仅当数组中的变量改变时才调用

```
useEffect(() => { getStuInfo({ id: stuId }); }, [getStuInfo, stuId]); //依赖项改变时调用getStuInfo函数
```

## useLayoutEffect

https://zhuanlan.zhihu.com/p/348701319

`useEffect` 是渲染完之后异步执行的，所以会导致 hello world 先被渲染到了屏幕上，再变成 world hello，就会出现闪烁现象。 `useLayoutEffect` 是<u>渲染之前同步执行</u>的，所以会等它执行完再渲染上去，就避免了闪烁现象。也就是说我们最好把操作 dom 的相关操作放到 `useLayouteEffect` 中去，避免导致闪烁。

总结

1. 优先使用 `useEffect`，因为它是异步执行的，不会阻塞渲染
2. 会影响到渲染的操作尽量放到 `useLayoutEffect`中去，避免出现闪烁问题
3. `useLayoutEffect`和`componentDidMount`，调用时机一致，会同步调用，阻塞渲染
4. 在服务端渲染的时候使用会有一个 warning，因为它可能导致首屏实际内容和服务端渲染出来的内容不一致。



**useContext****

```js
const value = useContext(MyContext); // useContext 的参数必须是 context 对象本身：
```

接收一个 context 对象并返回该 context 的当前值。当前的 context 值由上层组件中距离当前组件最近的 `<MyContext.Provider>` 的 `value` prop 决定。

离他最近的 `<MyContext.Provider>` 更新时，该 Hook 会触发重渲染，并使用最新传递给 `MyContext` provider 的 context `value` 值。即使祖先使用 [`React.memo`](https://zh-hans.reactjs.org/docs/react-api.html#reactmemo) 或 [`shouldComponentUpdate`](https://zh-hans.reactjs.org/docs/react-component.html#shouldcomponentupdate)，也会在组件本身使用 `useContext` 时重新渲染。

**useCallback 和 useMemo**

都是性能优化的手段，区别是
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

**useRef**

返回一个可变的 ref 对象。.current属性访问组件



# 虚拟dom

https://juejin.im/entry/5aedcfa351882506a36c664c

相对于操作 DOM 对象，原生的 JavaScript 对象处理起来更快，而且更简单。

用 JavaScript 对象表示 DOM 结构和属性信息，当状态变更的时候，重新渲染这个 JavaScript 的对象结构(真正的页面此时并没有改变) 。用新渲染的对象树去和旧的树进行对比，记录这两棵树差异。记录下来的不同就是我们需要对页面真正的 DOM 操作，然后把它们应用在真正的 DOM 树上，页面就变更了。

这样就可以做到：最后操作DOM的时候只变更有不同的地方。(**避免了整棵 DOM 树变更**)

步骤：

1. 用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中
2. 当状态变更的时候，重新构造一棵新的对象树。比较新树和旧树的差异
3. 把差异应用真正的DOM树上，视图就更新了



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

## React Diffing 算法

https://www.cnblogs.com/forcheng/p/13246874.html

**传统diff算法的问题**

使用循环递归对节点进行依次对比，复杂度为O(n^3),效率低下。

为了优化 diff 算法，React 提出了两个假设：

1. 两个不同类型的元素会产生出不同的树
2. 开发者可以通过 `key` prop 来暗示哪些子元素在不同的渲染下能保持稳定

基于上述假设，**React diff算法策略**

- 针对树结构(tree diff)：对UI层的DOM节点跨层级的移动操作进行忽略。（因为这种操作的数量很少）
- 针对组件结构(component diff)：拥有相同*类*的两个组件生成相似的树形结构，拥有不同*类*的两个组件会生成不同的属性结构。
- 针对元素结构(element-diff): 对于同一层级的一组节点，它们可以用*唯一性*的id区分 (key属性)

**diff 具体优化**

基于上述三个策略，React 分别对以下三个部分进行了 diff 算法优化

- tree diff
- component diff
- element diff

1.**tree diff（树形结构） 的特点**

- React 通过使用 updateDepth 对 虚拟DOM树进行层次遍历
- 两棵树只对同一层级节点进行比较，只要该节点不存在了，那么该节点与其所有子节点会被*完全删除*,不在进行进一步比较。
- 只需要遍历一次，便完成对整个DOM树的比较。

如果发生跨级操作，React 不能复用已有节点，可能会导致 React 进行大量重新创建操作，这会影响性能。所以 React 官方推荐尽量避免跨层级的操作。

2.**component diff的特点**

- 同类型组件，首先使用 `shouldComponentUpdate()`方法判断是否需要进行比较，如果返回`true`，继续按照 React diff 策略比较组件的虚拟 DOM 树，否则不需要比较
- 不同类型的组件，判断为 dirty component，替换整个组件及其下的所有子节点

3.**element diff 特点**

对于处于同一层级的节点，React diff 提供了三种节点操作

- 插入： 新的组件不在原来的集合中，是全新的节点，则对集合进行插入操作。
- 删除：组件已经在集合中，但集合已经更新，此时节点就需要删除。
- 移动：组件并没有发生更新，只是位置发生改变，例如：(A,B,C,D) → (A,D,B,C), 传统 diff  会在检测到旧集合中第二位为B，新集合第二位为D时删除B，插入D，后面的所有节点都要重新加载， **React diff** 则是通过向同一层的节点添加 *唯一key* 进行区分，并且移动。

**key的作用 ** https://juejin.cn/post/6967626390380216334#heading-2

**当同一层级的某个节点添加了key属性，当它在当前层级的位置发生了变化后。react diff算法通过新旧节点比较后，如果发现了key值相同的新旧节点，就会执行移动操作（然后依然按原策略深入节点内部的差异对比更新），而不会执行原策略的删除旧节点，创建新节点的操作。 提高了React性能和渲染效率**



**新技术与虚拟DOM**

react虚拟DOM深入https://juejin.im/post/5cb66fdaf265da0384128445#heading-10

他们的思想是**每次更新 dom 都尽量避免刷新整个页面，而是有针对性的去刷新那被更改的一部分，来释放掉被无效渲染占用的 gpu，cup 性能。**

angular

脏值检测查机制 所有使用了 ng 指令的 scope data 和 {{}} 语法的 scope data 都会被加入脏检测的队列

vue

vue 采用的是虚拟 dom 通过重写 setter ， getter 实现观察者监听 data 属性的变化生成新的虚拟 dom 通过 h 函数创建真实 dom 替换掉dom树上对应的旧 dom。

react**

react 也是通过虚拟 dom 和 setState 更改 data 生成新的虚拟 dom 以及 **diff 算法来计算和生成需要替换的 dom 做到局部更新的。**

# 高阶组件

https://juejin.cn/post/6844903576997724167 (入门)

https://juejin.cn/post/6844904050236850184  (进阶)

组件->组件

# 函数式编程

强调的是如何通过函数的组合变换去解决问题，而不是我通过写什么样的语句去解决问题，当你的代码越来越多的时候，这种函数的拆分和组合就会产生出强大的力量。



# [shouldComponentUpdate减少重复渲染](https://segmentfault.com/a/1190000016494335)

组件state或props被更新时可以通过这个生命周期判断是否继续渲染。

它接受两个参数`nextProps`、`nextState`，返回一个布尔值。

若不在代码中声明该生命周期，react默认的处理是：

```js
  shouldComponentUpdate(nextProps, nextState) {
    return true;
  }
```

编写代码的时候可以通过该生命周期来优化渲染

```kotlin
    shouldComponentUpdate(nextProps, nextState) {
        if (this.props.name === nextProps.name) {
            return false;
        }
        return true;
    }
```

使用shouldComponentUpdate()以让React知道当前状态或属性的改变是否不影响组件的输出，默认返回ture，返回false时不会重写render，而且该方法并不会在初始化渲染或当使用forceUpdate()时被调用，我们要做的只是这样：

```stylus
shouldComponentUpdate(nextProps, nextState) {
  return nextState.someData !== this.state.someData
}
```

但是，state里的数据这么多，还有对象，还有复杂类型数据  React.PureComponent解决这个问题

### React.PureComponent

React.PureComponent 与 React.Component 几乎完全相同，但 React.PureComponent 通过props和state的浅对比来实现 shouldComponentUpate()。如果对象包含复杂的数据结构，它可能会因深层的数据不一致而产生错误的否定判断(表现为对象深层的数据已改变视图却没有更新）



# Redux

问题来了，React 中一个组件里面维护数据只需要 state 和 setState 就可以轻松搞定。假如多个组件都需要维护这一份数据怎么办呢？

没有redux时，1. 都传给公共父组件，再由父组件一层层传给需要的组件。   2. 消息订阅与发布的方式，需要该数据的组件去订阅

Redux 核心的部分 Store，Store 中管理的数据独立于 React 组件之外，如果 React 某个组件中的某个数据在某个时刻改变了（可以称之为状态改变了），就可以直接更改这个 Store 中管理的数据，这样其他组件想要拿到此时的数据直接拿就行了，不需要传来传去。

Store 通常要和 Reducer 来配合使用，Store 存数据，Reducer 是个纯函数，它接收并更新数据。

提供可预测化状态管理。State以对象树形式存在store中。通过触发action来改变state.用到函数reducer

![img](http://www.ruanyifeng.com/blogimg/asset/2016/bg2016091802.jpg)

## **dva**

https://segmentfault.com/a/1190000011523348

Redux  需要编写页面的action creator，需要写reducer来更新state，最好对action 和 reducer 做页面为单位的分割，利用redux 给的API 构建容器组件包裹父组件来connect store拿到数据，然后再向下传递给functional component 来渲染，整个过程就实现了单向数据流。当应用复杂起来，一般的做法是配合react-router 做页面分割，光这个分割，你就得 做redux store 的创建，中间件的配置，路由的初始化，Provider 的 store 的绑定，saga 的初始化，还要处理 reducer, component, saga之间的联系...

每个页面下要有自己对应的action、reducer，一般还会有saga，这样的话每个页面下都要有四五个文件目录（还有components、containers），每个文件目录下估计还要有不同功能的action、reducer、saga...如果这能忍的话，你在组件里发起action有两个方案，第一：调用经过**层层传递**的action creator 或者 sagas，第二，让saga监听action，再在组件里直接dispatch相应action类型就行了，不用层层传递，但是得提前 fork -> watcher -> worker.....真的是非常复杂，容易出错。

dva 自动化了Redux 架构一些繁琐的步骤，比如上面所说的redux store 的创建，中间件的配置，路由的初始化等等,没有什么魔法，帮你做了redux + react-router + redux-saga 架构的繁琐、容易出错的步骤，只需写几行代码就可以实现上述步骤，它解决了背景所说的所有缺点。此外，dva重要的特性就是把一个路由下的state、reducer、sagas 写到一块了，清晰明了

## react高频面试题

https://zhuanlan.zhihu.com/p/82840768

https://www.bbsmax.com/A/nAJvyxMmzr/

项目：https://juejin.im/post/5bca74cfe51d450e9163351b

### React 渲染优化：diff 与 shouldComponentUpdate

https://ssshooter.com/2019-03-15-react-render/

## react传值方式

https://juejin.im/post/5cbea2535188250a52246717

https://www.jianshu.com/p/77467c15a0ce

# 受控组件和非受控组件

https://juejin.cn/post/6858276396968951822

在`React`中定义了一个`input`输入框的话，它并没有类似于`Vue`里`v-model`的这种双向绑定功能。<u>并没有一个指令能够将数据和输入框结合起来，用户在输入框中输入内容，然后数据同步更新。</u>

在HTML的表单元素中，它们通常自己维护一套`state`，并随着用户的输入自己进行`UI`上的更新，<u>这种行为是不被我们程序所管控的</u>。

如果将`React`里的<u>`state`属性和表单元素的值建立依赖关系</u>，再通过`onChange`事件与`setState()`结合更新`state`属性，就能达到控制用户输入过程中表单发生的操作。被`React`以这种方式控制取值的表单输入元素就叫做**受控组件**。

## react-router

如何工作？如果输入 '/a/b/c/index.html'应该怎么处理



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