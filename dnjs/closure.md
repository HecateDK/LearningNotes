### 作用域闭包
当函数可以记住并访问所在的词法作用域时，就产生了闭包，即使函数是在当前词法作用域之外执行的。
```javascript
// 下面代码并不是闭包，但bar()对a的引用方法是词法作用域的查找规则是闭包很重要的一部分
function foo(){
  var a = 2;
  function bar(){
    console.log(a);   // 2
  }
  bar();
}
foo();
```
什么是闭包呢？
```javascript
function foo(){
  var a = 2;
  function bar(){
    console.log(a);
  }
  return bar;
}
var baz = foo();
baz();            // 这就是闭包
// 函数bar()的词法作用域能够访问foo()内部作用域，然后我们将bar()函数本身当作一个值类型传递
// 在foo()执行后，其返回值（也就是内部的bar()函数）赋值给变量baz，实际上只是通过不同标识符引用了内部的函数bar()
// bar()在其定义的词法作用域以外的地方执行
```
> 在foo()执行后，通常会期待foo()的整个内部作用域都被销毁，因为引擎有垃圾回收器用来释放不再使用的内存空间。由于看上去foo()的内容不会再被使用，所以很自然地会考虑将其回收。  <br>
> 但是闭包就能阻止这种回收，但是事实上内部作用域依然存在，没有被回收。那是谁在使用这个内部作用域呢？  <br>
> 是bar()。因为bar()声明的位置，使得它拥有涵盖foo()内部作用域的闭包，使得该作用域能够一直存活，以供bar()在之后任何时间进行引用。 <br>
###### bar()依然持有对该作用域的引用，这个引用就叫做“闭包”。 <br>


无论使用何种方式对函数类型的值进行传递，当函数在别处被调用的时候都可以观察到闭包：
```javascript
function foo(){
  var a = 2;
  function baz(){
    console.log(a);           // 2
  }
  bar(baz);
}
function bar(fn){
  fn();                // 这就是闭包（把内部函数baz传递给bar，当调用这个内部函数时（也就是fn），它涵盖的foo()内部作用域的闭包就可以观察到了）
}
```
传递函数也可以是间接的：
```javascript
var fn;
function foo(){
  var a = 2;
  function baz(){
    console.log(a);           // 2
  }
  fn = baz;       // 将baz分配给全局变量
}
foo();
bar();                
```
> 无论通过何种手段将内部函数传递到所在的词法作用域以外，它都会持有对原始定义作用域的引用，无论在何处执行这个函数都会使用闭包。 <br>

```javascript
function wait(message){
  setTimeout(function timer(){
    console.log(message);
  },1000);
}
wait('XXX');
```
上面的代码把一个内部函数timer传递给setTimeout()。timer具有涵盖wait()作用域的闭包，因此还保持有对变量message的引用。 <br>
wait()执行1000毫秒后，它的内部作用域并不会消失，timer函数依然保持有wait()作用域的闭包。 <br>
在引擎内部，内置的工具函数setTimeout()持有对一个参数的引用，这个参数可能叫做fn或者func，或者其他类似的名字。引擎会调用这个函数，在例子中就是内部的timer函数，而词法作用域在这个过程中保持完整。 <br>
这就是闭包。 <br>
> 本质上无论何时何地，如果将（访问它们各自词法作用域的）函数当作第一级的值类型并到处传递，就会看到闭包在这些函数中的应用。<br>
在事件监听、定时器、Ajax请求、跨窗口通信、Web Workers或者任何的异步（或者同步）任务中，只要使用了回调函数，实际上就是在使用闭包。 <br>

