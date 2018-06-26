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


