---
title: css 阴影的奇技淫巧
date: 2019-10-15~ 09:56:48
tags: css
categories: web
---

box-shadow 在前端的 CSS 编写工作想必十分常见。但是 box-shadow 除去它的常规用法，其实还存在许多不为人知的奇技淫巧。
## box-shadow 常规用法

### 基础属性语法

`box-shadow` 属性向框添加一个或多个阴影。
** box-shadow: h-shadow v-shadow blur spread color inset; **
像这样 `box-shadow: 10px 10px 5px #888888;` 除此之外，我们要知道，box-shadow 是分外 shadow 和内 shadow 的，内阴影即是在属性上添加 inset 。

我们再看看 box-shadow 有什么其它妙用。

### box-shadow 模拟多边框

通常而言，我们会给许多元素添加边框 `border`，但是当如果需要多重边框，这个时候，由于 `border` 单重的限制，box-shadow 就可以闪亮登场了。我们可以轻松的使用外阴影或者内阴影来模拟边框效果。

<div style="margin: 50px auto;width: 200px;height: 100px;background: deeppink;box-shadow: inset 0 0 0 6px #fff,0 0 0 10px #333, 0 0 0 15px #aaa, 0 2px 5px 15px #666;"></div>

``` bash
<div></div>
div {
  margin: 50px auto;
  width: 200px;
  height: 100px;
  background: deeppink;
  box-shadow: 
    inset 0 0 0 6px #fff,
    0 0 0 10px #333, 
    0 0 0 15px #aaa, 
    0 2px 5px 15px #666;
}
```

可以看到，由内至外，这里利用 box-shadow ，设置了白色、黑色、灰色三层边框及最外层带模糊的阴影。

box-shadow 有一个参数是设置 blur 的，即是模糊的距离，在上面的例子中，即是第二重阴影 0 0 0 10px #333, 中的第三个 0 ，当 blur 的值为 0 ，那么阴影本身是不会模糊的，那么就很容易模拟出边框的效果。

而且，元素可以设置多重阴影，并且它们存在层叠关系，离 box-shadow 最近设置的层叠优先级最高，依次递减，这个对照代码很好理解。

当然，值得注意的是：

- 阴影并不是边框，它们并不占有实际的空间，也不能归属于 `box-sizing` 的范围。不过，你可以通过使用内边距或外边距（取决于阴影是内部的还是外部的）占据额外的空间来模拟。

- 上述示例模拟的边框是位于元素外部的。它不能捕获类似悬停和点击的鼠标事件。如果事件很重要，那么可以通过添加 inset 关键字让阴影出现在元素的内部。注意，你可能需要添加额外的内边距来扩充空间。

### box-shadow 模拟半透明遮罩层

很多时候，我们需要用到类似下图这样的遮罩层，通过半透明遮罩层把背景调暗，凸显某些 UI 组件，提升用户体验。
![blockchain](/uncleKim/images/cssBoxShadow.png)

常规的做法通常都会用到一个额外的元素，用作遮罩层，至少也是一个伪元素， before 或者 after。
不考虑低版本的兼容性的话，其实用 box-shadow 也可以模拟遮罩层这种效果。


<div style="width:200px;line-height:200px;text-align:center;background:#fff;margin:50px auto;box-shadow: 0 0 0 50px rgba(0, 0, 0, .5);">box-shadow模拟遮罩层</div>


``` bash
<div id="bg">我是背景我是背景我是背景我是背景我是背景我是背景</div>
<div id="foo">box-shadow模拟遮罩层</div>

#foo{
  width:200px;
  line-height:200px;
  text-align:center;
  background:#fff;
  margin:50px auto;
  box-shadow: 0 0 0 1920px rgba(0, 0, 0, .5);
}
```

当然，值得注意的是：

+ 使用这种方法不可避免的需要考虑兼容性，IE9+、Firefox、Chrome、Opera 以及 Safari 5.1.1 支持 box-shadow 属性。

+ 由于每个人的浏览器视口大小不一致，为了所有情况下 box-shadow 生成的阴影都能覆盖整个页面，可能需要将阴影的尺寸 spread 设置的很大。

