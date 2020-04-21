---
title: 小组分享之《和Vue.js交个朋友～》
tags:
  - JavaScript
  - Vue
categories:
  - Vue
date: 2018-01-22 17:58:00
---

# 小组分享之《和Vue.js交个朋友～》

小组分享－2017年11月



## [配套Slides-和Vue.js交个朋友～](http://slides.com/juniortour/make-friend-with-vuejs/#/)



## 零、引言

>（互动提问）了解一下大家对Vue.js的使用情况，了解过吗？写过吗？

基于上述回答，以及日常生活中和大家的交流，可以看出我们都对这个框架很有兴趣 ，是吧？

我呢，也很感兴趣，我是今年2月开始接触的Vue.js，原因也很简单一是火热流行；二是使用体验感觉很优雅 /  简单；

其实我的水平也不咋样，就是读了读文档而已，可能也并不比在做的各位强多少，所以接下来的内容难免有疏漏之处，大家见谅～



![vue-logo](http://juniortour.net/img/2018/01/make-friend-with-vuejs/vue-logo.png)



<!--more-->



## 一、起步！- What is Vue.js?

>（互动提问）大家对Vue.js是什么印象？



![vue-impression-word-cloud, made by wordart.com](http://juniortour.net/img/2018/01/make-friend-with-vuejs/vue-word-art.png)



老生常谈的一个话题，大家可能都知道Vue.js是一个mv vm的轻量框架，它的速度很快，blabla。

下面我想介绍一些大家可能不太了解的方面。



### 1. 渐进式－Progressive

`渐进式`是进入[Vue.js官网](https://cn.vuejs.org/)后首先映入眼帘的一个词。

那么什么是`progressive`呢？

先从字面意思上来说吧，progressive这个词在英文中本意是前进的，逐渐的，是progress进步、发展的形容词形式，进一步地可以引申为革命的、开明的，以及革命分子这种含义。



单从字面上来理解的话，可以感受到这个框架的一个主要关注点就在`“拓展/发展”`上。



具体来说，就是框架的内核只用于给用户提供基本的视图层的基础，比如说`“声明式渲染”`，开发者完全可以把Vue.js当成jquery用。

如果需要更多的功能，比如说组件系统、前端路由、状态管理等等，开发者可以通过`vue-loader, vue-router, vuex`等插件，有选择地、渐进地追加上这些功能。



这张图，直观地阐释了这个逻辑：

![vue-progressive-example](http://juniortour.net/img/2018/01/make-friend-with-vuejs/vue-progressive-eg.jpg)



可以和angular粗浅地对比一下，angular呢，就像一个工具箱，当你拿到他的时候，你就有了所有需要和不需要的工具，锤子、钳子、螺丝刀，啥都有了。

这其实还不错，工具多了，能处理的问题肯定也越多。

可随之而来的问题也很显著，你总得负担着沉重的一整箱工具，有很多工具也许你并不需要。

vue.js它本身更像是一整套工具中的一个，比如说是个锤子。当你只需要锤子的时候，使用vue.js用起来就已经很顺手了，很顺手、很方便。

当你后续需要更多工具的时候，它也有很多同一个系列的工具可供选择。并且因为它们属于一个生态体系，所以相互之间契合的也很完美。



### 2.数据驱动

这个特点是vue.js的核心特性之一，起源于vue.js在github上的第二个commit，提交于2013年7月就是这个[rename · vuejs/vue@871ed91](https://github.com/vuejs/vue/commit/871ed91#diff-14f0603881f56b2fe6878cb39fa721f0)，vue.js的第一个commit是一些grunt配置文件，所以这第二个commit可以说是vue.js的起点，下面我和大家一块学习一下这个精妙的commit。

[在线预览这个commit的主要html](https://htmlpreview.github.io/?https://github.com/vuejs/vue/blob/871ed9126639c9128c18bb2f19e6afd42c0c5ad9/explorations/getset-revits-style.html)

在这之前，我想先给大家介绍两个vuejs的基础示例，以便于理解这个commit的内容。一是[声明式渲染 - Hello Vue!](https://cn.vuejs.org/v2/guide/index.html#%E5%A3%B0%E6%98%8E%E5%BC%8F%E6%B8%B2%E6%9F%93)，二是[文本插值](https://cn.vuejs.org/v2/guide/index.html#%E5%A3%B0%E6%98%8E%E5%BC%8F%E6%B8%B2%E6%9F%93)，熟悉Angular的同学应该能很轻松地理解这两个例子。

从起初的一些commit来阅读大型项目的源码，也是一种很有益的思路，有助于更好的理解作者的整体思路，还能避免大型海量信息的误导。



我整理并注释了这个commit的主要内容，即`ideal.htm`，可以按照注释的序号来阅读这份代码，或者直接复制一份，自己调试着跑一跑。

```html
<!DOCTYPE html>
<html>
	<head>
		<title>ideal</title>
		<meta charset="utf-8">
	</head>
	<body>
		<div id="test">
			<!-- {{propName}} vue.js称之为“文本插值”，angular里叫“插值表达式”，英文都是 interpolation，修改、差值。-->
			<p>{{msg}}</p>
			<p>{{msg}}</p>
			<p>{{msg}}</p>
			<p>{{what}}</p>
			<p>{{hey}}</p>
		</div>
		<div>
			<button type="buttone" onclick="updateData()">更新数据（data.msg）</button>
		</div>
		<script>
			debugger;
			// 1. 声明绑定标记（即后来的“指令”，例如v-bind:title="message"）
			var bindingMark = 'data-element-binding'
			function Element (id, initData) {

				var self = this,
				    // 2. 记录下根元素，div#test
					el = self.el = document.getElementById(id)
					bindings = {} // the internal copy，数据内部拷贝及绑定目标的保存
					data = self.data = {} // the external interface，外部数据接口
					// 2. 记录下要替换渲染的变量名标记
					content  = el.innerHTML.replace(/\{\{(.*)\}\}/g, markToken)

                  // 4. 把bindingMark，'data-element-binding'属性（指令）替换了文本插值后的字符串
                  // 替换成根元素的html内容
                  // 至此：{{mustache}} => v-bindingMark
				el.innerHTML = content

				for (var variable in bindings) {
					// 5. 给html中带有bindingMark属性（指令）的元素创建数据绑定
					bind(variable)
				}

				if (initData) {
                      // 8. 根据一开始声明实例时的data，初始化一次html，
                      // 后续数据更新后，将继续使用set属性，更新渲染html
                      // 至此：v-bindingMark => <p>data.msg</p>
					for (var variable in initData) {
						data[variable] = initData[variable]
					}
				}

				function markToken (match, variable) {
					bindings[variable] = {}
					// 3. 给每个有文本插值的元素，用bindingMark作为html属性标记（指令）替换掉
					// 文本插值标记
					// 便于用 querySelectorAll 获取和保存元素的引用到bindings。
					return '<span ' + bindingMark + '="' + variable +'"></span>'
				}

				function bind (variable) {
					// 6. 记录下需要重新渲染的html元素的引用，并移除用于标记这些元素的属性（指令）
					bindings[variable].els = el.querySelectorAll('[' + bindingMark + '="' + variable + '"]');
                  	[].forEach.call(bindings[variable].els, function (e) {
				        e.removeAttribute(bindingMark)
				    })
					// 7. 定义绑定的“get和set属性”，用于后续数据更新时，驱动重新渲染更新html模版
					Object.defineProperty(data, variable, {
						set: function (newVal) {
							debugger;
						    [].forEach.call(bindings[variable].els, function (e) {
						        bindings[variable].value = e.textContent = newVal
						    })
						},
						get: function () {
						    return bindings[variable].value
						}
					})
				}
			}
			
			// 1. 创建Element实例，即后来的Vue实例
			var app = new Element('test', {
				msg: 'hello'
			})

			function updateData() {
				window.app.data.msg = 'Hello,JuniorTour'
			}

		</script>
	</body>
</html>
```



大致的流程如下：

<!--发现一个很神奇的bug，下一行不能出现：`-\{\{\}\}`这几个字符，否则hexo就会报错。-->

<!--注释里也不能出现，下文的<p>\{\{msg\}\}</p>会显示为<p></p>-->

首先，对Vue实例所挂载的 el 的 innerHTML 用正则匹配的方法将`文本插值`替换成带有`绑定标记-bindingMark(指令)的span元素`，例如`<p>\{\{msg\}\}</p>`会被修改为`<p><span data-element-binding="msg"></span></p>`。

之后，将替换后的 innerHTML 赋给el，再调用`bind()`建立绑定关系并储存带有`绑定标记`的元素，具体的做法是用`querySelector()`得到所有带有`绑定标记`的元素引用，并保存到数据内部拷贝`bindings[variable].els`之中。

最后，用Vue实例中的data，初始化一次内部拷贝`bindings[variable].els`的内容，帮这些内容都替换成绑定的属性值` bindings[variable].value = e.textContent = newVal`。

后续`variable`改变时，会触发 `set`接口，再做一次 textContent 的替换。

从而实现了`数据绑定`这一特性。





看完了这个Vue.js的“起点”，我们继续回到`“数据驱动”`这个概念上。

数据驱动，驱动什么呢？驱动的是视图，或者说就是模版。也就是根据数据来改变DOM、渲染页面。

作为Vue.js的核心概念之一，在Vue.js的文档中，有多处强调了这个概念，比如：

>  Vue.js 通常鼓励开发人员沿着“数据驱动”的方式思考，避免直接接触 DOM

> 在 Vue 中即使是过渡也是数据驱动的！动态过渡最基本的例子是通过 name 特性来绑定动态值。

> 另一方面，设计$refs主要是提供给 js 程序访问的，并不建议在模板中过度依赖使用它。因为这意味着在实例之外去访问实例状态，违背了 Vue 数据驱动的思想。

具体地说这种理念主要是致力于用数据实现对视图层的控制，让开发者从繁复的DOM操作从解放出来，Make Life Better。



以上就是我个人对Vue.js的一些粗浅的“理解”。



## 二、上手！看看vue-cli

![老夫写代码](http://juniortour.net/img/2018/01/make-friend-with-vuejs/my-programing-is-simple.jpg)

我们讲了半天的理论、原理，接下来这个板块我们就是干！用脚手架工具`vue-cli`干！

[Github－vue-cli](https://github.com/vuejs/vue-cli)

安装完vue-cli，并执行`vue init <template-name> <project-name>`初始化项目后，会生成一些模版文件，以最常用的`vue init webpack my-project`为例，我们一起来看看有哪些“成果”。



![vue-cli-category](http://juniortour.net/img/2018/01/make-friend-with-vuejs/vue-cli-category.png)



我们从下到上倒序介绍。



1. 首先是一些配置文件和package.json。
2. 接下来是test文件夹，里面是e2e端对端测试和unit单元测试两个文件夹，我个人对测试这块不太了解，就不多赘述了。

3. 然后是static文件夹，看名字似乎是静态文件的意思，确实如此。但它和src下的assets目录还是稍有不同的，可以参考这个文档[Handling Static Assets](http://vuejs-templates.github.io/webpack/static.html)。

简单地说就是，static目录中的内容，需要用`static/something.png`这样的`绝对路径`引用，打包编译后，static会原封不动的放在dist目录之中；

而src目录下的内容，包括assets可以用`./assets/logo.png`这样的`相对路径`来引用，并且还会经由webpack处理加工，比如图片通常就会被编译成base64格式，嵌入在html中。

在实际的开发使用中，可以在static里面放一些压缩编译好了的，不会变的类库文件，在src/assets里面放属于该项目的内容并且以后可能会改变内容的资源文件。



4. 下面就是我们熟悉的老朋友－src文件夹了！

但这次，这个老朋友有了新内容！

我们一块来看看，以这个项目为例，主要生成了三个文件夹：assets、component和router，以及`app.vue`根组件和`main.js`两个入口文件。

`app.vue`根组件是被嵌入到index.html中的组件，是整个项目的根基。

`main.js`其实相当于angular的`app.module`，主要的功能就是引入各种模块、创建根实例－`new Vue()`，此外还可以声明一些比较基础的配置，比如这个文件中的`Vue.config.productionTip = false`，再比如说如果引入一些第三方的库，一些初始化的逻辑，也写在这里，如：

```javascript
import FastClick from 'fastclick'

if ('addEventListener' in document) {
    document.addEventListener('DOMContentLoaded', function() {
        FastClick.attach(document.body);
    }, false);
}
```

`src`中的文件夹都可以根据我们的需求定制化的修改的。

比如说我们可以创建一个`pages`文件夹，这个文件夹里放一些我们用来当做一整个页面的“组件”，比如login、home等等。

此外，我们后续如果引用了其他的插件，比如`vuex`等，也可以单独用一个文件夹，放置它们的配置内容，这也是官方文档中推荐的最佳实践。

5.  `config`目录，这是环境配置文件。

6.  最后一个，也是最特别的`build`目录。
  [vue-cli#2.0 webpack 配置分析](https://github.com/DDFE/DDFE-blog/issues/10)

这个目录下的文件，基本都是`webpack`的配置文件。

比如用于编译打包的build.js；用于开发时热更新的dev-client.js / dev-server.js；webpack的基础配置文件等等。

像dev-server.js就是用express框架起了一个本地服务器来运行项目，同时利用webpack实现热更新等功能。



讲到这里呢，我想和大家讨论一个我个人观察到的，比较有意思的地方。

用angular-cli生成的项目看不到webpack的相关配置文件，但是vue-cli这个工具却把webpack的配置暴露给了用户，允许用户可以配置webpack，甚至是本地的dev服务器。

这种脚手架工具的差异也许也反映了这个两个框架在设计理念上的一些差异。






## 中场：讲到这里，我们可以一起来聊一聊感想！

......




## 三、性能！Vue.js的速度和项目体积

一个工具的性能和体积是我们选择时的必考点！对于需要高性能、高可用性的工程化项目，更是如此。

vue.js在性能上是有自己长处的。



### 1. 速度

我相信vue.js比较快这一点大家都有所耳闻，它地区很快，名不虚传。

我们来看一个benchmark，[Results for js web frameworks benchmark – round 5](http://www.stefankrause.net/js-frameworks-benchmark5/webdriver-ts/table.html)



![vue-benchmark](http://juniortour.net/img/2018/01/make-friend-with-vuejs/vue-benchmark.png)



从这个测试来看，vue.js确实很快，在创建一千行元素和更新元素内容等多项操作DOM的各项测试中都有很不错的表现。

同时，angular和react也不慢，这三大框架在速度是可以说是不分伯仲。



### 2. 项目体积

项目体积方面我没有找到比较客观全面的资料可供参考。

只在vuejs的文档中提到了和angular的对比：

>[大小和性能](https://cn.vuejs.org/v2/guide/comparison.html#大小和性能)

> 在性能方面，这两个框架都非常的快，我们也没有足够的实际应用数据来下一个结论。如果你一定想看些数据的话，你可以参考这个第三方跑分。单就这个跑分来看，Vue 似乎比 Angular 要更快一些。

> 在大小方面，最近的 Angular 版本中在使用了 AOT 和 tree-shaking 技术后使得最终的代码体积减小了许多。但即使如此，一个包含了 Vuex + Vue Router 的 Vue 项目 (30kB gzipped) 相比使用了这些优化的 angular-cli 生成的默认项目尺寸 (~130kB) 还是要小的多。



我个人的实际使用经验是这样的，自己用Vue.js做过的一个项目[`vue-weibo`](https://github.com/JuniorTour/vue-weibo)体积相当的小，引入了`vue-router——前端路由, vue-resource——ajax, vuex——应用状态管理`等单页应用必备的插件和几个较小的第三方的插件，再经过gzip后，主要的两个js文件`app.js`和`vendor.js`加起来也只有 62kb。比一个单独的jquery.slim.min.js（95.5kb）还要小。

angular项目，以我做过最简单的几乎相当于静态页面的内部导航页来看，仅仅是这样一个超轻量级的应用，其必要的4个bundle.js文件，体积之和也达到了111kb。估计是因为包含了许多在这个项目中并不必要的前端路由、模块等内容，拖累了项目的整体体积。



## 结语

百闻不如一见～

Vue.js还有很多值得了解的地方。

它有繁荣且活跃的生态体系——[awesome-vue](https://github.com/vuejs/awesome-vue)；

对[ssr（服务器端渲染](https://ssr.vuejs.org/)也有支持；

在跨平台方面也颇有建树——和用于安卓iOS原生平台应用开发的[weex](https://github.com/alibaba/weex)有官方合作；

也和适用于windows平台应用开发的[electron](https://github.com/electron/electron)也有良好的配合。



所以我还是推荐各位通过实践来亲身感受Vue.js的魅力～







参考资料：

[The Progressive Framework](https://docs.google.com/presentation/d/1WnYsxRMiNEArT3xz7xXHdKeH1C-jT92VxmptghJb5Es/edit#slide=id.g1631867b25_0_272)

[Vue2.0 中，“渐进式框架”和“自底向上增量开发的设计”这两个概念是什么？](https://www.zhihu.com/question/51907207)