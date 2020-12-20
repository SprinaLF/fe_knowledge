## 盒模型

![image-20200603230854512](C:\Users\sprina\AppData\Roaming\Typora\typora-user-images\image-20200603230854512.png)               ![image-20200603230906497](C:\Users\sprina\AppData\Roaming\Typora\typora-user-images\image-20200603230906497.png)

### box-sizing

https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-sizing

确定使用哪种盒模型 

1.content box(默认)、

2.border-box   IE盒模型

## CSS布局模型

1、流动模型（Flow）
 2、浮动模型 (Float)
 3、层模型（Layer）

层模型

每个图层能够精确**定位操作**

static：默认值。正常流

层模型有三种形式： 

1、绝对定位(position: absolute)

2、相对定位(position: relative)

3、固定定位(position: fixed)

绝对定位（相对于父类）

position:absolute(表示绝对定位)，将**元素从文档流中拖出来**，使用left、right、top、bottom属性相对于其最接近的一个具有定位属性的父包含块绝对定位。不存在则相对于body，即浏览器窗口。

如下面代码可以实现div元素相对于浏览器窗口移动div{

  width:200px;

  height:200px;

  border:2px red solid;

  position:absolute;

  left:100px; //右移

  top:50px; //下移

}

相对定位

position:relative。相对于以前的位置移动，偏移前的位置**保留**

固定定位

相对移动的坐标是**视图（屏幕内的网页窗口）**本身。不随浏览器窗口的滚动条滚动变化

 

Relative与Absolute组合使用

使用position:absolute可以实现被设置元素相对于浏览器（body）设置定位以后，大家有没有想过可不可以相对于其它元素进行定位呢？答案是肯定的，当然可以。使用position:relative来帮忙，但是必须遵守下面规范：

1、参照定位的元素必须是相对定位元素的前辈元素：

2、参照定位的元素必须加入position:relative;

3、定位元素加入position:absolute，便可以使用top、bottom、left、right来进行偏移定位了。

\#box2{

  position:absolute;

  top:20px;

  left:30px;     

}

这样box2就可以相对于父元素box1定位了（这里参照物就可以不是浏览器了）。

## 解决CSS浏览器兼容性问题的4种方案

**1. 浏览器CSS样式初始化**

**2. 浏览器私有属性**

在某个CSS的属性前添加一些前缀，比如-webkit-，-moz- ，-ms-，这些就是浏览器的私有属性

```
-webkit-transform:rotate(``-3``deg); ``/*为Chrome/Safari*/
```

**3. CSS hack**

**什么是CSS hack？**

不同浏览器对CSS的支持、解析不一样，呈现出不一致的页面展现效果。就需要针对不同的浏览器或不同版本写特定的CSS样式，我们把这个针对不同的浏览器/不同版本写相应的CSS code的过程，叫做CSS hack!

<u>简单的说：hack是针对不同的浏览器去写不同的CSS样式，从而让各浏览器能达到一致的渲染效果。</u>



通过在CSS样式中加入一些特殊的符号，让不同的浏览器识别不同的符号

比如`hack1{width:300px;_width:200px;}，`一般浏览器会先给元素使用`width:300px;`的样式，紧接着后面还有个`_width:200px;`由于下划线_width只有IE6可以识别，所以此样式在IE6中实际设置对象的宽度为200px

用来解决有些css属性在不同浏览器中显示的效果不一样的问题

例：条件hack

条件hack主要针对IE浏览器进行一些特殊的设置

```
<!--[if <keywords>? IE <version>?]> `` ``代码块，可以是html，css，js ``<![endif]-->
```



**4. 自动化插件**

Autoprefixer是一款自动管理浏览器前缀的插件，它可以解析CSS文件并且添加浏览器前缀到CSS内容里，使用Can I Use（caniuse网站）的数据来决定哪些前缀是需要的。

## 样式权重的优先级？

!important>内联样式> #id> .class>标签> \*>继承>默认      相同权重应用后来的

样式继承（权值最低）：文字样式

  border:1px,solid,red不继承

标签的权值为1，类选择符的权值为10，ID选择符的权值最高为100样式的权重可以叠加

层叠：后面的样式覆盖前面的样式。（内联样式表（标签内部）> 嵌入样式表（当前文件中）> 外部样式表（外部文件中）。）

## 浮动

### 	浮动布局

​	https://www.cnblogs.com/jing-tian/p/10987298.html

清楚浮动、BFChttps://segmentfault.com/a/1190000022016703

