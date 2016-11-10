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






