```javascript
for(var i = 1;i<=5;i++){
  setTimeout(function timer(){
    console.log(i);
  },1000);
}
```
我们尝试假设循环中每个迭代在运行时都会给自己“捕获”一个i副本。但是根据作用域的工作原理，实际上是尽管循环中的五个函数是在各个迭代中分别定义的，但是它们都被封闭在一个共享的全局作用域中，因此实际上只有一个i。 <br>
```javascript
// 如果作用域是空的，那么仅仅将它们进行封闭是不够的。IFEE只是一个什么都没有的空作用域，它需要一点实质内容才能为我们所用
// 它需要有自己的变量，用来在每个迭代中存储i的值
for(var i = 1; i <= 5;i++){
  (function(){
    var j = i;
    setTimeout(function timer(){
      console.log(j);
    },j*1000);
  })();
}

// 上诉代码的改进
for(var i=1;i<=5;i++){
  (function(j){
    setTimeout(function timer(){
      console.log(j);
    },j*1000);
  })(i);
}
```
在迭代内使用IIFE会为每一个迭代都生成一个新的作用域，使得延迟函数的回调可以将新的作用域封闭在每个迭代内部，每个迭代中都会含有一个具有正确值的变量供我们访问。

###### 块作用域+闭包
我们使用IIFE在每次迭代时都创建一个新的词法作用域，也就是说，每次迭代都需要一个块级作用域。 <br>
本质上，这是将一个块转换成一个可以被关闭的作用域。
```javascript
for(var i=1;i<=5;i++){
   let j = i;          // 闭包的作用域
   setTimeout(function timer(){
      console.log(j);
    },j*1000);
}

// 改进
for(let i = 1;i<=5;i++){
  setTimeout(function timer(){
    console.log(i);
  },i*1000);
}
```

##### 模块
```javascript
function foo(){
  var something = 'cool';
  var another = [1,2,3];
  function dosomthing(){
    console.log(something);
  }
  function doAnother(){
    console.log(another.join("!"));
  }
}
```
上面这段代码，没有明显的闭包，只有两个私有数据变量something和another，以及doSomething()和doAnother()两个内部函数，它们的词法作用域（而这就是闭包）也就是foo()的内部作用域。 <br>
请看下面代码：
```javascript
function CoolModule(){
  var something = 'cool';
  var another = [1,2,3];
  function doSomthing(){
    console.log(something);
  }
  function doAnother(){
    console.log(another.join("!"));
  }
  return {
    doSomething:doSomething,
    doAnother:doAnother
  };
}
var foo = CoolModule();
foo.doSomething();   // "cool"
foo.doAnother();     // 1!2!3 
```
这个模式在javascript中被称为模块。最常见的实现模块模式的方法通常被称为模块暴露，这里是其变体。 <br>
首先，CoolModule()只是一个函数，必须通过调用它来创建一个模块实例，如果不执行外部函数，内部作用域和闭包都无法被创建。 <br>
其次，CoolModule()返回一个用对象字面量语法{ key:value,... }来表示的对象，这个返回的对象中含有对内部函数而不是内部数据变量的引用，我们保持内部数据变量是隐藏且私有的状态。可以把这个对象类型的返回值看作本质上是模块的公共API。 <br>
这个对象类型的返回值最终被赋值给外部的变量foo，然后就可以通过它来访问API中的属性方法，比如foo.soSomething()。 <br>
> 从模块中返回一个实际的对象并不是必须的，也可以直接返回一个内部函数。jQuery和$标识符就是jQuery模块的公共API，但它们本身都是函数（由于函数也是对象，所以它们本身也可以拥有属性）。 <br>

doSomething()和doAnother()函数具有涵盖模块实例内部作用域的闭包（通过调用CoolModule()实现）。 <br>
###### 当通过返回一个含有属性引用的对象的方式的对象的方式来将函数传递到词法作用域外部时，我们已经创造了可以观察和实践闭包的条件。 <br>
> 模块模式需要具备两个必要条件：
* 必须有外部的封闭函数，该函数必须至少被调用一次（每次调用都会创建一个新的模块实例）
* 封闭函数必须返回至少一个内部函数，这样内部函数才能在私有作用域中形成闭包，并且可以访问或者修改私有的状态 <br>
 
上一个实例代码中有一个叫作CoolModule()的独立模块创建器，可以被调用任意多次，每次调用都会创建一个新的模块实例。当只需要一个实例的时候，可以像如下代码那样对这个模式改进：
```javascript

```








