注意：设置浮动的同时一定要先设置块状元素的宽度，且需要浮动的几个元素宽度加起来小于容器元素的宽度。

**浮动元素引起的问题：**

**\1. 父元素的高度无法被撑开，影响与父元素同级的元素**

\2. 与浮动元素同级的非浮动元素会跟随其后浮动起来

\3. 若非第一个元素浮动，则该元素之前的元素也需要浮动，否则会影响页面显示的结构

### BFC、IFC、GFC 和 FFC

https://juejin.im/entry/6844903480801525773

**BFC**

https://zhuanlan.zhihu.com/p/37416267

https://segmentfault.com/a/1190000022016703

> 块级格式化上下文
> 一个独立的布局环境
>
> 其中的元素布局不受外界影响
>
> 并且在一个BFC中，块盒与行盒（行盒由一行中所有的内联元素所组成）都会垂直的沿着其父元素的边框排列。

BFC的布局规则**（特性）**

- 内部的Box会在垂直方向，一个接一个地放置。

- Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠。

- **计算BFC的高度时，浮动元素也参与计算。**

- BFC的区域不会与float box重叠。

- 每个盒子（块盒与行盒）的margin box的左边，与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。

  结论：BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。

**BFC的原理（BFC的渲染规则）？** 

1. 在 BFC 的垂直方向上，边距会发生重叠
2. BFC 区域不会与浮动区域重叠**（用来清除浮动）**
3. BFC 在页面上是一个独立的容器，与其他元素互不影响
4. 计算 BFC 高度时，浮动元素也会参与计算





1、**float**的值不是`none`。
2、**position**的值不是`static`或者`relative`。
3、**display**的值是`inline-block`、`table-cell`、`flex`、`table-caption`或者`inline-flex`
4、**overflow**的值不是`visible`

**使用场景**

1. 解决边距重叠问题

当元素都设置了margin边距时，margin将取最大值。 为了不让边距重叠，可以给子元素加一个父元素，并设置该元素为BFC：

```
<div>
    <p> xx</p>
    <div style="overflow:hidden;">
        <p> yy </p>
    </div>
</div>复制代码
```

2. **解决面积重合问题** **（利用BFC不会和float重叠的特性）**

```
<section id="layout">
    <div class='left'> </div>
    <div class='right'> <div>
</section>
<style>
    #layout{background-color:steelblue;}
    #layout .left{float:left;width:100px; height:100px; background-color:tomato;}
    #layout .right{height:120px; background-color:yellow;overflow:hidden}
</style>
```

\3. 解决清除浮动（清除浮动的原理）

**BFC的作用**

1.利用BFC避免margin重叠。https://blog.csdn.net/weixin_43288747/article/details/82860605?utm_medium=distribute.pc_relevant.none-task-blog-baidujs-2
2.自适应两栏布局  https://www.cnblogs.com/vicky123/p/8866548.html 左侧固定，右侧流动
**3.清除浮动。**

## 清除浮动

> 清除浮动主要是为了解决，父元素因为子级元素浮动引起的内部高度为0的问题。

### 清除浮动的方法

BFC能清除浮动

#### 1. 额外标签法

> 在最后一个浮动标签后，新加一个标签，给其设置clear：both；(不推荐)

**优点**：通俗易懂，方便
**缺点**：添加无意义标签，语义化差

```
<style>
        .div1 {
            background: #00a2d4;
        }
        .left {
            float: left;
            width: 200px;
            height: 200px;
            background: #9889c1;
        }
        .right {
            float: right;
            width: 200px;
            height: 200px;
            background: orangered;
        }
        .clear {
            clear: both;
        }
    </style>
</head>
<body>
<div class="div1">
    <div class="left">Left</div>
    <div class="right">Right</div>
    <div class="clear"></div>
</div>
<div class="div2"></div>
</body>
```

#### 2.父级添加overflow属性

> 通过触发BFC方式，实现清除浮动。（不推荐）

**优点**：代码简洁
**缺点**：内容增多的时候容易造成不会自动换行导致内容被隐藏掉，无法显示要溢出的元素

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .div1 {
            background: #00a2d4;
            overflow: hidden;
        }
        .left {
            float: left;
            width: 200px;
            height: 200px;
            background: #9889c1;
        }
        .right {
            float: right;
            width: 200px;
            height: 200px;
            background: orangered;
        }
    </style>
</head>
<body>
<div class="div1">
    <div class="left">Left</div>
    <div class="right">Right</div>
