---
title: 几种常见的JavaScript继承方式
tags:
  - JavaScript
categories:
  - JavaScript
date: 2017-07-17 11:43:13
---
继承是面向对象语言的一大特性。通过继承，可以让一种类型拥有其他类型的属性和方法。
JavaScript也有自己实现继承的方式，本文将参考《JavaScript高级程序设计》总结并介绍几种常用常见的继承方式。
<!--more-->

### 0.原型链继承：
这种继承方法是通过**将子类的原型重写为父类的实例**从而实现继承的。
``` javascript
//父类：动物
function Animal() {
    this.isLife = true
    this.characteristic = ['living','eat']
}
Animal.prototype.earnLiving=function () {
    console.log('Animal need earn its living.')
}

//子类：鸟类
function Bird() {
    this.canFly = true
}
/*原型链继承：通过将子类的原型重写为父类的实例实现继承。*/
Bird.prototype = new Animal()
/*继承后，子类就会拥有父类的属性和方法，
这也是继承和原型链的目的。*/
Bird.prototype.flying = function (){
    console.log('I am bird, I am flying.')
}

let sparrow = new Bird()
/*继承后，子类就会拥有父类的属性和方法，
这也是继承/原型链的目的。*/
sparrow.earnLiving()
console.log('After inherit sparrow.isLife = '+sparrow.isLife)
sparrow.flying()

/*原型链继承的弊端：公用引用类型问题*/
sparrow.characteristic.push('grayFeather')  // ["living", "eat", "grayFeather"]

let eagle = new Bird()
/*公用引用类型问题是指，子类的所有实例都会共享原型链上的引用类型属性：
eagle也会继承sparrow的grayFeather，这往往不是想要的结果。*/
console.log(eagle.characteristic)   // ["living", "eat", "grayFeather"]
```
原型链继承还有一个弊端就是：不能在继承时传递参数。

原型链继承示意图：
![原型链继承示意图]()

### 1. 借用构造函数继承
这种方法是通过“借调”父类的构造函数，从而在子类**实例**的环境下，让子类的**实例**独立地继承父类的属性和方法，解决了原型链继承所导致的公用引用类型问题和不能在继承时传递参数的弊端。
``` javascript
//父类：动物
function Animal(type) {
    this.type = type
    this.isLife = true
    this.characteristic = ['living','eat']
}

//子类：鸟类
function Bird() {
    /*借用构造函数继承：通过在Bird类实例的环境下
    调用Animal构造函数来实现实例继承属性和方法。
    同时可以结局原型链继承所导致的公用引用类型问题，
    还能实现传递参数！*/
    Animal.call(this,'Bird')
    this.canFly = true
}
let sparrow = new Bird()

sparrow.characteristic.push('grayFeather')
console.log(sparrow.characteristic) // ["living", "eat", "grayFeather"]
console.log(sparrow.type)   //可以在继承时传递参数

let eagle = new Bird()
/*原型链继承的弊端不会在此出现，
sparrow和eagle都有各自独立的引用类型-数组属性。*/
console.log(eagle.characteristic)   //["living", "eat"]
```

借用构造函数继承示意图：
![借用构造函数继承示意图]()

### 2. 组合继承
这种方式将原型链继承和借用构造函数继承组合在一起，相互取长补短，既通过在原型上定义方法实现“函数复用”，用保证每个实例都有自己的属性，避免了公用引用类型问题。是JavaScript中最常用的一种继承方式。
``` javascript
//父类：动物
function Animal(type) {
    this.type = type
    this.isLife = true
    this.characteristic = ['living','eat']
}
//原型链继承：
Animal.prototype.earnLiving=function () {
    console.log('Animal need earn its living.')
}

//子类：鸟类
function Bird() {
    //借用构造函数继承
    Animal.call(this,'Bird')
    this.canFly = true
}
//原型链继承：
Bird.prototype = new Animal()

Bird.prototype.flying = function (){
    console.log('I am bird, I am flying.')
}

let sparrow = new Bird()

sparrow.characteristic.push('grayFeather')
console.log(sparrow.characteristic) // ["living", "eat", "grayFeather"]
console.log(sparrow.type)   //可以在继承时传递参数
//继承了父类方法
sparrow.flying()

let eagle = new Bird()
/*解决了原型链继承的公用引用类型弊端，
sparrow和eagle都有各自独立的引用类型-数组属性。*/
console.log(eagle.characteristic)   //["living", "eat"]
//继承了父类方法
eagle.earnLiving()
```

组合继承示意图：
![组合继承示意图]()


### 3. 识别实例和原型
原生JavaScript有很多方法可以用来检测原型和实例之间的关系，比如`something.prototype.isPrototypeOf(anInstance)`, `Object.getPrototypeOf(anInstance)`, `anInstance.hasOwnProperty('propNameStr')`, `anInstance instanceof anConstructor` 等等。