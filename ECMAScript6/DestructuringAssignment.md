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
a          //'a'
```
本质上，这种写法属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值，但事实上，用变量来描述并不恰当，因为我们可以对任意深度嵌套的数组进行结构，例如：
```javascrpt
let [a,[[b],c]] = [1,[[2],3]];
a   //1
b   //2
c   //3

let [,,d] = ['1','2','3'];      // 在对应位留空来跳过被解构数组中的某些元素
d   //3

let [x,,y] = [1,2,3];
x   //1
y   //3

let [head,...tail] = [1,2,3,4];   //通过“不定参数”模式捕获数组中的所有尾随元素
head   //1
tail   //[2,3,4]

let [x,y,...z] = ['a'];      //当访问空数组或越界访问数组时，对其解构与对其索引的行为一致，最终得到的结果都是undefined
x     //'a'
y     //undefined
z     //[]
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