</div>
<div class="div2"></div>
</body>
</html>
```

#### 3.使用after伪元素清除浮动（推荐使用）

**优点**：符合闭合浮动思想，结构语义化正确。
**缺点**：ie6-7不支持伪元素：after，使用zoom:1触发hasLayout。

```
    <style>
        .div1 {
            background: #00a2d4;
        }
        .left {
            float: left;
            width: 200px;
            height: 200px;
            background: #9889c1;
        }
        .right {
            float: right;
            width: 200px;
            height: 200px;
            background: orangered;
        }
        .clearfix:after {          //伪元素
            content: ""; /*内容为空*/
            display: block; /*转换为块级元素*/
            height: 0; /*高度为0*/
            clear: both; /*清除浮动*/
            visibility: hidden; /*隐藏盒子*/
        }
        .clearfix {
            *zoom: 1; /*IE6\7的处理方式*/
        }
    </style>
</head>
<body>
<div class="div1 clearfix">
    <div class="left">Left</div>
    <div class="right">Right</div>
</div>
```

#### 4.使用before和after双伪元素清除浮动

**优点**：不仅可以清除浮动，也可以解决高度塌陷的问题（给父盒子添加类名clearfix）
**缺点**：用zoom:1触发hasLayout.

```js
    .clearfix:after,.clearfix:before{
        content: "";
        display: table;
    }
    .clearfix:after{
        clear: both;
    }

    .clearfix{
        *zoom: 1;
    }


 	<div class="fahter clearfix">
        <div class="big">big</div>
        <div class="small">small</div>
    </div>
    <div class="footer"></div>

