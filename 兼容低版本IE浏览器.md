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

#### 4、RGBA颜色与兼容性的半透明背景色
RGBA颜色就是红+绿+蓝+Alpha透明的颜色，为CCS3新属性，与RGB的区别在于多了个A，也就是Alpha透明度，这一特性能够帮我们实现背景、边框、文字等透明效果。 <br>
一般的使用方法为：
```CSS
background:rgba(200, 54, 54, 0.5);
```
###### IE下背景半透明的方案
IE渐变滤镜：最简单的使用为： <br>
```CSS
filter:progid:DXImageTransform.Microsoft.Gradient(startColorStr=#AARRGGBB,endColorStr=i#AARRGGBB)
```
 其中有两个参数：startColorStr和EndColorStr。  <br>
 startColorStr为可选项，值为字符串(String)，意为检索色彩渐变的开始颜色和透明度。 <br>
　　其格式为 #AARRGGBB 。 AA 、 RR 、 GG 、 BB 为十六进制正整数。取值范围为 00 – FF 。 RR 指定红色值， GG 指定绿色值， BB 指定蓝色值， [参阅 #RRGGBB 颜色单位](http://www.w3school.com.cn/cssref/css_colors.asp) 。 AA 指定透明度。 00 是完全透明。 FF 是完全不透明。超出取值范围的值将被恢复为默认值。 <br>
　　取值范围为 #FF000000 – #FFFFFFFF 。默认值为 #FF0000FF 。不透明蓝色。 <br>
EndColorStr为可选项，值为字符串(String)意为检索色彩渐变的结束颜色和透明度。参阅 startColorStr 属性。默认值为 #FF000000 。不透明黑色。
###### rgba+filter实现兼容性的半透明背景效果
例如让所有主流浏览器下元素可以实现50%透明的黑色背景可如下代码所示: <br>
```CSS
background:rgba(0,0,0,0.5);
filter: progid:DXImageTransform.Microsoft.gradient(startcolorstr=#7F000000,endcolorstr=#7F000000);
```

#### 5、IE7 z-index的浏览器兼容性问题
z-index属性主要为了控制定位元素的层叠顺序，z-index属性值越大，表示其优先级越高（优先级越高的会遮盖住其他元素）。<br>
首先z-index只对定位元素有效，定位元素是指position的属性值为absolute、relative、fixed（inherit取决于父元素，如果父元素没有设置定位则z-index无效）。 <br>

