---
title: （译文）Bootstrap所采用的clearfix方式：A new micro clearfix hack
tags:
  - bootstrap
  - clearfix
  - CSS
id: 246
categories:
  - translation
date: 2016-07-21 21:45:15
---

（译注：这篇文章是bootstrap所采用的clearfix方法的作者[Nicolas Gallagher对这个方法的介绍文章。

这个方法在我学习bootstrap时曾使用过，效果很好，故推荐给大家。）



APRIL 21, 2011

<!--more-->

# 一种新的轻型清除浮动技巧


作者：[Nicolas Gallagher](http://nicolasgallagher.com/).

原文地址：[A new micro clearfix hack](http://nicolasgallagher.com/micro-clearfix-hack/)



`清除浮动（clearfix）`是在不借助显式标记（presentational markup）的情况下包含浮动的常用方式。本文将呈现一种能极大地减少所需CSS的新型 clearfix 手段。



DEMO：[micro-clearfix-hack-demo](http://nicolasgallagher.com/micro-clearfix-hack/demo/)




已知的支持浏览器：Firefox 3.5+, Safari 4+, Chrome, Opera 9+, IE 6+



这种“轻型清浮”方法适用于各种现代浏览器，它基于 Thierry Koblentz 所创造的引入了`:before` 和`:after `伪元素的 “[clearfix reloaded](http://www.yuiblog.com/blog/2010/09/27/clearfix-reloaded-overflowhidden-demystified/)” 技巧。



以下是更新了的代码。（我已经使用了更短的类名）

（译注：即把`clearfix`类写作了`cf`类。在bootstrap 3.5中仍是 `.clearfix` 。）


``` css
/**

* 对于现代浏览器

* 1. 空格内容是为了避免当contenteditable属性在在Opera浏览器的页面中出现时

* 	 产生的bug 。这个bug会导致空格出现在被清浮的元素顶部或底部。

* 2. 使用 “table” 而非 “block” 只在使用 “:before” 包含子元素顶部外边距时才需要。

*/

.cf:before, .cf:after {

   content: " "; /* 1 */

   display: table; /* 2 */

}

.cf:after {

   clear: both;

}

/**

* For IE 6/7 only

* 包括下面这条规则从而引起 hasLayout 并包括浮动元素。

*/

.cf {

	zoom: 1;

}

```



这种轻型清浮产生伪元素并且将其`display` 属性设置为`table `值。这产生了匿名表格元素（[anonymous table-cell](https://www.w3.org/TR/CSS2/tables.html#anonymous-boxes)）以及新的块级格式化上下文（block formatting context），也就是用 `:before`伪元素防止顶部外边距塌陷。用`:after`伪元素来清除浮动。从而不用隐藏任何生成的内容，且减少了代码量。



包括` :before` 选择符对于清除浮动来说并不必要，但是它防止了顶部外边距在现代浏览器之中的塌陷。.



这有两个好处：


*   这保证了这种技巧和其他通过 `overflow:hidden`等方式来产生新的块级格式化上下文的浮动包含技术具有了视觉连续性（visual consistency）。
*   这还保证了这种技巧和在 IE 6/7 上使用`zoom:1` 时的视觉连续性。

**注意**：IE 6/7可能在某些情况下不会在块级格式化上下文之中包含浮动（译注：元素）的底部外边距。进一步的细节可以在这里找到：[Better float containment in IE using CSS expressions](http://nicolasgallagher.com/better-float-containment-in-ie/).



使用 `content:" "`（注意内容字符串中的空格）避免了一个Opera浏览器的bug。

这个bug是：如果 `contenteditable`属性也在 HTML 之中出现的话，会产生多余的空格围绕被清浮的元素。

感谢 Sergio Cerrutti 发现这种修复方式。另一种可选的修复方式是使用 `font:0/0 a;`。



## 对于Firefox旧版本


3.5版本以前的Firefox更适合使用 Thierry 的手段，他的这种手段用额外的 `visibility:hidden`隐藏了插入的字符。这是因为旧的 Firefox 需要`content:"."` 来避免多余的空格出现在body和其第一个子元素之中，这会出现在某些特定环境之中（比如：[jsfiddle.net/necolas/K538S/](jsfiddle.net/necolas/K538S/)）。

此外也可以通过应用`overflow:hidden`或`display:inline-block` 产生新的块级格式化上下文来避免Firefox旧版本的这种行为。




Copyright © Nicolas Gallagher.