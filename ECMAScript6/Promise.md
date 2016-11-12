### Promise
###### 什么是Promise？
> JavaScript Promise迷你书：Promise是抽象异步处理对象以及对其进行各种操作的组件。  <br>
> MDN：所谓Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。 <br>

Promise是一个对象，与其他的javascript对象的用法没什么不一样；Promise在异步编程中，主要起到代理作用，充当异步操作与回调函数之间的中介。它使得异步操作具备同步操作的接口，使得程序具备正常的同步运行的流程，回调函数不必再一层层嵌套。
Promise的思想，其实就是每一个异步任务立刻返回一个Promise对象，因为是立即返回的，所以可以采用同步操作的流程。
##### JavaScript的异步执行
javascript语言的执行环境是“单线程”的，也就是说javascript一次只能完成一个任务，如果有多个任务，就必须排队，前面一个任务完成了，下一个任务才能开始执行。 <br>
单线程模式实现起来比较简单，执行环境也相对单纯，但是如果有一个任务耗时很长，那么后面的任务就必须排队等候，这样整个程序的执行时间就会被拖长。这经常会造成浏览器无响应（假死），整个页面卡在某个地方，其他任务无法执行。 <br>
如果只有两三个可能的事件，单线程语言编写的面向事件的代码要比多线程代码简单很多，但是如果有很多事件，同时要求数据的状态能够从上一个事件传递到下一个事件，那么就会像下面代码那样，出现【回调地狱】：
```javascript
operation1(function(err, result) {
    operation2(function(err, result) {
        operation3(function(err, result) {
            operation4(function(err, result) {
                operation5(function(err, result) {
                    // do something useful
                })
            })
        })
    })
})
```
javascript语言本身运行起来并不慢，慢的是读写外部数据，例如等待ajax请求返回的数据。为了解决这个问题，javascript语言将任务的执行模式分为：同步（Synchronous）和异步（Asynchronous）。
* 同步模式：传统做法，下一个任务要等上一个任务执行结束，才能执行，程序的执行顺序与任务的排列顺序是一致、同步的。
* 异步模式：把每一个任务分成两段，第一段代码包含对外部数据的请求，第二段代码被写成一个回调函数，包含了对外部数据的处理。第一段代码执行完，并不是立即执行第二段代码，而是将程序的执行权交给第二个任务。等到外部数据返回了

###### 异步模式编程的几种常用方法是：回调函数、事件监听、发布/订阅
###### 回调函数
如果函数f2需要等待f1的执行结果，并且f1是一个耗时很长的任务，那么可以考虑把f2写成f1的回调函数。
```javascript
var f1 = function(callback) {
    // do something here
    ...
    callback.apply(this, para);
}；
var f2 = function(parameter) {
    // do someting in customer callback
}；
// call the fn with callback as parameter
f1(f2)；

// 使用setTimeout()或者setInterval()也可以实现回调函数的异步调用
function f1(callback) {
  setTimeout(function () {
    // f1的任务代码
    // ...
    callback();
  }, 0);
}
// 执行代码
f1(f2);
```
这样就把同步操作变成了异步操作，setTimeout(fn,0)将fn放到下一轮事件循环执行。f1不会堵塞程序运行，相当于先执行程序的主要逻辑，将耗时的操作推迟执行。 <br>
虽然回调函数简单、容易理解和部署，但是各部分耦合度高，程序结构混乱，回调函数嵌套的时候难以追踪流程，并且每一个任务只能指定一个回调函数。

###### 事件监听
事件驱动模式——任务的执行不取决于代码的顺序，而取决于某个事件是否发生。例如：
```javascript
f1.on('done',f2);   // 当f1发生done事件，就执行f2
function f1(){
  setTimeout(function (){
    // f1的任务代码
    f1.trrigger('done');     // 执行完成后，立即触发done事件，从而开始执行f2
  },1000)
}
```
事件监听可以绑定多个事件，每个事件可以指定多个回调函数，很好地解决了“耦合度高”的问题，但是这样子整个程序就会变成事件驱动，运行流程会变得很不清晰。

