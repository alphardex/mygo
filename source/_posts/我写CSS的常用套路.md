title: 我写 CSS 的常用套路
author: alphardex
tags: []
categories: []
abbrlink: 17061
date: 2019-12-28 17:23:00

---

# 前言

本文是笔者写 CSS 时常用的套路。不论效果再怎么华丽，万变不离其宗。

<!--more-->

# 交错动画

![1702ee051447ef55~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/vAiVQzrM56Ix2OJ.gif)

有时候，我们需要给多个元素添加同一个动画，播放后，不难发现它们会一起运动，一起结束，这样就会显得很平淡无奇。

那么如何将动画变得稍微有趣一点呢？很简单，既然它们都是同一时刻开始运动的，那么让它们不在同一时刻运动不就可以了吗。如何让它们不在同一时刻运动呢？注意到 CSS 动画有延迟（`delay`）这一属性。举个栗子，比如有十个元素播放十个动画，将第二个元素的动画播放时间设定为比第一个元素晚 0.5 秒（也就是将延迟设为 0.5 秒），其他元素以此类推，这样它们就会错开来，形成一种独特的视觉效果。

![1702ee0514569bd2~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/rla8w1jDdo5xYkC.gif)

这就是所谓的交错动画：通过设置不同的延迟时间，达到动画交错播放的效果。