+ 如果你真的想尝试这个方法，box-shadow 从性能角度而言属于 耗性能样式，不同样式在消耗性能方面是不同的，box-shadow 从渲染角度来讲十分耗性能，原因就是与其他样式相比，它们的绘制代码执行时间过长。虽然有 GPU 的 3D 加速，但是具体使用的时候还是值得斟酌考虑。不过你要知道，没有不变的事情，在今天性能很差的样式，可能明天就被优化，并且浏览器之间也存在差异。

### 多重 box-shadow 之简单图形

从本质上讲，box-shadow 是将自身投影到另一个地方，在很多情况下，我们是可以利用 box-shadow 来复制自身的！
我们来实现一个云朵的图形
其中的云层，就是利用了 多重box-shaodw 在一个伪元素内生成的。

<div style="width:100px;height:100px;margin:50px auto;background:deeppink;border-radius:50%;box-shadow:120px 0px 0 -10px #795548,95px 20px 0 0px #607D8B,30px 30px 0 -10px green,90px -20px 0 0px #FFC107,40px -40px 0 0px #2196F3;display：inline-block"></div>

<div style="width:100px;height:100px;margin:50px auto;background:#999;border-radius:50%;box-shadow:120px 0px 0 -10px #999,95px 20px 0 0px #999,30px 30px 0 -10px #999,90px -20px 0 0px #999,40px -40px 0 0px #999;display：inline-block;margin-left:200px"></div>

``` bash
<div></div>

div{
  width:100px;
  height:100px;
  margin:50px auto;
  background:deeppink;
  border-radius:50%;
  box-shadow:
    120px 0px 0 -10px #795548,
    95px 20px 0 0px #607D8B,
    30px 30px 0 -10px green,
    90px -20px 0 0px #FFC107,
    40px -40px 0 0px #2196F3;
}
```

### 多重 box-shadow 实现立体感

这种方法运用在 text-shadow 上同样可以，可以实现文字的立体感。

运用多重 box-shadow ，不断偏移 1px ，就可以产生十分立体的感觉。

<div style="width:200px;line-height:68px;text-align:center;font-size:16px;margin:50px auto;color:#fff;background:hsl(0, 0%, 60%);cursor:pointer;box-shadow:1px 1px 0 0 hsl(0, 0%, 65%),2px 2px 0 0 hsl(0, 0%, 70%),3px 3px 0 0 hsl(0, 0%, 75%),4px 4px 0 0 hsl(0, 0%, 80%),5px 5px 0 0 hsl(0, 0%, 85%);"> 立体按钮 </div>

``` bash
<div> 立体按钮 </div>

div{
  width:200px;
  line-height:68px;
  text-align:center;
  font-size:16px;
  margin:50px auto;
  color:#fff;
  background:hsl(0, 0%, 60%);
  cursor:pointer;
  box-shadow:
    1px 1px 0 0 hsl(0, 0%, 65%),
    2px 2px 0 0 hsl(0, 0%, 70%),
    3px 3px 0 0 hsl(0, 0%, 75%),
    4px 4px 0 0 hsl(0, 0%, 80%),
    5px 5px 0 0 hsl(0, 0%, 85%);
}
```

<div style="width:200px;line-height:68px;text-align:center;font-size:48px;margin:50px auto;color:hsl(0, 0%, 60%);text-shadow:1px 1px 0 hsl(0, 0%, 65%),2px 2px 0 hsl(0, 0%, 70%),3px 3px 0 hsl(0, 0%, 75%),4px 4px 0 hsl(0, 0%, 80%),5px 5px 0 hsl(0, 0%, 85%);">Shadow</div>

``` bash
<div>Shadow</div>

div{
  width:200px;
  line-height:68px;
  text-align:center;
  font-size:48px;
  margin:50px auto;
  color:hsl(0, 0%, 60%);
  text-shadow:
    1px 1px 0 hsl(0, 0%, 65%),
    2px 2px 0 hsl(0, 0%, 70%),
    3px 3px 0 hsl(0, 0%, 75%),
    4px 4px 0 hsl(0, 0%, 80%),
    5px 5px 0 hsl(0, 0%, 85%);
}
```

*box-shadow效果的本质实际上是对自身的复制*

## filter:drop-shadow

其实说到 box-shadow，就不得不提到另一个和它极为相似的 CSS3 新属性 filter:drop-shadow，filter 即是 CSS 滤镜，可以在元素呈现之前，为元素的渲染提供一些效果，如模糊、颜色转移之类的。滤镜常用于调整图像、背景、边框的渲染。