###### 发布/订阅
发布/订阅模式，又称为观察者模式，如果把事件理解成“信号”，并且存在一个“信号中心”，某个任务执行完成后，就向信号中心publish一个信号，其他任务可以向信号中心订阅这个信号，从而知道什么时候自己可以开始执行。
```javascript
// 首先f2向“信号中心”订阅done信号
jQuery.subscribe('done',f2);
// f1代码块
function f1(){
  setTimeout(function(){
    // f1的任务代码
    jQuery.publish('done');   // f1执行完成后，向‘信号中心’发布done信号，从而引发f2的执行
  },1000);
}
// f2执行完成后，可以对其进行取消订阅操作
jQuery.unsubscribe('done',f2);
```
这种方法我们可以通过查看“消息中心”，了解存在多少信号、每个信号的订阅情况，从而监控程序的运行。


##### 创建promise对象
* new Promsie(fn)返回一个promise对象 <br>
* 在fn中指定异步等处理
    * 处理结果正常就调用resolve（处理结果值）
    * 处理结果错误就调用reject（Erro对象） <br>
    

> Promise对象只有三种状态：
* 异步操作“未完成”（pending）
* 异步操作“已完成”（resolved/fulfilled）
* 异步操作“失败”(rejected)  <br>

> 这三种状态的途径只有两种：
* 异步操作从“未完成”到“已完成”
* 异步操作从“未完成”到“失败”  <br>

> 这种变化只能发生一次，所以一旦当前状态变成“已完成”或“失败”，就不会再有新的状态变化了。 <br>

> 所以Promise对象的最终结果为：
* 异步操作成功，Promise对象传回一个值，状态变为resolved
* 异步操作失败，Promise对象抛出一个错误，状态变为rejected  <br>

所以Promise的基本用法如下：
```javascript
var promise = new Promise(function(resolve,reject){   // Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve方法和reject
  if(/* 异步操作成功 */){     // 如果异步操作成功，则用resolve方法把Promise对象的状态变为“成功”（即从pending变为resolved）
    resolve(value);
  }else{                     // 如果异步操作失败，则用reject方法将状态改为“失败”（即从pending变为rejected）
    reject(error);
  }
});

promise.then(function(value){      // promise实例生成后，可以用then方法和reject方法的回调函数
  // success
},function(value){
  // faile
});
```
###### 下面用Promise来通过异步处理方式来获取XMLHttpRequest(XHR)的数据
首先创建一个用Promise把XHR处理包装起来名为getURL的函数体
```javascript
function getURL(URL){
    return new Promise(function(resolve,reject){
        var req = new XMLHttpRequest();
        req.open('GET',URL,true);
        req.onload = function(){
            if(req.status === 200){         // getURL只有在通过XHR取得结果状态为200时才会调用resolve
                resolve(req.responseText);  // 在response的内容中加入参数（resolve方法的参数没有特别规定，基本上把要传给回调函数参数放进去就可以了，then方法就可以接收到这个参数）
            }else{
                reject(new Error(req.statusText));   // 发生错误时创建一个Error对象后再将具体的值传进去（传给reject的参数也没有特别规定，一般只要是Error对象就可以了）
                // 传给reject的参数，一般包含了reject原因的Error对象，这里是因为状态值不为200所以被reject，所以reject中放入statusText（该参数能被then方法的第二个参数或catch方法中使用）
            }
        };
        req.onerror = function(){    // XHR中onerror事件被触发的时候就是发生错误，调用reject
            reject(new Error(req.statusText));
        };
        req.send();
    });
}
// 运行实例
var URL = 'XXX';          // getURL('XXX'); 能够返回promise对象
getURL(URL).then(function onFulfilled(value){    // getURL函数中的 resolve(req.responseText) 会将promise对象变为resolve状态，同时使用其调用onFulfilled函数
    console.log(vaue);
}).catch(function onRejected(error){     //  等同于 getURL().then(onFulfilled,onRejected)
    console.log(error);
});
```
###### Promise对象实现Ajax操作的例子
```javascript
var getJSON = function(url){
    var promise = new Promise(function(resolve,reject){
        var client = new XMLHttpRequest();
        client.open('GET',url);
        client.onreadystatechange = handler;
        client.responseType = 'json';
        client.setRequestHeader('Accept','application/json');
        client.send();
        
        function handler(){
            if(this.status === 200){
                resolve(this.response);
            }else{
                reject(new Error(this.statusText));
            }
        };
    });
    return promise;
};

getJSON('/json/posts.json').then(function(json){
    console.log('Contents:' + json);
},function(error){
    console.error('出错了',error);
});
```

