### CSS3动画
CSS不是网站开发的关键，但绝对是用户体验的关键。CSS3动画挺炫酷的，能够做出很多复杂的功能——相应地代码也会偏复杂，所以这里只是简单地过一下CSS动画的两大组成部分：transition和animation。   <br>

#### trasition:
transform呈现的是一种变形结果，而transation呈现的是一种过渡，通俗点说就是一种动画转换过程，如渐显、渐弱、动画快慢等。 <br>
在引入trasition、之前，所有属性都是没有时间轴的，也就是说所有状态变化都是即时完成的。 <br>
而transition的作用在于，指定状态变化所需要的时间。 <br>
transition的语法如下
```CSS
  E:{
    transition ： [<'transition-property'> || <'transition-duration'> || <'transition-timing-function'> || <'transition-delay'>];
    /* 
      transition-property:执行变换的属性
      transition-duration:变换延续的时间
      transition-timing-function:变换的速率变化
      transition-delay:变换延迟时间
    */
  } 
```
##### 以下代码效果可点击 [这里](http://codepen.io/minyillee/pen/ENjdqv?editors=1100)
###### 基本用法
transition的作用在于，指定状态变化所需要的时间。
```CSS
img{
    height:15px;
    width:15px;
}

img:hover{
    height: 450px;
    width: 450px;
    transition: 1s;   /* 图片放大的过程需要1秒 */
}
```
还可以指定transition适用的属性，比如只适用于height:
```CSS
img{
    height:15px;
    width:15px;
}

img:hover{
    height: 450px;
    width: 450px;
    transition: 1s height;   /* 只有图片height的变化需要1秒实现，其他变化（主要是width）依然瞬间实现 */
}
```
######  transition-property
transition-property可以设置要以动画方式变换的CSS属性，默认值all表示变换所有的属性，如果只针对单个或者多个CSS属性进行变换，就可以用这个属性来进行单独设置。 <br>
取值有：none(没有属性改变)；all（所有属性改变）这个也是其默认值；indent（元素属性名）。 <br>
当其值为none时，transition马上停止执行，当指定为all时，则元素产生任何属性值变化时都将执行transition效果，ident是可以指定元素的某一个属性值。 <br>
W3C官网中列出了所有可以实现transition效果的CSS属性值以及值的类型，大家可以点 [这里](https://www.w3.org/TR/css3-transitions/#properties-from-css-) 了解详情。

###### transition-duration:
transition-duration用来定义转换动画的时间长度，即从旧属性换到新属性花费的时间，单位为秒。默认情况下动画过渡时间为0秒。
取值有：0（不执行过渡，直接看到结果）、time（指定动画执行的时间）。

###### transition-timing-function
transition-timing-function的值允许你根据时间的推进去改变属性值的变换速率，transition-timing-function有6个可能值： <br>
1、ease：（逐渐变慢）默认值，ease函数等同于贝塞尔曲线(0.25, 0.1, 0.25, 1.0).  <br>
2、linear：（匀速），linear 函数等同于贝塞尔曲线(0.0, 0.0, 1.0, 1.0).   <br>
3、ease-in：(加速)，ease-in 函数等同于贝塞尔曲线(0.42, 0, 1.0, 1.0).   <br>
4、ease-out：（减速），ease-out 函数等同于贝塞尔曲线(0, 0, 0.58, 1.0).  <br>
5、ease-in-out：（加速然后减速），ease-in-out 函数等同于贝塞尔曲线(0.42, 0, 0.58, 1.0)  <br>
6、cubic-bezier：（该值允许你去自定义一个时间曲线）， 特定的cubic-bezier曲线。 (x1, y1, x2, y2)四个值特定于曲线上点P1和点P2。所有值需在[0, 1]区域内，否则无效。  <br>
```css
img{
    height:15px;
    width:15px;
    display: block;
    margin:15px auto;
    transition:1s height cubic-bezier(.83,.97,.05,1.44);    /* 产生一个最后阶段放大过度、然后回缩的效果 */
}
img:hover{
    height: 450px;
    width: 450px;
}
```
部分个属性的示意图如下： ![](./views/transition-timing-function.png)
###### transition-delay
```css
div{
transition-delay: 2s;   /* 在过渡效果开始前等待 2 秒 */
-moz-transition-delay: 2s; /* Firefox 4 */
-webkit-transition-delay: 2s; /* Safari 和 Chrome */
-o-transition-delay: 2s; /* Opera */
}
img{
    height: 450px;
    width: 450px;
    transition: 1s height, 1s 1s width;   /* width在1秒之后，再开始变化，也就是延迟（delay）1秒 */
}
```
###### transition
transition的开始状态和结束状态的具体数值，才能计算出中间状态。 <br>
比如，height从0px变化到100px，transition可以算出中间状态。但是，transition没法算出0px到auto的中间状态，也就是说，如果开始或结束的设置是height: auto，那么就不会产生动画效果。类似的情况还有，display: none到block，background: url(foo.jpg)到url(bar.jpg)等等。


#### animation
CSS Animation需要指定动画一个周期持续的时间，以及动画效果的名称。 <br>
```css
div:hover {
  animation: 1s rainbow;
}
```
当鼠标悬停在div元素上时，会产生名为rainbow的动画效果，持续时间为1秒。为此，我们还需要用keyframes关键字，定义rainbow效果。
```css
@keyframes rainbow {       /* rainbow效果一共有三个状态，分别为起始（0%）、中点（50%）和结束（100%） */
  0% { background: #c00; }
  50% { background: orange; }
  100% { background: yellowgreen; }
}
```
animation的语法是：
```css
E{
  animation: name duration timing-function delay iteration-count direction;
  /* 
    animation-name:规定需要绑定到选择器的 keyframe 名称
    animation-duration:规定完成动画所花费的时间，以秒或毫秒计
    animation-timing-function:规定动画的速度曲线
    animation-delay:规定在动画开始之前的延迟
    animation-iteration-count:规定动画应该播放的次数
    animation-direction:规定是否应该轮流反向播放动画
  */
}

```
  animate.css 是一个来自国外的 CSS3 动画库，它预设了抖动（shake）、闪烁（flash）、弹跳（bounce）、翻转（flip）、旋转（rotateIn/rotateOut）、淡入淡出（fadeIn/fadeOut）等多达 60 多种动画效果，几乎包含了所有常见的动画效果。可点击 [这里](http://www.dowebok.com/demo/2014/98/)
  
###### 

































