### ECMAScript6
>ECMAScript 6.0（以下简称ES6）是JavaScript语言的下一代标准，已经在2015年6月正式发布了。它的目标，是使得JavaScript语言可以用来编写复杂的大型应用程序，成为企业级开发语言。 <br>
>参考 [ECMAScript6入门](http://es6.ruanyifeng.com/)

* [简介](#简介)
* [let, const](#let和const)
* [解构赋值](https://github.com/HecateDK/LearningNotes/blob/master/ECMAScript6/DestructuringAssignment.md)
* [箭头函数](https://github.com/HecateDK/LearningNotes/blob/master/ECMAScript6/ArrowFunctions.md)
* [Promise对象](https://github.com/HecateDK/LearningNotes/blob/master/ECMAScript6/Promise.md)
* [Class类](https://github.com/HecateDK/LearningNotes/blob/master/ECMAScript6/Class.md)
* [Module模板](https://github.com/HecateDK/LearningNotes/blob/master/ECMAScript6/Module.md)

#### 简介
ES6中比较常用的10个特性为： <br>
* Block-Scoped Constructs Let and Const（块作用域构造Let and Const）
* Destructuring Assignment （解构赋值）
* Arrow Functions （箭头函数）
* Promise对象
* Class（类）
* Module（模块）
* Enhanced Object Literals （增强的对象文本）
* Default Parameters（默认参数）
* Template Literals （模板文本）
* Multi-line Strings （多行字符串）

这些改变将给javascript开发者带来更酷的开发体验。 <br>
但是尽管很多的浏览器对ES6支持性都很好，但还是免不了要对ES6进行转码。 <br>
这就需要用到Babel——一个广泛使用的ES6转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。这意味着，你可以用ES6的方式编写程序，又不用担心现有环境是否支持。 <br>
例如：
```javascript
//转码前
arr.map(( item,index ) => item + 1);      // => 是function的简写形式 
//转码后
arr.map(function( item,index ){
  return item + 1;
});
```
Babel的使用说明可以点击 [Babel用户手册](https://github.com/thejameskyle/babel-handbook/blob/master/translations/zh-Hans/user-handbook.md)，喜欢看英文文档的可以直接点击 [这里](https://babeljs.io/)。

#### let和const
let、const命令的用途与var类似，都是用来声明变量的，但是现实运用中还是有差别的，请看下面例子： 
```javascript
var a = 'aaa';
while(true){
  var a ='aaaaaa';
  console.log(a);     //aaaaaa
  break;
}
console.log(a);       //aaaaaa
```
因为ES5中只有全局作用域和函数作用域，没有块级作用域，所以使用var两次输出都是aaaaaa，这就会带来很多不方便的地方了： <br>
第一种情况是：内层变量覆盖外层变量：
```javascript
//var 声明变量
var b = new Date();
function f(){
  console.log(b);
  if(false){
    var b = 'bbb';
  }
}
f();    //输出结果为undefined，原因在于变量提升，导致内层的b覆盖了外层的b变量
//let声明变量
let c = 'ccc';
while(true){
  let c = 'cccccc';
  console.log(c);    //cccccc
  break;
}
consloe.log(c);     //ccc
```
第二种情况是：用于计数的循环变量泄漏为全局变量。
```javascript
var a = [];
for( var i = 0; i < 10;i++ ){
  a[i] = function(){
    console.log(i);
  };
}
a[6]();      //输出结果为10，因为变量i是var声明的，在全局范围内有效，所以每一次循环。新的i值就会覆盖旧值，导致最后输出的结果为最后一轮的i值，即10

var a = [];
for( let i = 0;i < 10; i++ ){
  a[i] = function(){
    consoe.log(i);
  };
}
a[6]();    //输出结果为6，因为变量i是let声明的，当前的i只在本轮循环内有效，所以每一次循环的i值其实都是一个新的变量，所以a[6]()的结果为6

```
当然我们也可以利用闭包来解决“计数循环变量泄漏为全局变量”的问题，例如：
```javascript
//闭包：可以访问外部函数作用域中变量的函数，是为了被内部函数访问的外部函数的变量可以保存在外部函数作用域中而不被回收。
var a = [];
for(var i = 0; i < 2;i++){
  a[i] = (function()i{
    return function(){
      console.log(i);
    };
  }(i));
};
a[6]();    
```
也可利用属性方式：
```javascript
var a = [];
for(var i=0;i<2;i++){
  (a[i] = function self(){
    console.log(self.x);
  }).x = i;
};
a[i]();
```
###### let
从上面的例子可知，let命令是用来声明变量的，用法和var类似，但是let所声明的变量，只在let命令所在代码块内有效。 
所以for循环的计数器就很适合使用let命令。<br>


>let命令有四大主要特性：存在块级作用域，没有变量提升，暂时性死区，不允许重复声明。

###### let命令不存在变量提升
let声明的变量存在块级作用域，函数本身的作用域，只存在其所在的块级作用域之内。 <br>
什么是变量提升呢？
```
function test(){
  foo();    //TypeError:'foo is not a function'
  bar();    //'this will run'
  var foo = function(){
    alert('this won't run!');
  }
  function bar(){
    alert('this will run!');
  }
}
test();
```
上述例子中，foo()函数调用时候出错了，但是bar却能够正常调用。这是因为var foo 首先会上升到函数顶部，然而此时的foo为undefined。所以执行报错。 <br>
而对于函数bar，函数本身也是一种变量，所以能够存在变量提升现象，但是它上升了整个函数，所以bar()才能顺利执行。
```javascript
function f(){ console.log('I am outside!'); }
if(true){
  //重复声明一次函数f
  function f(){ console.log('I am inside!'); }
}
f();
```
上面的代码如果再ES5中运行输出的结果会是'I am outside!'，因为f被条件语句中的f上升覆盖了，在ES6中输出的是'I am inside!'，块级作用域中定义的函数不会影响外部。
###### 暂时性死区
什么是暂时性死区？let和const声明的变量都拥有暂时性死区（TDZ）——当进入它的作用域，它不能被访问（获取或者设置）直到执行到达声明。 <br>
和var有何区别呢？
* 当进入var变量的作用域（包含它的函数），立即为它创建/绑定存储空间，变量会立即初始化并赋值为undefined。
* 当执行到变量声明的时候，如果变量定义了值就会被赋值。
而通过let声明的变量拥有暂时性死区，生命周期为：
* 当进入let变量的作用域（包含它的语法块），会立即为它创建/绑定存储空间，但此时变量尚未初始化。
* 获取或设置未初始化的变量就会抛出异常ReferenceError。
* 当执行到变量声明的时候，如果变量定义了值就会被赋值，如果没有定义值，则会被赋值为undefined。
```javascript
//在TDZ内部，如果获取或设置变量将抛出异常
if(true){   //enter new scope, TDZ starts
  //Uninitialized binding for `tmp` is created
  tmp = 'abc';
  console.log(tmp);  //ReferenceError
  let tmp;   //TDZ ends, `tmp` is initialized with `undefined`
  console.log(tmp);  //undefined
  tmp = 123;
  console.log(tmp);   //123
}

//死区是真正短暂的和不受位置制约的
if(true){
  const func = function(){
    console.log(myVar);   //OK
  };
  //Here we are within the TDZ and accessing `myVar` would cause a `ReferenceError`
  let myVar = 3;   //TDZ ends
  func();   //called outside TDZ
}
```
>变量在暂时性死区内无法被访问，所以无法对它使用typeof。
>总之，暂时性死区的本质就是只要一进入当前的作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。

###### 不允许重复声明 
let不可以在相同作用域内重复声明同一个变量，包括不能和var、const变量名重复的变量，例如：
```javascript
let a = 1;
let a = 1; // 报错 let不可重复声明

var b = 1;
let b = 1; // 报错 let不可重复声明

const c = 1;
let c = 1; // 报错 let不可重复声明
```
同样，不能在函数内部重新声明参数。
```javascript
function func(arg) {
  let arg; // 报错
}

function func(arg) {
  {
    let arg; // 不报错
  }
}
```
##### 块级作用域
let为javascript新增了块级作用域。
```javascript
function f(){
  let n = 5;
  if(true){
    let n = 10;
  }
  console.log(n);   //5
}
```
上面函数的两个代码块都声明了变量n，运行后输出5.这表明外层代码块不受内层代码块的影响。但是如果使用var声明，那么最终输出的就是10.
```javascript
//ES6允许块级作用域的任意嵌套
{{{{{let insane = 'Hello World'}}}}};     //这里使用了五层的块级作用域

//外层作用域无法读取内部作用域的变量
{{{{
  {let insane = 'Hello World'}
  console.log(insane); // 报错
}}}};

//内层作用域可以定义外层作用域的同名变量
{{{{
  let insane = 'Hello World';
  {let insane = 'Hello World'}
}}}};

```
块级作用域的出现，使得立即执行匿名函数不再必要了。
```javascript
// IIFE写法
(function () {
  var tmp = ...;
  ...
}());

// 块级作用域写法
{
  let tmp = ...;
  ...
}
```
##### ES6中引入了块级作用域，允许在块级作用域之中声明函数。
```javascript
// ES5严格模式
'use strict';
if (true) {
  function f() {}    // 报错
}

// ES6严格模式
'use strict';
if (true) {
  function f() {}    // 不报错
}
```
ES6中规定，块级作用域之中，函数声明语句的行为类似于let，在块级作用域之外不可引用。 <br>
但是考虑到浏览器的支持情况，使用应该避免在块级作用域内声明函数，如果确实需要，也应该写成函数表达式，而不是函数声明语句。
```javascript
//函数声明语句
{
  let a = 'secret';
  function f(){
    return a;
  }
}
//函数表达式
{
  let a = 'secret';
  let f = function(){
    return a;
  }
}
```
另外，ES6的块级作用域允许声明函数的规则，只在大括号的情况下成立，如果没有使用大括号，就会报错。
```javascript
// 不报错
'use strict';
if (true) {
  let f = function() {}
}

// 报错
'use strict';
if (true)
  let f = function() {}
```

###### const
const声明的是一个常量，也就是说const一旦声明，就必须初始化，不能留到以后赋值。 <br>
const的作用域与let相同：只在声明所在的块级作用域内有效； <br>
const命令声明的常量也是不提升的，同样存在暂时性死区，只能在声明的位置后面使用； <br>
const声明的常量也与let一样不可重复声明。 <br>

对于复合类型的变量，变量名不指向数据，而是指向数据所在的地址，const命令只是保证变量名指向的地址不变，并不保证该地址的数据不变。
```javascript
const foo = {};   
foo.prop = 123;  
foo.prop   //123
foo = {};  // TypeError: "foo" is read-only
//foo存储的是一个地址，这个地址指向一个对象，不可变的知识这个地址，所以不能把foo指向另一个地址，但对象本身是可变的，所以依然可以为其添加新属性。

//常量a是一个数组，这个数组本身是可写的，但是如果把另一个数组赋值给a，就会报错。
const a = [];
a.push('Hello');   //可执行
a.length = 0;      //可执行
a = ['dave'];      //报错
```

>const有一个很好的应用场景，就是当我们引用第三方库的时候声明的变量，用const来声明可以避免未来不小心重命名而导致出现的bug。

```javascript
const momen = require('moment')
```
###### 冻结对象——Object.freeze方法
```javascript
const foo = Object.freeze({});
//常规模式时，下面的代码不起作用
//严格模式时，该行代码会报错
foo.prop = 123;          
```
除了将对象本身冻结，还可以对对象的属性进行冻结。
```javascript
var constantize = (obj) => {
  Object.freeze(obj);
  Object.keys(obj).forEach( (key,value) => {
    if( typeof obj[key] === 'object' ){
      constantize( obj[key] );
    }
  } );
};
```

##### 顶层对象
顶层对象，在浏览器环境中指的是window，但是在Node中指的却是global对象。 <br>
在ES5中，顶层对象的属性和全局变量的属性是等价的。
```javascript
//顶层对象的属性赋值和全局变量的赋值，是同一件事。
window.a = 1;
a             //1
a = 2;
window.a      //2
```
ES6中规定，为了保持兼容性，var命令和function命令声明的全局变量，依旧是顶层对象的属性； <br>
let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性。
```javascript
var a = 1;
// 如果在Node的REPL环境，可以写成global.a
// 或者采用通用方法，写成this.a
window.a // 1

let b = 1;
window.b // undefined
```
同一段代码为了能够在各种环境下都能读取到顶层对象，一般使用this，但也有其局限性。 <br>
很难找到一种方法，可以在所有情况下都能读取到顶层对象，下面是两种勉强可以使用的方法： <br>
```javascript
// 方法一
(typeof window !== 'undefined'
   ? window
   : (typeof process === 'object' &&
      typeof require === 'function' &&
      typeof global === 'object')
     ? global
     : this);

// 方法二
var getGlobal = function () {
  if (typeof self !== 'undefined') { return self; }
  if (typeof window !== 'undefined') { return window; }
  if (typeof global !== 'undefined') { return global; }
  throw new Error('unable to locate global object');
};
```



















