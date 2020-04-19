---
title: 详解line-height属性：HTML中一行文字是如何排布的？什么是内容区、行内框、行框？
tags:
  - CSS
  - line-height
id: 176
categories:
  - CSS
  - 前端-front-end
date: 2016-06-07 18:25:06
---
不知道你有没有考虑过一行文字是按照什么规律在HTML中排布的？
本文就将向你详细介绍这毫厘之中的奥秘。
你将会从本文中了解到line-height、font-size等属性的”真正“作用以及em框、内容区、行内框和行框等概念。
<!--more-->

#### 1.font-size真正的作用：生成em框与内容区：

首先，一个字符受到font-size决定，在其周围产生了一个看不见的**em框（em square）**（如下图），**注意！**font-size并不决定实际显示的字符尺寸，只是指定em框的大小。
[Design with FontForge: EM square](http://designwithfontforge.com/zh-CN/The_EM_Square.html)

em框与其中字符的具体大小、位置关系受到字体内置值的影响，也就是说不同的字体，即使是同样font-size的字符在不同字体中也会位于em框的不同位置。

一个元素中的em框组合在一起，就形成了**“内容区”**（如下图粉色外边框所圈起来的部分）。
内容区的高度就是font-size的值，而宽度则视内容（如一行中文字的长度）而定。

![em-box-and-content-area](http://juniortour.net/img/2016/06/em-box-and-content-area.jpg)


#### 2.line-height和基线的作用：

这个属性名为“行高”，实际上它规定的是两行**“基线”**之间的最小距离。

基线是印刷行业中常用的一个术语，维基百科这样描述它：“A baseline is a line that is a base for measurement or for construction.”（基线是一条用于测量和构造的线）。
基线相当于一把尺子，水平地串在em框里，一个个字符就以基线为参照，整齐地“站”在基线上（如下图）。

![baseline-line-height](http://juniortour.net/img/2016/06/baseline-line-height-1024x366.jpg)

基线的位置因字体而异由字体创作者决定位于em框中特定的位置。

![where-is-baseline](http://juniortour.net/img/2016/06/where-is-baseline-300x222.jpg)

因为基线无法在浏览器中看到，所以我们以上图中每行文字的underline为参照来看基线，上述三行分别为不同的字体，但有相同的font-size。

这三行相同的字（如落、孤等）可以明显地看到与基线有着不同的位置关系，第一行字底部与基线有一点重合，而其余两行则和基线几乎没有重合。这就说明这三种字体的基线在em框之中的位置是不同的。

声明行高的值后，两行的基线便按照这个值上下分开，行与行也因此上下分开。

当行高大于字体高度，即line-height的值减去font-size的值**大于**0时，基线相隔的距离大于该行的行框高度时，两行文字便被分开。

![l-h big than f-s](http://juniortour.net/img/2016/06/l-h-big-than-f-s.jpg)
当行高小于字体高度，即line-height的值减去font-size的值**小于**0时，基线相隔的距离小于该行的行框高度时，两行文字便重合在一起。

![line-height small than font-size](http://juniortour.net/img/2016/06/line-height-small-than-font-size.jpg)


#### 3.行间距、行内框与行框是什么：

内容区的上下还会有**行间距（leading）**，即行高减去字符大小的一半，(line-height减font-size)除**2**。
行间距可能是负值（行高小于字符尺寸），这时就会产生上图所呈现的行与行相互重合的现象。
**行内框**即内容区加行间距，其高度的计算公式是：font-size加(line-height减font-size)，去掉括号即line-height，也就是说行内框的高度就等于line-height的值。同一行中可以有不同高度的行内框（如下图）。

**行框**的上边界由最高的行内框决定，其下边界有最低的行内框决定。

![line-box](http://juniortour.net/img/2016/06/line-box-1-1024x452.jpg)


#### 4.最终行与行之间的相对位置

一个行框包含着一整行，各个行框之间**上下紧密贴合**排列，形成了最终的行与行之间的位置关系。

## 总结：
font-size的值决定一个字符的em框的高度，该字符的大小由字体内置值决定受em框影响；
一个行内元素（包括匿名元素）之中的所有em框组合形成“**内容区**”，内容区加内边距即行内元素背景颜色所应用的范围；
内容区的上下加上line-height和font-size的差即行间距，形成行内框；
最高的和最低的行内框决定了一个行框的高度；
最终，各个行框上下紧密贴合形成了行与行之间的位置关系。

### 这一部分内容十分重要，但又不容易理解，我推荐大家参考以下资料来加深理解。


##### 主要参考资料:
1.《css权威指南（第三版）》，第六章、文本属性和第七章、视觉格式化
2.W3C规范－[9 Visual formatting model（可视化模型](https://www.w3.org/TR/2011/REC-CSS2-20110607/visuren.html#q9.0)）/ [中文版](http://www.ayqy.net/doc/css2-1/visuren.html)
3.另一篇同样主题的博文：[深入理解CSS中的行高](http://www.cnblogs.com/rainman/archive/2011/08/05/2128068.html)，相互对照，加深理解。
4.慕课网上张鑫旭老师的两节课：[line-height与内联元素的高度机制](http://www.imooc.com/video/7922)
