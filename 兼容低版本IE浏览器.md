###兼容低版本IE浏览器

>收录一些做项目过程中兼容低版本ie浏览器的解决方法，避免下次调兼容性调一整天。。。。 <br>
>虽然现在浏览器支持性都好很多了，而且很少要兼容IE6、IE7，不过其实调兼容性还挺好玩的。


#### 1、inline-block兼容性问题
IE6、IE7不识别inline-block但可以触发块元素。 <br>
其它主流浏览器均支持inline-block。  <br>
解决IE6、IE7兼容性的方法：  <br>
1、首先设置inline-block触发块元素，具有了layout的特性，然后设置display:inline使块元素呈现内联元素，此时layout的特性不会消失。  <br>
2、直接设置display:inline，使用zoom:1触发layout。  <br>
兼容所有浏览器的方法是：  <br>
```CSS
display:inline-block;
*display:inline;
*zoom:1;
```



#### 2、float right 不会像其他浏览器那样在当前行居右显示，而是另起一行再居右显示
例如：
```HTML
<label>left</label><label style="float: right;">right</label> 
```
期待的结果是: <br>
------- |left|          |right| ------ <br>
IE7中显示的结果是： <br>
-------         ------ |left| ------                   <br>              
|right|                 -------  <br>

解决方法是把所有的float right的元素放在没有float的元素之前，即:
```HTML
<label style="float: right;">right</label><label>left</label>
```

#### 3、ie6,ie7下设置overflow:auto下滚动条不起作用
遇到一个比较特殊的情况：ie6,ie7下设置overflow:auto下滚动条出来了但是滚动条不起任何作用，但在火狐，ie8，ie9，谷歌等浏览器下正常显示。通过查询终于找到原因，只需要加一个position:relative;属性就能解决问题！
```javascript
$(document.body).css({'overflow':'hidden'});  
====$('body').css('overflow','hidden')
```
一般的浏览器(ex. chrome and firefox)会初始付值给
```CSS
html{overflow:visible;}
```
但 <br>
IE6 初始付值html
```CSS
 {overflow-x:auto;overflow-y:scroll;}
```
IE7 初始付值
```CSS
html{overflow-x:visible;overflow-y:scroll;}
```
设置了body{overflow:hidden}，还会出现滚动条，不过这个滚动条不是body的，是html的
只有你设置html{overflow:visible;} body{overflow的值}才能传递到html{}中去
这样html的值就变成了{overflow:hidden}
所以你css里要加一句：
```CSS
html {
    overflow: visible;
}
```
