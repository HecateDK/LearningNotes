### Q.js【译文】

* [Getting Start](#getting-start)
* [Resources](#resources)
* [Tutorial](#tutorial)
  * [Propagation](#propagation)
  * Chaining
  * 组合
  * 序列
  * 错误处理
  * 进度通知
  * 结束
  * 开始
  * 中间
  * Over the Wire
  * 适配Node
  * 跟踪长堆栈
*  测试
* 关于作者



> 这是从Git分支而来的Q版本`1.0`。本文档适用于最新的版本1.0和版本0.9发行版，这些版本都是稳定的，在0.9.7版本之后就没有0.9版本的进一步发行了，所以这相当于就是1.0版本。
> `q@~1.0`的所有进一步版本都将被兼容到。2.0发行版变化显著，并且其向后不兼容的更改，此刻正在实验着。 <br>

如果一个`function`没有返回一个值，或者没有抛出一个异常阻塞，它就可以返回一个promise。Promise是一个对象，表示返回值或者是函数最终提供的的抛出的异常。
Promise也可以作为一个[远程对象](https://github.com/kriskowal/q-connection)的代理来克服延迟问题。  <br>

在第一次实现的时候，Promise是为了拯救[“Pyramid of Doom”](http://calculist.org/blog/2011/12/14/why-coroutines-wont-work-on-the-web/)： the situation where code marches to the right faster
than it marches forward.  <br>
```javascript
step1( function( value1 ){
  step2( function( value2 ){
    step3( function( value3 ){
      step4( function( value4 ){ 
        // Do something with value4
    })
  })
})
```
有了promise库，你就可以压平金字塔了。
```javascript
Q.fcall( promisedStep1 )
.then( promisedStep2 )
.then( promisedStep3 )
.then( promisedStep4 )
.then( function( value4 ){
  // Do something with value4
})
.catch( function( error ){
  // Handle any error from all above steps
})
.done();
```
运用promise，你也能得到通过隐式传播的错误，就像`try`,`catch`,`finally`。在`promisedStep1`产生的错误就会一路传向`catch`函数，在`catch`里面就可以捕获到这个错误，并且可以对它做出相应的处理。
（这里的'promisedStpeN'只是`stepN`的另一个形式，它会返回一个promise。） <br>
回调方法称为“控制反转”，一个函数接收一个回调而不是返回一个值，此函数的意思是：“别找我，我会找你的了”。 <br>

Promise不会[反转](http://www.slideshare.net/domenicdenicola/callbacks-promises-and-coroutines-oh-my-the-evolution-of-asynchronicity-in-javascript)倒转，
清晰地分离从控制流参数组输入的参数。
这简化了用法和创建了API，侧重体现在variadic、rest和spread arguments。  

#### Getting Start
Q可以用以下方式加载：
* `<script>`标签（创建一个Q的全局变量）：简化和压缩后 ~2.5KB 
* Node.js或CommonJS模块，可用[npm](https://www.npmjs.com/)里[q](https://www.npmjs.com/package/q)的包
* AMD模板
* 使用[component](https://github.com/componentjs/component)如同`microjs/q`
* 使用[bower](https://bower.io/)如同`q#1.0.1`
* 使用[NuGet](http://www.nuget.org/)如同[Q](https://www.nuget.org/packages/q)

Q能够和 jQuery, Dojo, When.js, WinJS,等等混合使用。



#### Resources
[wiki](https://github.com/kriskowal/q/wiki)包含了很多有用的资源，包括：
* 渐进式的[Q API reference](https://github.com/kriskowal/q/wiki/API-Reference)
* 正在完善的[examples gallery](https://github.com/kriskowal/q/wiki/Examples-Gallery)，给我们展示为什么Q能够让所有事情更美好。从XHR到数据库，access to accessing the Flickr API，Q为你而生
* 有很多库再开发和采用Q promise，从文件系统/数据库访问、RPC到模板。一些很受欢迎的列表，可以点击这里查看[Libraries](https://github.com/kriskowal/q/wiki/Libraries)
* 如果你想要介绍promise概念的一些资料，那么以下的教程不大适合你，查看我们的收藏夹[presentations, blog posts, and podcasts](https://github.com/kriskowal/q/wiki/General-Promise-Resources)
* [Coming from jQuery`$.Deferred`](https://github.com/kriskowal/q/wiki/Coming-from-jQuery)的指南
我们非常欢迎你加入我们的[网上论坛](https://groups.google.com/forum/#!forum/q-continuum)


#### Tutorial
Promise拥有`then`方法：可以用来获取程序返回的值——fulfillment，或者是捕获程序抛出的异常——rejection。   
```javascript
promiseMeSomething()
.then(function (value) {
}, function (reason) {
});
```
如果`promiseMeSomething`函数返回一个promise对象是程序的返回值，那么第一个函数（the fulfillment handler）就会访问这个值。但是如果`promiseMeSomething`函数得到的是一个程序抛出的异常，那么第二个函数（the rejection handler）就会对这个异常进行相应处理。 <br>

需要注意的是：promise的解析是针对异步机制的。也就是说，成功或者失败处理程序将会在下一个事件循环中被调用（即Node中的`process.nextTick`）。这给我们在跟踪代码流的时候一个很好地保障。换句话说，`then`将总是在任何处理程序执行之前返回。   <br>
在本教程中，我们将从如何使用、运用promise进行开发开始。我们将会讲解如何去创建它们，创建一个类似'promiseMeSomething'会返回一个promise对象的函数。

##### Propagation
`then`方法返回一个promise对象，在下面的这个例子中，我们设为'outputPromise' 
```javascript
var outputPromise = getInputPromise()
.then(function (input) {
}, function (reason) {
});
```
`outputPromise`变量将在程序结束前作为一个promise对象，返回一个值。此时函数只能返回一个值或者抛出一个异常。只有一个处理程序将会被调用，它将负责处理'outputPromise'。   
* 如果处理程序返回的是一个值，'outputPromise'的状态就变成“成功(fulfilled)”
* 如果程序抛出一个异常，'outputPromise'的状态就变成“失败(rejected)”
* 如果在处理程序中返回了一个promise对象，'outputPromise'就会"变成"那个promise。能够成为一个新的promise对于管理延迟、组合结果，或者是恢复错误都是很有帮助的。 <br>

如果`getInputPromise()`promise结果失败，并且你省略了对于rejection的处理程序，那么error就会返回到`outputPromise`：
```javascript
var outputPromise = getInputPromise()
.then(function (value) {
});
```
 
