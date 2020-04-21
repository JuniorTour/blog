---
title: 块级元素的格式化-盒模型和负外边距原理讲解
tags:
  - CSS
id: 295
categories:
  - CSS
  - 前端-front-end
date: 2016-10-18 21:48:13
---

前段时间，读过《CSS权威指南》第七章《基本视觉格式化》之后，有很多收获，今天在这篇文章之中做些笔记用来总结。

本文将主要依据《基本视觉格式化》这一章的内容和[W3C官方CSS2.1规范](https://www.w3.org/TR/CSS2/)（超链），介绍块级元素的盒模型在布局（即格式化）时所遵循的一些规则，并附带介绍负外边距神奇效果的原理。

<!--more-->

# 一、重要概念

### 1.正常流（Normal flow）

指西方语言文本从左向右、从上向下的布局。要把元素排除出正常流，只能通过使之成为浮动元素或定位元素。

所以相应的也有浮动流、定位流等不规范的名词，w3c文档只是称呼其为浮动和定位。

### 2.基本框

CSS假定每个元素都会生成一个或多个相互嵌套的框，这称之为元素框。元素框由一个内容区（Content area）和**可选的**内边距、外边距和边框构成。（所谓可选就是说他们的宽度可以被设置为0）

# ![boxdim](http://juniortour.net/img/2016/10/boxdim.jpg)

### 3.块级元素

指display属性的值为 block值的元素，这种元素在正常流之中会在其框之前和之后产生“换行”，相邻的块级元素都会隔行排列；其宽度默认填满父元素宽度；块级元素可以设置width和height属性。如div元素、p元素等。



### 4.行内元素

指display属性的值为 inline值的元素，这种元素在正常流之中**不会**在其框之前和之后产生“换行”，相邻的行内元素会排列在一行之中；其宽度由内容决定；行内元素设置width和height属性**无效**。如span元素、strong元素等。


# 二、水平格式化

### 1.水平格式化定律

块级元素水平方向上的七大属性必须等于元素包含块的宽度，即：

**margin-left+border-left+padding-left+width+padding-right+border-right+margin-right=元素包含块的width。**

（注：box-sizing属性可以用来决定width指的是内容宽度还是整个元素可见框的宽度。其属性约定为：box-sizing: content-box | border-box | inherit ;）


看到这个定律，像我一样的初学者可能会感到疑惑。觉得有时候对一个块级元素只是显式的声明了width属性，并没有声明其他的边框、外边距或内边距，或者只声明了部分边框、外边距或内边距，声明的这个宽度可以小于父元素的宽度啊，那不就打破这个定律了吗？


比如例（一）：

## **[点我打开demo页面](http://juniortour.net/lab/2016/10/formatting.html)**

``` css
div#parent {
  width:500px;
}

div#child-0 {
  background-color: pink;
  width:200px;
  height:200px;
  padding-left:50px;
  border:10px solid red;
  margin-right: 50px;
}
```

其实定律并没有被打破。

在浏览器之中检查上面这个例子，你会看到尽管DevTools的右侧computed选项卡所显示的盒模型和设定的一模一样。（如下图）

但是当你鼠标悬停在DevTools左侧element选项卡的**div#child**上时，你会发现**div#child**的右外边距（下图黄色部分）实际上非常宽，填满了子元素与父元素的空白区域。（如下图）

![child0](http://juniortour.net/img/2016/10/child0-1024x673.jpg)

实际上，这种现象《CSS权威指南》中用专业术语称之为**Over-Constrained**（过度受限）。即当上述的定律**不能**满足时，margin-right总会被强制声明为auto值，从而满足定律。（这一段掺杂着我个人的理解，因为我通过谷歌、维基、stackoverflow和w3c等途径均没能找到相关资料详细解释这个概念。）


还可以举出各种各样的例子，但这个水平格式化定律绝对是颠簸不破的真理，甚至在绝对定位之中也是成立的。


### 2.auto值的效果

在应用上述水平格式化定律时，还要考虑一个特殊的值“auto”。


首先，auto值只能应用于外边距margin和宽度width，对于内边距和边框这个属性值都是无效的，会被浏览器忽略。

那么，在margin-left、margin-right和width三个属性之中，auto值的声明就有了以下三种可能。

## **[点我打开demo页面](http://juniortour.net/lab/2016/10/formatting.html)**

*   只有一个auto值时
如demo之中的例（二）：当width为auto，margin-left、margin -right显式声明固定值时，width会被设置为所需的值，以满足水平格式化定律。

如demo之中的例（三）：当width显式声明，而margin-left、margin -right其中之一值为auto时，这个auto值的外边距会被用来“填充”父元素，以满足水平格式化定律，比如上面的例（一）中，margin -right就被用来填充了。

*   两个auto值时
如demo之中的例（四）：当width和一个左右margin为auto时，设置为auto的margin会被设置为0，而width则会尽可能伸展来“填充”父元素。

如demo之中的例（五）：当width显式声明，而左右margin为auto时，就是我们常用的水平居中。

*   三个auto值时
如demo之中的例（六）：当这三个值均为auto时，左右margin会被设置为0，而width则“填充”父元素。


### 3.负外边距的效果及原理

负外边距是一个很有意思的值。当它出现时，布局往往会产生十分特别的“拉近”效果。

网上有不少文章讲解负外边距的具体应用，但是多数都没有说清楚其原理是什么，下面我来试着讲讲我的理解。

首先，再次复习水平格式化定律：

**margin-left+border-left+padding-left+width+padding-right+border-right+margin-right=元素包含块的width。**

**负外边距的效果也遵循这个定律。**


先看例（七）：
``` css
#child-7 {
  width:auto;
  margin-left: -50px;
  margin-right:50px;
}
```

#child-7的宽度为auto，同时有-50px的负左外边距和50px的右外边距。

![child7](http://juniortour.net/img/2016/10/child7.jpg)

可以看到#child-7向左超出了父元素一定的宽度，这个宽度**看起来**很接近右侧的50px右外边距空白。

这样的布局很特别，子元素竟然超出了父元素，这还符合水平格式化定律吗？？？


应用水平格式化定律计算#child-7的水平布局：margin-left+border-left+padding-left+width+padding-right+border-right+margin-right=元素包含块的width。

在#child-7之中对应的值是：

-50 px +5 px +0 px +auto+0 px +5 px +50 px =300 px


此时的auto值应该是多少呢？应该是290px。

右键审查之，#child-7的width计算值**确实是290px**。也就是说水平格式化定律在负外边距出现时，**依然成立**！

这也就解释了为什么子元素#child-7会超出父元素一部分（50px），其实是由于负左外边距的出现使得元素width属性的 auto值的计算结果增加了50px ，也就是宽度增加了50px而产生的。


如果你在调试时试着注释掉margin-left: -50px;，你就会发现#child-7的宽度正好减少了50px，成了240px。