```

## 三栏布局的七种实现方式

三栏布局：左右模块固定宽度，中间模块随浏览器变化自适应

https://zhuanlan.zhihu.com/p/25070186

见html文件

重点记 圣杯，双飞翼，flex

深入理解圣杯，双飞翼：https://juejin.im/post/5caf4043f265da039f0eff94

**几种布局方式** 

圣杯布局，双飞翼布局，Flex布局，绝对定位布局，表格布局，网格布局

圣杯布局，双飞翼布局：https://www.zhihu.com/question/21504052

## 居中

[https://blog.csdn.net/weixin_37580235/article/details/82317240#%E8%A1%8C%E5%86%85%E5%85%83%E7%B4%A0

### 水平居中



### 垂直居中

一.**行内元素**

单行：设行高  line-height

多行：给父元素设置**display:table-cell;**和**vertical-align: middle**

二.**块元素**

​	1. 首先设置父元素为相对定位，再设置子元素为绝对定位，设置子元素的**top: 50%**，即让子元素的左上角垂直居中；

**定高度：**设置绝对子元素的 margin-top: -元素高度的一半px;或者设置transform: translateY(-50%);

**不定高度：**利用css3新增属性**transform: translateY(-50%);**

2. flext   display  align-items: center

### 水平&垂直居中



## 伪类和伪元素

https://www.cnblogs.com/xmbg/p/11608268.html

**伪元素**

创建一些**不存在原有dom结构树种的元素**，例如：用::before和::after在一些存在的元素前后添加文字样式等，这些被添加的内容会以具体的UI显示出来，被用户所看到的，这些内容不会改变文档的内容，不会出现在DOM中，不可复制，仅仅是在CSS渲染层加入。CSS3中建议使用::表示伪元素，如：div::before。

- **伪元素匹配虚拟元素。它用于设置元素的 指定部分的样式**。我们经常使用类似“ :: after :: before :: first-line”。

CSS **伪类**

 是添加到选择器的关键字，<u>指定要选择的元素的特殊状态</u>。已存在的某个元素处于某种状态，但是通过dom树又无法表示这种状态，就可以通过伪类来为其添加样式。(动态设置元素样式)

例如a元素的:hover, :active(用户按下按键和松开按键之间的时间)等。。

CSS3中建议使用:表示伪元素，如：a:hover

## display, visibility, opacity

1. display：none，改变页面布局，可理解成在页面中把该元素删除。
2. visibility：hidden，占位，不触发该元素已经绑定的事件
3. opacity：0     占位， 触发

**补充：**

`z-index` 属性设定了一个定位元素及其后代元素或 flex 项目的 z-order。 当元素之间重叠的时候， z-index 较大的元素会覆盖较小的元素在上层进行显示。

| css                                                          | 特性                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| display: none;                                               | 不占据空间，无法点击                                         |
| visibility: hidden;                                          | 占据空间，无法点击                                           |
| position: absolute; top: -999em;                             | 不占据空间，无法点击                                         |
| position: relative; top: -999em;                             | 占据空间，无法点击                                           |
| position: absolute; visibility: hidden;                      | 不占据空间，无法点击                                         |
| height: 0; overflow: hidden;                                 | 不占据空间，无法点击                                         |
| opacity: 0; filter:Alpha(opacity=0);                         | 占据空间，可以点击                                           |
| position: absolute; opacity: 0; filter:Alpha(opacity=0);     | 不占据空间，可以点击                                         |
| zoom: 0.001;-moz-transform: scale(0); -webkit-transform: scale(0); -o-transform: scale(0);transform: scale(0); | IE6/IE7/IE9不占据空间，IE8/FireFox/Chrome/Opera占据空间。都无法点击 |

## js的各种位置，比如clientHeight,scrollHeight,o...

请你讲一下对于js中各种位置的理解，比如clientHeight,scrollHeight,offsetHeight ,以及scrollTop, offsetTop,clientTop的各自表示什么，它们的区别是什么？

## flex布局***

移动端**

灵活性

***容器需开启display: flex, 创建容器，里面的子元素称为*flex item(伸缩项目)

**容器属性**

> - flex-direction     决定主轴的方向（即项目的排列方向） 默认水平从左向右
> - flex-wrap     如果一条轴线排不下，如何换行  默认不换行44
> - flex-flow     flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap
> - justify-content     项目在主轴上的对齐方式。flex-start（默认值）主轴起点对齐
> - align-items       属性定义项目在交叉轴上如何对齐。 flex-start（默认值）
> - align-content     多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

**项目属性**

1.  order  越小排列越靠前 默认0

2. flex-grow    项目的放大比例，默认为0，即如果存在剩余空间，也**不放大**。

3. flex-shrink     项目的缩小比例，默认为1，即如果空间不足，该项目**将缩小**

4. flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。默认值为auto，项目的本来大小。

5. flex:  flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。

   当 `flex` 取值为一个非负数字，则该数字为 `flex-grow` 值，`flex-shrink` 取 1，`flex-basis` 取 0%

   flex取值详细：

   https://www.jianshu.com/p/57a94430dcbe

6. align-self    允许单个项目有与其他项目不一样的对齐方式。默认值auto，继承父元素的align-items属性

应用案例：https://blog.csdn.net/DFF1993/article/details/81201773

1. 类似圣杯布局效果      中间、内容区可缩放（flex-grow ，flex-shrink），两边flex设不可缩放
2. 类似双飞翼布局效果   

圣杯布局和双飞翼布局基本上是一致的，都是两边固定宽度，中间自适应的三栏布局，**其中，中间栏放到文档流前面，保证先行渲染。**（flex的order让左边排在前面）

圣杯布局(float + 负margin + padding + position)      双飞翼布局(float + 负margin + margin)

3. 垂直居中

   ```js
   //容器样式：
   display: flex;
   justify-content: space-around;
   align-items: center;    //垂直居中
   height: 500px;  /*需设置高度，垂直方向的设置才有效*/
   ```

补充：文本居中：只需要简单地把 `line-height` 设置为那个对象的 `height` 值就可以使文本居中了（或者用flex）

## CSS3有什么样的新特性？

· 圆角（border-radius:8px）阴影（box-shadow:10px）   文字特效（text-shadow）

![阴影效果!](https://www.runoob.com/images/text_shadow_effect.gif)

· 线性渐变

· 转换 transform（对元素进行移动、缩放、转动、拉长或拉伸  2d,3d ）

· 过渡     元素从一种样式逐渐改变为另一种的效果

· **CSS3 动画**

· 弹性盒子布局flex

· 更多CSS选择器

### CSS3动画

https://www.w3school.com.cn/css3/css3_animation.asp

需 @keyframes 规则。在 @keyframes 中规定某项 CSS 样式，就能创建由当前样式逐渐改为新样式的动画效果

1. 不同浏览器创建动画前缀不同。例：

```
@keyframes myfirst
{
from {background: red;}
to {background: yellow;}
}
@-moz-keyframes myfirst /* Firefox */
{
from {background: red;}
to {background: yellow;}
}

