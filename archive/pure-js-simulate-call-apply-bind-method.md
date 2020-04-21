---
title: 简介并用原生Js模拟call、apply和bind方法
tags:
  - JavaScript
id: 176
categories:
  - javascript
  - 前端-front-end
date: 2017-06-13 10:52:00
---
# 

call、apply和bind方法都是函数对象的原型方法（Function.prototype），也都有很特殊的用处，初学JavaScript不太容易理解其具体机制，本文就将通过逐步模拟这些方法的实现来介绍这些方法并帮助理解。
相信通过这篇文章，读者一定能对call、apply和bind方法有一个更加全面的认识。

<!--more-->

## 一、Function.prototype.call()：
### 1. 简介call():
语法：`func.call(thisArg[, arg1[, arg2[, ...]]])`
简单来说，call方法的作用就是：“在指定一个this值和若干个指定的参数值的前提下调用某个函数或方法”。

因为接下来介绍的这些方法和this都有很密切的关系，所以，我向各位推荐一篇文章帮助理解this：[《关于 this 你想知道的一切都在这里》](https://segmentfault.com/a/1190000008156495)。

举个栗子来说明：
``` JavaScript
function foo () {
console.log(this.val)
}

let obj = {
val: 1
}

console.log('foo.call(obj) : ')
foo.call(obj) //1
```

### 2. 模拟实现：
我们模拟的步骤是：
1. 将函数设为对象的方法。
2. 执行该函数,
    - 注意三点：
        1. A.call方法可带“任意个”参数。
        2. B.参数可以为null和undefined。
        3. C.并且函数可以有返回值。
3. 删除该函数，返回返回值。

``` JavaScript
Function.prototype.myCall=function(context) {
    context=context||window    //B.处理参数为null和undefined的情况
    context.fn=this                    //1.把调用myCall的函数设为对象的方法
    let targetArg=[]                   //A.注意call方法可带“任意个”参数
    //处理传入的参数：
    for (let i=1, len=arguments.lengthi<leni++) {
        //targetArg.push(arguments[i])
        /*直接像上一句这样处理，最后的targetArg是： ["me", 21]，经过'context.fn('+targetArg+')'之后，
        * 是"context.fn(me,21)"，把me当成了一个变量，而非一个字符串，所以不行。*/

        targetArg.push('arguments['+i+']')
        /*targetArg是:["arguments[1]", "arguments[2]"]，经过...后，
        * 是"context.fn(arguments[1],arguments[2])"，可行！*/

        /*也就是说之所以选择用字符串拼接，是因为考虑到参数可能是字符串，直接传给eval会导致转换成一个变量，无法执行。*/
    }
    let result=eval('context.fn('+targetArg+')')   //2.执行该函数；C.函数可以有返回值。
    //context.fn()
    delete context.fn
    return result
}
```

### 3. 嗅探兼容：
这种给内置对象扩展方法的实现形式，其实叫做：“Monkey patching(猴子补丁)”，其实可以做一步“嗅探”来保证兼容：

``` JavaScript
Function.prototype.myCall=Function.prototype.call || function(context) {/*...*/}
/*这一步的含义是：如果Function.prototype.call存在，就用原生的这个方法，否则用我们自己模拟实现的。*/
```
后续的几个模拟实现，都可以这样做。

## 二、Function.prototype.apply()：
### 1.简介apply()：
语法：`func.apply(thisArg, [argsArray])`
apply和call的用法基本相同，两者的第一个参数都是func函数运行时的this值。
唯一的一个细节上的区别在于，call可以接受传入多个参数，而bind只接受一个数组传入作为参数。

### 2. 模拟实现：
``` JavaScript
Function.prototype.myApply=function (context,argArr) {
    context=context||window
    context.fn=this
    let result

    if (argArr) {
        let targetArg=[]
        for (let i= 0,len=argArr.length;i<len;i++) {
            //注意，此处是argArr参数，而非arguments了。
            targetArg.push('argArr['+i+']')
        }
        result=eval('context.fn('+targetArg+')')
    } else {
        result=context.fn()
    }
    return result
}
```

## 三、Function.prototype.bind()：
### 1. 简介bind()：
语法：`func.bind(thisArg[, arg1[, arg2[, ...]]])`
这个方法比较特别，该方法创建一个**新新新新新新**函数并返回，称之为*绑定函数*，绑定函数会以创建它时传入bind方法的第一个参数作为this值， 第二个以及以后的参数，将当做这个新的绑定函数的**初始预设参数**。
之后调用这个新的绑定函数时传递给绑定函数的其他参数会跟在预设参数之后传入。

看一个栗子 就明白了：
``` JavaScript
  function bindFoo() {
    //用call把arguments指定为this值用于slice()处理，slice 方法可以把一个类数组（Array-like）对象/集合转换成一个数组。
    console.log('[].slice.call(arguments)  = ',[].slice.call(arguments))
  }
  bindFoo(1,2,3);  //[1,2,3]

  let bindedFoo=bindFoo.bind(undefined,'预先绑定的参数');
  bindedFoo();    //[“预先绑定的参数”]
  bindedFoo(1,2,3);   //[“预先绑定的参数”,1,2,3]
```

### 2. 模拟实现：
注意bind()的几个特点：
1. 返回一个新函数，新函数会继承原来构造函数的原型。
2. 可以传入参数，用作绑定的绑定预设参数。
3. 有构造函数的效果:一个绑定函数也能使用new操作符创建对象。
>要注意，[MDN上对于这种用法有一个警告](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#作为构造函数使用的绑定函数)：“警告 :这部分演示了 JavaScript 的能力并且记录了 bind() 的超前用法。以下展示的方法并不是最佳的解决方案且可能不应该用在任何生产环境中。”

因为模拟构造函数的效果比较不容易理解，且不被推荐，所以在这里我们分别来实现。

#### 先不模拟构造函数的效果：
``` JavaScript
Function.prototype.myBind = function (context) {
    if (typeof this !== 'function') {
      throw new Error('Function.prototype.bind - what is trying to be bound is not callable.')
    }

    let self = this
    //获取传入的从下标为[1]开始的参数作为预设参数：
    let args = Array.prototype.slice.call(arguments, 1)

    return fBound = function () {
      //这是预设参数之外传入的参数，即bind返回的新函数调用时传入的参数
      let bindArgs = Array.prototype.slice.call(arguments)

      // 改变绑定函数中 this 的指向
      self.myApply(context,args.concat(bindArgs))
    }
}
```

#### 模拟构造函数的效果版：
``` JavaScript
      Function.prototype.myBind = function (context) {
        if (typeof this !== 'function') {
          throw new Error('Function.prototype.bind - what is trying to be bound is not callable.')
        }

        let self = this
        //获取传入的从[1]开始的参数作为预设参数：
        let args = Array.prototype.slice.call(arguments, 1)

        let fBound = function () {
        //2.这是预设参数之外传入的参数，即bind返回的新函数调用时传入的参数
          let bindArgs = Array.prototype.slice.call(arguments)
        /* 3.“this instanceof self”用于判断是否用作构造函数。
         * 因为在使用new时，绑定函数内的this值会被改成fBound，fBound是self（即originConstructor）的实例，
         * 因为它来自于originConstructor.myBind(func, 'daisy')，通过Object.getPrototypeOf(this) === self.prototype，可以检测出来。
         * 而没有使用new时，this则是Window，不是self的实例。
         *
         * A:如果结果为true，用作了构造函数，那么原来绑定的this值要被忽略，构造函数中的this要指向实例fBound，用于构造新的实例。
         * 此时在myApply内部最终会指向：context.fn(argArr[0],argArr[1])。
         * 即fBound.originConstructor(argArr[0],argArr[1])。
         *
         * B:如果结果为false，那么就是没有用作构造函数，用myApply把预先绑定的this传入即可。*/

          self.myApply(this instanceof self ? this : context, args.concat(bindArgs))
          // self.myApply(context,args.concat(bindArgs))
        }

        //1.new构造出一个实例的时候，也会继承原来构造函数的原型：
        /*如果只是fBound.prototype = this.prototype，当我们直接修改fBound.prototype的时候，
        也会一并顺带修改绑定函数的prototype，这不是我们想要的结果。
        这个时候，我们可以通过一个空函数来进行中转：*/
        let fEmpty = function () {}
        fEmpty.prototype = this.prototype
        fBound.prototype = new fEmpty()

        return fBound
      }
```

这部分的内容已经逐步深入到JavaScript的内部实现原理之中，确实有些晦涩，我们可以通过亲手写代码并多多调试观察加深理解。

### 参考文献：
[JavaScript深入之call和apply的模拟实现](https://segmentfault.com/a/1190000009257663)，
[JavaScript深入之bind的模拟实现](https://segmentfault.com/a/1190000009271416)，
[可能遇到假的面试题：不用call和apply方法模拟实现ES5的bind方法](https://segmentfault.com/a/1190000009265185)