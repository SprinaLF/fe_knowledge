## 引用类型与基本数据类型

6种基本：string number Boolean undefined null  symbol

引用类型有这几种：object、Array、RegExp、Date、Function、特殊的基本包装类型(String、Number、Boolean)以及单体内置对象(Global、Math)。**ES6 新增：**set, map

let obj = {name: 'xiaoming'}; let obj2 = obj; obj2.name = "xiaohong"
 Obj.name = ??   //xiaohong

obj = null , obj2.name =??   //仍然为xiaohong   obj不指向这块堆区域了



### Map

对象的键必须是字符串。map的键可以是数字或其他数据类型

实现： 两个数组相对应

```js
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
m.get('Michael'); // 95

var m = new Map(); // 空Map
m.set('Adam', 67); // 添加新的key-value

m.has('Adam'); // 是否存在key 'Adam': true
m.get('Adam'); // 67
m.delete('Adam'); // 删除key 'Adam'
m.get('Adam'); // undefined
```

### Set

`Set`和`Map`类似，也是一组key的集合，不存储value。在`Set`中，没有重复的key。

```
var s1 = new Set(); // 空Set
var s2 = new Set([1, 2, 3]); // 含1, 2, 3
```

重复元素在`Set`中自动被过滤：

```
var s = new Set([1, 2, 3, 3, '3']);
s; // Set {1, 2, 3, "3"}
```

通过`add(key)`方法可以添加元素到`Set`中，可以重复添加，但不会有效果：

```
s.add(4);
s; // Set {1, 2, 3, 4}
s.add(4);
s; // 仍然是 Set {1, 2, 3, 4}
```

通过`delete(key)`方法可以删除元素：

```
var s = new Set([1, 2, 3]);
s; // Set {1, 2, 3}
s.delete(3);
s; // Set {1, 2}
```



## JS的内存空间

https://juejin.cn/post/6844903615300108302

https://juejin.im/post/6844903873992196110

分为栈(stack)、堆(heap)、池(一般也会归类为栈中)。

其中栈存放变量，堆存放复杂对象，池存放常量(也叫常量池)。

引用数据类型存储在堆内存中，因为引用数据类型占据空间大、大小不固定。 如果存储在栈中，将会影响程序运行的性能； <u>引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。</u> 当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体

