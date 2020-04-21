---
title: JavaScript中创建对象的各种模式-《JavaScript高程设》笔记
tags:
  - ECMAScript
  - JavaScript
id: 205
categories:
  - 前端-front-end
date: 2016-11-14 20:30:00
---
JavaScript作为ECMAScript的具体实现，它虽然不是OO（Obeject Oriented）语言，但它却像OO语言一样可以创建对象。不过作为没有“类”概念的对象，JavaScript的对象其实严格来讲只是一组名字和值相对应的“名值对”。

本文将依据《JavaScript高级程序设计（第三版）》第六章的内容，介绍六种创建对象的常用方式。

<!--more-->

### 一、原始模式：

``` javascript
var Person=new Object();
Person.name="JuniorTour";
Person.job="SoftwareEngineer";
Person.sayHello=function () {
  alert("你好！");
};
```
这种模式是通过先创建一个Object()的实例，再为这个实例添加属性和方法实现的。

*   缺点：
    *   重复代码太多。要打很多遍对象的名字，麻烦！
    *   不便于创建相似的多个对象。

### 二、对象字面量模式：

``` javascript
var Person = {
  name:"JuniorTour",
  job:"Software Engineer",
  sayHello:function() {
    alert("你好！");
  }
}
```
这种模式只是使用了不同的语法，但创建的实例和方法一是一样的。

*   优点：
    *   少了一些重复代码。
*   缺点
    *   重复代码还是有点多。
    *   也不便于创建相似的多个对象。

那么，如果我想要创建多个类似的对象（可能有同样的属性和方法，但是却有不同值的），采用上述两种办法得多麻烦啊！该怎么解决呢？


### 三、工厂模式

``` javascript
function createPerson(name,job) {
  var tempPerson=new Object;
  tempPerson.name=name;
  tempPerson.job=job;
  tempPerson.sayHello=function () {
    alert("Hello,I am"+this.name+"!");
  };
  return tempPerson; //这个return是该工厂方式与后续几个方式主要的不同之处。
}

var person1=createPerson("JuniorTour","Software Engineer");
var person2=createPerson("yourname","yourjob");
//注意！也不需要new运算符。这也是与后续的方式不同之处。

//检测实例的类型：
alert("person1 instanceof Object : "+(person1 instanceof Object));  //true
alert("typeof person1 : "+(typeof person1));  //object
alert("person1 instanceof Person : "+(person1 instanceof Person));   // Uncaught ReferenceError: Person is not defined
```

这种模式通过声明能返回特定对象的工厂函数，从而可以方便地创建多个拥有相同属性和方法，却有不同值的相似对象。

这种模式的几个特点是：显式地创建了暂时的中间对象；通过return取得想要的对象；不需要new操作符。

*   优点：
    *   类似OO语言的“类”。
    *   通过封装，提升了重复利用率。
*   缺点
    *   不便于获知对象的“类型”，如上面代码中用来检测实例类型的三行alert()，将得到的结果与下面的构造函数模式对比，就能看出这一缺点。

### 四、构造函数模式

``` javascript
function Person(name,job) {
  this.name=name;
  this.job=job;
  this.sayName=function () {
    alert(this.name);
  };
}

var person1 = new Person("juniortour","Software engineer");
var person2 = new Person("somebody","some job");

  //检测实例的类型：
  alert("person1 instanceof Person : "+(person1 instanceof Person));  //true
  alert("person1 instanceof Object : "+(person1 instanceof Object));  //true
  alert("person2 instanceof Person : "+(person1 instanceof Person));  //true
  alert("person2 instanceof Object : "+(person1 instanceof Object));  //true
  alert("typeof person1 : "+(typeof person1)); //object
  //检测实例的方法是否是同一个Fuction的实例：
  alert(person1.sayName==person2.sayName);    //false
```

这种模式的**特点**是：
1. 没有显式地创建对象；
2. 不用return；
3. 必须用 "new" 操作符。

*   优点：
    *   便于获知对象的“类型”。可以将构造函数所创造的实例标志成一种“类型”（如上例之中的Person“类型”），借助instanceof操作符就可以获知实例对象是否是这种“类型”。
    *   通过封装，提升了重复利用率。
*   缺点
    *   无法实现“函数复用”。用构造函数模式创建的每个对象实例都会拥有一个名为sayName()方法的 Function 实例，可以参考下面这个例子理解这个缺点。

``` javascript
function Person(name,job) {
  this.age=name;
  this.job=job;
  this.sayHello = new Function("alert(this.name)");
  //这与上例在逻辑上是等价的。
}
```

### 五、原型模式

``` javascript
function Person() {}

Person.prototype.name="juniortour";
Person.prototype.job="Software engineer";
Person.prototype.sayName=function () {
  alert(this.name);
};

var person1 = new Person();
person1.sayName();  //"juniortour"

var person2 = new Person();
person2.sayName();  //"juniortour"

//检测实例的方法是否是同一个Fuction的实例：
alert(person1.sayName==person2.sayName);  //true,说明这两个实例共享着同一个sayName函数。
```
我们创建的每一个函数都有一个 prototype 属性，这个属性是一个指针，它指向一个对象，这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。

我的理解是：通过原型属性所指向的原型对象来给创建的实例设置“默认”的属性及其值。

*   优点：
    *   实现了函数复用。
*   缺点
    *   没有用来初始化的参数。
    *   共享属性，这作为原型的一个特性，有些时候却可能不符合我们的目的。比如下面这个例子：
``` javascript
function Person() {}

Person.prototype = {
  constructor:Person,
  name:"juniortour",
  job:"software engineer",
  friends:["xiaoming","xiaohong"]
}

var person1 = new Person();
var person2 = new Person();

person1.friends.push("xiaogang");  //只对person1的friends属性推入"xiaogang"项。

alert(person1.firneds);  //"xiaoming","xiaohong","xiaogang"
alert(person2.firneds);  //"xiaoming","xiaohong","xiaogang"
alert(person1.friends==person2.friends);  //true
```
在这个例子之中，我们首先采用了对象字面量语法重写了整个原型对象，然后只对person1的friends属性用Array的栈方法推入"xiaogang"项，可是person2却也同时获得了"xiaogang"这一项。这就是共享属性。

有时这可能并不是我们想要的结果。

为了避免这种意外，就有了下面这种模式。


### 六、混合模式-组合使用构造函数模式和原型模式

``` javascript
function Person(name,job) {
    this.name=name;
    this.job=job;
    this.friends=["xiaoming","xiaohong"];
}
Person.prototype = {
    constructor:Person,
    sayHello:function () {
        alert("Hello,I am "+this.name+".");
    }
};

var person1 = new Person("JuniorTour","Software Engineer");
var person2 = new Person("somebody","some job");

person1.friends.push("xiaogang");    //只对person1的friends属性推入"xiaogang"项。

alert(person1.friends);    //"xiaoming","xiaohong","xiaogang"
alert(person2.friends);    //"xiaoming","xiaohong"
alert(person1.friends==person2.friends);    //false
alert(person1.sayName==person2.sayName);    //true
alert("person1 instanceof Person : "+(person1 instanceof Person)); //true
alert("person1 instanceof Object : "+(person1 instanceof Object)); //true
```
显然，这种模式同时具有构造函数模式和原型模式两种模式的优点（函数复用、封装、便于获知类型等），同时又解决了各自的缺点（不能函数复用、共享属性、没有初始化的参数等）。

《高程设》一书中说这种模式“是目前在ECMAScript中使用最广泛、认同度最高的一种创建自定义类型的方法。可以说这是用来定义引用类型的一种默认模式。”