本 demo 地址：[Staggered Wave Loading](https://codepen.io/alphardex/pen/XWWWBmQ)

## 用 JS 分割文本

还有一种经常用到的玩法：用 JS 将句子或单词分割成字母，并给每个字母加上不同延时的动画，同样也很华丽

![6df36dee47cf479ba5e911a05ecf7a7b~tplv-k3u1fbpfcp-zoom-1.gif](https://s2.loli.net/2024/05/22/KkuphtmaOES3D1n.gif)

本 demo 地址：[Staggered LandIn Text](https://codepen.io/alphardex/full/KKwvKGY)

## 不同位置的交错动画

一般我们都是从第一个元素开始交错的。但如果要从中间元素开始交错的话，就要给当前元素的延时各加上一个值，这个值就是中间元素的下标到当前元素的下标的距离（也就是下标之差的绝对值）与步长的乘积

![1702ee051b0e892b~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/zIraCNRjeSHio6k.gif)

本 demo 地址：[Reveal Text](https://codepen.io/alphardex/full/eYYMYXJ)

## 随机粒子动画

说到随机性，我们可以实现一种更疯狂的效果：给几百个粒子添加交错动画，并且交错时间随机，位置大小也都是随机。如此一来我们就能用纯 CSS 模拟出下雪的效果。

![1702ee051cbfc17a~tplv-t2oaga2asx-image.png](https://s2.loli.net/2024/05/22/jWh7egcS95CoxrO.png)

本 demo 地址：[Snow (Pure CSS)](https://codepen.io/alphardex/full/dyPorwJ)

# 伪类和伪元素

## 伪类

![1702ee051cf6ac5e~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/Fhr2INqpJmHiMac.gif)

HTML 元素的状态是可以动态变化的。举个栗子，当你的鼠标悬浮到一个按钮上时，按钮就会变成“悬浮”状态，这时我们就可以利用伪类`:hover`来选中这一状态的按钮，并对其样式进行改变。

`:hover`是笔者最最常用的一个伪类。还有一个很常用的伪类是`:nth-child`，用于选中元素的某一个子元素。其他的类似`:focus`、`:focus-within`等也有一定的使用。

本 demo 地址：[Button Hover Border Stroke With Float Text](https://codepen.io/alphardex/pen/pooYKVa)

### 绝对定位实现多重边框

谁规定按钮只能有一套边框的？利用绝对定位和`padding`，我们可以给按钮做出 3 套大小不一的边框来，这样效果更炫了

![1702ee05232b8519~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/ObXAPfkolcYVHh8.gif)

本 demo 地址：[Button Hover Multiple Border Stroke](https://codepen.io/alphardex/full/ZEYXomW)

## 伪元素

![1702ee056d1fae71~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/FVDXpaiTh1tG2Zg.gif)

简而言之，伪元素就是在原先的元素基础上插入额外的元素，而且这个元素不充当 HTML 的标签，这样就能保持 HTML 结构的整洁。

我们知道每个元素都有`::before`和`::after`这两个伪元素，也就是说每个元素都提供了 3 个矩形（元素本身 1 个，伪元素 2 个）来供我们进行形状的绘制。现在又有了`clip-path`这个属性，几乎任意的形状都可以被绘制出来，全凭你的想象力

上面的动图是条子划过文本的动画，条子就是每个文本所对应的伪元素，对每个文本和其伪元素应用动画，就能达到上图的效果了

本 demo 地址：[Header With Slide Bar](https://codepen.io/alphardex/pen/jOEOEzZ)

### attr()生成文本内容

元素可以有自定义的属性值，它的命名格式通常为`data-*`

`attr()`用于获取元素的这种自定义属性值，并赋值给其伪元素的`content`作为其生成的内容

利用这个函数，我们可以用伪元素在原先文本的基础上“复制”出另一个文本，如下图所示

![1702ee05bd378d59~tplv-t2oaga2asx-image.png](https://s2.loli.net/2024/05/22/8DKbCyXaGQ9uWlg.png)

看上去有点乱糟糟的对吧？没事，给它加上`overflow: hidden`，把多余的文本遮住。通过 JS 分割文本并应用交错动画，就得到了如下的效果，这也是接下来本文要讲的`overflow`障眼法

![1702ee07142525a2~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/MxyPecR2LZIBSqs.gif)

本 demo 地址：[Staggered Float Text Menu](https://codepen.io/alphardex/full/wvBeXjd)

# overflow 障眼法

之前有做过闪光按钮的效果：鼠标悬浮按钮上时一道光从左到右划过去。

笔者就用渐变来模拟那道光，通过`transform: translateX()`将其平移至右边

![1702ee05e9a4d10a~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/YFX9udk5NfGvqeT.gif)

但这样明显不对啊，这光为啥能被看见呢？不应该把它给“挡”起来吗？

于是乎，给按钮加上`overflow: hidden`，光在按钮外的位置时就被隐藏起来了

![1702ee05f2da6fd5~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/iYcXuTsyK12vjME.gif)

本 demo 地址：[Button Hover Shining](https://codepen.io/alphardex/pen/eYYzXBZ)

# 自定义 input

默认的`input`太丑怎么办？那就把它先抹掉，设置`opacity: 0`即可

与`input`密切相关的是`label`元素，因为当用户点击`label`元素时同样会触发`input`状态的变化。利用这一特性，可以给`label`自定义样式，不仅如此，还可以在`label`旁边添加更多的元素，用兄弟选择符`~`选中它们来定义样式，利用`:checked`伪类监听原`input`的状态变化，再加上一点动画，就完成了对`input`的定制

目前来说，`input`元素也支持伪元素了，这就带来了另一种思路：用`appearance: none`消除`input`的默认样式，再用伪元素对其进行定制，这样`label`就可以保留它原先的样式了。

![1702ee063382adbe~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/pibomkMrPI6veJ2.gif)

本 demo 地址：[Todo List](https://codepen.io/alphardex/full/rNNPQwa)

# CSS 特性

善用某些 CSS 特性，也可以为你的作品增色不少哦

## animation

此处包括`transition`和`transform`

以下是我的个人名片，猜猜看里面用到了哪些技巧？

![1702ee06bb76a3b3~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/fLx8FVXMQCcHW3P.gif)

首先，刚开始的四条边框出现的动画用到了`overflow`障眼法

其次，条子划过文本用到了伪元素动画

最后，社交图标的依次出现用到了交错动画

本 demo 地址：[Profile Card](https://codepen.io/alphardex/full/jOExoLp)

## border-radius

为盒子添加圆角，经常用来美化按钮等组件

如果设定为`50%`则是圆形，也很常用

### 不规则的曲边形状

调整多个顶点的`border-radius`可以做出不规则的曲边形状

![1702ee06bbbfc49d~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/WUqPp2CEKVr1GQ6.gif)

本 demo 地址：[Nav Tab](https://codepen.io/alphardex/full/abbWOPR)

## box-shadow

为盒子添加阴影，增加盒子的立体感，可以多层叠加，并且会使阴影更加丝滑

![1702ee06bc72977a~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/Qvh2XAlBa8McCfy.gif)

本 demo 地址：[Pagination](https://codepen.io/alphardex/full/QWwwwpp)

### 内发光

注意到`box-shadow`还有个`inset`，用于盒子内部发光

利用这个特性我们可以在盒子内部的某个范围内设定颜色，做出一个新月形

![1702ee06bdae7a82~tplv-t2oaga2asx-image.png](https://s2.loli.net/2024/05/22/geTqtApo97Vr4Wu.png)

再加点动画和滤镜效果，“猩红之月”闪亮登场！

![1702ee0710ee13f8~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/9uV4sKJlrdfw6Pj.gif)

注意到它散发着淡淡的红光，其实就是 2 个伪元素应用了模糊滤镜所产生的效果

本 demo 地址：[Crimson Crescent Loading](https://codepen.io/alphardex/full/eYmGEGp)

## text-shadow

文本阴影，本质上和`box-shadow`相同，只不过是相对于文本而言，常用于文本发光，也可通过多层叠加来制作霓虹文本和伪 3D 文本等效果

### 发光文本

![1702ee0751c9e13e~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/ut9WgLvMszbZDNe.gif)

本 demo 地址：[Staggered GlowIn Text](https://codepen.io/alphardex/full/Exxodoq)

### 霓虹文本

![1702ee076a90febb~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/9VlAmCQ3KNu7FUZ.gif)

本 demo 地址：[Neon Text](https://codepen.io/alphardex/full/rNNwmZz)

### 伪 3D 文本

![1702ee0770616364~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/thSzBxDNoYbsMu4.gif)

本 demo 地址：[Staggered Bouncing 3D Loading](https://codepen.io/alphardex/full/QWWavvx)

## background-clip:text

能将背景裁剪成文字的前景色，常用来和`color: transparent`配合生成渐变文本

![1702ee07ea23e5ac~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/tGYljKnsbVhEDev.gif)

本 demo 地址：[Menu Hover Fill Text](https://codepen.io/alphardex/full/QWwveZG)

## gradient

渐变可以作为背景图片的一种，具有很强的色彩效果，甚至可以用来模拟光

### linear-gradient

线性渐变是笔者最常用的渐变

![1702ee08051eab40~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/gwTpHGLNAF5IRYr.gif)

这个作品用到了 HTML 的`dialog`标签，线性渐变背景，动画以及`overflow`障眼法，细心的你看出来了吗:)

本 demo 地址：[Confirm Modal](https://codepen.io/alphardex/full/eYYxzBm)

### radial-gradient

径向渐变常用于生成圆形背景，上面例子中[Snow 的背景](https://codepen.io/alphardex/full/dyPorwJ)就是一个椭圆形的径向渐变

此外，由于背景可以叠加，我们可以叠加多个不同位置大小的径向渐变来生成圆点群，再加上动画就产生了一种微粒效果，无需多余的`div`元素

![1702ee0834f82a09~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/pDHh5sMWyFJuEav.gif)

本 demo 地址：[Particle Button](https://codepen.io/alphardex/full/OJPvMGx)

### conic-gradient

圆锥渐变可以用于制作饼图

![1702ee08546f0887~tplv-t2oaga2asx-image.png](https://s2.loli.net/2024/05/22/ReugptVA49b8CZF.png)

用一个伪元素叠在饼图上面，并将`content`设为某个值（这个值通过 CSS 变量计算出来），就能制作出度量计的效果，障眼法又一次完成了它的使命

![1702ee08695df44f~tplv-t2oaga2asx-image.png](https://s2.loli.net/2024/05/22/lriNF9Zuzoc2nLv.png)

本 demo 地址：[Gauge (No SVG)](https://codepen.io/alphardex/full/BaydVvQ)

## filter

PS 里的滤镜，`blur`最常用

### 融合效果

当`blur`滤镜和`contrast`滤镜一起使用时，会产生一种融合（`gooey`）的奇特效果

![1702ee089a916bc7~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/1QPWIRU8O3u7dZy.gif)

本 demo 地址：[Snow Scratch](https://codepen.io/alphardex/full/BaBevXm)

## backdrop-filter

对背景应用滤镜，产生毛玻璃的效果

![1702ee08b2840155~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/NgvmnA7UMhYxfQw.gif)

本 demo 地址：[Frosted Glass](https://codepen.io/alphardex/full/pooQMVp)

## mix-blend-mode

PS 里的混合模式，常用于文本在背景下的特殊效果

以下利用滤色模式（`screen`）实现文本视频蒙版效果

![1702ee08ca1668c6~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/1LMV6aZelywcuPD.gif)

本 demo 地址：[Video Mask Text](https://codepen.io/alphardex/full/wvvLYpV)

## clip-path

PS 里的裁切，可以制作各种不规则形状。如果和动画结合也会相当有意思

![1702ee08d117617e~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/NfD3HBuMplxAZRX.gif)

本 demo 地址：[Name Card Hover Expand](https://codepen.io/alphardex/full/ZEEBRrq)

### 故障效果

由于`clip-path`有裁切功能，因此可以将多个文字叠在一起，并按比例裁切成多分，再应用交错动画，就能制作出酷炫的故障效果（`glitch`）。

![1702ee090c67ca6a~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/LPBazWwmeqlMfSH.gif)

本 demo 地址：[Cross Bar Glitch Text](https://codepen.io/alphardex/full/VwLLLNG)

## mask

PS 里的遮罩。所谓遮罩，就是原始图片只显示遮罩图片非透明的部分

### 镂空效果

虽然`clip-path`能裁切出形状，但它无法镂空，因为形状的里面它管不着

可能有人（包括我）会用伪元素来“模拟”镂空（通过设置同样的背景色），但这样并非真的镂空，换了个背景或浮在图片上就会暴露出来，这时我们就要求助于遮罩了

假设，你想制作一个空心的圆环，那么你只需将一个径向渐变作为元素的遮罩，并且第一个`color-stop`设置为透明，其他的`color-stop`设置为其他颜色即可，因为遮罩的定义就是只显示遮罩图片非透明的部分

注意：为了消除锯齿，这个径向渐变的中间需要有一个额外的`color-stop`用于缓冲，长度设置为原长度加 0.5px 即可

![1703e7bb22182d04~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/BayPhjgmxEb2vIU.gif)

本 demo 地址：[Circle Arrow Nav](https://codepen.io/alphardex/full/MWwadod)

## -webkit-box-reflect

投影效果，不怎么常用，适合立体感强的作品

![1702ee091c74c545~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/kVoTmfj1aYlicxD.gif)

本 demo 地址：[Card Flip Reflection](https://codepen.io/alphardex/full/ExaZgxp)

## web animations

虽然这并不是一个 CSS 特性，但是它经常用于完成那些 CSS 所做不到的事情

那么何时用它呢？当 CSS 动画中有属性无法从 CSS 中获取时，自然就会使用到它了

### 跟踪鼠标的位置

目前 CSS 还尚未有获取鼠标位置的 API，因此考虑用 JS 来进行

通过查阅相关的 DOM API，发现在监听鼠标事件的 API 中，可通过`e.clientX`和`e.clientY`来获得鼠标当前的位置

既然能够获取鼠标的位置，那么跟踪鼠标的位置也就不是什么难事了：通过监听`mouseenter`和`mouseleave`事件，来获取鼠标出入一个元素时的位置，并用此坐标来当作鼠标的位移距离，监听`mousemove`事件，来获取鼠标在元素上移动时的位置，同样地用此坐标来当作鼠标的位移距离，这样一个跟踪鼠标的效果就实现了

![1702ee0949a76b86~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/4r5t6puWVOJG7RH.gif)

本 demo 地址：[Menu Hover Image](https://codepen.io/alphardex/full/OJPmQGz)

## CSS Houdini

CSS Houdini 是 CSS 的底层 API，它使我们能够通过这套接口来扩展 CSS 的功能

### 让渐变动起来

目前来说，我们无法直接给渐变添加动画，因为浏览器不理解要改变的值是什么类型

这时，我们就可以利用`CSS.registerProperty()`来注册我们的自定义变量，并声明其语法类型（`syntax`）为颜色类型`<color>`，这样浏览器就能理解并对颜色应用插值方法来进行动画

还记得上文提到的圆锥渐变`conic-gradient()`吗？既然它可以用来制作饼图，那么我们能不能让饼图动起来呢？答案是肯定的，定义三个变量：`--color1`、`--color2`和`--pos`，其中`--pos`的语法类型为长度百分比`<length-percentage>`，将其从`0`变为`100%`，饼图就会顺时针旋转出现

![1702ee09cad803ba~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/CzFuDG7RMVexqHN.gif)

利用绝对定位和层叠上下文，我们可以叠加多个从小到大的饼图，再给它们设置不同的颜色，应用交错动画，就有了下面这个炫丽的效果

![1702ee0a2594a4c1~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/AnfrxPwFZjl7Hzq.gif)

本 demo 地址：[Mawaru](https://codepen.io/alphardex/full/RwNxpXQ)

# 彩蛋

![1702ee0a43b8c2ed~tplv-t2oaga2asx-image.gif](https://s2.loli.net/2024/05/22/TiMcKnpxVLugw4q.gif)

将交错动画和伪类伪元素结合起来写出来的慎重勇者风格的菜单

本 demo 地址：[Shinchou Menu](https://codepen.io/alphardex/full/ExavZdV)

# 最后

恭喜你将本文读完了。不论是过了一场视觉盛宴也好，还是学到了不少东西也好，还是直接从书签那导航到这里也好（笑），CSS 的力量始终超乎你的想象。只要敢于创作，你就是这个世界的神。