![img](https://user-gold-cdn.xitu.io/2019/6/25/16b8c0b5752823f6?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



**栈内存和堆内存的优缺点**

JS中，基本数据类型变量大小固定，操作简单，所以把放入栈中存储。

引用类型变量大小不固定，**把它们分配给堆中，让他们申请空间时自己确定大小**，<u>这样把它们分开存储能够使得程序运行起来占用的内存最小。</u>

栈内存由于它的特点，所以它的系统效率较高。 堆内存需要分配空间和地址，还要把地址存到栈中，效率低。

**垃圾回收**

栈内存中变量一般在当前执行环境结束就会被销毁， 

堆内存中的变量只有在**所有对它的引用**都结束的时候才会被回收。

详细：https://juejin.im/post/6844903869525262349

### 闭包与堆内存

闭包中的变量并不保存中栈内存中，而是保存在堆内存中。 这也就解释了函数调用之后之后为什么闭包还能引用到函数内的变量。

函数 A 弹出调用栈后，函数 A 中的变量这时候是存储在堆上的，所以函数B依旧能引用到函数A中的变量。 现在的 JS 引擎可以通过逃逸分析辨别出哪些变量需要存储在堆上，哪些需要存储在栈上。

## 闭包

https://segmentfault.com/a/1190000021725949

变量作用域->      <u>有权访问另一个函数作用域中变量的**函数**</u>

函数 A 返回了一个函数 B，并且函数 B 中使用了函数 A 的变量，函数 B 就被称为闭包。

例：

```js
function fn(){
    let num=10
    let fun=function(){   //闭包    函数fun称为闭包
        console.log(num)
    }
    fun()
}
fn()
```

**闭包特点：**

第一，闭包是一个函数，而且存在于另一个函数当中
第二，闭包可以访问到父级函数的变量，且该变量不会销毁

**闭包作用**

```
一个在fn外部作用域可以读取函数内的局部变量，延申了变量的作用范围；
另一个就是让这些变量的值始终保持在内存中，不会在fn调用后被自动清除。
隐藏变量，避免全局污染
```

为什么会这样呢？原因就在于f1是f2的父函数，而f2被赋给了一个全局变量，这导致f2始终在内存中，而f2的存在依赖于f1，因此f1也始终在内存中，不会在调用结束后，被垃圾回收机制（garbage collection）回收。

闭包的**优点**是可以避免全局变量的污染，

**缺点**是闭包会常驻内存，不会被垃圾回收机制回收，可能造成内存泄露。

修改上例：

```js
function fn(){
    let num=10
    let fun=function(){   //闭包
        num++
        console.log(num)
    }
    return fun    ////不是调用是返回
    //或者直接return function(){ console.log(num) }
}
let f1=fn()
let f2=fn()
f1()   //11    闭包中的num
f1()   //12
f2()   //11
```

**原理**

（1）Function是引用类型：保存在堆中，变量f1,f2是保存在栈中； 

  （2）闭包：一个函数（产生新的作用域）定义的局部变量、子函数的作用域在函数内， 

​       但是一旦离开了这个函数，局部变量就无法访问，所有通过返回子函数到一个变量f1的方法，让 

​       f1指向堆中的函数作用域，这样可以使用局部变量. 

  (3)  过程： 

​    第一次f1()  :f1=Foo()中，先执行Foo(): i = 0,return值返回给f1 

   (f1指向子函数    **f1()=function(){.....},因为子函数没有**      **定义i，所以向上找到父函数定义的 i**:   )并执行子函数 输出i=0,再自加 i =1(覆盖了父函数Foo 的 i值);

   第二次f1() : 执行的是子函数 Function(){  ..},输出的是父函数 的 i=1,再自加 i =2; 

   第一次f2():同第一次f1(),不同的是 **f2指向堆中一个新的对象 function(){  ...},**所有此i非彼i

**注意点：**

（1）闭包会使得函数中的变量都被保存在内存中，**内存消耗很大，所以不能滥用闭包**。解决方法是，在退出函数之前，将不使用的局部变量全部删除。

（2）闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。

**为什么使用闭包时变量不会被垃圾回收机制收销毁？**

JS垃圾回收机制：
 JS规定在一个函数作用域内，程序执行完以后变量就会被销毁，这样可节省内存；**使用闭包时，按照作用域链的特点，闭包（函数）外面的变量不会被销毁，因为函数会一直被调用，所以一直存在，如果闭包使用过多会造成内存销毁。**

#### 闭包应用

防抖节流函数

实现3秒后打印所有li的内容

```js
let lis=document.querySelector('.nav').querySelectorAll('li')
for(let i=0;i<lis.length;i++){
    (function(i){    //立即执行函数
        setTimeout(function(){
            console.log(lis[i].innerHTML)   //三秒后同时打印对应内容
        },3000)
    })(i)
}
```

## 声明提前

1.变量

**声明统一提前，赋值原地不变**

let 不行

2.函数声明提前

**函数创建的三种写法：**

a.函数声明：function fun(a){console.log(a)};(只有它存在函数声明提前)

b.函数表达式：var fun = function(a){console.log(a)};

c.构造函数：var fun = new Function("a",console.log(a));

## 原型链

构造函数：初始化对象

构造函数原型prototype （每一个构造函数都有prototype属性）,它指向另一个对象，共享方法

每个实例都有一个\__proto__ 指向prototype,  去prototype查找某个方法

原型也是对象，因此它也有自己的原型。这样构成一个原型链。

![image-20200531220537262](C:\Users\sprina\AppData\Roaming\Typora\typora-user-images\image-20200531220537262.png)

ps: 对象实例也有一条constructor指向它的构造函数

可在原型链追加方法，实现对内置方法的扩展

更多https://zhuanlan.zhihu.com/p/87667349/

```
if(!(this instanceof Person)){ //避免使用者不小心讲Person当作普通函数执行
　　　　　　　　 throw new Error(''请使用 new Person"); //仿ES6 class 中的写法
}
```

### 继承

（组合继承）

![image-20210519173630513](/Users/wenxin/Library/Application Support/typora-user-images/image-20210519173630513.png)

定义父类

```javascript
function Animal (name) {
  this.name = name || 'Animal';
  this.sleep = function(){
    console.log(this.name + '正在睡觉！');
  }
}
Animal.prototype.eat = function(food) {
  console.log(this.name + '正在吃：' + food);
};
```

1. **原型链继承**
   **将父类的实例作为子类的原型( 子类.prototype=new 父类() )**

```javascript
function Cat(){ 
}
Cat.prototype = new Animal();   //无法实现多继承
Cat.prototype.name = 'cat';

//Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.eat('fish'));
console.log(cat.sleep());
console.log(cat instanceof Animal); //true 
console.log(cat instanceof Cat); //true
```

优点：

- 非常纯粹的继承关系，实例是子类的实例，也是父类的实例
- 父类新增原型方法/原型属性，子类都能访问到，很好的实现了方法的共享； 
- 简单
  **缺点**：
- 无法实现多继承 
- 来自原型对象的引用属性是所有实例共享的(一个实例的修改，其他也会发生变化)
- 创建子类实例时，无法向父类构造函数传参

2. **构造继承**
   **使用父类的构造函数来增强子类实例，等于是复制父类的实例属性给子类（父类.call(this)）**

```javascript
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
 
// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // false
console.log(cat instanceof Cat); // true
```

优点：

- 解决了1中 子类实例共享父类引用属性问题（不是prototype）
- 创建子类实例时，可以向父类传递参数
- 可实现多继承（call多个父类对象）
  缺点：
- 实例并不是父类的实例，只是子类的实例
- 只能继承父类的实例属性和方法，**不能继承父类原型对象上的属性/方法**
- **无法实现函数复用，每个子类都有父类实例函数的副本**，影响性能

3. **原型式继承**
   **为父类实例添加新特性，作为子类实例返回(子类=new 父类())**

```javascript
function Cat(name){
  var instance = new Animal();
  instance.name = name || 'Tom';
  return instance;
}
 
// Test Code
var cat = new Cat(); //或者去掉new
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); // false
```

优点：

- 不限制调用方式，new 子类()和 子类(),返回的对象有相同效果
  缺点：
- 实例是父类的实例，不是子类的实例
- 不支持多继承

核心：原型式继承的object方法本质上是对参数对象的一个浅复制。
优点：父类方法可以复用
缺点：父类的引用属性会被所有子类实例共享，子类构建实例时**不能向父类传递参数**

```
function Cat(o){
  function F(){}
  F.prototype = o;
  return new F();
}

let cat = object(person);
cat.name = "Bob";
anotherPerson.friends.push("Rob");
```

ECMAScript 5 通过新增 Object.create()方法规范化了原型式继承。这个方法接收两个参数:一 个用作新对象原型的对象和(可选的)一个为新对象定义额外属性的对象。在传入一个参数的情况下， Object.create()与 object()方法的行为相同。
所以上文中代码可以转变为  

var yetAnotherPerson = object(person); => let cat=Object.create(Animal);

4. **拷贝继承**
   **子类.prototype.属性=new 父类().属性**

```javascript
function Cat(name){
  var animal = new Animal();
  for(var p in animal){
    Cat.prototype[p] = animal[p];
  }
  Cat.prototype.name = name || 'Tom';
}
 
// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // false
console.log(cat instanceof Cat); // true
```

优点：

- 支持多继承
  缺点：
- 效率较低，内存占用高（要拷贝父类的属性）
- 无法获取父类不可枚举的方法，原型上的可以获取（不可枚举方法，不能使用for in 访问到:需手动设置enurable为false:Object.defineProperty()  和symbol() ）  Object.keys()不能获取原型上的   Object.getOwnPropertyNames()原型上无法遍历，自身可枚举和不可枚举都可以遍历

5. **组合继承**（原型，构造函数继承的组合）
   **通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用(父类.call(this),子类.prototype=new 父类())**

```javascript
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
Cat.prototype = new Animal();
 
// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); // true
```

优点:

- 可以继承实例属性/方法，也可以继承原型属性/方法（原型继承）
- 既是子类的实例，也是父类的实例（原型链）
- 不存在引用属性共享问题（构造函数继承）
- 可传参 （构造函数继承）
- 函数可复用（原型链继承）
  缺点：
  -调用了两次父类构造函数，生成了两份实例（子类实例将子类原型上的那份屏蔽了）

6. **寄生组合继承**
   **通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次实例方法/属性，避免的组合继承的缺点（父类.call(this).var Super=function(){},Super.prototype=父类.prototype,子类.prototype=new Super）**

```javascript
function Cat(name){
  Animal.call(this);    //实例的方法和属性
  this.name = name || 'Tom';
}
(function(){
  // 创建一个没有实例方法的类
  var Super = function(){};
  Super.prototype = Animal.prototype;
  //将实例作为子类的原型
  Cat.prototype = new Super();
})();
 
// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); //true
```

优点：

- 完美
  缺点：
- 复杂

7. **ES6继承**
   **class 子类 extends 父类{constructor(){super()}}**

```javascript
class Cat extends Animal {
  constructor(name,food){
    //调用父类的constructor(name)bixvd
    super(name);   //必须调用super方法
    this.food = food
  }
  eat(food){
    //调用父类的方法
    super.eat(food); 
  }
}

// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); //true
```

**class本质是构造函数。class的继承 extends本质为构造函数的原型链的继承**

js执行时，会将class转会为构造函数定义的类执行。所以 ES6 class的写法实质就是构造函数。

**另一个ES6继承的例子：**

```kotlin
class People{
    constructor(name,age){
        this.name = name;
        this.age = age;
    }
    sayName(){
        return '我的名字是：'+this.name;
    }
}

class har extends People{
    constructor(name,age,sex){
        super(name,age);  //调用父类的constructor(name,age)
        this.sex = sex;
    }
    haha(){
        return this.sex + ' ' + super.sayName();//调用父类的sayName() 
    }
}
```

**子类是没有自己的 `this` 对象的，它只能继承自父类的 `this` 对象，然后对其进行加工，而`super( )`就是将父类中的this对象继承给子类的。没有 `super`，子类就得不到 `this` 对象，！！！！**

### 继承手写题(类，面向对象思想)

1. 创建一个 Person 类，其包含公有属性 name 和私有属性 age 以及公有方法 setAge ；创建一个 Teacher 类，使其继承 Person ，并包含私有属性 studentCount 和私有方法 setStudentCount 。

没看懂公有属性和私有属性啥意思。我还问了JS没有公有私有的语法吧。然后就把共有属性那些都放`prototype`里了。被怼说这都是你那本红宝书里着重大篇幅讲过的，可见你面向对象用的就不熟练。

面试完查了一下，私有属性就是用闭包定义在构造函数里的局部变量。这几个名词整的真是玄，还有共享属性和静态属性。您稍微提醒一下闭包我不就会了嘛。参考代码：



```jsx
function Person(name) {
  var age   // 私有
  this.setAge = function(value) {
    age = value
  }
  this.getAge = function() {
    return age
  }
  this.name = name   // 公有
}

function Teacher(name) {
  Person.call(this, name)
  var studentCount
  this.setStudentCount = function(value) {
    studentCount = value
  }
  this.getStudentCount = function() {
    return studentCount
  }
}
Teacher.prototype = Object.create(Person.prototype)
Teacher.prototype.constructor = Teacher
```



## DOM

### 节点创建型api

**createElement**

创建出来后，要调用appendChild或insertBefore等方法将其添加到HTML文档树中。

**createTextNode**

创建一个文本节点：

```
var textNode = document.createTextNode("一个TextNode");
```

**createDocumentFragment**

DocumentFragment表示一种轻量级的文档，它的作用主要是存储临时的节点用来准备添加到文档中。
createDocumentFragment方法主要是用于添加大量节点到文档中时会使用到。

每次一创建一个新的元素，然后添加到文档树中，这个过程会造成浏览器的回流（重排）。如果添加的元素太多，会造成性能问题。这个时候，就是使用createDocumentFragment了。
<u>DocumentFragment不是文档树的一部分，它保存在内存中，不会造成回流问题</u>。

```js
//添加100个li
document.getElementById("btnAdd").onclick = function(){
	var list = document.getElementById("list");	
	var fragment = document.createDocumentFragment();

	for(var i = 0;i < 100; i++){
	  var li = document.createElement("li");
		li.textContent = i;
		fragment.appendChild(li);
	}
	list.appendChild(fragment);
}
```

**避免频繁DOM操作**，优化后的代码主要是创建了一个fragment，每次生成的li节点先添加到fragment，最后一次性添加到list



创建型api主要包括createElement，createTextNode，cloneNode和createDocumentFragment四个方法，需要注意下面几点：
（1）它们创建的节点只是一个孤立的节点，要通过appendChild添加到文档中
（2）cloneNode要注意如果被复制的节点是否包含子节点以及事件绑定等问题
（3）使用createDocumentFragment来解决添加大量节点时的性能问题

### 页面修改型API

appendChild，insertBefore，removeChild，replaceChild。

**appendChild**

将指定的节点添加到调用该方法的节点的子元素的末尾。


appendChild这个方法很简单，但是还有有一点需要注意：如果被添加的节点是一个页面中存在的节点，则执行后这个节点将会添加到指定位置，其原本所在的位置将移除该节点。child绑定了事件，被移动时，它依然绑定着该事件。

**insertBefore**

insertBefore用来添加一个节点到一个参照节点之前

parentNode表示新节点被添加后的父节点
newNode表示要添加的节点
refNode表示参照节点，新节点会添加到这个节点之前
我们来看这个[例子](http://runjs.cn/detail/p2rs1tmy)

```
<div id="parent">
    父节点
    <div id="child">				
        子元素
    </div>
</div>
<input type="button" id="insertNode" value="插入节点" />

var parent = document.getElementById("parent");
var child = document.getElementById("child");
document.getElementById("insertNode").onclick = function(){
	var newNode = document.createElement("div");
	newNode.textContent = "新节点"
	parent.insertBefore(newNode,child);
}
```

这段代码创建了一个新节点，然后添加到child节点之前。
和appendChild一样，如果插入的节点是页面上的节点，则会移动该节点到指定位置，并且保留其绑定的事件。

关于第二个参数参照节点还有几个注意的地方：
（1）refNode是必传的，如果不传该参数会报错
（2）如果refNode是undefined或null，则insertBefore会将节点添加到子元素的末尾

**removeChild**

**replaceChild**

replaceChild用于使用一个节点替换另一个节点，用法如下

```
parent.replaceChild(newChild,oldChild);
```

### 节点查询型API

**document.getElementById**

**document.getElementsByTagName**

**document.getElementsByClassName**

**document.querySelector和document.querySelectorAll**

<u>**document.querySelector**</u>   返回第一个匹配的元素，如果没有匹配的元素，则返回null。

**<u>querySelectorAll</u>**

### 节点关系型api

**父关系型**

parentNode：Element的父节点可能是Element，Document或DocumentFragment。
parentElement：返回元素的父元素节点，与parentNode的区别在于，其父节点必须是一个Element

**兄弟关系型**

previousSibling：有可能拿到的节点是文本节点或注释节点，与预期的不符，要进行处理一下。
previousElementSibling

nextSibling
nextElementSibling：返回后一个元素节点，后一个节点必须是Element

**子关系型**

```
.childNodes
```

childNodes：子节点可能会包含文本节点，注释节点等。
children：子节点都是Element
firstNode
lastNode
hasChildNodes方法：可以用来判断是否包含子节点。

### 元素属性型api

**setAttribute**

```
element.setAttribute("id","test");

element.id = "test";
```

**getAttribute**

### 元素样式型api

**window.getComputedStyle**

获取应用到元素后的样式，假设某个元素并未设置高度而是通过其内容将其高度撑开，这时候要获取它的高度就要用到getComputedStyle，用法如下：

```
var style = window.getComputedStyle(element[, pseudoElt]);
```

element是要获取的元素，pseudoElt指定一个伪元素进行匹配。
返回的style是一个CSSStyleDeclaration对象。
通过style可以访问到元素计算后的样式

**getBoundingClientRect**

getBoundingClientRect用来返回元素的大小以及相对于浏览器可视窗口的位置，用法如下：

```
var clientRect = element.getBoundingClientRect();
```

clientRect是一个DOMRect对象，包含left，top，right，bottom，它是相对于可视窗口的距离，滚动位置发生改变时，它们的值是会发生变化的。

## BOM

核心为`window`对象，它是BOM的顶层对象，表示在浏览器环境中的一个全局的顶级对象，所有在浏览器环境中使用的对象都是`window`对象的子对象。

### 1. 浏览器对象模型

`BOM`是指浏览器对象模型，它是对一系列在浏览器环境中使用对象的统称，这些对象提供了访问浏览器的功能。

在`BOM`对象中，`window`对象是最顶层对象，在浏览器环境中它是一个Global全局对象，其它对象（如：[DOM](http://itbilu.com/javascript/js/Vyxodm_1g.html)对象）对是这个对象的属性（子对象）。下面是`BOM`对象的组成结构：

![BOM－浏览器对象模型](https://itbilu.com/img/demo/bom.png)

### 2. `BOM`中的对象

**2.1 `window`对象**

`window`对象对象表示一个浏览器窗口或一个`frame`框架，它处于对象层次的最顶端，它提供了处理浏览器窗口的方法和属性。

`window`对象是浏览器对象中的默认对象，所以可以**隐式**地引用`window`对象的属性和方法。JavaScript中的标准内置对象，在浏览器环境中也是做为`window`的方法和属性出现的。

如，以下两行代码是等价的：

```
new Date();
// 等价于
new window.Date();
```

window.innerHeight - 浏览器窗口的内部高度(包括滚动条)

![视口宽高、位置与滚动高度](https://www.w3cplus.com/sites/default/files/blogs/2017/1711/window-scroll-4.png)

- window.open() - 打开新窗口
- window.close() - 关闭当前窗口
- window.moveTo() - 移动当前窗口
- window.resizeTo() - 调整当前窗口的尺寸

**2.2 `DOM`（`document`）相关对象**

`DOM`可以认为是`BOM`的一个子集，`DOM`中文档操作相关对象，如：[`Node`](http://itbilu.com/javascript/js/Ny3B0ddWg.html)、[`Document`](http://itbilu.com/javascript/js/N1PfJNgMe.html)、[`Element`](http://itbilu.com/javascript/js/VJYOZrWml.html)等`DOM`节点类型对象，都是做为`window`对象的子属性出现的。

HTML DOM 的 document 也是 window 对象的属性之一：

```
window.document.getElementById("header");
```

`document`是一个`Document`对象实例，表示当前窗口中文档对象。通过该对象，可以对文档和文档中元素、节点等进行操作。

**2.3 `frames`对象**

`frames`对象是一个集合，表示当前页面中使用的子框架。如果页面中使用了框架，将产生一个框架集合`frames`，在集合中可以用数字下标（从0开始）或名字索引框架。集全中的每一个对象，包含了框架的页面布局信息，以及每一个框架所对应的`window`对象。

**2.4 `navigator`对象**

`navigator`是指浏览器对象，提供了当前正在使用的浏览器的信息。`navigator`对象中的属性是只读的

```
navigator.userAgent      //浏览器用于 HTTP 请求的用户代理头的值。
		  appName        //浏览器名称
		  appVersion     //返回浏览器的平台和版本信息。
```

**2.5 `history`对象**

`history`对象来保存浏览器历史记录信息，也就是用户访问的页面。

- history.back() - 与在浏览器点击后退按钮相同
- history.forward() - 与在浏览器中点击向前按钮相同
- history.go(number|URL)   加载历史列表中的某个具体的页面。

**2.6 `location`对象**

用于获得当前页面的地址 (URL)，并把浏览器重定向到新的页面。

- location.hostname 设置或返回当前 URL 的主机名。
- **location.href**  返回（当前页面的）整个 URL
- location.pathname  设置或返回当前 URL 的路径部分。
- location.host 设置或返回主机名和当前 URL 的端口号。（80 或 443）
- location.protocol 返回所使用的 web 协议     http: 或 https：
- location.assign("https://www.runoob.com")  加载新文档
- location.search  可设置或返回当前 URL 的查询部分（问号 ? 之后的部分）

**2.7 `screen`对象**

用户显示器屏幕相关信息。显示器屏幕宽、高、色深等信息。

- screen.width / screen.height    屏幕宽度/高度

- screen.availWidth /screen.availHeight - 可用的屏幕宽度/高度  (不包括任务栏)
- screen.pixelDepth    色彩分辨率

## 判断array的方法

**1. instanceof（判断引用类型）**

判断某个构造函数的prototype属性所指向的對象是否存在于另外一个要检测对象的原型链上。 

```
object instanceof constructor
```

例子：

```
const a = [];
const b = {};
console.log(a instanceof Array);//true
console.log(a instanceof Object);//true,在数组的原型链上也能找到Object构造函数
console.log(b instanceof Array);//false
```

思考：

![image-20201123172535646](C:\Users\sprina\AppData\Roaming\Typora\typora-user-images\image-20201123172535646.png)

**2. constructor**

也可用于判断某个实例的构造函数p.constructor==Person

```
const a = [];
console.log(a.constructor);//function Array(){ [native code] }
```

例子：

```
const a = [];
console.log(a.constructor == Array);//true
const b = [];
//constructor属性被修改之后，无法判断了。
b.contrtuctor = Object;
console.log(b.constructor == Array);//false
console.log(b.constructor == Object);//true
```

**3.Object的toString方法**

再加上call或者apply方法来改变toString方法的执行上下文来判断，每一个继承自Object的对象都拥有toString的方法。如果一个对象的toString方法**没有被重写过的话**，那么toString方法将会返回"[object type]"。

```js
const a = ['Hello','Howard'];
const b = {0:'Hello',1:'Howard'};
const c = 'Hello Howard';
Object.prototype.toStrisng.call(a);//"[object Array]"
Object.prototype.toString.call(b);//"[object Object]"
Object.prototype.toString.call(c);//"[object String]"
Object.prototype.toString.apply(a);//"[object Array]"
Object.prototype.toString.apply(b);//"[object Object]"
Object.prototype.toString.apply(c);//"[object String]"
```

例子:

```js
const isArray = (something)=>{
    return Object.prototype.toString.call(something) === '[object Array]';
}
cosnt a = [];
const b = {};
isArray(a);//true
isArray(b);//false

//作死重写了toString方法
Object.prototype.toString = () => {
    alert('你吃过了么？');
}
//调用String方法
const a = [];
Object.prototype.toString.call(a);//弹框你吃过饭没有
```

补充： 数组的toString方法

https://www.jianshu.com/p/0c6a2427e47f

**4.用Array对象的isArray方法判断**

```
Array.isArray(a);//true
```

例子：

```
//重写Object.prototype.toString方法，不影响判断结果
Object.prototype.toString = ()=>{
    console.log('Hello Howard');
}
const a = [];
Array.isArray(a);//true

//修改constructor对象，不影响判断结果
const a = [];
const b = {};
a.constructor = b.constructor;
Array.isArray(a);//true

//Array.isArray是ES5标准中增加的方法，部分比较老的浏览器可能会有兼容问题，为了增强健壮性，给这个方法先进行判断
if (!Array.isArray) {
  Array.isArray = function(arg) {
    return Object.prototype.toString.call(arg) === '[object Array]';
  };
}
```

5.  a.\__proto__ === Array.prototype

### 其他：typeof运算符

typeof是javascript原生提供的判断数据类型的运算符， 

```
const a = null;  //object
console.log(typeof(a)); 
```

typeof并没有办法区分数组，对象，null等原型链上都有Object的数据类型。能区分"function"

typeof p=="symbol"

**typeof null == 'object'**

原理是这样的， 不同的对象在底层都表示为二进制， 在 JavaScript 中二进制前三位都为 0 的话会被判断为 object 类型， null 的二进制表示是全 0， 自然前三位也是 0， 所以执行 typeof 时会返回“object”。

其实这个是一个历史遗留的bug，在 javascript 的最初版本中，使用的 32 位系统，为了性能考虑使用低位存储了变量的类型信息：

·   000：对象

·   1：整数

·   010：浮点数

·   100：字符串

·   110：布尔

有 2 个值比较特殊：

·   undefined：用 - （−2^30）表示。

·   null：对应机器码的 NULL 指针，一般是全零。

所以当你使用typeof null时返回就是object了

```js
const myInstanceof = (target, origin) => {
    while (target) {
      if (target.__proto__ === origin.prototype) {
        return true
      }
      target = target.__proto__
    }
    return false
  }
```

### 

#### 返回新数组：slice   map   filter  concat

### forEach,map,filter,every,some区别

https://juejin.im/post/5ca96c76f265da24d5070563

forEach:

​	**当数组中元素是值类型，forEach绝对不会改变数组；当是引用类型，则可以改变数组**

## js 判断一个 object 对象是否为空

1.  for...in...遍历属性，为真则为“非空数组”；否则为“空数组”

```js
for (var i in obj) { 
    return true
}
return false 
```

2.通过 `JSON` 自带的 `stringify()` :

`JSON.stringify()` 方法用于将 `JavaScript` 值转换为 **`JSON` 字符串**。

```js
if (JSON.stringify(data) === '{}') {
    return false // 如果为空,返回false
}
return true
```

需要注意为什么不用 `toString()`，因为返回的不是我们需要的。

```js
var a = {}
a.toString() // "[object Object]"
```

3.`ES6` 新增的方法 `Object.keys()`:

`Object.keys()` 方法返回一个由一个给定对象的自身可枚举属性组成的数组。

如果我们的对象为空，他会返回一个空数组，如下：

```js
var a = {}
Object.keys(a) // []
```

判断它的长度来知道是否为空。

```js
Object.keys(object).length === 0
```

## 求数组的最大元素

```
//第一种：遍历数组每一项与当前最大值比较
Array.prototype.max = function(){
    let max = this[0];
    this.forEach((item,index) => {
    max = max > item ? max : item;
    })
    return max;
}
console.log(arr.max())

//第二种：数组的内置函数sort()排序，a-b从小到大，b-a从大到小
let arr1 = arr.sort((a,b)=>{
	return b-a;
})[0];
console.log(arr1)

//第三种：数组的内置函数reduce(),回调函数中两个必要的参数，累计器和当前值
let arr2 = arr.reduce((a,b)=>{
	return a > b ? a : b
})
console.log(arr2)

//第四种：内置函数Math.max(),支持传递多个参数，但不支持直接传递一个数组，只能借助apply()
let arr3 = Math.max.apply(null,arr)
console.log(arr3)

//第五种：ES6...扩展运算符
let arr3 = Math.max(...arr)
```

其中第一种方法标准循环运行速度最快，其次是第四种内置函数max方法；稍快一点的是内置的reduce函数方法; 最慢的是ES6扩展

## 常见手写

### reduce实现map；

```js
Array.prototype.myMap = function(fn,thisValue){
    var res = [];
    thisValue = thisValue||[];
    this.reduce(function(pre,cur,index,arr){
       return res.push(fn.call(thisValue,cur,index,arr));
    },[]);
    return res;
}

var arr = [2,3,1,5];
arr.myMap(function(item,index,arr){
    console.log(item,index,arr);
})
```

### 手写instanceof

**判断一个实例是否是其父类或者祖先类型的实例。**

**instanceof** <u>**在查找的过程中会遍历左边变量的原型链，直到找到右边变量的 prototype**</u>查找失败，返回 false

补充：instance和typeof实现原理

https://juejin.im/post/6844903613584654344

### 数组扁平化

数组扁平化就是把多维数组转化成一维数组

https://juejin.im/post/5c971ee16fb9a070ce31b64e#heading-3

**es6提供的新方法 flat(depth)**

```
let a = [1,[2,3]];  
a.flat(); // [1,2,3]  
a.flat(1); //[1,2,3]
```

其实还有一种更简单的办法，无需知道数组的维度，直接将目标数组变成1维数组。 depth的值设置为Infinity。

```
let a = [1,[2,3,[4,[5]]]];  
a.flat(Infinity); // [1,2,3,4,5]  a是4维数组
```

### 函数柯里化

函数式编程： 比起[指令式编程](https://zh.wikipedia.org/wiki/指令式編程)，函数式编程更加强调程序执行的结果而非执行的过程，倡导利用若干简单的执行单元让计算结果不断渐进，逐层推导复杂的运算，而不是设计一个复杂的执行过程。

在函数式编程中，函数是[第一类对象](https://zh.wikipedia.org/wiki/第一类对象)，意思是说一个函数，既可以作为其它函数的参数（输入值），也可以从函数中返回（输入值），被修改或者被分配给一个变量。

在数学和计算机科学中的柯里化函数，一次只能传递一个参数；

Javascript实际应用中的柯里化函数，可以传递一个或多个参数。对于柯里化的定义：<u>接收一部分参数，返回一个函数接收剩余参数，接收足够参数后，执行原函数。</u>

https://juejin.im/post/6844903882208837645

### 深拷贝和浅拷贝

https://www.jianshu.com/p/68e563c54f63

浅拷贝：只拷贝一层，更深层的对象级别的只拷贝引用

深拷贝：拷贝多层，**每一级别的数据都会拷贝**。这样更改拷贝值就不影响另外的对象

ES6浅拷贝方法：Object.assign(target,...sources)

```js
let obj={
    id:1,
    name:'Tom',
    msg:{
        age:18
    }
}
let o={}
//实现深拷贝  递归    可以用于生命游戏那个题对二维数组的拷贝，
//但比较麻烦，因为已知元素都是值，直接复制就行，无需判断
function deepCopy(newObj,oldObj){
    for(var k in oldObj){
        let item=oldObj[k]
        //判断是数组？对象？简单类型？
        if(item instanceof Array){
            newObj[k]=[]
            deepCopy(newObj[k],item)
        }else if(item instanceof Object){
            newObj[k]={}
            deepCopy(newObj[k],item)
        }else{  //简单数据类型，直接赋值
            newObj[k]=item
        }
    }
}
```

### 手写call, apply, bind

**手写call**

```js
Function.prototype.myCall=function(context=window){  // 函数的方法，所以写在Fuction原型对象上
    if(typeof this !=="function"){   // 这里if其实没必要，会自动抛出错误
        throw new Error("不是函数")
    }
    const obj=context||window   //这里可用ES6方法，为参数添加默认值，js严格模式全局作用域this为undefined
    obj.fn=this      //this为调用的上下文,this此处为函数，将这个函数作为obj的方法
    const arg=[...arguments].slice(1)   //第一个为obj,删除   伪数组转为数组******
    res=obj.fn(...arg)
    delete obj.fn   // 不删除会导致context属性越来越多
    return res
}

//用法：f.call(obj,arg1)
function f(a,b){
    console.log(a+b)
    console.log(this.name)
}
let obj={
    name:1
}
f.myCall(obj,1,2) //否则this指向window
```

obj.greet.call({name: 'Spike'})  //打出来的是 Spike

**手写apply**(arguments[this, [参数1，参数2.....] ])

```js
Function.prototype.myApply=function(context){  // 箭头函数从不具有参数对象！！！！！这里不能写成箭头函数
    let obj=context||window
    obj.fn=this
    const arg=arguments[1]||[]    //若有参数，得到的是数组
    let res=obj.fn(...arg)
    delete obj.fn
    return res
}   
function f(a,b){
    console.log(a,b)
    console.log(this.name)
}
let obj={
    name:'张三'
}
f.myApply(obj,[1,2])  //arguments[1]
```

**手写bind**

https://juejin.im/post/5dd4ee86f265da47e750bb99#heading-19

```js
this.value = 2
var foo = {
    value: 1
};
var bar = function(name, age, school){
  console.log(name) // 'An'
  console.log(age) // 22
  console.log(school) // '家里蹲大学'
}
var result = bar.bind(foo, 'An') //预置了部分参数'An'
result(22, '家里蹲大学') //这个参数会和预置的参数合并到一起放入bar中
```

简单版本

```js
Function.prototype.bind = function(context, ...outerArgs) {
    var fn = this;
    return function(...innerArgs) {   //返回了一个函数，...rest为实际调用时传入的参数
        return fn.apply(context,[...outerArgs, ...innerArgs]);  //返回改变了this的函数，
                                                      //参数合并
    }
}
```

new失败的原因：

例：

```js
// 声明一个上下文
let thovino = {
  name: 'thovino'
}

// 声明一个构造函数
let eat = function (food) {
  this.food = food
  console.log(`${this.name} eat ${this.food}`)
}
eat.prototype.sayFuncName = function () {
  console.log('func name : eat')
}

// bind一下
let thovinoEat = eat.bind(thovino)
let instance = new thovinoEat('orange')  //实际上orange放到了thovino里面
console.log('instance:', instance) // {}
```

生成的实例是个空对象

在`new`操作符执行时，我们的`thovinoEat`函数可以看作是这样：

```
function thovinoEat (...innerArgs) {
  eat.call(thovino, ...outerArgs, ...innerArgs)
}
```

<u>在new操作符进行到第三步的操作`thovinoEat.call(obj, ...args)`时，这里的`obj`是new操作符自己创建的那个简单空对象`{}`，但它其实并没有替换掉`thovinoEat`函数内部的那个上下文对象`thovino`</u>。这已经超出了`call`的能力范围，因为这个时候要替换的已经不是`thovinoEat`函数内部的`this`指向，而应该是`thovino`对象。

**换句话说，我们希望的是`new`操作符将`eat`内的`this`指向操作符自己创建的那个空对象。但是实际上指向了`thovino`，`new`操作符的第三步动作并没有成功**！

可new可继承版本

```js
Function.prototype.bind = function (context, ...outerArgs) {
  let that = this;

  function res (...innerArgs) {
    if (this instanceof res) {
      // new操作符执行时
      // 这里的this在new操作符第三步操作时，会指向new自身创建的那个简单空对象{}
      that.call(this, ...outerArgs, ...innerArgs)
    } else {
      // 普通bind
      that.call(context, ...outerArgs, ...innerArgs)
    }
  }
  res.prototype = this.prototype //！！！
  return res
}
```

### 手动实现new

```js
function Person(name,age){
    this.name=name
    this.age=age
}
Person.prototype.sayHi=function(){
    console.log('Hi！我是'+this.name)
}
let p1=new Person('张三',18)

// 手动实现new
function create(){
    let obj={}
    //获取构造函数
    let fn=[].shift.call(arguments)  //将arguments对象提出来转化为数组，arguments并不是数组而是对象    ！！！这种方法删除了arguments数组的第一个元素，！！这里的空数组里面填不填元素都没关系，不影响arguments的结果      或者let arg = [].slice.call(arguments,1)
    obj.__proto__=fn.prototype
    let res=fn.apply(obj,arguments)    //改变this指向，为实例添加方法和属性
    // 确保返回的是一个对象(万一fn不是构造函数)
    return typeof res==='object'?res:obj
}
let p2=create(Person,'李四',19)    //////调用
p2.sayHi()
```

细节：

```js
[].shift.call(arguments)  也可写成：

	let arg=[...arguments]
    let fn=arg.shift()  //使得arguments能调用数组方法,第一个参数为构造函数
    obj.__proto__=fn.prototype
    //改变this指向，为实例添加方法和属性
    let res=fn.apply(obj,arg)
```

文字描述过程：

1. 创建一个空对象 obj;
2. 将空对象的隐式原型（__proto__）指向构造函数的prototype。
3. 使用 call 改变 this 的指向
4. 如果无返回值或者返回一个非对象值，则将 obj 返回作为新对象；如果返回值是一个新对象的话那么直接直接返回该对象。

### 函数实现一秒钟输出一个数

更多方法： https://blog.csdn.net/weixin_39147099/article/details/83830587

1.

```js
for(let i=0;i<=10;i++){   //用var打印的都是11
    setTimeout(()=>{
        console.log(i);
    },1000*i)
}
```

2. 不用let的 ES5 方法： 原理是用立即执行函数创造一个块级作用域

   ```js
   for(var i = 1; i <= 10; i++){
       (function (i) {
           setTimeout(function () {
               console.log(i);
           }, 1000 * i)
       })(i);
   }
   ```


### 实现事件订阅eventBus

https://www.cnblogs.com/yalong/p/14294497.html

```js
      //声明类
      class EventBus {
        constructor() {
          this.eventList = {} //创建对象收集事件
        }
        //发布事件
        $on(eventName, fn) {
          //判断是否发布过事件名称? 添加发布 : 创建并添加发布
          this.eventList[eventName]
            ? this.eventList[eventName].push(fn)
            : (this.eventList[eventName] = [fn])
        }
        //订阅事件
        $emit(eventName) {
          if (!eventName) throw new Error('请传入事件名')
          //获取订阅传参
          const data = [...arguments].slice(1)
          if (this.eventList[eventName]) {
            this.eventList[eventName].forEach((i) => {
              try {
                i(...data) //轮询事件
              } catch (e) {
                console.error(e + 'eventName:' + eventName) //收集执行时的报错
              }
            })
          }
        }
        //执行一次
        $once(eventName, fn) {
          const _this = this
          function onceHandle() {
            fn.apply(null, arguments)
            _this.$off(eventName, onceHandle) //执行成功后取消监听
          }
          this.$on(eventName, onceHandle)
        }
        //取消订阅
        $off(eventName, fn) {
          //不传入参数时取消全部订阅
          if (!arguments.length) {
            return (this.eventList = {})
          }
          //eventName传入的是数组时,取消多个订阅
          if (Array.isArray(eventName)) {
            return eventName.forEach((event) => {
              this.$off(event, fn)
            })
          }
          //不传入fn时取消事件名下的所有队列
          if (arguments.length === 1 || !fn) {
            this.eventList[eventName] = []
          }
          //取消事件名下的fn
          this.eventList[eventName] = this.eventList[eventName].filter(
            (f) => f !== fn
          )
        }
      }
      const event = new EventBus()

      let b = function (v1, v2, v3) {
        console.log('b', v1, v2, v3)
      }
      let a = function () {
        console.log('a')
      }
      event.$once('test', a)
      event.$on('test', b)
      event.$emit('test', 1, 2, 3, 45, 123)

      event.$off(['test'], b)

      event.$emit('test', 1, 2, 3, 45, 123)

```

## Object.create和new的区别



## Object.assign

**页面优化之滚动优化**

https://www.cnblogs.com/coco1s/p/5499469.html

## 节流和防抖

- window对象频繁的onresize,onscroll等事件
- 拖拽的mousemove事件 (每次mouseover就会触发一次函数)
- 射击游戏的mousedown，keydown事件
- 文字输入，自动完成的keyup事件

比如，又比如每次搜索一下就会向服务器发送一个请求，这样既没有意义，也很浪费资源。

实际需求大多为<u>停止改变大小n毫秒后</u>执行后续处理；而其他事件大多数的需求是以<u>一定的频率</u>执行后续处理。

**节流**
 mouseover，resize这种事件，每当有变化的时候，就会触发一次函数，这样很浪费资源。而节流就是每隔n的时间调用一次函数，减少资源浪费。（触发后一段时间内不调用）

**防抖**
 A持续说了一段时间的话后停止讲话，过了10秒之后，判定A讲完了，B开始回答；如果10秒内A又继续讲话，那么我们判定A没讲完，B不响应，等A再次停止后，我们再次计算停止的时间，如果超过10秒B响应，如果没有则B不响应。(防抖以最后一次触发为准)

**节流与防抖的区别**
 前提都是某个行为持续地触发，不同之处只要判断是要优化到减少它的执行次数还是只执行一次就行。

- 节流例子（连续不断动都需要调用时用，设一时间间隔），像dom的拖拽，如果用消抖的话，就会出现卡顿的感觉，因为只在停止的时候执行了一次，这个时候就应该用节流，在一定时间内多次执行，会流畅很多。
- 防抖例子（连续不断触发时不调用，触发完后过一段时间调用），像仿百度搜索，就应该用防抖，当我连续不断输入时，不会发送请求；当我一段时间内不输入了，才会发送一次请求；如果小于这段时间继续输入的话，时间会重新计算，也不会发送请求。

https://www.jianshu.com/p/c8b86b09daf0

节流：指连续触发事件但是在 n 秒中只执行一次函数       时间戳版和定时器版  

防抖： 指触发事件后在 n 秒内函数只能执行一次，如果在 n 秒内又触发了事件，则会重新计算函数执行时间。

**防抖函数**https://segmentfault.com/a/1190000018445196

```js
const input1 = document.getElementById('input1')

// let timer = null
// input1.addEventListener('keyup', function () {
//   if (timer) {
//     clearTimeout(timer)
//   }
//   timer = setTimeout(() => {
//     console.log(input1.value)   //模拟触发事件

//     //清空定时器
//     timer = null
//   }, 500)
// })

上述注释掉的代码存在什么问题？   timer暴露在外面，容易被篡改

function debounce(fn, delay) {
    if(typeof fn!=='function') {
        throw new TypeError('fn不是函数')
	}
    let timer; // 维护一个 timer
    return function () {
        var _this = this; // 取debounce执行作用域的this(原函数挂载到的对象)
        var args = arguments;
        if (timer) {
            clearTimeout(timer);
        }
        timer = setTimeout(function () {
            fn.apply(_this, args); // 用apply指向调用debounce的对象，相当于_this.fn(args);
        }, delay);
    };
}

input1.addEventListener('keyup', debounce(() => {
  console.log(input1.value)
}), 600)

```

**节流函数**

```javascript
const div1 = document.getElementById("div1")

// let timer = null
// div1.addEventListener('drag', function (e) {
//   if (timer) {
//     return
//   }
//   timer = setTimeout(() => {
//     console.log(e.offsetX, e.offsetY)

//     timer = null
//   }, 100)
// })

上述注释掉的代码存在什么问题？    timer暴露在外面，容易被篡改

// 节流
function throttle(fn, delay) {
    let timer;
    return function () {
        var _this = this;     //
        var args = arguments;  //
        if (timer) {
            return;
        }
        timer = setTimeout(function () {
            fn.apply(_this, args); // 这里args接收的是外边返回的函数的参数，不能用arguments
         // fn.apply(_this, arguments); 需要注意：Chrome 14 以及 Internet Explorer 9 仍然不接受类数组对象。如果传入类数组对象，它们会抛出异常。
            timer = null; // 在delay后执行完fn之后清空timer，此时timer为假，throttle触发可以进入计时器
        }, delay)
    }
}

div1.addEventListener('drag', throttle((e) => {
  console.log(e.offsetX, e.offsetY)
}, 100))
```

## AJAX

**注意**：AJAX 只能向同源网址（协议、域名、端口都相同）发出 HTTP 请求，如果发出跨域请求，就会报错。

### 步骤

1. 创建 XMLHttpRequest 实例

2. 发出 HTTP 请求

3. 服务器返回 XML 格式的字符串

4. JS 解析 XML，并更新局部页面

   不过随着历史进程的推进，XML 已经被淘汰，取而代之的是 **[JSON](https://www.json.org/json-zh.html)。**

### 手写原生AJAX

了解了属性和方法之后，根据 AJAX 的步骤，手写最简单的 GET 请求。

version 1.0：

```js
myButton.addEventListener('click', function() {
    ajax()
})

function ajax() {
    let xhr = new XMLHttpRequest() //实例化，以调用方法
    xhr.open('get', 'https://www.google.com')  //参数2，url。参数三：异步
    xhr.onreadystatechange = () => {  //每当 readyState 属性改变时，就会调用该函数。
        if (xhr.readyState === 4) {  //XMLHttpRequest 代理当前所处状态。
            if (xhr.status >= 200 && xhr.status < 300) {  //200-300请求成功
                let string = request.responseText
                //JSON.parse() 方法用来解析JSON字符串，构造由字符串描述的JavaScript值或对象
                let object = JSON.parse(string)
            }
        }
    }
    request.send() //用于实际发出 HTTP 请求。不带参数为GET请求
}
```

promise实现

```js
 function ajax(url){
    const p=new Promise((resolve,reject)=>{
        let xhr=new XMLHttpRequest()
        xhr.open('get',url)
        xhr.onreadystatechange=()=>{
            if(xhr.readyState==4){
                if(xhr.status>=200&&xhr.status<=300){
                    resolve(JSON.parse(xhr.responseText))
                }else{
                    reject('请求出错')
                }
            }
        }
        xhr.send()  //发送hppt请求
    })
    return p
 }
 let url='/data.json'
 ajax(url).then(res=>console.log(res))
 .catch(reason=>console.log(reason))
```

### 手写Promise加载图片

```
function getData(url){
	return new Promise((resolve,reject)=>{
		$.ajax({
			url,
			success(data){
				resolve(data)
			},
			error(err){
				reject(err)
			}
		})
	})
}
const url1='./data1.json'
const url2='./data2.json'
const url3='./data3.json'
getData(url1).then(data1=>{
	console.log(data1)
	return getData(url2)
}).then(data2=>{
	console.log(data2)
	return getData(url3)
}).then(data3=>
	console.log(data3)
).catch(err=>
	console.error(err)
)
```

## 创建10个标签，点击的时候弹出来对应的序号？

```js
var a
for(let i=0;i<10;i++){
	a=document.createElement('a')
	a.innerHTML=i+'<br>'
	a.addEventListener('click',function(e){
        console.log(this)  //this为当前点击的<a>
		e.preventDefault()  //如果调用这个方法，默认事件行为将不再触发。
    //例如，在执行这个方法后，如果点击一个链接（a标签），浏览器不会跳转到新的 URL 去了。我们可以用 event.isDefaultPrevented() 来确定这个方法是否(在那个事件对象上)被调用过了。
		alert(i)
	})
    const d=document.querySelector('div')
    d.appendChild(a)  //append向一个已存在的元素追加该元素。
}
```

## js严格模式

https://juejin.im/post/5c66c24df265da2da83560d7

**ES5新增**      严格条件下运行js代码

1.消除js语法不合理严谨的地方，减少怪异行为

   变量必须声明才能使用

2.消除代码运行不安全之处

3.提高编译器效率，增加运行速度

4.禁用未来版本的可能语法。如某些保留字（class,enum,expory,super,extends……不能做变量名）

**开启：**

1.为脚本开启严格模式   

​	<script>标签中或<script>标签立即执行函数中

  	‘use strict’    

2.为函数开启严格模式

​	写在函数内第一行

**1、普通变量**

```
//严格模式下对不可写属性赋值，将报错。
var demo2 = {};
Object.defineProperty(demo2, "x", { value: 42, writable: false });
demo2.x = 9; //报错
 
//严格模式下对只读属性赋值，将报错。
var demo3 = { get x() { return 17; } };
demo3.x = 5;  //报错
 
//严格模式下对禁止扩展的对象添加新属性，将报错。
var demo4 = {};
Object.preventExtensions(demo4);
demo4.newProp = "ohai"; //报错
 
//严格模式下删除一个不可删除的属性，将报错。
delete Object.prototype; //报错
 
//严格模式下无法删除变量。只有configurable设置为true的对象属性，才能被删除。
var x;
delete x; // 报错
 
var demo5 = Object.create(null, {'x': {
    value: 1,
    configurable: true
}});
delete demo5.x; // 删除成功
 
```

**3、禁止this关键字指向全局对象**

全局作用域的函数中的this不再指向全局而是undefined。
 如果使用构造函数时，如果忘了加new，this不再指向全局对象，而是undefined报错。

```
function demo7_1(){
    console.log(this);
}
function demo7_2(){
    function demo7_3(){
    console.log(this);
    }
    demo7_3();
}
demo7_1(); //undefined
demo7_2(); //undefined
 
function demo7_4(){
   this.a = 1;
};
demo7_4();// 报错，使用构造函数时，如果忘了加new，this不再指向全局对象，而是undefined.a。
```

**4、静态绑定**

- 禁止使用with语句
- eval语句本身就是一个作用域，它所生成的变量只能用于eval内部。

```
//严格模式下禁用with
var demo8 = 1;
with (o){ // 报错 
  demo8 = 2;
}
 
//正常模式下，eval语句的作用域，取决于它处于全局作用域，还是处于函数作用域。
//严格模式下，eval语句本身就是一个作用域，不再能够生成全局变量了，它所生成的变量只能用于eval内部。
//严格模式下，eval语句内传入的字符串也是按照严格模式执行。
function demo9() {
    var x = 2;
    eval("var y = 1; console.log(y); "); //1
    eval("var x = 12");
    console.log(x); //2
    console.log(y); //报错：y is not defined
}
demo9();
```

**5、aruments对象的限制**

```
//不允许对arguments赋值
arguments++; //报错
 
//arguments不再追踪参数的变化
//在非严格模式中,修改arguments对象中某个索引属性的值,和这个属性对应的形参变量的值也会同时变化,反之亦然。
//在严格模式中arguments 对象会以形参变量的拷贝的形式被创建和初始化，因此arguments对象的改变不会影响形参。
function demo10_1(a) {
    a = 2;
    return [a, arguments[0]];
}
console.log(demo10_1(1)); // 正常模式为[2,2]
 
function demo10_2(a) {
    "use strict"
    a = 2;
    return [a, arguments[0]];
}
console.log(demo10_2(1)); // 严格模式为[2,1]
 
//禁止使用arguments.callee
var demo11 = function() { return arguments.callee; };
demo11(); // 报错
复制代码
```

**6、禁止在函数内部遍历调用栈**

```
function demo12(){
    demo12.caller; // 报错
    demo12.arguments; // 报错
}
demo12();
```

**7、保留字**

使用未来保留字(也许会在ECMAScript 6中使用):implements, interface, let, package, private, protected, public, static,和yield作为变量名或函数名会报错。

​	1.变量必须先声明再使用（之前默认全局变量）

​	2.不能删除已声明的变量    //严格模式下无法删除变量。只有configurable设置为true的对象属性，才能被删除。 var x; delete x; // 报错

​	3.**this指向问题** 

​		之前全局作用this指向window,；**严格模式为undefined**

​		之前构造函数不加new直接调用, this指向window, 相当与给window添加属性；严格模式报错（不能给		undefined添加属性）

​	   定时器this仍然指向window

​		事件、对象仍指向调用者

​	4.函数中不能有重名参数

```
//正常模式下，如果函数有多个重名的参数，可以用arguments[i]读取。
function demo6(a,a,b){return ;} //报错
```

​	5. 不允许在非函数代码块内声明函数（if,for……）

## [JS基础——同步异步的区别](https://segmentfault.com/a/1190000017996968)

### 单线程的JavaScript是如何实现异步的

https://juejin.cn/post/6844904159385223175   这个讲的很清楚！！

JavaScript是脚本语言，它需要在一个宿主环境里才能运行，显然我们接触较多的宿主环境就是--浏览器！虽说JavaScript是单线程的，然而浏览器却不是！

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/5/15/172140aad60f19a3~tplv-t2oaga2asx-watermark.awebp)



如图所求，JavaScript引擎线程称为主线程，它负责解析JavaScript代码；其他可以称为辅助线程，这些辅助线程便是JavaScript实现异步的关键了！

如（代码片段2）：主线程负责自上而下顺序执行，当遇到setTimeout函数后，便将其交给定时器线程去执行，自己继续执行下面的代码！从而达到异步的目的。

不仅如此，更关键的是：

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/5/15/172140eedca0d03b~tplv-t2oaga2asx-watermark.awebp)



**什么是异步操作？**

```
最基础的异步是setTimeout和setInterval函数，
因为它们可以控制js的执行顺序。
可简单理解为：可以改变程序正常执行顺序的操作就可以看成是异步操作。
```

**js为什么是单线程的 ？**(同一个时间只能做一件事)

源于其语言特性， `JavaScript` 是浏览器脚本语言，它可以操纵 `DOM` ，可以渲染动画，与用户交互，如果是多线程的话，执行顺序无法预知，而且操作以哪个线程为准也是个难题。

为了避免复杂性，从一诞生，JavaScript就是单线程(核心特征)

`HTML5` 时代，浏览器为了充分发挥 `CPU` 性能优势，允许 `JavaScript` 创建多个线程，<u>但即使能额外创建线程，这些子线程仍然是受到主线程控制，而且不得操作 `DOM`</u>，类似于开辟一个线程来运算复杂性任务，运算好了通知主线程运算完毕，结果给你，这类似于异步的处理方式，没有改变 `JavaScript` 单线程的本质。

**同步和异步，无论如何，做事情的时候都是只有一条流水线（单线程），**
**同步和异步的差别就在于这条流水线上各个流程的执行顺序不同。**

1. JS 是单线程的，只有一个主线程
2. 函数内的代码从上到下顺序执行，遇到被调用的函数先进入被调用函数执行，待完成后继续执行
3. **遇到异步事件，浏览器另开一个线程，主线程继续执行**，待结果返回后，执行回调函数

 **`JS` 这个语言是运行在宿主环境中**，比如 `浏览器环境`，`nodeJs环境`

- 浏览器中，浏览器负责提供这个额外的线程（调用栈先进行顺序调用，一旦发现异步操作的时候就会交给浏览器内核的其他模块进行处理，对于 `Chrome` 浏览器来说，这个模块就是 `webcore` 模块，上面提到的异步API，`webcore` 分别提供了 `DOM Binding` 、 `network`、`timer` 模块进行处理。等到这些模块处理完这些操作的时候将回调函数放入任务队列中，之后等栈中的任务执行完之后再去执行任务队列之中的回调函数。）
-  `Node` 中，`Node.js` 借助 `libuv` 来作为抽象封装层， 从而屏蔽不同操作系统的差异，`Node`可以借助`libuv`来实现多线程。

`javaScript` 只有一个主线程和一个调用栈（`call stack`）

而这个异步线程又分为 `微任务` 和 `宏任务`

所有任务分成两种：

```
同步任务：主线程上排队执行的任务，前一个任务执行完毕，才能执行后一个任务； 

异步任务：不进入主线程、而进入"任务队列"（task queue），只有等主线程任务执行完毕，"任务队列"开始通知主线程，请求执行任务，该任务才会进入主线程执行。
```

**发展：**

单线程->调用栈原理 ->在调用栈中，前一个函数在执行的时候，下面的函数全部需要等待前一个任务执行完毕，才能执行。有很多任务需要很长时间才能完成，如果一直都在等待的话，调用栈的效率极其低下，这时，`JavaScript` 语言设计者意识到，这些任务主线程根本不需要等待，只要将这些任务挂起，先运算后面的任务，等到执行完毕了，再回头将此任务进行下去，于是就有了 `任务队列` 的概念。

**"任务队列"中的事件，除了IO设备的事件以外，**
**还包括一些用户产生的事件（比如鼠标点击、页面滚动等等），**
**比如$(selectot).click(function)，这些都是相对耗时的操作。**
**只要指定过这些事件的回调函数，这些事件发生时就会进入"任务队列"，等待主线程读取。**

所谓"回调函数"（callback），就是那些会被主线程挂起来的代码，前面说的点击事件$(selectot).click(function)中的function就是一个回调函数。异步任务必须指定回调函数，当主线程开始执行异步任务，就是执行对应的回调函数。例如ajax的success，complete，error也都指定了各自的回调函数，这些函数就会加入“任务队列”中，等待执行。



### 前端多线程--Web Worker

**为什么引入多线程？**

Javascript 的确是单线程的，阻塞和其他异步的需求的确是通过事件循环来解决的，但是这套机制当线程需要处理大规模的计算的时候就不大适用了

为了利用多核 `CPU` 的计算能力，**`HTML5` 提出 `Web Worker` 标准（多线程解决方案），允许 `JavaScript` 脚本创建多个线程**。<u>但子线程完全受主线程控制，不得操作 `DOM`</u>。没有改变 `JavaScript` 单线程的本质。

**使用场景：**
1.复杂数据处理场景
	某些检索、排序、过滤、分析会非常耗费时间，这时可以使用Web Worker来进行，不占用主线程。
2预渲染
	在某些渲染场景下，比如渲染复杂的canvas的时候需要计算的效果比如反射、折射、光影、材料等，这些计算的逻辑可以使用Worker线程来执行，也可以使用多个Worker线程

3.页头消息状态更新，比如页头的消息个数通知

4.高频用户交互，拼写检查，譬如：根据用户的输入习惯、历史记录以及缓存等信息来协助用户完成输入的纠错、校正功能等

5.加密：加密有时候会非常地耗时，特别是如果当你需要经常加密很多数据的时候（比如，发往服务器前加密数据）。

​	<u>加密是一个使用 `Web Worker` 的绝佳场景，因为它并不需要访问 `DOM` ，只是纯粹使用算法进行计算。</u>随着大众对个人敏感数据的日益重视，信息安全和加密也成为重中之重。这可以从近期的 12306 用户数据泄露事件中体现出来。

​	在 Worker 进行计算，对于用户来说是无缝地且不会影响到用户体验。

6.**预取数据**：为了优化网站或者网络应用及提升数据加载时间，你可以使用 `Workers` 来提前加载部分数据以备不时之需。  预加载图片：有时候一个页面有很多图片，或者有几个很大的图片的时候，如果业务限制不考虑懒加载，也可以使用Web Worker来加载图片



**`workers` 和主线程间的数据传递**：

​	双方都使用 `postMessage()` 方法发送各自的消息，使用`onmessage` 事件处理函数来响应消息（消息被包含在 `Message` 事件的 `data` 属性中）。

​	这个过程中数据并不是被共享而是被复制。

**注意**

- 虽然使用worker线程不会占用主线程，但是启动worker会比较耗费资源
- 主线程中使用XMLHttpRequest在请求过程中浏览器另开了一个异步http请求线程，但是交互过程中还是要消耗主线程资源

### 浏览器的event loop

https://juejin.im/post/5b8f76675188255c7c653811#heading-4

宏队列  setTimeout,  setInterval, setImmediate (Node独有)

微队列   promise, process.nextTick (promise内部正常执行，.then...方法里面的语句放入队列)

​				asycn, await（await 语句和它之前的立刻执行，await后面的相当于promise的then）

执行一个JavaScript代码的具体流程：

1. 执行全局Script代码，这些同步代码有一些是同步语句，有一些是异步语句（比如setTimeout等）；
2. 全局Script代码执行完毕后，调用栈Stack会清空；
3. 从微队列microtask queue中取出位于队首的回调任务，放入调用栈Stack中执行，执行完后microtask queue长度减1；
4. 继续取出位于队首的任务，放入调用栈Stack中执行，以此类推，直到直到把microtask queue中的所有任务都执行完毕。**注意，如果在执行microtask的过程中，又产生了microtask，那么会加入到队列的末尾，也会在这个周期被调用执行**；
5. microtask queue中的所有任务都执行完毕，此时microtask queue为空队列，调用栈Stack也为空；
6. 取出宏队列macrotask queue中位于队首的任务，放入Stack中执行；
7. 执行完毕后，调用栈Stack为空；
8. 重复第3-7个步骤；
9. 重复第3-7个步骤；
10. ......

**这就是浏览器事件循环Event Loop**

3个重点：

1. 宏队列一次只取一个任务执行，执行完后去执行微任务队列中的任务；
2. 微任务队列中所有任务都会依次取出来执行，直到microtask queue空；
3. 图中没有画UI rendering的节点，因为这个是由浏览器自行判断决定的，但是只要执行UI rendering，它的节点是在执行完所有的microtask之后，下一个macrotask之前，紧跟着执行UI render

https://www.jianshu.com/p/9d64450f547c  案例，判断输出顺序https://juejin.im/post/5c3d8956e51d4511dc72c200#heading-7  (中间部分)async 和await

**nextTick 放到微队列最前面**

**setImmediate保持在宏队列最后面（同类型谁先来谁执行）**

[]:   <u>setTimeout指定的时间不是从运行完setTimeout开始计算而是从当前执行栈执行完开始计算。</u>

### **JS 异步编程六种方案

https://juejin.im/post/5c30375851882525ec200027#heading-1

### 回调地狱->promise->async await

`Promise`是避免回调地狱的一个进步。此方法并没有移除回调函数，但是将函数的调用连接起来并且简化了代码，使得代码更易于阅读。

**promise**

https://blog.csdn.net/qq_34645412/article/details/81170576

**then方法** 可以接受两个参数，第一个对应resolve的回调，第二个对应reject的回调。（也就是说then方法中接受两个回调，一个成功的回调函数，一个失败的回调函数，并且能在回调函数中拿到成功的数据和失败的原因），所以我们能够分别拿到成功和失败传过来的数据就有以上的运行结果

**catch** <u>和then方法中接受的第二参数rejected的回调是一样的</u>。它还有另外一个作用：执行resolve的回调（也就是上面then中的第一个参数）时，**如果抛出异常了（代码出错了），并不会报错卡死js，而是会进到这个catch方法中**。

```
Promise.reject(reason);
```

**all的用法**

与then同级的另一个方法，all方法，该方法提供了并行执行异步操作的能力，并且在所有异步操作执行完后并且执行结果都是成功的时候才执行回调。<u>all会把所有异步操作的结果放进一个数组中传给then</u>，然后再执行then方法的成功回调将结果接收

![img](https://img-blog.csdn.net/20180725102513955?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0NjQ1NDEy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```js
Promise
		.all([promiseClick3(), promiseClick2(), promiseClick1()])
		.then(function(results){
			console.log(results);
		});
		
/// 手写promise.all
// 静态all方法
  static all(promiseArr) {   // 它返回的也是一个promise对象
    let count = 0
    let result = []
    return new Promise((resolve, reject) => {
      if (!promiseArr.length)  return resolve(result)
      promiseArr.forEach((item, i) => {   //当前项，索引
        Promise.resolve(item).then(value => {  //then里接收两个参数，成功时的回调和失败时的回调   Promise.resolve(value)方法返回一个以给定值解析后的Promise 对象
          count++
          result[i] = value
          if (count === promiseArr.length) {
            resolve(result)
          }
        }, error => {
          reject(error)
        })
      })
    })
  }
```

**race的用法**

all是等所有的异步操作都执行完了再执行then方法，那么race方法就是相反的，谁先执行完成就先执行回调。先执行完的不管是进行了race的成功回调还是失败回调，其余的将不会再进入race的任何回调

```js
statuc race(arr) {
    return new Promise((resolve,reject)=>{
        arr.forEach(item=>{
            Promise.resolve(p).then(value=>{
                resolve(value)
            },err=>{reject(err)})
        })
    })
}
```

**promise.finally**

无论成功失败都执行callback

p.then().catch().finall()  参数都是函数

```
Promise.prototype.finally = function (callback) {
  let P = this.constructor;
  return this.then(
    value  => {
          callback();
          return value;
     },
    reason => {
        callback();
        throw reason
    }
  );
};
```



### Promise中的then第二个参数和catch有什么区别？

第一，reject是用来抛出异常的，catch是用来处理异常的；第二：reject是Promise的方法，而then和catch是Promise实例的方法（Promise.prototype.then 和 Promise.prototype.catch）。

主要区别就是，如果在then的第一个函数里抛出了异常，后面的catch能捕获到，而then的第二个函数捕获不到。

https://www.cnblogs.com/yuanjili666/p/12203253.html

手写promise:  处理异步， 加载图片

```js
//callback hell
// 获取第一份数据
$get(url1, (data1) =>{
    console.log(data1)
    // 获取第二份数据
    $get(url2, (data2) => {
        console.log(data2)
        // 获取第三份数据
        $get(url3, (data3) => {
            console.log(data3)
            // 获取更多数据
        })
    })
})
```

https://blog.csdn.net/express_yourself/article/details/106212762

解决方法：使用promise

```js
// promise
function getData(url) {
    return new Promise((resolve, reject) => {
        .$ajax({
            url,
            success(data) {
                resolve(data)
            },
            error(err) {
                reject(err)
            }
        })
    })
}

const url1 = 'data1.json'
const url2 = 'data2.json'
const url3 = 'data3.json'
getData(url1).then(data1 => {
    console.log(data1)
    return getData(url2)
}).then(data2 => {
    console.log(data2)
    return getData(url3)
}).then(data3 => {
    console.log(data3)
}).catch(er => console.error(err))    // 串联形式
```

**原生promise实现**

https://juejin.im/post/5cb2f5f3f265da034d2a069d

如果有大量的异步请求的时候，流程复杂的情况下，会发现充满了屏幕的`then`，看起来非常吃力，而ES7的Async/Await的出现就是为了解决这种复杂的情况。

https://segmentfault.com/a/1190000016788484

**async await**

`async`用于申明一个`function`是异步的，而`await` 用于等待一个异步方法执行完成。

**使用 Async.js 库**

`async.waterfall()` 和 `async.series()`。

async.waterfall()

当你要一个接一个地运行某些任务，然后将结果从上一个任务传到下一个任务时，这个函数非常有用。它需要一个函数“任务”数组和一个最终的“回调”函数，它会在“任务”数组中所有的函数完成后，或者用错误对象调用“回调”之后被调用。

async.series()

当你要运行一个函数然后在所有函数成功执行后需要获取结果时，它很有用。  `async.waterfall()` 和 `async.series()` 之间的主要区别在于， `async.series()` 不会将数据从一个函数传递到另一个函数。



## JS连续等号赋值问题 ***

https://blog.csdn.net/Chouglas/article/details/50828611

```js
var foo = {n: 1};
var bar = foo;
foo.x = foo = {n: 2};
console.log(foo.x);  // undefined
console.log(bar); //  Object {n: 1, x: {n:2}}
```

JS的赋值表达式是右结合的，所以 foo.x = foo = {n:2} 等价于 foo.x = ( foo = { n : 2} )

根据计算顺序，计算出括号内的值，然后赋给 foo.x。
得到 foo.x = { n : 2 }，同时 bar.x = { n : 2 } ，左边第一个等号的计算结束。
然后计算括号内的表达式，即 foo = { n : 2 }。
**foo 被赋予新的对象，不再是原来对象的引用，指向了 { n : 2 }，所以foo.x**
**得到的是 undefined**， 但是 bar 依旧指向原来的对象，所以bar值没有改变。
此段代码等同于如下的代码：

var foo = {n: 1};
var bar = foo;
var foo1 = {n: 2};
foo.x = foo1;
foo = foo1;
console.log(foo.x,bar);

## 函数调用的方法

 4 种

1. 作为一个函数调用

a(); 这样一个最简单的函数，不属于任何一个对象，就是一个函数，这样的情况this在 JavaScript 的在浏览器中的非严格模式默认是属于全局对象 window 的，在严格模式，就是 undefined。

1. 函数作为方法调用

**this** **永远指向最后调用它的那个对象**”

1. 使用构造函数调用函数

如果函数调用前使用了 new 关键字, 则是调用了构造函数。
 这看起来就像创建了新的函数，但实际上 JavaScript 函数是重新创建的对象

作为函数方法调用函数（call、apply）

## 立即执行函数

~~~javascript
(function() {
    ```
   // 这里开始写功能需求
 })();  
~~~

这是我们常说的立即执行函数(IIFE)，顾名思义，也就是说这个函数是立即执行函数体的，不需要额外调用，一般情况下我们只对匿名函数使用IIFE，这么做有两个目的：

> 一是不必为函数命名，避免了污染全局变量 二是IIFE内部形成了一个单独的作用域，可以封装一些外部无法读取的私有变量。
>
> **块级作用域的出现，实际上使得获得广泛应用的匿名立即执行函数表达式（匿名 IIFE）不再必要了。**
>
> ```javascript
> // IIFE 写法
> (function () {
>   var tmp = ...;
>   ...
> }());
> 
> // 块级作用域写法
> {
>   let tmp = ...;
>   ...
> }
> ```

## 高阶函数

函数作为 参数或返回值   （如作为回调函数）

```js
function fn(callback){`

	`callback&&callback()   //有就调用`
}
fn(function(){alert('hello')})
```

```js
function fn(){
	return fuction() { }
}
fn()
```

## sort原理

作用：排序，默认情况下按照数组元素们的Unicode码进行升序排序。

语法：数组名.sort();

特殊：

  允许自定义排序函数，从而实现对数字的升序或降序的排序

  ex:

   var arr=[12,6,4,115,78];

 //排序函数（升序）

 function sortAsc(a,b){

  return a-b;

 }

 arr.sort(sortAsc);

 原理：

  1.指定排序行数sortAsc，定义两个参数a和b，表示数组中相邻的两个数字2.将排序函数指定给数组sort()函数，数组会自动传递数据到sortAsc()中，

 如果sortAsc()的返回值>0，则交互两个数字的位置，否则不变。

 使用函数完成升序排序：

   arr.sort(

​    function(a,b){ //匿名函数

​     return a-b;

​    })

```
var numbers = [4, 2, 5, 1, 3];
numbers.sort(function(a, b) {
  return a - b;
});
console.log(numbers);
也可以写成：
numbers.sort((a, b) => a - b); 
console.log(numbers);
```

**数组sort方法用的是哪种排序算法**

不同浏览器实现原理不同，火狐是归并排序，谷歌是插入和快速排序的结合 大于10用快速排序

## 事件流

1.原始事件模型（DOM 0级事件模型：没有事件流，事件发生，立刻处理）

2.IE事件模型 （目标和冒泡）

3.DOM2事件模型

 *“DOM2**级事件**”中规定的事件流同时支持了事件捕获阶段和事件冒泡阶段，而作为开发者，我们可以选择事件处理函数在哪一个阶段被调用。*
 一次事件的发生包含三个过程：

(1)capturing phase:**事件捕获阶段**。
 事件被从document一直向下传播到目标元素,在这过程中依次检查经过的节点是否注册了**该事件的监听函数**，若有则执行。   （给元素绑定监听器）

(2)target phase:事件处理阶段（处于目标阶段）。
 事件到达目标元素,执行目标元素的事件处理函数.

(3)bubbling phase:事件冒泡阶段。
 事件从目标元素上升一直到达document，同样依次检查经过的节点是否注册了该事件的监听函数，有则执行。   （执行监听函数）

事件监听

`在``addEventListener()` 函数中, 我们具体化了两个参数——我们想要将处理器应用上去的事件名称，和包含我们用来回应事件的函数的代码。注意将这些代码全部放到一个匿名函数中是可行的:

```
btn.addEventListener('click', function() {
  var rndCol = 'rgb(' + random(255) + ',' + random(255) + ',' + random(255) + ')';
  document.body.style.backgroundColor = rndCol;});
```

这个机制带来了一些相较于旧方式的优点。有一个相对应的方法，`removeEventListener()``，`这个方法移除事件监听器。在大型的、复杂的项目中就非常有用了，可以非常高效地清除不用的事件处理器，另外在其他的一些场景中也非常有效——比如您需要在不同环境下运行不同的事件处理器，您只需要恰当地删除或者添加事件处理器即可。

您也可以给同一个监听器注册多个处理器，下面这种方式不能实现这一点：

```
myElement.onclick = functionA;
myElement.onclick = functionB;
```

第二行会覆盖第一行，但是下面这种方式就会正常工作了：

```
myElement.addEventListener('click', functionA);
myElement.addEventListener('click', functionB);
```

当元素被点击时两个函数都会工作：

 

（DOM Level 2 Events (`addEventListener()`, etc.)）的主要优点是，如果需要的话，可以使用`removeEventListener()`删除事件处理程序代码，而且如果有需要，您可以向同一类型的元素添加多个监听器。例如，您可以在一个元素上多次调用`addEventListener('click', function() { ... })`，并可在第二个参数中指定不同的函数。对于事件处理程序属性来说，这是不可能的，因为后面任何设置的属性都会尝试覆盖较早的属性，例如：

```
element.onclick = function1;
element.onclick = function2;
etc.
```

[stopPropagation()](https://developer.mozilla.org/en-US/docs/Web/API/Event/stopPropagation)的函数, **当在事件对象上调用该函数时，它只会让当前事件处理程序运行，但事件不会在冒泡链上进一步扩大，因此将不会有更多事件处理器被运行(不会向上冒泡)**

**事件委托应用场景**

因为有时候需要给某一类元素加事件（例如给所有li加点击事件），因为网页内容经常改变，这类元素随时会增加或者减少，为了保证所有这类元素都有事件，也为了节约内存，所以都需要采用事件委托才可实现！ （只绑定一次事件

4，可应用于多个元素，即使元素是后添加的->绑定给元素的共同祖先元素)

## JS事件绑定通用函数

1.简易版

```
// 通用事件绑定函数
function bindEvent(elem,type,fn) {
	elem.addEventListener(type,fn)
}

const div1 = document.getElementById("div1")
bindEvent(div1,'click',event => {
	console.log("点击")
	console.log(event.target)
})
```

2.使用事件代理

```js
function bindEvent(elem, type, selector, fn){
	// 不使用代理时，即只有三个参数时，将第三个参数（函数）赋值给第四个参数，再将第三个参数设置为null
    //判断一下selecter这个参数是否存在
	if(fn == null) {
		fn = selector;
		selector = null;
	}
	elem.addEventListener(type, function (e) {
		var target;
		if (selector) {    //selector是选择条件（比如选择class为a的类）
			target = e.target;   //js中事件是会冒泡的，所以this是可以变化的，但event.target不会变化，它永远是直接接受事件的目标DOM元素
			if(target.matches(selector)) {
				fn.call(target, e); /////从elem触发函数变为e.target触发函数(从ul触发变为li触发)
			}
		} else {
			fn(e)
		}
	})
}
```

【】

代码实例：给所有的li绑定点击事件，极为繁琐，这时候需要用到事件代理。

> ul.addEventListener("click",function(e) { 
>
> 	if(e.target && e.target.nodeName.toLowerCase() == "li") { // 检查事件源e.target是否为Li 
> 												
> 	 console.log("List item ",e.target.id.replace("post-","")," was clicked!"); // 打印当前点击是第几个item 
>
> } 

## 关于js中map集合中调用parseInt输出结果的问题

结果是1,NaN,NaN,NaN，首先说一下parseInt函数接收的参数，arg1接收的是要转换的数据，arg2接收的是进制，若未进行传递或传递的是0的话则默认为10进制数字，当map集合中调用该方法时，像内部传递的两个参数分别是当前元素和数组的下标，第一次调用的是，parseInt(1,0)得到的是1，第二次调用的是parseInt(2,1)第三次调用的是parseInt(3,2),所以返回的是NaN，因为前面的参数是要以后面的参数作为进制来处理，就好比parseInt(3,2)就是要把3当作2进制来处理，肯定不行啊，返回的就是NaN，后面的都是同理。只有能处理的才会返回正确的数字



## 正则表达式

常考: 匹配手机号，匹配不同文件类型

**正则匹配手机**


 \* 手机号的规则：
 \*   1 3567890123 （11位）
 \*   1. 以1开头
 \* 2. 第二位3-9任意数字
 \*   3. 三位以后任意数字9个

 \* ^1  [3-9] [0-9]{9}$   

 var phoneStr = "13067890123";
 var phoneReg = /^1\[3-9][0-9]{9}$/;    //  $表示在此之后不能再有任何内容   ^表示在此之前不能再有任何内容 
 console.log(phoneReg.test(phoneStr));

用于匹配字符串中字符组合的模式  



验证表单、字符串替换、提取字符串特定部分(手机->手机卡、手机壳)

用法：开发中直接复制。但能根据实际情况修改

```js
1. var re = /ab+c/;
```

```js
2. var re = new RegExp("ab+c");
```

/^A/  开头    /t$/ 结尾

常用正则表达式 https://wiki.jikexueyuan.com/project/regex/common-use.html

一、校验数字的表达式

数字：`^[0-9]*$`

n位的数字：`^d{n}$`

至少n位的数字：`^d{n,}$`

m-n位的数字：`^d{m,n}$`

零和非零开头的数字：`^(0|[1-9][0-9]*)$`

非零开头的最多带两位小数的数字：`^([1-9][0-9]*)+(.[0-9]{1,2})?$`

带1-2位小数的正数或负数：`^(-)?d+(.d{1,2})?$`

正数、负数、和小数：`^(-|+)?d+(.d+)?$`

有两位小数的正实数：`^[0-9]+(.[0-9]{2})?$`

有1~3位小数的正实数：`^[0-9]+(.[0-9]{1,3})?$`

非零的正整数：`^[1-9]d*$` 或 `^([1-9][0-9]*){1,3}$` 或 `^+?[1-9][0-9]*$`

非零的负整数：`^-[1-9][]0-9"*$` 或 `^-[1-9]d*$`

非负整数：`^d+$` 或 `^[1-9]d*|0$`

非正整数：`^-[1-9]d*|0$` 或 `^((-d+)|(0+))$`

非负浮点数：`^d+(.d+)?$` 或 `^[1-9]d*.d*|0.d*[1-9]d*|0?.0+|0$`

非正浮点数：`^((-d+(.d+)?)|(0+(.0+)?))$` 或 `^(-([1-9]d*.d*|0.d*[1-9]d*))|0?.0+|0$`

正浮点数：`^[1-9]d*.d*|0.d*[1-9]d*$` 或 `^(([0-9]+.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*.[0-9]+)|([0-9]*[1-9][0-9]*))$`

负浮点数：`^-([1-9]d*.d*|0.d*[1-9]d*)$` 或 `^(-(([0-9]+.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*.[0-9]+)|([0-9]*[1-9][0-9]*)))$`

浮点数：`^(-?d+)(.d+)?$` 或 `^-?([1-9]d*.d*|0.d*[1-9]d*|0?.0+|0)$`

二、校验字符的表达式

汉字：`^[\u4e00-\u9fa5]+$`

英文和数字：`^[A-Za-z0-9]+$` 或 `^[A-Za-z0-9]{4,40}$`

长度为3-20的所有字符：`^.{3,20}$`

由26个英文字母组成的字符串：`^[A-Za-z]+$`

由26个大写英文字母组成的字符串：`^[A-Z]+$`

由26个小写英文字母组成的字符串：`^[a-z]+$`

由数字和26个英文字母组成的字符串：`^[A-Za-z0-9]+$`

由数字、26个英文字母或者下划线组成的字符串：`^w+$` 或 `^w{3,20}$`

三、特殊需求表达式

去掉左右空格: `str.replace(/(^\s*)|(\s*$)/g, '')`

去掉所有空格: `str.replace(/\s+/g, '')`

密码需由8位以上大写字母、小写字母、数字及特殊符号组成: `/^(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9])(?=.*?[#?!.,@$%^&*-]).{8,}$/`

Email地址：`^w+([-+.]w+)*@w+([-.]w+)*.w+([-.]w+)*$`

域名：[a-zA-Z0-9][-a-zA-Z0-9]{0,62}(/.[a-zA-Z0-9][-a-zA-Z0-9]{0,62})+/.?

InternetURL：[a-zA-z]+://[`^s]* 或`^http://([w-]+.)+[w-]+(/[w-./?%&=]*)?$`

手机号码：`^(13[0-9]|14[5|7]|15[0|1|2|3|5|6|7|8|9]|18[0|1|2|3|5|6|7|8|9])d{8}$`

电话号码(“XXX-XXXXXXX”、”XXXX-XXXXXXXX”、”XXX-XXXXXXX”、”XXX-XXXXXXXX”、”XXXXXXX”和”XXXXXXXX)：`^((d{3,4}-)|d{3.4}-)?d{7,8}$`

国内电话号码`(0511-4405222、021-87888822)：d{3}-d{8}|d{4}-d{7}`

身份证号(15位、18位数字)：`^d{15}|d{18}$`

短身份证号码(数字、字母x结尾)：`^([0-9]){7,18}(x|X)?$` 或 `^d{8,18}|[0-9x]{8,18}|[0-9X]{8,18}?$`

帐号是否合法(字母开头，允许5-16字节，允许字母数字下划线)：`^[a-zA-Z][a-zA-Z0-9_]{4,15}$`

密码(以字母开头，长度在6~18之间，只能包含字母、数字和下划线)：`^[a-zA-Z]w{5,17}$`

强密码(必须包含大小写字母和数字的组合，不能使用特殊字符，长度在8-10之间)：`^(?=.*d)(?=.*[a-z])(?=.*[A-Z]).{8,10}$`

日期格式：`^d{4}-d{1,2}-d{1,2}`

一年的12个月(01～09和1～12)：`^(0?[1-9]|1[0-2])$`

一个月的31天(01～09和1～31)：`^((0?[1-9])|((1|2)[0-9])|30|31)$` xml文件：`^([a-zA-Z]+-?)+[a-zA-Z0-9]+\.[x|X][m|M][l|L]$`

空白行的正则表达式：`s*` (可以用来删除空白行)

HTML标记的正则表达式：`<(S*?)[`^>]*>.*?</>|<.*? />` (网上流传的版本太糟糕，上面这个也仅仅能部分，对于复杂的嵌套标记依旧无能为力)

首尾空白字符的正则表达式：`^s*|s*$`或(`^s*)|(s*$`) (可以用来删除行首行尾的空白字符(包括空格、制表符、换页符等等)，非常有用的表达式)

腾讯QQ号：`[1-9][0-9]{4,}` (腾讯QQ号从10000开始)

中国邮政编码：`[1-9]d{5}(?!d)` (中国邮政编码为6位数字)

IP地址：`d+.d+.d+.d+` (提取IP地址时有用)

IP地址：`((?:(?:25[0-5]|2[0-4]\d|[01]?\d?\d)\.){3}(?:25[0-5]|2[0-4]\d|[01]?\d?\d))`

**钱的输入格式：**

1. 有四种钱的表示形式我们可以接受:”10000.00” 和 “10,000.00”, 和没有 “分” 的 “10000” 和 “10,000”：`^[1-9][0-9]*$`
2. 这表示任意一个不以0开头的数字,但是,这也意味着一个字符”0”不通过,所以我们采用下面的形式：`^(0|[1-9][0-9]*)$`
3. 一个0或者一个不以0开头的数字.我们还可以允许开头有一个负号：`^(0|-?[1-9][0-9]*)$`
4. 这表示一个0或者一个可能为负的开头不为0的数字.让用户以0开头好了.把负号的也去掉,因为钱总不能是负的吧.下面我们要加的是说明可能的小数部分：`^[0-9]+(.[0-9]+)?$`
5. 必须说明的是,小数点后面至少应该有1位数,所以”10.”是不通过的,但是 “10” 和 “10.2” 是通过的：`^[0-9]+(.[0-9]{2})?$`
6. 这样我们规定小数点后面必须有两位,如果你认为太苛刻了,可以这样：`^[0-9]+(.[0-9]{1,2})?$`
7. 这样就允许用户只写一位小数.下面我们该考虑数字中的逗号了,我们可以这样：`^[0-9]{1,3}(,[0-9]{3})*(.[0-9]{1,2})?$`
8. 1到3个数字,后面跟着任意个 逗号+3个数字,逗号成为可选,而不是必须：`^([0-9]+|[0-9]{1,3}(,[0-9]{3})*)(.[0-9]{1,2})?$`

**正则表达式的简单使用方法**

 以判断是否为中文字符为例，使用JavaScript的`test()`方法，写一个函数。

- `test()` 方法用于检测一个字符串是否匹配某个模式.如果字符串中有匹配的值返回 true ，否则返回 false。

```
//是否含有中文（也包含日文和韩文）
function isChineseChar(str){   
   var reg = /[\u4E00-\u9FA5\uF900-\uFA2D]/;
   return reg.test(str);
}

isChineseChar('122') //false
isChineseChar('一二三') //true
```

## ES6

### let, const

ES6以后，顶层对象逐渐与全局对象脱钩

```javascript
let b = 1;
window.b // undefined
```

下面的代码如果使用`var`，最后输出的是`10`。

```javascript
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
```

**上面代码中，变量`i`是`var`命令声明的，在全局范围内都有效，全局只有一个变量`i`。每一次循环，变量`i`的值都会发生改变，**而循环内被赋给数组`a`的函数内部的`console.log(i)`，里面的`i`指向的就是全局的`i`。也就是说，所有数组`a`的成员里面的`i`，指向的都是同一个`i`，导致运行时输出的是最后一轮的`i`的值，也就是 10。

如果使用`let`，**声明的变量仅在块级作用域内有效**，最后输出的是 6。

```javascript
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6
```

上面代码中，变量`i`是`let`声明的，当前的`i`只在本轮循环有效，<u>所以每一次循环的`i`其实都是一个新的变量，所以最后输出的是`6`。</u>你可能会问，如果每一轮循环的变量`i`都是重新声明的，那它怎么知道上一轮循环的值，从而计算出本轮循环的值？这是因为 JavaScript 引擎内部会记住上一轮循环的值，初始化本轮的变量`i`时，就在上一轮循环的基础上进行计算。

另外，`for`循环还有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域。

```javascript
for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}
// abc
// abc
// abc
```

上面代码正确运行，输出了 3 次`abc`。这表明函数内部的变量`i`与循环变量`i`不在同一个作用域，有各自单独的作用域。

**const**

**并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动**。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，等同于常量。复合类型的数据（主要是对象和数组），变量指向的内存地址保存的只是一个指向实际数据的指针，`const`只能保证这个指针是固定的（即总是指向另一个固定的地址）。因此，将一个对象声明为常量必须非常小心。

```javascript
const foo = {};

// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop // 123

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError: "foo" is read-only
```

上面代码中，常量`foo`储存的是一个地址，这个地址指向一个对象。**不可变的只是这个地址，即不能把`foo`指向另一个地址，但对象本身是可变的，所以依然可以为其添加新属性。**

下面是另一个例子。

```javascript
const a = [];
a.push('Hello'); // 可执行
a.length = 0;    // 可执行
a = ['Dave'];    // 报错
```

上面代码中，常量`a`是一个数组，这个数组本身是可写的，但是如果将另一个数组赋值给`a`，就会报错。

**解决const对象可变属性问题：对象冻结** **Object.freeze**

冻结对象本身&&对象中的对象属性（彻底冻结）

```js
let constantize=(obj)=>{
    Object.freeze(obj)  //
    Object.keys(obj).forEach(key=>{    //(也可以用values遍历每个属性对应的值)
        if(typeof obj[key]=='object'){
            constantize(obj[key])   //冻结更深层嵌套的对象
        }
    }
    )
}
```

### var let const 区别

https://juejin.cn/post/6950193996538839077

`var`存在变量提升，而`let`和`const`不存在变量提升

<u>`var`声明的变量会添加进window对象中，而`let`和`const`声明的变量不会</u>

`let`和`const`声明的变量不可以重复声明

`let`和`const`声明的变量存在暂时性死区

`const`声明的基础类型不可修改，const声明的引用类型只能修改该引用类型的属性而不能给该变量重新赋值（`const`确定了一个地址，该地址不能被修改）

`let`和`const`存在块级作用域，而var不存在

let在for循环中每循环一次就会重新声明一次（因为let有块级作用域）

#### **为什么Let不能重复声明？不能变量提升？**

#### 坑1：let，const的暂时性死区

所谓暂时死区，就是不能在初始化之前，使用变量。

https://www.jianshu.com/p/0f49c88cf169

1. let 的「创建」过程被提升了，但是初始化没有提升。
2. var 的「创建」和「初始化」都被提升了。
3. function 的「创建」「初始化」和「赋值」都被提升了。

最后看 const，其实 const 和 let 只有一个区别，那就是 const 只有「创建」和「初始化」，没有「赋值」过程。

**let的执行过程**

1. 先是声明变量 //**暂时性死区开始**
2. 初始化 //赋值为undefined
3. 赋值//**暂时性死区结束**

结论

1.我们应该关注的是暂时性死区，不要在暂时性死区的时候去使用。

2.提升不如说是暂时性死区的开始

3.从let出现那一刻开始,let暂时性死区开始，直到被初始化undefined或者赋值结束

**坑2：**

[如何理解 let x = x 报错之后，再次 let x 依然会报错？](https://link.jianshu.com?t=https%3A%2F%2Fwww.zhihu.com%2Fquestion%2F62966713)

这个问题说明：如果 let x 的初始化过程失败了，那么

1. x 变量就将永远处于 created 状态。
2. **你无法再次对 x 进行初始化（初始化只有一次机会，而那次机会你失败了）。**
3. **由于 x 无法被初始化，所以 x 永远处在暂时死区**！
4. 有人会觉得 JS 坑，怎么能出现这种情况；其实问题不大，因为此时代码已经报错了，后面的代码想执行也没机会。

### 解构赋值

数组解构： 

​			let [a,b,c]=[1,2,3]      console.log(a)

​			多余元素为undefined

对象解构：

1.  `let person={name:person, age:18}`

​		`let {name, age}=person`

​		`console.log(name)`

​		`console.log(age)`

 2. 起别名

    `let {name:a, age:b}=person`       //name:属性匹配，a为真正的变量

    `console.log(a)`

    `console.log(b)`

### 箭头函数

**箭头函数表达式**的语法比[函数表达式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/function)更简洁，并且没有自己的`this`，`arguments`，`super`或`new.target`。箭头函数表达式更适用于那些本来需要匿名函数的地方，并且它不能用作构造函数。

- **箭头函数this指向**

https://zhuanlan.zhihu.com/p/26475137

箭头函数中的this，指向与一般function定义的函数不同，

箭头函数中的this是在定义函数的时候绑定，而不是在执行函数的时候绑定。

所谓的定义时候绑定，就是this是继承自父执行上下文！！中的this，比如这里的箭头函数中的this.x，箭头函数本身与say平级以key:value的形式，也就是箭头函数本身所在的对象为obj，而obj的父执行上下文就是window

https://segmentfault.com/a/1190000014027459

https://juejin.im/post/5aa1eb056fb9a028b77a66fd JS中的箭头函数与this

 如果箭头函数被非箭头函数包含，则`this`绑定的是最近一层非箭头函数的`this`，否则`this`的值则被设置为全局对象

简化函数定义语法

```js
const fn=()=>{
    console.log('hello')
}
fn()
//只有一句代码且执行结果为函数返回值，大括号可省
const add=(x,y)=>x+y
//形参一个，小括号可省
const fn1=i=>alert(i)
fn1(10)
```

箭头函数不指定this关键字，箭头函数中的this指的是函数定义位置的上下文this

在箭头函数中的this指向箭头函数定义位置中的this

例：

```js
let obj={
    age:10,
    say:()=>console.log(this),  //没有对象作用域，相当于定义在了全局作用域下
    say2:function(){       //函数作用域
        console.log(this)   
    }
}
obj.say()    //指向window 
obj.say2()   //指向obj (调用函数的对象)
```

- 箭头函数的参数获取

箭头函数没有arguments，用ES6剩余参数获取参数

```js
 let fn = (a,b,...others) => {

      console.log(a,b,others);

 }

fn(1,2,5,6,7,8)
```



### 箭头函数使用禁忌

https://segmentfault.com/a/1190000010946164

1. 主要是因为箭头函数的this指向问题，对象中的方法用this会导致无法指向调用方法的这个对象(返回undefined)

2. this是js中非常强大的特点，他让函数可以根据其调用方式**动态的改变上下文**，然后箭头函数直接在声明时就绑定了this对象，所以不再是动态的。

   在客户端，在dom元素上绑定事件监听函数是非常普遍的行为，在dom事件被触发时，回调函数中的this指向该dom,可当我们使用箭头函数时：

   button.addEventListener('click', () => {  
       **console.log(this === window); // => true**
       this.innerHTML = 'Clicked button';
   });
   因为这个回调的箭头函数是在全局上下文中被定义的，所以他的this是window。所以当this是由目标对象决定时，我们应该使用函数表达式：

   button.addEventListener('click', function() {  
       console.log(this === button); // => true
       this.innerHTML = 'Clicked button';
   });

   

   3. **构造函数不能用箭头函数，输出不是个constructor**

   var Message = (text) => {  
     this.text = text;
   };
   // Throws "TypeError: Message is not a constructor"
   var helloMessage = new Message('Hello World!');  

## **js this指向

https://juejin.im/post/5c0c87b35188252e8966c78a

1. 构造函数的this指向：实例化前，构造函数指向window，实例化之后，指向new的这个实例

```js
// Q1
var a = 1;

function print () {
  console.log(this.a)  1
}

print()
// Q2
const obj = {
   a: 2,
   print: function () { console.log(this.a) }   2
}

obj.print();

// Q3
const obj = {
   a: 3,
   print: function () { console.log(this.a) }   
}

const foo = obj.print; 
foo()
undefined

// Q4
const obj = {
   a: 4,
   print: () => { console.log(this.a) }
}
obj.print();
undefined

// Q5
var a = 5
const obj = {
   a: 6,
   print: () => { console.log(this.a) }
}
obj.print.call({a: 7});
箭头函数 undefined

// Q6
function Person () {
  this.a = 8
  this.print = function () {console.log(this.a)}
  return {a: 9}
}

const p = new Person() // new中this指向return的对象(想想new的原理)
console.log(p.a)     9
console.log(p.print())  // 报错 this指向的对象没有这个方法


// Q7
'use strict';
var a = 1;

function print () {
  console.log(this.a)
}
print()  报错 this为undefined
```



### Promise

[JavaScript异步编程史：回调函数到Promise到Async/Await](https://blog.fundebug.com/2018/07/11/javscriot-callback-promise-async-await/)

**回调函数**

回调函数(callback function)就是给另外一个宿主函数做参数的函数。回调函数在宿主函数内执行，执行结果返回给宿主函数。

**回调函数可以在特定代码执行完成之后再执行**。在执行一些比较耗时的代码时，比如读取文件，不需要阻塞整个代码去等待它完成，而可以继续执行其他代码；而当文件读取完成后，代码中所绑定给文件读取的回调函数会自动执行。

```js
// 当一切正常时，调用resolve函数；否则调用reject函数
var promise = new Promise(function(resolve, reject)
{
    if ( /* everything turned out fine */ )
    {
        resolve("Stuff worked!");
    }
    else
    {
        reject(Error("It broke"));
    }
});
```

**Promise**是一个特殊对象，它可以表示异步操作的成功或者失败，同时返回异步操作的执行结果。

**Async/Await**

采用同步的方式调用Promise函数，提高异步代码的可读性。

Async/Await是基于Promise的语法糖，它让我们可以使用同步的方式写异步代码。

在定义函数时，在其前面添加一个**async**关键字，就可以在函数内使用**await**了。当await一个Promise时，代码会采用非阻塞的方式继续执行下去。当Promise成功resolve了，await语句会正真执行结束，并获取resolve的值。当Promise失败reject了，await语句初会throw一个错误。

async/await提高了代码的可读性。出错处理非常方便，因为可以把同步代码和异步代码写在同一个try…catch…语句中。代码调试更加方便，使用Promise时，我们无法设置断点，而async/await代码可以像同步代码一样设置断点。

 

## **模块化

https://juejin.im/post/5dd956b8518825732e6668a8#heading-11

### 剩余参数

将不定数量参数表示为一个数组

```js
const sum=(...args)=>{
    let total=0
    args.forEach(item=>{
        total+=item
    })
    return total
}
let person=['Tom','Jerry','Rick']
let [s1,...s2]=person
///s1为第一个元素，s2为剩余元素组成的数组
```

### 扩展运算符

...arr   将数组或对象转为用逗号分隔的参数序列

1. 合并数组

```js
let arr1=[1,2,3]
let arr2=[4,5,6]
let arr3=[...arr1,...arr2]
//或者
arr1.push(...arr2)
```

2. 将类数组或可遍历对象转换为真正的数组(可以调用数组方法)

```js
let divs=document.getElementsByTagName('div')
let arr=[...divs]
arr.push('a')
```

### Array的扩展方法

数组方法加案例：https://www.cnblogs.com/sqh17/p/8529401.html

#### Array.from()

将类数组或可遍历对象转换为真正的数组(有length的)

```js
let arrayLick={
    0:'a',
    1:'b',
    m:'3',
    2:'df',
    length:3   //根据length找索引0，1，2
}
let arr=Array.from(arrayLick,item=>item+'hello')
console.log(arr)  //['ahello',bhello','dfhello']
```

#### Array.find( )

寻找数组的满足条件的第一个元素

#### Array.findIndex( )

#### Array.includes( )

### 模板字符串

1.    let s=\`hello,我是 ${name}`     //斜引号
2.    里面的内容可换行
3.   里面可调用函数     let s=\`hello,我是 ${sayName()}\`

### set数据结构、map

​	类似数组，但没有重复值

```
let s=new Set([1,1,2,3])
let arr=[...s] 
console.log(arr)    {1,1,2,3}=>[1,2,3]    //自动过滤传入的重复值
```

   	set.size    //长度

​	  方法： add(value)      delete(value)      has(value)        clear() 清空

  	遍历： s.forEach(value=>console.log(value))      (forEach:对每个成员执行某种操作，但没有返回值)   

由于 Set 结构没有键名，只有键值（或者说键名和键值是同一个值），所以`keys`方法和`values`方法的行为完全一致。

```javascript
let set = new Set(['red', 'green', 'blue']);
for (let item of set.keys()) {
  console.log(item);
}

for (let item of set.values()) {
  console.log(item);
}

for (let x of set) {
  console.log(x);
}
```

#### **map  **

​		https://es6.ruanyifeng.com/#docs/set-map#WeakSet

#### **map和set的区别**,set应用求数组并交差

https://blog.csdn.net/weixin_42152035/article/details/88345527?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase

#### map集合中调用parseInt输出结果的问题

https://blog.csdn.net/qq_31853247/article/details/84994933?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.nonecase

## ==  和===

**NaN 与任何值都不相等，包括 NaN 自身**

对于Array,Object等高级类型，==和===是没有区别的
 基础类型与高级类型，==和===是有区别的
 对于==，将高级转化为基础类型，进行“值”比较。===结果为false，因为类型不同

> 1. 如果x不是正常值（比如抛出一个错误），中断执行。
> 2. 如果y不是正常值，中断执行。
> 3. 如果Type(x)与Type(y)相同，执行严格相等运算x === y。
> 4. 如果x是null，y是undefined，返回true。
> 5. 如果x是undefined，y是null，返回true。
> 6. 如果Type(x)是数值，Type(y)是字符串，返回x == ToNumber(y)的结果。
> 7. 如果Type(x)是字符串，Type(y)是数值，返回ToNumber(x) == y的结果。
> 8. 如果Type(x)是布尔值，返回ToNumber(x) == y的结果。
> 9. 如果Type(y)是布尔值，返回x == ToNumber(y)的结果。
> 10. 如果Type(x)是字符串或数值或Symbol值，Type(y)是对象，返回x == ToPrimitive(y)的结果。
> 11. 如果Type(x)是对象，Type(y)是字符串或数值或Symbol值，返回ToPrimitive(x) == y的结果。
> 12. 返回false。

由于0的类型是数值，null的类型是Null（这是规格[4.3.13小节](https://link.zhihu.com/?target=http%3A//www.ecma-international.org/ecma-262/6.0/%23sec-4.3.13)的规定，是内部Type运算的结果，跟typeof运算符无关）。因此上面的前11步都得不到结果，要到第12步才能得到false。

```text
0 == null // false
```

https://juejin.im/entry/584918612f301e005716add6

### 对象全等

https://www.jianshu.com/p/1e44c1947156

## JavaScript中 0==null为何是false？

https://www.zhihu.com/question/52666420

## 对象转基本类型(转字符，转数字)

可重写toString() 方法、valueOf()方法   默认[object Object]  数值运算会输出NaN

obj+'abc'      obj*2

*******  == 运算符会把对象转为基本数据类型（调用toString,  valueOf方法）

https://blog.csdn.net/CAPEVERDE77/article/details/97270835

## ES6、7、8、9,10新特性

https://blog.csdn.net/qq_34586870/article/details/89515336

## web worker??

## 虚拟DOM

https://juejin.im/entry/5aedcfa351882506a36c664c

## Generator函数的Trunk输出的是什么

http://www.ruanyifeng.com/blog/2015/05/thunk.html

## 基本包装类型

**q: JavaScript中为什么string可以拥有方法？**

https://www.cnblogs.com/SheilaSun/p/4765394.html

JavaScript还提供了三种特殊引用类型：String、Number和Boolean，方便操作对应的基本类型。

继续看上面的剪辑字符串的例子，有没有注意到，尽管使用了substring方法，realMessage本身的值是不会变的，调用这个方法只是返回了一个新的字符串。

这就是基本包装类型的作用了。想用方法的时候，你尽管调，对应的基本包装类型有这个方法就行。例如上面的substring方法，string这个基本类型是不可能有这个方法的，但是String这个包装类型有啊，它会把这个方法执行完把结果返回。

执行到这行代码时，发生了很多事。：

```
realMessage.substring(5,15)
```

首先，它会从内存中读取realMessage的值。当处于这种读取模式下的时候，后台就开始干活了：

> 1.创建String类型的一个实例；
> 2.在实例上调用指定的方法；
> 3.销毁这个实例

上面的例子可以用这样的代码说明：

```
var _realMessage=new String("Said I love you but I lied");
var myMessage=_realMessage.substring(5,15);
_realMessgae=null; //方法调用后即销毁
```

所以，并不是基本类型string执行了自身方法，是后台为它创建了一个对应的基本包装类型String，它根据基本类型的值实例化出了一个实例，让这个实例去调用指定方法，最后销毁自己。

注意最后一步基本包装类型“会销毁”的特性，这决定了我们不能为基本类型值添加自定义属性和方法。

```
var me="sunjing";
me.age=18;
console.log(me.age);//undefined
```

![image-20200915174745706](C:\Users\sprina\AppData\Roaming\Typora\typora-user-images\image-20200915174745706.png)

## 前端常用设计模式

https://www.runoob.com/design-pattern/state-pattern.html  最全

设计模式是为了更好的代码重用性，可读性，可靠性，可维护性。

### **1、工厂模式**

> 常见的实例化对象模式，**相当于创建实例对象的new，提供一个创建对象的接口**

```
   // 某个需要创建的具体对象
    class Product {
        constructor (name) {
            this.name = name;
        }
        init () {}
    }
    // 工厂对象
    class Creator {
        create (name) {
            return new Product(name);
        }
    }
    const creator = new Creator();
    const p = creator.create(); // 通过工厂对象创建出来的具体对象
复制代码
```

应用场景：JQuery中的$、Vue.component异步组件、React.createElement等

### **2、单例模式**

> 保证一个类仅有一个实例，并提供一个访问它的全局访问点，一般登录、购物车等都是一个单例。

```
    // 单例对象
    class SingleObject {
        login () {}
    }
    // 访问方法
    SingleObject.getInstance = (function () {
        let instance;    //闭包
        return function () {
            if (!instance) {
                instance = new SingleObject();
            }
            return instance;     //有的话返回共同的instance
        }
    })()
    const obj1 = SingleObject.getInstance();
    const obj2 = SingleObject.getInstance();
    console.log(obj1 === obj2); // true
```

应用场景：JQuery中的$、Vuex中的Store、Redux中的Store等

**3、适配器模式**

> 用来解决两个接口不兼容问题，由一个对象来包装不兼容的对象，比如参数转换，允许直接访问

```
    class Adapter {
        specificRequest () {
            return '德国标准插头';
        }
    }
    // 适配器对象，对原来不兼容对象进行包装处理
    class Target {
        constructor () {
            this.adapter = new Adapter();
        }
        request () {
            const info = this.adapter.specificRequest();
            console.log(`${info} - 转换器 - 中国标准插头`)
        }
    }
    const target = new Target();
    console.log(target.request()); // 德国标准插头 - 转换器 - 中国标准插头
复制代码
```

应用场景：Vue的computed、旧的JSON格式转换成新的格式等

**4、装饰器模式**

> 在不改变对象自身的基础上，动态的给某个对象添加新的功能，同时又不改变其接口

```
    class Plane {
        fire () {
            console.log('发送普通子弹');
        }
    }
    // 装饰过的对象
    class Missile {
        constructor (plane) {
            this.plane = plane;
        }
        fire () {
            this.plane.fire();
            console.log('发射导弹');
        }
    }
    let plane = new Plane();
    plane = new Missile(plane);
    console.log(plane.fire()); // 依次打印 发送普通子弹 发射导弹
复制代码
```

利用AOP给函数动态添加功能，即Function的after或者before

```
Function.prototype.before = function (beforeFn) {
  const _self = this;
  return function () {
    beforeFn.apply(this, arguments);
    return _self.apply(this, arguments);
  }
}

Function.prototype.after = function (afterFn) {
  const _self = this;
  return function () {
    const ret = _self.apply(this, arguments);
    afterFn.apply(this, arguments);
    return ret;
  }
}

let func = function () {
  console.log('2');
}

func = func.before(function() {
  console.log('1');
}).after(function() {
  console.log('3');
})

func();
console.log(func()); // 依次打印 1 2 3
复制代码
```

应用场景：ES7装饰器、Vuex中1.0版本混入Vue时，重写init方法、Vue中数组变异方法实现等

**5、代理模式**

> 为其他对象提供一种代理，便以控制对这个对象的访问，不能直接访问目标对象

```
class Flower {}
// 源对象
class Jack {
    constructor (target) {
        this.target = target;
    }
    sendFlower (target) {
        const flower = new Flower();
        this.target.receiveFlower(flower)
    }
}
// 目标对象
class Rose {
    receiveFlower (flower) {
        console.log('收到花: ' + flower)
    }
}
// 代理对象
class ProxyObj {
    constructor () {
        this.target = new Rose();
    }
    receiveFlower (flower) {
        this.sendFlower(flower)
    }
    sendFlower (flower) {
        this.target.receiveFlower(flower)
    }
}
const proxyObj = new ProxyObj();
const jack = new Jack(proxyObj);
jack.sendFlower(proxyObj); // 收到花：[object Object]
复制代码
```

应用场景：ES6 Proxy、Vuex中对于getters访问、图片预加载等

**6、外观模式**

> 为一组复杂的子系统接口提供一个更高级的统一接口，通过这个接口使得对子系统接口的访问更容易，不符合单一职责原则和开放封闭原则

```
 class A {
    eat () {}
}
class  B {
    eat () {}
}
class C {
    eat () {
        const a = new A();
        const b = new B();
        a.eat();
        b.eat();
    }
}
// 跨浏览器事件侦听器
function addEvent(el, type, fn) {
    if (window.addEventListener) {
        el.addEventListener(type, fn, false);
    } else if (window.attachEvent) {
        el.attachEvent('on' + type, fn);
    } else {
        el['on' + type] = fn;
    }
}
复制代码
```

应用场景：JS事件不同浏览器兼容处理、同一方法可以传入不同参数兼容处理等

### **7、观察者模式**

> 定义对象间的一种一对多的依赖关系，**一个对象的状态发生改变时，所有依赖于它的对象都得到通知**

```
class Subject {
  constructor () {
    this.state = 0;
    this.observers = [];
  }
  getState () {
    return this.state;
  }
  setState (state) {         /////////
    this.state = state;
    this.notify();
  }
  notify () {          ///////////这里是重点   状态改变时，会调用所有观察者的update方法，控制台打印改变后的状态
    this.observers.forEach(observer => {
      observer.update();
    })
  }
  attach (observer) {
    this.observers.push(observer);
  }
}

class Observer {
  constructor (name, subject) {
    this.name = name;
    this.subject = subject;
    this.subject.attach(this);
  }
  update () {
    console.log(`${this.name} update, state: ${this.subject.getState()}`);
  }
}

let sub = new Subject();
let observer1 = new Observer('o1', sub);
let observer2 = new Observer('o2', sub);

sub.setState(1);
```

**观察者模式与发布/订阅模式区别: 本质上的区别是调度的地方不同**

虽然两种模式都存在订阅者和发布者（具体观察者可认为是订阅者、具体目标可认为是发布者），但是**观察者模式是由具体目标调度的，而发布/订阅模式是统一由调度中心调的，所以观察者模式的订阅者与发布者之间是存在依赖的，而发布/订阅模式则不会。**

---观察者模式：目标提供维护观察者的一系列方法，观察者提供更新接口。具体观察者和具体目标继承各自的基类，然后具体观察者把自己注册到具体目标里，在具体目标发生变化时候，调度观察者的更新方法。
 比如有个“天气中心”的具体目标A，专门监听天气变化，而有个显示天气的界面的观察者B，B就把自己注册到A里，当A触发天气变化，就调度B的更新方法，并带上自己的上下文。

---发布/订阅模式：订阅者把自己想订阅的事件注册到调度中心，当该事件触发时候，发布者发布该事件到调度中心（顺带上下文），由调度中心统一调度订阅者注册到调度中心的处理代码。
 比如有个界面是实时显示天气，它就订阅天气事件（注册到调度中心，包括处理程序），当天气变化时（定时获取数据），就作为发布者发布天气信息到调度中心，调度中心就调度订阅者的天气处理程序。

应用场景：JS事件、JS Promise、JQuery.$CallBack、Vue watch、NodeJS自定义事件，文件流等

**8、迭代器模式**

> 提供一种方法顺序访问一个聚合对象中各个元素, 而又无须暴露该对象的内部表示

可分为：内部迭代器和外部迭代器

内部迭代器： 内部已经定义好迭代规则，外部只需要调用一次即可。

```
const each = (args, fn) => {
  for (let i = 0, len = args.length; i < len; i++) {
    const value = fn(args[i], i, args);

    if (value === false) break;
  }
}
复制代码
```

应用场景： JQuery.each方法

外部迭代器：必须显示的请求迭代下一个元素。

```
// 迭代器
class Iterator {
  constructor (list) {
    this.list = list;
    this.index = 0;
  }
  next () {
    if (this.hasNext()) {
      return this.list[this.index++]
    }
    return null;
  }
  hasNext () {
    if (this.index === this.list.length) {
      return false;
    }
    return true;
  }
}
const arr = [1, 2, 3, 4, 5, 6];
const ite = new Iterator();

while(ite.hasNext()) {
  console.log(ite.next()); // 依次打印 1 2 3 4 5 6
}
复制代码
```

应用场景：JS Iterator、JS Generator

### **9、状态模式**

> 对象在**内部状态发生改变时改变它的行为**

```
// 红灯
class RedLight {
    constructor (state) {
        this.state = state;
    }
    light () {
        console.log('turn to red light');
        this.state.setState(this.state.greenLight)
    }
}
// 绿灯
class greenLight {
    constructor (state) {
        this.state = state;
    }
    light () {
        console.log('turn to green light');
        this.state.setState(this.state.yellowLight)
    }
}
// 黄灯
class yellowLight {
    constructor (state) {
        this.state = state;
    }
    light () {
        console.log('turn to yellow light');
        this.state.setState(this.state.redLight)
    }
}
class State {
    constructor () {
        this.redLight = new RedLight(this)
        this.greenLight = new greenLight(this)
        this.yellowLight = new yellowLight(this)
        this.setState(this.redLight) // 初始化为红灯
    }
    setState (state) {
        this.currState = state;
    }
}
const state = new State();
state.currState.light() // turn to red light
setInterval(() => {
    state.currState.light() // 每隔3秒依次打印红灯、绿灯、黄灯
}, 3000)
复制代码
```

应用场景：灯泡状态、红绿灯切换等
