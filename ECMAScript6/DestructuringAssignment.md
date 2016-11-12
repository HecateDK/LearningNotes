### 变量的解构赋值

什么是解构赋值？
对象字面量和数组字面量提供了一种简单的定义一个特性的数组的方法，一旦我们创建了这类数据组，我们就可以用任意的方法使用这些数据组。
###### 解构赋值允许我们使用类似数组或对象字面量的语法将数组和对象的属性赋值给各种变量。
这种赋值语法极度简洁，同时比传统的属性访问方法更为清晰。
以前我们可能通过以下方法访问数组中的前三个元素：
```javascript
var a = sArray[0],
    b = sArray[1],
    c = sArray[2];
```
但是如果使用结构赋值的特性，将会使得代码更加简洁和可读性更强：
```javascrpt
var [a,b,c] = sArray;
```

##### 数组的结构赋值
###### 基本用法
```javascript
var [a, b] = [1, 2];
console.log(a); // 1
console.log(b); // 2

var [a, b, ...rest] = [1, 2, 3, 4, 5]
console.log(a); // 1
console.log(b); // 2
console.log(rest); //[3,4,5]

var {a, b} = {a:1, b:2};
console.log(a); // 1
console.log(b); // 2
```
从上面的代码，可以看到我们可以直接从数组中提取值，按照对应位置，对变量赋值（如果你想在赋值的同时声明变量，可在赋值语句前加入var、let或const关键字）。 <br>
对于set结构，也可以使用数组的解构赋值：
```javascript
[a,b,c] = new Set(['a','b','c'])
console.log(a);          //'a'
```
本质上，这种写法属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值，但事实上，用变量来描述并不恰当，因为我们可以对任意深度嵌套的数组进行结构，例如：
```javascript
let [a,[[b],c]] = [1,[[2],3]];
console.log(a);   //1
console.log(b);   //2
console.log(c);   //3

let [,,d] = ['1','2','3'];      // 在对应位留空来跳过被解构数组中的某些元素
console.log(d);   //3

let [x,,y] = [1,2,3];
console.log(x);   //1
console.log(y);   //3

let [head,...tail] = [1,2,3,4];   //通过“不定参数”模式捕获数组中的所有尾随元素
console.log(head);   //1
console.log(tail);   //[2,3,4]

let [x,y,...z] = ['a'];      //当访问空数组或越界访问数组时，对其解构与对其索引的行为一致，最终得到的结果都是undefined
console.log(x);     //'a'
console.log(y);     //undefined
console.log(z);     //[]
```
如果解构不成功，变量的值就会等于undefined。
```javascript
//以下解构不成功，foo的值等于undefined
var [foo] = [];
var [foo] = 1;
var [foo] = 'Hello';
var [foo] = false;
var [foo] = NaN;
var [bar,foo] = [1];
var [foo] = {};
```
另外，存在不完全解构情况：
```javascript
var [x,y] = [1,2,3];          //x和y都可以顺利取到
```
###### 解构赋值的规则是：只要等号右边的值不是对象，就会先将其转为对象。
> 但是如果对undefined或null进行结构，就会报错。这是因为解构只能用于数组或对象，其他原始类型的值都可以转为相应的对象，但是'undefined'和'null'不能转为对象，因此会报错。

```javascript
// 报错
var [foo] = undefined;
var [foo] = null;
```
事实上，只要某种数据结构具有Iterator接口（迭代器对象——为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署Iterator接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）），都可以采用数组形式的解构进行赋值。
```javascript
function* fibs() {
      var a = 0;
      var b = 1;
      while (true) {
        yield a;
        [a, b] = [b, a + b];
      }
    }
    var [first, second, third, fourth, fifth, sixth] = fibs();
    console.log(sixth);        // 5
```



##### 对象的结构赋值
通过解构对象，我们可以把它的属性与不同的变量绑定，首先指定被绑定的属性，然后紧跟一个要解构的变量。
```javascript
var robotA = { name:'aaaa' },
    robotB = { name:'bbbb' },
    { name:nameA } = robotS,
    { name:nameB } = robotB;
console.log(nameA);        // 'aaaa'
console.log(nameB);        // 'bbbb'
```
>对象的解构与数组有一个很不同的地方在于：数组元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性名同名，才能取到正确的值。

