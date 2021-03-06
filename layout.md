
---
layout: book
title: 弹性布局和弹性媒介
---

响应式网站设计主要包括三部分：弹性布局、媒体查询和弹性媒介。这一章我们主要介绍一下弹性布局和弹性媒介。另外，再介绍一下目前最新最先进的 Flexbox CSS 布局。

### 弹性布局

所谓的弹性布局，说的是在创建网站的时候，采用流体网格布局方式，动态地调整网站布局宽度以此来适应各种设备屏幕大小。

弹性布局不使用固定的测量单位，比如像素或者英寸。原因显而易见，视窗（ viewport ）的高度和宽度在不同设备之间是不断变化的，那么弹性网站布局为了适应这种变化，所以就不可能明确的指定网站内容的宽度和高度，只能使用相对长度单位。

那么怎么得到相对长度呢？ 响应式网站设计一词的缔造者 Ethan 给出了一个可靠的计算公式，可以用来确定弹性布局的比例值。

{% highlight html %}
target / parent = result
{% endhighlight %}

上面这个公式的含义是，用一个元素的宽度（ target ）除以它的父元素的宽度（ parent ），那得到的计算结果（ result ）就是这个元素的相对宽度。

### 流体网格

说到弹性布局，自然要提到流体网格（ fluid grid ）。流体网格使用相对长度单位，最常用的是百分比或 em 单位。流体网格的属性值，比如 width、margin
或者 padding 都是用相对长度表示的。

接下来看一个应用上面流体网格公式的例子，如何把一个固定宽度的两栏页面转变成一个弹性布局页面。

{% highlight html %}
<div class="main">
  <div class="content"></div>
  <div class="sidebar"></div>
</div>
{% endhighlight %}

