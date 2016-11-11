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


































