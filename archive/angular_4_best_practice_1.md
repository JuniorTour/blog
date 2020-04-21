---
title: Angular 4 最佳实践总结（一）
tags:
  - JavaScript
  - Angular 
categories:
  - Angular 
date: 2017-12-13 11:32:06
---

# Angular 4 最佳实践总结（一）

2017/9/15 - 2017/11

### 1. toggle button 开关按钮 

<!--more-->

```JavaScript
<button
	[class.toggled]="isToggled"
	class="btn"
	(click)="isToggled = !isToggled">
	开关按钮
</button>
```

### 2. 异步获取的数据渲染页面避免报错undefined

```
　<h1 [title]="xhrData?.name"></h1>
```

通过给要绑定属性加上问号❓－安全导航操作符（[https://v2.angular.cn/docs/ts/latest/guide/template-syntax.html#!#safe-navigation-operator](https://v2.angular.cn/docs/ts/latest/guide/template-syntax.html#!#safe-navigation-operator)）来实现。




### 3. 监测子组件@Input()属性的变化：

如果需要监测子组件输入属性的变化，有很多需要注意的地方。

一般情况下，可以直接使用angular自带的生命周期钩子来监测：

```JavaScript
ngOnChanges(changes: SimpleChanges) {
    this.doSomething(changes.categoryId.currentValue);
    // You can also use categoryId.previousValue and 
    // categoryId.firstChange for comparing old and new values

}
```

但是，但是，但是，这种结局放啊有很**严重的弊端**，即当输入属性的值是一个引用（reference）时，`ngOnChanges `就不会监测到这个值的变化。

这对于angular来说，是正确的行为，因为从Angular的视角来看，输入属性的值－－`引用`没有发生变化，自然也就没有什么需要报告的变化了。

比如说，最常见的，当@Input()属性中有Array<object>的值变化时，`ngOnChanges `就不会触发。

例如arrObj这个值：

```JavaScript
arrObj = [
	{name: string},
	{name: string}
]
```

当父组件中 arrObj 中一个对象的 name 值变化时，子组件：

```JavaScript
@Input() arrObj: any;

// ...

ngOnChanges(changes: SimpleChanges) {
	console.log(changes);
}

```

并不会触发ngOnChanges钩子，因为 angular 认为 arrObj 这个数组并没有变化（这个数组的引用没有发生变化），

更详细、多样的资料可以参考以下链接：

[https://stackoverflow.com/questions/38571812/how-to-detect-when-an-input-value-changes-in-angular2](https://stackoverflow.com/questions/38571812/how-to-detect-when-an-input-value-changes-in-angular2)

[https://stackoverflow.com/questions/34796901/angular2-change-detection-ngonchanges-not-firing-for-nested-object](https://stackoverflow.com/questions/34796901/angular2-change-detection-ngonchanges-not-firing-for-nested-object)

[https://v2.angular.cn/docs/ts/latest/guide/lifecycle-hooks.html#!#onchanges](https://v2.angular.cn/docs/ts/latest/guide/lifecycle-hooks.html#!#onchanges)



### 4. 解决数据视图不同步问题

可以考虑`ChangeDetectorRef`

参考：

[https://stackoverflow.com/questions/41364386/whats-the-difference-between-markforcheck-and-detectchanges](https://stackoverflow.com/questions/41364386/whats-the-difference-between-markforcheck-and-detectchanges)

[https://angular.io/api/core/ChangeDetectorRef](https://angular.io/api/core/ChangeDetectorRef)

以及一个鲜活的例子，便于理解：[https://angular.io/api/core/ChangeDetectorRef](https://angular.io/api/core/ChangeDetectorRef)



### 5.  监听表单元素输入值的变化：


1. angular api

当在表单中使用[(ngModel)]时，必须要定义name属性，否则ngModel不会生效！


```JavaScript
<select class="form-control" 
	[ngModel]="selectedWorkout" 
	(ngModelChange)="onChange($event)">
    <option *ngFor="#workout of workouts" 
    		[value]="workout.id">
    		{{workout.name}}
    </option>
</select>

```

2. standard js api-原生JS的API

```JavaScript
<select class="form-control" 
		[(ngModel)]="selectedWorkout" 
		(change)="onChange($event.target.value)">
    <option *ngFor="#workout of workouts" 
    		[value]="workout.id" >
    		{{workout.name}}
    </option>
</select>
```



### 6. 获取当前url

情况1：非app.component根组件时

```JavaScript
import { Router } from '@angular/router';
export class MyComponent implements OnInit {

    constructor(private router:Router) { ... }

    ngOnInit() {
        let currentUrl = this.router.url;
        // ......
    }

}
```

情况2: 根组件app.component的ngOnInit()之中

在app.component的ngOnInit()之中，直接获取`this.router.url`只会得到‘／’这个空路径，

>	I think there is some confusion here related to where this code is used. If you use it in the app.component which hosts the <router-outlet> tag it will always return / while in the child components it works properly. – Raul A. Jun 14 at 4:10
>[https://stackoverflow.com/questions/42538251/angular-2-get-current-route](https://stackoverflow.com/questions/42538251/angular-2-get-current-route)


需要一些hack才能获取到当前访问的url。

```JavaScript
import { Router, Event, NavigationEnd} from '@angular/router';

    this.router.events.subscribe( (event: Event) => {
      if (event instanceof NavigationEnd ) {
        this.currentUrl = event.url;
      }
    });
```



### 7. vim常用操作记录

`$` - end of line

`0` - start of line

`shift/option` - end of file

在控制台中用起来其实还是不实用、不好用，╮(╯▽╰)╭。



### 8. TypeScript special syntax

1. constructor

定义一个Hero类，具有一个构造函数和两个属性：id和name。

利用 TypeScript 提供的简写形式 —— 用构造函数的参数直接定义属性。

```JavaScript
// src/app/hero.ts (excerpt)

//  变量赋值风格
  title = 'Tour of Heroes';
  myHero = 'Windstorm';

//  构造函数风格
export class Hero {
  constructor(
    public id: number,
    public name: string) { }
}
```

构造函数风格的简写语法做了很多事情：

- 声明了一个构造函数参数及其类型
- 声明了一个同名的公共属性
- 当我们new出该类的一个实例时，把该属性初始化为相应的参数值


使用构造函数还是变量初始化？

为了让本应用更加简短，应该采用更简单的“变量赋值”风格。



### 9. 术语及基本概念总结

- @NgModule装饰器`修饰`的类；@NgModule接受一个元数据对象，告诉 Angular 如何编译和启动应用；
- imports数组中应该只有NgModule类。不要放置其它类型的类。
  `NgZorroAntdModule.forRoot()`
- declarations数组中可以放置自己创建的类、管道、指令。
- providers数组中用于依赖注入时，注册服务商。
- {{文本插值表达式}}

### 10. Angular引用html元素

```JavaScript
import { Component,
         ViewChild,
         AfterViewInit,
         ElementRef } from '@angular/core';
// 别忘了在这里引入ElementRef类型

@Component({
  selector: 'app-root',
  template: `<input #someInput></input>`,
  styleUrls: ['./app.component.css']
})
export class AppComponent implements AfterViewInit {
  @ViewChild('someInput') someInput: ElementRef;
  // 类型是ElementRef，注意和引用子组件的区别！

  ngAfterViewInit() {
    this.someInput.nativeElement.value = "Anchovies! 🍕🍕";
  }
}
```



### 11. Angular引用子组件，从而获得其属性和方法

```JavaScript
import { Component,
         ViewChild,
         AfterViewInit } from '@angular/core';

import { ChildComponent } from './child.component';
// 别忘了在这里引入ChildComponent类型

@Component({
  selector: 'app-root',
  template: `<app-child-component #ChildComponent></app-child-component>`,
  styleUrls: ['./app.component.css']
})
export class AppComponent implements AfterViewInit {
  @ViewChild(ChildComponent) child: ChildComponent;
  // 类型是ChildComponent，注意和引用html元素的区别！

  ngAfterViewInit() {
    console.log(this.child.whoAmI());  // 👶 I am a child!
  }
}
```