```javascript
var { bar,foo } = { foo:'aaaa',bar:'bbbb' };   // 等号两边的两个变量的次序，与等号右边两个同名属性的次序不一样，这对取值是没有影响的
console.log(foo);    // 'aaaa'
console.log(bar);    // 'bbbb'
var { baz } = { foo:'aaaa',bar:'bbbb' };     // 变量没有对应的同名属性，导致取不到值，最后值为undefined
console.log(baz);    //undefined
```
如果变量名与属性名不一致，必须写成下面这样：
```javascript
var { foo:baz } = { foo:'zzz',bar:'xxx' };
console.log(baz);   // 'zzz'

let obj = { first:'aaa',last:'bbb' };
let { first:f,last:l } = obj;
console.log(f);     // 'aaa'
console.log(l);    //'bbb'
```
所以，对象的解构可以写成以下简写形式：
```javascript
var { foo: foo, bar: bar } = { foo: "aaa", bar: "bbb" };
```
###### 也就是说，对象的解构赋值的内部机制，是先找同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。
```javascript
var { foo:baz } = { foo:'aaaa',bar:'bbbb' };
console.log(baz);          // 'aaaa'
console.log(foo);          // error:foo is not defined
```
对对象进行解构赋值时，变量的声明和赋值是一体的，对于let和const来说，变量不能重新声明，所以一旦赋值的变量声明过，就会报错。 <br>
但是如果没有第二个let命令行，这种情况下就不会报错：
```javascript
let foo;
let {foo} = {foo:1};   // SyntaxError: Duplicate declaration "foo"
let baz;
let {bar:baz} = {bar:1};    // SyntaxError: Duplicate declaration "baz"

let foo;
({foo} = {foo:1});    //success
let baz;
({bar:baz} = {bar:1});   //success
```
当我们解构对象并赋值变量时，如果已经声明或不打算声明这些变量（也就是说赋值语句前没有let、const或var关键字），这时候就会遇到如下的语法错误：
```javascript
{foo} = {foo:10};             // Syntax erro
```
因为解析器会将起首的大括号，理解成一个代码块，而不是赋值语句。解决方案是将整个表达式用一对小括号包裹：
```javascript
let baz;
({bar:baz} = {bar:1});   //success
```
###### 和数组一样，解构也可以嵌套结构的对象。
```javascript
var obj = {
    p:[                // p是模式，不是变量，不会被赋值
        'Hello',
        {y:'World'}
    ]
};
var {p:[x,{y}]} = obj;
console.log(x);         // 'Hello'
console.log(y);         // 'World'

var node = {
    loc:{         // 只有line是变量，start和loc都是模式，不会被赋值
        start:{
            line:1,
            column:5
        }
    }
};
var { loc:{ start:{ line } } } = node;
console.log(start);        // error:start is undefined  
console.log(loc);          // error:loc is undefined
console.log(line);         // 1
```
请看一个嵌套赋值的例子：
```javascript
let obj = {};
let arr = [];
( { foo:obj.prop,bar:arr[0] } ={ foo:123,bar:true } );
console.log(obj);         // '{prop:123}'
console.log(arr);         // '[true]'
```
> 同样的，如果解构失败，变量的值就等于undefined； <br>
> 如果解构模式是嵌套的对象，但是对象所在的父属性不存在，就会报错。

```javascript
var {foo} = {bar:'baz'};
console.log(foo);       // undefined

var {foo:{bar}} = {baz:'baz'};         // 等号左边对象的foo属性，对应一个子对象，但是该子对象的bar属性，解构时就会报错（因为此时foo为undefined，再取子属性就会报错）

var _tmp = {baz:'baz'};
console.log(_tmp.foo.bar);             // 报错
```

##### 字符串的结构赋值
字符串解构赋值时，字符串是被转换成一个类似数组的对象。同时，也可以对字符串的length属性，进行解构赋值。
```javascript
const [a,b,c,d,e] = 'Hello';
console.log(a);    // 'H'
console.log(c);    // 'l'
console.log(e);    // 'o'

let {length:len} = 'hello';
console.log(len);    // 5
```

##### 解构的实际应用
###### 函数参数定义
设计API的时候，通常的做法是为函数设计一个对象作为参数，然后把不同的实际参数作为对象属性，以避免让API使用者记住多个参数的使用顺序。 <br>
现在我们就可以利用解构避免这种问题了，当我们想要引用其中一个属性时，就不必反复使用这种单一参数对象。
```javascript
// Firefox开发工具javascript调试器的代码片段
function removeBreakpoint({ url,line,colum }){
    //  ....
}
```

###### 配置对象参数
当我们构造一个提供配置的对象，并且需要这个对象的属性携带默认值时，就需要用到解构了。 <br>
例如：jquery的ajax函数使用一个配置对象作为它的第二参数，就可以像下面一样重写函数定义：
```javascript
jQuery.ajax = function(url,{
    async = true,
    beforeSend = noop,
    cache = true,
    complete = noop,
    crossBomain = false,
    global = true,
    //   .....更多配置
}){
    //  ...dosomething
};
```
如此一来，我们就可以避免配置对象的每个属性都要重复 var foo = config.foo || theDefaultFoo:

###### 与ES6迭代器协议协同使用
ES6中定义了一个迭代器协议，当我们迭代Maps（ES6标准库中新加入的一种对象）后，我们就会得到一系列如[key,value]的键值对，所以我们就可以对这些键值对进行解构，就能更轻松地访问键和值。
```javascript
var map = new Map();
map.set(window,'the global');
map.set(document,'the document');
for(var [key,value] of map){
    console.log(key + 'is' + value);
}
// '[object Window] is the global'
// '[object HTMLDocument] is the document'

// 只遍历键
for(var [key] of map)[
    // ...
}
// 只遍历值
for(var [,value] of map){
    //....  
}
```
###### 多重返回值
利用解构可以返回一个数组并将结果解构：
```javascript
function returnMultipleValues() {
    return [1,2];
}
var [foo,bar] = returnMultipleValues();

//也可以用一个对象作为容器并为返回值命名
function returnMultipleValues(){
    return {
        foo:1,
        bar:2
    };
}
var tmp = returnMultipleValues(),
    foo = tmp.foo,
    bar = tmp.bar;
    
//或者使用CPS变换
function returnMultipleValues(k){
    K(1,2);
}
returnMultipleValues ((foo,bar) => ...);
```

###### 使用解构导入部分ComonJS模块
当我们导入CommonJS模块X时，很可能在模块X中导入了很多我们根本就不打算用的函数，通过解构，可以显式地定义模块的一部分来拆分使用，同时还不污染命名空间：

```javascript
const { SOurceMapConsumer,SourceNode } = require('source-map');
```

参考 Firefox开发者工具工程师Nick Fitzgerald最初在他的博客上发布了一篇文章 [ES6的解构赋值](http://fitzgeraldnick.com/2013/08/02/testing-source-maps.html)

















