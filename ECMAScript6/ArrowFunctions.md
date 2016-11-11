### 箭头函数 Arrow Functions
> ES6的箭头函数的语法，与coffeeScript中的 => 语法一样，共享this上下文。 

###### 箭头函数的产生，主要是为了更简洁的语法和与父作用域共享关键字this

先来看一下六种语言中简单的函数示例：
```javascript
function (a){ return a>0; }    // js
[](int a){ return a>0; }       // C++
(lambda (a) (a>0)) ; ;         // Lisp
lambda a : a>0                 // python
a => a > 0                     // C#
a -> a > 0                     // java
```
#### 新的函数语法
CoffeeScript很简洁，特别是函数语法，不同于传统的javascript函数语法，传统的函数语法没有提供任何的灵活性，每一次需要定义一个函数的时候，都必须输入function(){}。 <br>
当我们有大量的回调函数时，箭头函数就能看到很明显的优势了。
```javascript
// ES5
function getVerifiedToken(selector){
  return getUsers(selector)
      .then(function (users) { return users[0]; })
      .then(verifyUser)
      .then(function(user,verifiedToken) { return verifiedToken; })
      .cache(function (err) { console.log(err.stack); });
}

// ES6
function getVerifiedToken(selector){
  return getUsers(selector)
      .then(users => users[0])
      .then(verifyUser)
      .then((user,verifiedToken) => verfiedToken )
      .cache(err => log(err.stack));
}
```
从上面的重写代码中，我们可以看到箭头函数的一些语法：
* function和{}都消失了，所有的回调函数都只出现在一行代码里
* 当只有一个参数时，（）也消失了（但rest参数时一个例外，如（...args）=> ...）
* 当{}消失后，return关键字也可以跟着消失，单行的箭头函数会提供一个隐式的return（注意：仅仅当箭头函数为单行形式的时候，才会出现隐式的return。当箭头函数伴随着{}被声明，那么即使它是单行的，也不会有隐式return）
* 如果我们的函数内只有一条声明，我们可以不写{}
> 注意，使用了块语句的箭头函数不会自动返回值，你需要使用return语句将所需值返回

当我们需要写一个接受多重参数（也可能没有参数，或是不定参数、默认参数、参数解构）的函数，就需要用小括号包裹参数list
```javascript
// ES5
var total = values.reduce(function (a,b){
  return a + b;
},0);

// ES6
var total = values.reduce((a,b) => a + b,0);
```
> 当使用箭头函数创建普通对象时，需要将对象包裹在小括号里。

```javascript
var foo = pupies.map(puppy => {});        // 报错：空对象{}和空白函数代码块{}一模一样，{}被解析为空白函数代码块
var foo = pupies.map(puppy => ({}));      
```
来看一个更完整的例子：
```javascript
function () { return 1; }
() => { return 1; }
() => 1
 
function (a) { return a * 2; }
(a) => { return a * 2; }
(a) => a * 2
a => a * 2
 
function (a, b) { return a * b; }
(a, b) => { return a * b; }
(a, b) => a * b
 
function () { return arguments[0]; }
(...args) => args[0]
 
() => {} // undefined
() => ({}) // {}
```


##### this
###### 普通function函数和箭头函数的行为的一个微妙的区别是——箭头函数没有它自己的this值，箭头函数内的this值继承自外围作用域。
javascript里的this，无论我们是否需要它，function函数总会自动接收一个this值。
> this的工作原理可以点击 [这里](http://stackoverflow.com/questions/3127429/how-does-the-this-keyword-work)
例如：使用jquery来展示一个每秒都会更新的时钟：
```javascript
$('.current_time').each(function () {         // 当尝试在setInterval的回调中使用this来引用DOM元素时，我们得到的却是一个属于回调函数自身上下文的this
  setInterval(function(){
    $(this).text(Date.now());
  },1000);
});
```

因为内层函数并没有从外层函数继承this的值，在内层函数里，this会是window或undefined。临时变量self用来将外部的this值导入内部函数（还有另一种方法是在内部函数上执行.bind(this)）：
```javascript
$('.current_time').each(function(){
  var self = this;
  setInterval(function(){
   $(self).text(Date.now()); 
  },1000);
});
```

但是当我们使用箭头函数的时候，这个问题就不存在了，因为箭头函数会产生一个不属于它自己的上下文this。但要遵循以下规则：
* 通过object.method()语法调用的方法用非箭头函数定义，这些函数需要从调用者的作用域中获取一个有意义的this值
* 其它情况全都用箭头函数
上面的js可以用箭头函数简写为：
```javascript
$('.current_time').each(function(){
  setInterval(() => $(this).text(Date.now()),1000);
});
```
再来看一个例子：
```javascript
// ES5
{
  ...
  addAll:function addAll(pieces){
     var self = this;
     _each(pieces,function(piece){
      self.add(piece);     // 内层函数并没从外层函数继承this的值
     });
  },
  ...
}

// ES6
{
  ...
  addAll:function addAll(pieces){
    _each(pieces,piece => this.add(piece));
  },
  ...
}

// ES6 使用对象字面量方法：
{
  ...
  addAll(pieces){
    _each(pieces,piece => this.add(piece));
  },
  ...
}
```

###### 箭头函数与普通函数的另一个区别是——它没有自己的arguments变量
```javascript
function log(msg){
  const print = () => console.log(arguments[0]);
  print('LOG:${msg}');
}
log('hello');     // 'hello'
```
###### 箭头函数没有属于自己的this和arguments，但是我们可以通过rest参数来得到所有存入的参数数组。
```javascript
function log(msg){
  const print = (...args) => console.log(args[0]);
  print('LOG:$(msg)');
}
log('hello');         // LOG:hello
```

=> 替代function是非常便捷的，但是不是什么情况（例如只使用=>来声明函数的代码）都适合用=>，推荐存在以下两种情况时候使用箭头函数：
* 当只有一条声明（statement）语句时，隐式return
* 需要使用到父作用域中的this


原文 [An Introduction to JavaScript ES6 Arrow Functions](https://strongloop.com/strongblog/an-introduction-to-javascript-es6-arrow-functions/)


























