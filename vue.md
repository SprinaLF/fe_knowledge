## 入口文件

https://blog.csdn.net/qq_36145914/article/details/86497123

## SPA（单页面应用）和MPA（多页面应用）

https://www.jianshu.com/p/a02eb15d2d70

## vue通信、传值的多种方式

https://blog.csdn.net/qq_35430000/article/details/79291287

**一、通过路由带参数进行传值**

**①两个组件 A和B,A组件通过query把orderId传递给B组件（触发事件可以是点击事件、钩子函数等）**

```
this.$router.push({ path: '/conponentsB', query: { orderId: 123 } }) // 跳转到B
```

**②在B组件中获取A组件传递过来的参数**

```
this.$route.query.orderId
```

**二、通过设置 Session Storage缓存的形式进行传递**

①两个组件A和B，在A组件中设置缓存orderData

```
const orderData = { 'orderId': 123, 'price': 88 }
sessionStorage.setItem('缓存名称', JSON.stringify(orderData))
```

②B组件就可以获取在A中设置的缓存了

```js
const dataB = JSON.parse(sessionStorage.getItem('缓存名称')) // 此时 dataB 就是数据 orderData
```

可以百度下 Session Storage（程序退出销毁） 和 Local Storage（长期保存） 的区别。

**组件传值**：

1. 父->子： props

2. 子->父： 通过emit事件

​				this.$emit('事件名', '参数')

3.  **不同组件之间传值，通过eventBus（小项目少页面用eventBus，大项目多页面使用 vuex）**

​       eventBus.$emit 传递

​       eventBus.$on 接收

 4. vuex

    为什么使用vuex?

    vuex主要是是做数据交互，父子组件传值可以很容易办到，但兄弟组件间传值（兄弟组件下又有父子组件），或者大型spa单页面框架项目，页面多并且一层嵌套一层的传值，异常麻烦，用vuex来维护共有的状态或数据会显得得心应手。

​        维护公共状态或数据

​     步骤：新建store文件夹，分开维护actions mutations getters ---> 在store/index.js文件中新建vuex 的store实例 --->actions(给action注册事件处理函数。当这个函数被触发时候，将状态提交到mutations中处理)--->提交 mutations是更改Vuex状态的唯一合法方法--->getters获取最终状态信息--->在main.js中导入 store实例