@-webkit-keyframes myfirst /* Safari 和 Chrome */
{
from {background: red;}
to {background: yellow;}
}
```

\2. 在 @keyframes 中创建动画时，请把它捆绑到某个选择器，否则不会产生动画效果。

通过规定至少动画的名称，时长，即可将动画绑定到选择器：

**实例**

把 "myfirst" 动画捆绑到 div 元素，时长：5 秒：

```
div
{
animation: myfirst 5s;
-moz-animation: myfirst 5s;     /* Firefox */
-webkit-animation: myfirst 5s;  /* Safari 和 Chrome */
-o-animation: myfirst 5s;       /* Opera */
}
```

\3. 用百分比来规定变化发生的时间，或用关键词 "from" 和 "to"，等同于 0% 和 100%。0% 是动画开始，100% 是动画完成。

**实例**

当动画为 25% 及 50% 时改变背景色，然后当动画 100% 完成时再次改变：

改变背景色和位置：

@keyframes myfirst

{

0%  {background: red; left:0px; top:0px;}

25% {background: yellow; left:200px; top:0px;}

50% {background: blue; left:200px; top:200px;}

75% {background: green; left:0px; top:200px;}

100% {background: red; left:0px; top:0px;}

}

\4. 常见属性

| [@keyframes](https://www.w3school.com.cn/cssref/pr_keyframes.asp) | 规定动画。                                               |      |
| ------------------------------------------------------------ | -------------------------------------------------------- | ---- |
| [animation](https://www.w3school.com.cn/cssref/pr_animation.asp) | 所有动画属性的简写属性，除了 animation-play-state 属性。 | 3    |
| [animation-name](https://www.w3school.com.cn/cssref/pr_animation-name.asp) | 规定  @keyframes 动画的名称。                            | 3    |
| [animation-duration](https://www.w3school.com.cn/cssref/pr_animation-duration.asp) | 规定动画完成一个周期所花费的秒或毫秒。默认是 0。         | 3    |
| [animation-timing-function](https://www.w3school.com.cn/cssref/pr_animation-timing-function.asp) | 规定动画的速度曲线。默认是 "ease"。                      | 3    |
| [animation-delay](https://www.w3school.com.cn/cssref/pr_animation-delay.asp) | 规定动画何时开始。默认是 0。                             | 3    |
| [animation-iteration-count](https://www.w3school.com.cn/cssref/pr_animation-iteration-count.asp) | 规定动画被播放的次数。默认是 1。                         | 3    |
| [animation-direction](https://www.w3school.com.cn/cssref/pr_animation-direction.asp) | 规定动画是否在下一周期逆向地播放。默认是 "normal"。      | 3    |
| [animation-play-state](https://www.w3school.com.cn/cssref/pr_animation-play-state.asp) | 规定动画是否正在运行或暂停。默认是 "running"。           | 3    |
| [animation-fill-mode](https://www.w3school.com.cn/cssref/pr_animation-fill-mode.asp) | 规定对象动画时间之外的状态。                             |      |

## em和rem,rpx

- `rem`是相对于html根元素的`em`， 
- `em`是相对长度单位，相对于当前对象内文本的字体尺寸。如当前对行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸。事实上，根据W3标准，em单位是相对于使用em单位的元素的字体大小。父元素的字体大小可以影响em值是因为继承。
- 总结：`rem`单位翻译为像素值是由html元素的字体大小决定的。此字体大小会被浏览器中字体大小的设置影响，除非显式重写一个具体单位。`em`单位转为像素值，取决于他们使用的字体大小。 此字体大小受从父元素继承过来的字体大小，除非显式重写与一个具体单位。

2、em会继承父级元素的字体大小（参考物是父元素的font-size；）；

3、em中所有的字体都是相对于父元素的大小决定的；所以如果一个设置了font-size:1.2em的元素在另一个设置了font-size:1.2em的元素里，而这个元素又在另一个设置了font-size:1.2em的元素里，那么最后计算的结果是1.2X1.2X1.2=1.728em (**会叠加**)

**rpx**

> rpx（responsive pixel）: 可以根据屏幕宽度进行自适应。规定屏幕宽为750rpx。如在 iPhone6 上，屏幕宽度为375px，共有750个物理像素，则750rpx = 375px = 750物理像素，1rpx = 0.5px = 1物理像素。
> rpx 为小程序中使用的相对单位，用法和rem类似， 1rpx = 屏幕宽度/750 px, 所以在屏幕宽度为750的设计稿中，1rpx = 1px。

### px、em、rem、rpx 用法 与 区别

https://blog.csdn.net/qq_36208461/article/details/80665125

###  rel”是什么意思`<link href=" " rel="stylesheet"/>`？

<link>中的<u>“ rel”代表HTML文档和外部链接文件之间的**关系**</u>。“ rel”的值包括样式表，索引，节，帮助，书签，next，prev，版权等。

### `<link>`VS的外部CSS`@import`` `

· **link**功能较多**，可以定义 RSS，定义 Rel 等作用，而**@import只能用于加载 CSS

· 当解析到**link**时，页面会**同步加载**所引的 css，而**@import**所引用的 css 会**等**到页面加载完才被加载

· **@import**需要 **IE5** **以上**才能使用

· link可以使用 js 动态引入，@import不行

### 其他

#### 溢出部分省略号

```css
overflow:hidden; //超出的文本隐藏

