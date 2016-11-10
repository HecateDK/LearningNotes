### ECMAScript6
>ECMAScript 6.0（以下简称ES6）是JavaScript语言的下一代标准，已经在2015年6月正式发布了。它的目标，是使得JavaScript语言可以用来编写复杂的大型应用程序，成为企业级开发语言。

* [简介](#简介)
* [let, const](#let, const)
* []()
* []()
* []()
* []()
* []()
* []()
* []()
* []()
* []()
* []()
* []()
* []()

##### 简介
最常用的ES6语法是let, const, class, extends, super, arrow functions, template string, destructuring, default, rest arguments等，这些改变将给javascript开发者带来更酷的开发体验，
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

##### let, const
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
var clickBoxs = document.querySelectorAll('.clickBox');
for(var i=0;i<clickBoxs.length;i++){
  clickBoxs[i].onclick = function(){
    consloe.log(i);         //希望点击不同的clickBox，输出不同的i，但无论我们点击哪个clickBox，输出的都是5
  }
}
//采用闭包解决
function iteratorFactory(i){
  var onclick = function(e){
    console.log(i);
  }
  return onclick;
}
var clickBoxs = document.querySelectAll('.clickBox');
for( var i = 0; i < clickBoxs.length;i++ ){
  clickBoxs[i].onclick = iteratorFactory(i);
}
```
###### let
从上面的例子可知，let命令是用来声明变量的，用法和var类似，但是let所声明的变量，只在let命令所在代码块内有效。 <br>
所以for循环的计数器就很适合使用let命令。
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