当然这里我们只说 filter:drop-shadow。

### 与box-shadow比较
#### 兼容性比较
CSS3 box-shadow从IE9浏览器开始就支持了，兼容性如下截图：
![blockchain](/uncleKim/images/boxShadow.png)

而filter中的drop-shadowIE13才开始支持，移动端Android4.4才开始支持，细想一下，其实离在移动端愉快使用就差一口气。
兼容性如下图：
![blockchain](/uncleKim/images/css3Filter.png)

#### 同样的参数值，表现效果有差异

filter中的drop-shadow语法如下：
`filter: drop-shadow(x偏移, y偏移, 模糊大小, 色值);`
eg:`filter:drop-shadow(5px 5px 10px black)`
表示右下5像素偏移，10像素模糊的黑色阴影。
`box-shadow: 5px 5px 10px black;`
*filter*
<img src="/uncleKim/images/test.jpg" width="100px" style="filter:drop-shadow(5px 5px 10px black);border:none">
*box-shadow*
<img src="/uncleKim/images/test.jpg" width="100px" style="box-shadow: 5px 5px 10px black;border:0;padding:0">

#### drop-shadow没有内阴影效果

box-shadow支持inset内阴影，如：

`box-shadow: inset 5px 5px 10px black;`
但是，drop-shadow却没有。

#### drop-shadow不能阴影叠加

box-shadow有个超屌的特性，就是阴影可以任意累加，因此，理论上我们可以使用box-shadow生成任意的图片。

<div style="box-shadow: 11px 8px #626262,11px 9px #626262,11px 10px #626262,11px 11px #626262,11px 12px #626262,11px 13px #626262,11px 14px #626262,4px 15px #626262,5px 15px #626262,6px 15px #626262,7px 15px #626262,8px 15px #626262,9px 15px #626262,10px 15px #626262,11px 15px #626262,12px 15px #626262,13px 15px #626262,14px 15px #626262,15px 15px #626262,16px 15px #626262,17px 15px #626262,18px 15px #626262,11px 16px #626262,11px 17px #626262,11px 18px #626262,11px 19px #626262,11px 20px #626262,11px 21px #626262,11px 22px #626262;width:1px;height:1px;"></div>

<div style="position: absolute; margin: -5px 0 0 9px; height: 4px; border-left: 1px solid #bdbdbd; box-shadow: 1px 1px #bdbdbd, 2px 2px #bdbdbd, 3px 3px #bdbdbd, 4px 4px #bdbdbd, 5px 5px #bdbdbd, -1px 1px #bdbdbd, -2px 2px #bdbdbd, -3px 3px #bdbdbd, -4px 4px #bdbdbd, -5px 5px #bdbdbd;"></div>
<div></div>
用box-shadow实现的图片