#### Promise提供的各种方法
![](../views/promises.png)
###### Promise.resolve
静态方法Promise.resolve(value)可以认为是new Promise()方法的快捷方式:
```javascript
new Promise(function(resolve){               
    resolve(42);        // resolve(42)会让这个promise对象立即进入resolved状态，并将42传递给后面then指定的onFulfilled函数
});
// 上述代码的语法糖为：
Promise.resolve(42);   

// Promise.resolve(value);的返回值也是一个promise对象，所以能够像下面的代码那样对其返回值进行.then调用
Promise.resolve(42).then(function(value){
    console.log(value);
});
```

Promise.resolve方法的另一个作用就是将thenable对象转换为promise对象 <br>
thenable对象，是一个非常类似promise的东西，指的是一个具有.then方法的对象。最简单的thenable例子就是jQuery.ajax()，它的返回值就是thenable
```javascript
$.ajax('/json/area.json');   // 因为jQuery.ajax()的返回值是jqXHR Object对象，拥有'.then'方法的对象
```
###### 将thenable对象转换为promise对象
```javascript
var promise = Promise.resolve($.ajax('/json/area.json'));     // promise对象
promise.then(function(value){
    console.log(value);
});
```
> 简单来说，Promise.resolve方法的作用就是将传递给它的参数填充到promise对象后并返回这个promise对象 <br>
Promise很多处理内部也是使用Promise.resolve方法把值转为promise对象后再进行处理。


###### resolve方法的参数除了正常的值以外，还可能是另一个Promise实例
```javascript
// p1/p2都是Promise的实例，p2的resolve方法将p1作为参数，这时p1的状态就会传递给p2.
// 如果调用的时候，p1的状态是pending，那么p2的回调函数就会等待p1的状态改变；
// 如果p1的状态已经是fulfilled或者rejected，那么p2的回调函数立马执行
var p1 = new Promise(function(resolve,reject){
    // ....dosomething
});
var p2 = new Promise(function(resolve,reject){
    // ...dosomething
    resolve(p1);
})
```
###### Promise.reject
Promise.reject(error)是和Promise.resolve(value)类似的静态方法，是 new Promise()方法的快捷方式。
```javascript
new Promise(function(resolve,reject){
    reject(new Error("出错了"));
});
// 语法糖
Promise.reject(new Error('出错了'));

Promise.reject(new Error('BOM!')).catch(function(error){
    console.log(error);
});
```
###### Promise.prototype.then方法：链式操作
因为Promise.prototype.then和 Promise.prototype.catch方法返回 promises对象, 所以它们可以被链式调用—— 一种被称为 composition 的操作。 <br>
```javascript
aPromise.then(function taskA(value){
    // taskA
}).then(function taskB(value){
    // taskB
}).catch(function onRejected(error){
    console.log(error);
});
```
.then有四种写法，各有差异：
```javascript
// then方法接一个回调函数finalHandler
// 写法一：finalHandler回调函数的参数是doSomethingElse函数运行的结果
doSomething().then(function(){
    return doSomenthingElse();
}).then(finalHandler);

// finalHandler回调函数的参数是undefined
doSomething().then(function(){
    doSomenthingElse();
    return;
}).then(finalHandler);

// finalHandler回调函数的参数，是doSomethingElse函数返回的回调函数的运行结果
doSomething().then(doSomenthingElse()).then(finalHandler);

// doSomethingElse会接收到doSomething()返回的结果
doSomething().then(doSomenthingElse).then(finalHandler);
```

###### Promise.prototype.catch方法：捕捉错误
实际上 Promise.prototype.catch 只是 promise.then(undefined, onRejected); 方法的一个别名而已。 也就是说，这个方法用来注册当promise对象状态变为Rejected时的回调函数。


