一个父元素 main, 包含了两个子元素，子元素 content 居左，另一个子元素 menu 居右，[查看效果](http://book.haoduoshipin.com/go-responsive/demo/layout/fixed/)。
为了叙述方便，抽取了一些 CSS 样式：

{% highlight css %}
.main {
  width: 900px;
}
.content, .menu {
  margin: 30px 20px;
}
.content {
  float: left;
  width: 600px;
}
.menu {
  float: right;
  width: 210px;
}
{% endhighlight %}

现在依照上面的流体网格公式，把 content 和 menu 两个区块的宽度，以及它们的页边距用相对长度来表示，以 menu 区块为例：

{% highlight css %}
.menu {
  float: right;
  width: 23.33333333%; /* 210 / 900 = 0.2333333333 */
}
{% endhighlight %}

注意，无论父元素 main 变得有多宽，content 和 menu 区块的宽度和页边距都会按比例缩放。[点击这里](http://book.haoduoshipin.com/go-responsive/demo/layout/flexible/)，查看弹性布局效果。你需要调整浏览器窗口的大小才能看到变化。

知道了弹性布局的概念和流体网格计算公式，并将其运用起来，你就能创建一个完全动态的网站，网站内容按照视窗尺寸缩放。为了更好的掌控弹性布局，你也可以对
min-width、max-width、 min-height 和 max-height 属性值采用相对长度单位。

### 弹性媒介

虽然现在能创建一个动态的网站了，但是要达到完美，还需要做一些事情。比如说，网站中的图片、视频、Flash 动画等等，在变为响应式的时候，就不能简单的按照流体网格计算公式来变化了。

接下来说一说，响应式网站设计的另一重要部分，弹性媒介。网站上的图片、视频、Flash 等就是所谓的媒介。随着浏览器窗口大小的改变，网站上媒介的布局不一定变得很合适，有时候它们会超出其容器的界限，当浏览器窗口变得比较小的情况下，这时页面看起来会很糟糕。因此图片、视频和其它媒介类型应该具备可伸缩性，能根据浏览器窗口的大小，自动调整自身尺寸，保证页面布局整齐一致。

让图片等媒介具有可伸缩性，一个简单有效的方法，就是把它的 max-width 属性值设置为 100%，意思是说它的最大宽度就是其容器的宽度。这样就能确保当浏览器窗口变小的时候，则网站上的媒介会依据其容器的大小而缩小尺寸，始终包裹在容器内。代码如下所示：

{% highlight css %}
img, video, canvas {
  max-width: 100%;
}
{% endhighlight %}

本书作者身在秦皇岛，于是就从网上找了一张山海关的图片来做演示。[查看 demo](http://book.haoduoshipin.com/go-responsive/demo/layout/image/)

![](images/layout/pass.jpg)

### 弹性嵌入式媒介

不幸的是，这个 max-width 属性并不适用所有的媒介应用场景，尤其是对于嵌入式媒介，像通过 `<iframe>` 标签 或 在 HTML5 中使用 `<video>` 标签嵌入的视频，这种情况下，仅仅一个 max-width 属性是不能解决问题的。

实现嵌入式媒介的响应式布局，嵌入式媒介在其父元素中的摆放位置，需要采用绝对定位方式确定。父元素的宽度要设置为100%，这样父元素就能基于视窗宽度来调整其大小。另外，为了触发 IE 浏览器的 hasLayout 机制，父元素的高度需要设置为0。

父元素的下内边距 padding-bottom 的属性值要与视频的长宽比保持一致。这是为了让父元素的高度与其宽度是相称的。记得前面那个流体网格计算公式吗？ 如果一个视频的长宽比是 16:9，9除以16等于 .5625，这样父元素的 padding-bottom 为56.25%。设置 padding-bottom 而不是 padding-top 是为了专门阻止 IE 5.5 把父元素当做绝对定位元素看待。实例代码如下：

{% highlight html %}
<div class="video">
  <video src="video/code.mov"></video>
</div>
{% endhighlight %}

{% highlight css %}
.video {
  height: 0;
  padding-bottom: 56.25%; /* 16:9 */
  position: relative;
  width: 100%;
}
video {
  height: 100%;
  left: 0;
  position: absolute;
  top: 0;
  width: 100%;
}
{% endhighlight %}

[查看 demo](http://book.haoduoshipin.com/go-responsive/demo/layout/video/)，视频片段来自[《多数学校不会教的》](http://v.youku.com/v_show/id_XNTIzNzE2NzQ4.html?from=s1.8-1-1.2)

![](images/layout/code.png)

### 相对长度单位

自从 CSS3 问世之后，相对长度家族成员也壮大起来，除了已有的 % 和 em 单位之外，新加入了 rem 单位，还增加了一组相对视窗长度单位如
vw、vh、vmin 和 vmax。目前浏览器对这些新加入成员的支持还不太好，所以它们还没有普遍使用起来。下面简单介绍一下 em、rem 还有百分比。

先聊聊 em 单位。em 是相对于其所在元素的字体而言的。

若使用 em 单位的元素定义了 font-size 为 12px，则此时 1em == 12px，那 2em == 24px；若没有，则找其父元素的字体大小，依次类推直到
html 元素；若整个页面没有定义字体大小，而是使用浏览器默认字体大小 16px，则 1em == 16px。

{% highlight html %}
<div class="main">
  <div class="content">em relative length unit</div>
</div>
{% endhighlight %}

{% highlight html %}
.main {
  font-size: 20px;
}
.content {
  margin: 1.5em; /* 1em == 20px */
}
{% endhighlight %}

这样若某些 CSS 属性使用了 em 单位，只要改变与其相关联的字体大小，就能改变这些属性值。

再稍微说一下 rem（ root em 的简写 ），它与 em 相似，但 rem 仅仅是相对于 html 根元素的 font-size
而言的。这样一旦设置了一个基本字体大小，定义其它长度就容易了，比 em 使用起来简单。不足之处就是，牵一发而动全身。

对于百分比，就是根据 Ethan 的公式，得到的结果乘以 100%。既然对这三个相对长度单位有了一定了解，那在响应式网站中如何使用它们呢？

一般而言，对于响应式网站，水平方向为流体网格布局，那宽度、左右间距和左右内边距的长度自然采用百分比。像高度、上下间距，还有字体大小等与垂直方向相关的属性则应该使用 em 或者 rem 单位。

### Flexbox 布局

为了适应各种类型的显示设备和屏幕大小，在 CSS3 中添加了 Flexbox 特性，它是一个精巧的布局模块，目的是为我们提供一种更加有效的弹性布局方式。

随着响应式网站设计的兴起，借助传统的 CSS 工具创建弹性网站布局，是一件让每个 web 开发者都感到痛苦的事情。而使用 flexbox，你就不用关心浮动、垂直居中、竖直居中等等与布局相关的问题了，而这些都是在以前创建一个网站时，必然会遇到的挑战。

flex 布局的设计思想是这样的，让一个容器（ flex 容器）能够自动调整容器内元素（子元素）的宽度、高度以及顺序，并尽可能以最佳的方式来适应容器自身空间大小。也就是说，一个
flex 容器，当空间变大时，其中的子元素也会随之扩大； 当空间变小时，其中的子元素也会随之缩小。无论容器如何变化，子元素都能自如缩放。

最重要的是，相对于常规布局，如块布局是基于垂直方向的，而 flexbox 的布局方向是不可知的。虽然 flexbox 布局在小网站中，表现很好，但是由于它们缺乏灵活性，对比较大的或复杂的网站（尤其是遇到取向改变，调整大小，伸展，收缩等问题的时候）支持并不友好。

注意： Flexbox 布局最适用于网站中的组件，和小规模布局，而网格布局则用于大规模布局。

Flexbox 是一个非常新颖的想法，它是如何工作的？ 什么时候应该使用它？ 弄明白这些问题，我们还是举例说明吧。

### 参考链接

- <http://learn.shayhowe.com/advanced-html-css/responsive-web-design/>

- <https://medium.com/@simurai/sizing-web-components-8f433689736f>
http://clearleft.com/thinks/anequalheightgridusingflexbox/