text-overflow:ellipsis; //溢出用省略号显示

white-space:nowrap; //溢出不换行
```

**//上面是一行内容的时候，当有两行的内容时候**

```css
overflow: hidden;

text-overflow: ellipsis;

display:-webkit-box; //作为弹性伸缩盒子模型显示。

-webkit-box-orient:vertical; //设置伸缩盒子的子元素排列方式--从上到下垂直排列

-webkit-line-clamp:2; //显示的行
```

#### **相邻块元素垂直外边距的合并**

（外边距塌陷）

上下相邻的两个块元素相遇，上面的元素有下外边距margin-bottom，下面的元素有上外边距margin-top，<u>它们间的垂直间距不是margin-botton和margin-top之和，而是取较大者。</u>

#### **嵌套块元素垂直外边距的合并**

当块级元素进行嵌套时，如果父盒子没有设置上边框和上内边距的话，子盒子的上外边距和父盒子的上外边距会进行合并。合并后的外边距为两者中的较大者，即使父元素的上外边距为0，也会发生合并。

如果希望外边距不合并，那么可以为父元素定义1像素的上边框或上内边距。

#### CSS绘制基本图形

圆形

1 #circle 

{width: 100px; 

 height: 100px;4  

background: red;

-moz-border-radius: 50px;  

 -webkit-border-radius: 50px;   

border-radius: 50px; }

 

三角形、梯形

此时元素由上下左右4个三角形“拼接”而成；那么，为了实现最终的效果，即保留最下方的三角形，只需要把其它border边的颜色设置为**白色**或**透明色**：

```
div {
    width: 0;
    height: 0;
    border: 40px solid;
    border-color: transparent transparent red;}  //上，左右，下
```

​                               

不过，被“隐藏”的上border仍然占据着空间，要想使得绘制出的三角形尺寸最小化，还需要将上border的宽度设置为0（其它情况同理）：

```
div {
    width: 0;
    height: 0;
    border-width: 0 40px 40px;
    border-style: solid;
    border-color: transparent transparent red;}
```

 ![image-20201031161359981](C:\Users\sprina\AppData\Roaming\Typora\typora-user-images\image-20201031161359981.png)

 

 ![image-20201031161425123](C:\Users\sprina\AppData\Roaming\Typora\typora-user-images\image-20201031161425123.png)

.test1{

  width: 0;

  height: 0;

  border-right: 50px solid transparent;

  border-bottom: 50px solid blue;

  border-left: 50px solid transparent;

}

有宽高时可绘制梯形

 ![image-20201031161438123](C:\Users\sprina\AppData\Roaming\Typora\typora-user-images\image-20201031161438123.png)

若要绘制直角三角形，则将其中两个方向的 border 设置成“transparent”，例如把 border-top 和 border-right 设置成透明的，

例：

div{
   margin: auto;
   height: 0px;
   width: 0px;
   border-top:50px solid transparent ;
   /*不设置颜色，默认黑色*/
   border-right:50px solid blue;
   border-bottom: 50px solid blue;
   border-left: 50px solid transparent;
 }

效果：

 ![image-20201031161449336](C:\Users\sprina\AppData\Roaming\Typora\typora-user-images\image-20201031161449336.png)

响应式图片的CSS
 根据图片所在容器的大小来实时地按比例缩放。设置img标签的width:100%即可。

img {

width:100%;}

如果 max-width 属性设置为 100%, 图片永远不会大于其原始大小：

img {

max-width: 100%;}

#### 标准化CSS VS重置CSS

。(Reset重置浏览器提供的的css默认属性，统一起始开发。浏览器的品种不同，样式不同，然后重置，让他们统一)规范化只是对一些常见错误的更正。

![image-20201031161614054](C:\Users\sprina\AppData\Roaming\Typora\typora-user-images\image-20201031161614054.png)