[一个牛逼的demo](https://www.zhangxinxu.com/study/201311/box-shadow-convert-to-image.html)

canvas上下文有个getImageData方法，可以获取画布每一个像素点的颜色信息、透明度信息。于是，利用drawImage将无名小主靓照原封不动迁到画布上，再利用getImageData获得的像素点信息，转换成box-shadow值，然后呈现之。

理论上，任意的图片，box-shadow都可以呈现。

但是filter中的drop-shadow就只能抱歉了，我就是一锤子买卖。没钱也这么任性！

说到现在，体现的尽是drop-shadow的不好，兼容性不够，内阴影不支持，多阴影也不支持。

真的是这样吗？显然非也！所谓存在既有道理。

drop-shadow有一个很厉害的特性，也就这一个特性，让其以后有足够的机会大放异彩！那就是，drop-shadow才是真正意义上的投影，而box-shadow只是盒阴影而已。

什么意思呢？

#### 阴影 vs 盒阴影

实践出真知，下面我们用CSS border写一个虚线框，例如：
`border: 10px dashed #beceeb;`
结果长相如下：
<div style="width:50px; height:50px;border: 10px dashed #beceeb;"></div>

然后，我们分别应用box-shadow和drop-shadow滤镜：

`border: 10px dashed #beceeb; box-shadow: 5px 5px 10px black;`
`border: 10px dashed #beceeb; filter: drop-shadow(5px 5px 10px black);`
结果：
<div style="width:50px; height:50px;border: 10px dashed #beceeb; box-shadow: 5px 5px 10px black;"></div>
<div style="width:50px; height:50px;border: 10px dashed #beceeb; filter: drop-shadow(5px 5px 10px black);"></div>

怎么样？是不是本性暴露了！

box-shadow顾名思意“盒阴影”，只是盒子的阴影；你想啊，这盒子中间明明是透明的，结果，阴影的时候，居然光线没有穿透；但是drop-shadow就符合真实世界的投影，非透明的颜色，我就有投影；透明部分，光线穿过，没投影，而什么盒子不盒子的，跟我没有任何关系。

drop-shadow不仅可以穿透代码构建的元素的透明部分，PNG图片的透明部分也是可以穿透的，如下图：

<img src="/uncleKim/images/cactus.png" width="100px"  style="filter:drop-shadow(5px 5px 10px black);border:none;border: 1px solid #beceeb;">

于是，曾经困扰我们的一些老大难的问题就有了很好的解决思路了！

### drop-shadow的实际应用

#### 带有箭头指向的浮层面板

<div style="background: #f7f7f7;height: 693px;">
<div class="box box-shadow" style=" margin: 40px; padding: 50px;background-color: #fff;position: relative;font-size: 24px;box-shadow: 5px 5px 10px black;">
	<i class="cor" style="position: absolute;left: -40px;width: 0; height: 0;overflow: hidden;border: 20px solid transparent;border-right-color: #fff;"></i>
	box-shadow
</div>
<div class="box drop-shadow" style=" margin: 40px; padding: 50px;background-color: #fff;position: relative;font-size: 24px;filter: drop-shadow(5px 5px 10px black);">
	<i class="cor" style="position: absolute;left: -40px;widtd: 0; height: 0;overflow: hidden;border: 20px solid transparent;border-right-color: #fff;"></i>
	filter: drop-shadow
</div>
</div>

``` bash
.box {
    margin: 40px; padding: 50px;
    background-color: #fff;
    position: relative;
    font-size: 24px;
}
.cor {
    position: absolute;
    left: -40px;
    widtd: 0; height: 0;
    overflow: hidden;
    border: 20px solid transparent;
    border-right-color: #fff;
}
.box-shadow {
    box-shadow: 5px 5px 10px black;
}
.drop-shadow {
    filter: drop-shadow(5px 5px 10px black);
}
HTML代码：
<div class="box box-shadow">
    <i class="cor"></i>
    box-shadow
</div>
<div class="box drop-shadow">
    <i class="cor"></i>
    filter: drop-shadow
</div>
```
#### PNG格式小图标

<i class="icon" style="display: inline-block;width: 20px; height: 20px;overflow: hidden;"><i class="icon icon-del" style="position: relative;left: -20px;border-right: 20px solid transparent;-webkit-filter: drop-shadow(rgb(51, 204, 0) 20px 0);filter: drop-shadow(rgb(51, 204, 0) 20px 0);background: url(/uncleKim/images/delete.png) no-repeat;"></i></i>

原理其实很简单，使用了CSS3滤镜filter中的drop-shadow，drop-shadow滤镜可以给元素或图片非透明区域添加投影。

在Chrome浏览器下，drop-shadow有一个如下的呈现特性：
>在Chrome浏览器下，如果一个元素的主体部分，无论以何种方式，只要在页面中不可见，其drop-shadow是不可见的；实体部分哪怕有1像素可见，则drop-shadow完全可见。

## 各种各样的阴影

### 单侧阴影

先说单侧投影，关于 box-shadow，大部分时候，我们使用它都是用来生成一个两侧的投影，或者一个四侧的投影。如下：
![blockchain](/uncleKim/images/dancetouying.png)
OK，那如果要生成一个单侧的投影呢？

我们来看看 box-shadow 的用法定义：
``` bash
{
    box-shadow: none | [inset? && [ <offset-x> <offset-y> <blur-radius>? <spread-radius>? <color>? ] ]#
}
```
以 box-shadow: 1px 2px 3px 4px #333 为例，4 个数值的含义分别是，x 方向偏移值、y 方向偏移值 、模糊半径、扩张半径。

这里有一个小技巧，扩张半径可以为负值。

继续，如果阴影的模糊半径，与负的扩张半径一致，那么我们将看不到任何阴影，因为生成的阴影将被包含在原来的元素之下，除非给它设定一个方向的偏移量。所以这个时候，我们给定一个方向的偏移值，即可实现单侧投影：
![blockchain](/uncleKim/images/danceyinying2.png)

### 投影背景 / 背景动画

很明显，0 = -0，所以当 box-shadow 的模糊半径和扩张半径都为 0 的时候，我们也可以得到一个和元素大小一样的阴影，只不过被元素本身遮挡住了，我们尝试将其偏移出来。
CSS代码如下：

```
div {
    width: 80px;
    height: 80px;
    border: 1px solid #333;
    box-sizing: border-box;
    box-shadow: 80px 80px 0 0 #000;
}
```
得到如下结果：

<div style="width: 80px;height: 80px;border: 1px solid #333;box-sizing: border-box;box-shadow: 80px 80px 0 0 #000;"></div>
<div style="height:80px;"></div>
有什么用呢？好像没什么意义啊。

额，确实好像没什么用。不过我们注意到，box-shadow 是可以设置多层的，也就是多层阴影，而且可以进行过渡变换动画（补间动画）。但是 background-image: linear-gradient()，也就是渐变背景是不能进行补间动画的。

![blockchain](/uncleKim/images/jinbian.png)
用 box-shadow，实现它的 CSS 代码如下（可以更简化）：
```
.shadow {
    position: relative;
    width: 250px;
    height: 250px;
}
 
.shadow::before {
    content: "";
    position: absolute;
    width: 50px;
    height: 50px;
    top: -50px;
    left: -50px;
    box-shadow:
        50px 50px #000, 150px 50px #000, 250px 50px #000,
        50px 100px #000, 150px 100px #000, 250px 100px #000,
        50px 150px #000, 150px 150px #000, 250px 150px #000,
        50px 200px #000, 150px 200px #000, 250px 200px #000,
        50px 250px #000, 150px 250px #000, 250px 250px #000;
}
```
用渐变来实现的话，只需要这样：
```
.gradient {
    width: 250px;
    height: 250px;
    background-image: linear-gradient(90deg, #000 0%, #000 50%, #fff 50%, #fff 100%);
    background-size:  100px 100px;
}
```
[CodePen Demo -- box-shadow实现背景动画](https://codepen.io/Chokcoco/pen/WaBYZL)
[CodePen Demo -- CSS Checker Illusion( By David Khourshid )](https://codepen.io/davidkpiano/pen/LVzxPV)

嗯，很有意思，就是实际用途可能不大。

### 立体投影

[CodePen Demo -- 立体投影](https://codepen.io/Chokcoco/pen/LgdRKE?editors=1100)

所以总结一下：

立体投影的关键点在于利于伪元素生成一个大小与父元素相近的元素，然后对其进行 rotate 以及定位到合适位置，再赋于阴影操作
颜色的运用也很重要，阴影的颜色通常比本身颜色要更深，这里使用 hsl 表示颜色更容易操作，l 控制颜色的明暗度
还有其他很多场景：

### 文字立体投影 / 文字长阴影

[CodePen Demo -- 立体文字阴影](https://codepen.io/Chokcoco/pen/JmgNNa)
[线性渐变配合阴影实现条纹立体阴影条纹字](https://codepen.io/Chokcoco/pen/XxQJEB?editors=1100)

### 使用 box-shadow 实现的灯光效果

[CodePen Demo -- box-shadow实现霓虹氖灯文字效果](https://codepen.io/Chokcoco/pen/WaLdwX)

[CodePen Demo -- 使用box-shadow实现阴影灯光show](https://codepen.io/Chokcoco/pen/ReOgvq)


引用：
>https://www.cnblogs.com/coco1s
>https://www.zhangxinxu.com/
>公众号：前端早读课

<!-- 推荐：
>http://www.ruanyifeng.com/blog/
>https://www.liaoxuefeng.com/
>https://github.com/chokcoco/CSS-Inspiration
>https://vuepress.vuejs.org/zh/
>http://mi-dev.shalltry.com:90/ssp-docs/zh/sdk/
>https://hexo.io/zh-cn/
>http://zhangwenli.com/ -->