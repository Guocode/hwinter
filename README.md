# hwinter
nodejs study notes
================================================
event loop

// 引入 events 模块
var events = require('events');
// 创建 eventEmitter 对象 <事件触发器>
var eventEmitter = new events.EventEmitter();
// 绑定事件及事件的处理程序 <绑定事件名及执行函数> 执行函数的定义 
/*
var eventHandler = function func_name() {
    //
}
*/
eventEmitter.on('eventName', eventHandler);
我们可以通过程序触发事件：
// 触发事件
eventEmitter.emit('eventName');

The difference is that functionOne is a function expression and so !only defined when that line is reached!, whereas functionTwo is a function declaration and is defined !as soon as its surrounding function or script is executed (due to 函数提升（Hoisting）)!.

For example, a function expression:

// TypeError: functionOne is not a function
functionOne();
var functionOne = function() {
  console.log("Hello!");
};

And, a function declaration:

// Outputs: "Hello!"
functionTwo();

function functionTwo() {
  console.log("Hello!");
}
This also means you can't conditionally define functions using function declarations:

if (test) {
   // Error or misbehavior
   function functionThree() { doSomething(); }
}
The above actually defines functionThree irrespective of test's value — unless use strict is in effect, in which case it simply raises an error.

============================================================================
变量提升：函数声明和变量声明总是会被解释器悄悄地被"提升"到方法体的最顶部。
JavaScript 只有声明的变量会提升，初始化的不会。
    var x = 5; // 初始化 x
    x = 5; // 变量 x 设置为 5
    var x; // 声明 x

非严格模式下给未声明变量赋值创建的全局变量，是全局对象的可配置属性，可以删除。
var var1 = 1; // 不可配置全局属性
var2 = 2; // 没有使用 var 声明，可配置全局属性

console.log(this.var1); // 1
console.log(window.var1); // 1

delete var1; // false 无法删除
console.log(var1); //1

delete var2; 
console.log(delete var2); // true
console.log(var2); // 已经删除 报错变量未定义

==================================================================================
JavaScript 闭包
还记得函数自我调用吗？该函数会做什么？
实例
var add = (function () {
    var counter = 0;
    return function () {return counter += 1;}
})();
 
add();
add();
add();
 
// 计数器为 3 !!-add是一个函数，这个函数相当于从自调用的函数中抽离出来一个给count加一的函数-!!

实例解析
变量 add 指定了函数自我调用的返回字值。
自我调用函数只执行一次。设置计数器为 0。并返回函数表达式。
add变量可以作为一个函数使用。非常棒的部分是它可以访问函数上一层作用域的计数器。
这个叫作 JavaScript 闭包。它使得函数拥有私有变量变成可能。
计数器受匿名函数的作用域保护，只能通过 add 方法修改。
Note	闭包是可访问上一层函数作用域里变量的函数，即便上一层函数已经关闭。

 (function(){})是一个标准的函数定义，但是没有复制给任何变量。所以是没有名字的函数，叫匿名函数。没有名字就无法像普通函数那样随时随地调用了，所以在他定义完成后就马上调用他，后面的括号()是运行这个函数的意思
 
===
变量声明时如果不使用 var 关键字，那么它就是一个全局变量，即便它在函数内定义。
===

var x = (function () {
    ...
    return function(){
    ...}//return给x的function。自调用函数体一直全局存在，相当于一个无法直接调用的匿名函数，自调用函数中的return会赋给等号左边的var，可以return一个         //值或一个函数对象，如果是函数对象就可以在外部调用此函数来操作内部属性，这个函数就被称为闭包。通俗地讲就是将一个匿名函数的内部方法的指针抽离         //（暴露）出来供外部使用，有些类似于java中私有属性的共有方法操作。
})();
