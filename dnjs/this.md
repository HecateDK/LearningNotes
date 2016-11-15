### this
为什么要使用this?
```javascript
function identify(){
  return this.name.toUpperCase();
}
function speak(){
  var greeting = "Hello" + identify.call(this);
  console.log(greeting);
}
var me = {
  name : "kely"
};
var you = {
  name : "Reader"
};
identify.call(me);   // KELY
identify.call(you);  // READER
speak.call(me);      // Hello,kely
speak.call(you);     // Hello,Reader
```
上述代码可以在不同的上下文对象(me和you)中重复使用函数identity()和speak()，不用针对每个对象编写不同版本的函数。 <br>
如果不使用this，那就需要给identify()和speak()显式地存入一个上下文对象： 
```javascript
function identify(content){
  return content.name.toUpperCase();
}
function speak(content){
  var greeting = "Hello" + identify(content);
  console.log(greeting);
}
var me = {
  name : "kely"
};
var you = {
  name : "Reader"
};
identify.call(me);   // KELY
identify.call(you);  // READER
speak.call(me);      // Hello,kely
speak.call(you);     // Hello,Reader
```
随着使用的模式越来越多，显式地传递上下文对象会让代码变得越来越乱。而this提供了一种更优雅的方式“传递”一个对象引用，因此可以将API设计得更加简洁并且易于复用。 <br>
###### this并不像我们所想的那样指向函数本身
###### this并不全是指向函数的作用域
```javascript
function foo(num){
  console.log("foo:" + num);
  // 记录foo被调用的次数
  this.count++;
}
foo.count = 0;
var i;
for(i=0;i<10;i++){
  if(i>5){
    console.log(foo(i));  // foo:6  foo:7  foo:8  foo:9
  }
}
console.log(foo.count);  // 0
```
foo()确实被调用了4次，执行foo.count = 0 时，的确向函数对象foo添加了一个属性count，但是函数内部代码this.count中的this并不是指向那个函数对象，所以属性名相同，根对象却并不相同。




待续。。。







































































