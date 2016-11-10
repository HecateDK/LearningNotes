### ECMAScript6
  ECMAScript 6.0（以下简称ES6）是JavaScript语言的下一代标准，已经在2015年6月正式发布了。它的目标，是使得JavaScript语言可以用来编写复杂的大型应用程序，成为企业级开发语言。
###### 简介
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





















