### CSS3小记

#### 1、CSS3简介
HMLT5、CSS3、ES6，已经趋向成熟，各大浏览器也对它们的语法也越来越兼容了，无疑HTML5能做出很多酷炫的效果，但CSS3带给我们的才是真正充满想象的视觉效果：旋转、缩放、二维和三维动画、动态和装饰性的文字效果、阴影、圆角和渐变填充，毫无疑问我对简简单单几行css代码就能做出以前需要javascript或gif图才能实现的效果很喜欢，难得有时间，整理一下以前用过的CSS3新属性、并系统地学习巩固一下CSS3的知识点。 <br>
CSS3已经分成了很多个模块而不是一份单独、完整的CSS3规范文档，主要有一下几个板块： <br>
* 媒体查询
* 选择器
* 字体文本效果
* 背景/边框
* 2D/3D转换
* 动画
* 布局
* 渐变/透明
使用CSS3之前，页面需要添加一行代码: <br>
```HTML
<name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">   
<!-- 
     width = device-width：宽度等于当前设备的宽度
     initial-scale： 初始的缩放比例（默认设置为1.0）
     minimum-scale：允许用户缩放到的最小比例（默认设置为1.0）
     maximum-scale：允许用户缩放到的最大比例（默认设置为1.0）
     user-scalable：用户是否可以手动缩放（默认设置为no，因为我们不希望用户放大缩小页面）
-->
```
并且对于不支持HTML5/CSS3 media的浏览器，可以借助使用Google的html5shiv包
```HTML
<!--[if lt IE9]> 
<script src="http://html5shiv.googlecode.com/svn/trunk/html5.js"></script>
<![endif]-->
```
#### 2、媒体查询
以前的一些网站打算为提供方便移动访问的版本， 很多时候会建立一个子域，使用不同于父站点的样式表和HTML模板，图片也不得不重新调整大小以适应移动设备的的小屏幕，另外还需要重新创建脚本，检测用户是否使用了移动浏览器，然后重定向到相应的移动站点上。毫无疑问这是需要大量的开发成本和维护成本的。<br>
媒体查询解决了这一问题，它们会基于设备的属性检测设备，这样就不用使用浏览器探测脚本，之后允许直接安装设备的功能去设定目标样式。 <br>
###### CCS2中的media
具体用法，就是在HTML页面的heaad标签中插入如下的一段代码,例如: <br>
```HTML
<link rel="stylesheet" type="text/css" media="screen and (max-width:960px)" href="style.css">  <!-- 让页面宽度小于960的执行指定的样式文件 -->
```
但这样的做法就会增加页面http的请求次数，增加了页面负担，所以CCS3样式都写在一个文件里面。
###### CSS3的 media
我们常用的CSS3参数是大小、等于、小于，但其实CSS3提供了很多参数： <br>
>width:浏览器可视宽度。  <br>
        height:浏览器可视高度。   <br>
        device-width:设备屏幕的宽度。   <br>
        device-height:设备屏幕的高度。   <br>
        orientation:检测设备目前处于横向还是纵向状态。   <br>
        aspect-ratio:检测浏览器可视宽度和高度的比例。(例如：aspect-ratio:16/9)   <br>
        device-aspect-ratio:检测设备的宽度和高度的比例。   <br>
        color:检测颜色的位数。（例如：min-color:32就会检测设备是否拥有32位颜色）   <br>
        color-index:检查设备颜色索引表中的颜色，他的值不能是负数。   <br>
        monochrome:检测单色楨缓冲区域中的每个像素的位数。（这个太高级，估计咱很少会用的到）   <br>
        resolution:检测屏幕或打印机的分辨率。(例如：min-resolution:300dpi或min-resolution:118dpcm)。   <br>
        grid:检测输出的设备是网格的还是位图设备。   <br>

例如： <br>
1）方向  <br>
orientation：有两个值可选，landscape（浏览器宽度大于高度时候被应用，浏览器默认值）、portrait（浏览器高度大于宽度时被使用）:
```CSS
/* 默认情况下，li元素横跨页面叠放起来，当orienttation:portrait（页面重新调整让其高度大于其宽度、在其具有纵向模式的设备中查看页面）浮动就被去掉，li元素垂直堆叠起来 */
ul { overfolw:hidden; }
li { float:left; }
@media only screen and (orienttation:portrait){
  li { float:none; }
}
```
2）像素比  <br>
在小屏幕上访问网站，通常需要放大显示，导致屏幕像素大于CSS像素，这对于位图格式图片，放大时会导致严重失真，现在很多手机使用更高像素密度比的屏幕，能够以高分辨率显示而没有任何失真。 <br>
媒体特征可以根据设备的像素密度去判断设备——device-pixel-ratio（在mobile webkit中带有前缀-webkit-） <br>
``CSS
/* 屏幕在标准（或是低分辨率）像素比设备的浏览器上将使用标准图片，像素比至少在1.5的设备则使用高分辨率的图片 */
E { background-image:url('lores.png'); }
@media screen and (-webkit-min-device-pixel-ratio:1.5){ 
  background-image:url('hires.png');
  background-size:100% 100%;
}
``
3） 多种媒体特性 <br>
and操作符添加样式，可以把多条查询链接在同一个媒体类型上，这种语法在应用选定规则之前，会先检测两个表达式是否匹配： <br>
```CSS
/* 16:9宽屏格式的小屏幕 */
@media only screen and (aspect-ratio:15/10) and (aspect-ratio:16/9) and (aspect-ratio:16/10){ rules } 
```


#### 3、选择器
选择器大体可以划分为两种，第一种是直接作用于文档树中定义的元素，包含类选择器、类型选择器、属性选择器；第二种是包含伪选择器，作用于文档树之外的元素或信息（比如说父元素的最后一个元素、段落中的第一个字母）。 <br>
##### 1)CSS3新属性选择器：
###### 开始子串属性值选择器  E[attr^ = 'value'] {}
```CSS
a[href^ = 'mailto']{ background-image:url('email_img.png'); }
a[href^ = 'ftp']{ background-image:url('folder_img.png'); }
a[href^ = 'https']{ background-image:url('lock_img.png'); }
``` 
###### 结束子串属性值选择器——选择以指定的值结束的属性   E[attr$ = 'value'] {}
```CSS
a[title$ = '.pdf']{ background-image:url('pdf_img.png'); }
a[title$ = '.doc']{ background-image:url('doc_img.png'); }
a[title$ = '.rss']{ background-image:url('feed_img.png'); }
``` 
###### 任意子串属性值选择器——在指定的属性字符串内部任意位置搜索指定的子串         E[attr* = 'value'] {}
```CSS
a[href* = '.pdf']{ background-image:url('pdf.png'); }
```
###### 多属性选择器——可以把多个选择器串联在一起
```CSS
/* 寻找这样一个a元素：具有一个href属性，是以http://开始，以.pdf结束，并且在中间包含了/floder2/ */
a[href^ = 'http://'][href* = '/folder2/'][href$ = '.pdf']{}
```
##### 2）普通兄弟连结符
```CSS
E + F {}  /* 相连兄弟连结符——文档树中的同一层级中，紧邻在元素E之后的任意元素F */
E ~ F {}  /* 普通兄弟连结符——文档树中的同一层级中，位于元素E之后的任意元素F */
```

##### 3）伪类
###### 结构伪类
```CSS
E:nth-*(even) {}        /* 相当于E:nth-*(2n){} 代表2的每一个倍数（2/4/6/8、、、、） */
E:nth-*(odd) {}         /* 相当于E:nth-*(2n+1){} 代表2的每一个倍数（1/3/5/7、、、、） */
E:nth-child(n) {}       /* 相当于 E:nth-of-type(n){} 简单地选择了属于类型E的所有子元素  */      
E:nth-child(2n) {}      /* 选择了类型E中所有处于偶数位的元素，计数的时候包括各种类型的兄弟元素 */
E:nth-of-type(2n) {}    /* 选择了E类型中所有处于偶数位的元素，但是在计数的时候只包含那些同类型的元素 */
E:nth-last-child(n){}   /* 选择器匹配属于其元素的第 N 个子元素的每个元素，不论元素的类型，从最后一个子元素开始计数 */
E:nth-last-of-type(n){} /* 选择器匹配属于父元素的特定类型的第 N 个子元素的每个元素，从最后一个子元素开始计数 */
E:first-of-type{}       /* 选择器匹配属于其父元素的特定类型的首个子元素的每个元素   p:first-of-type等同于p:nth-of-type(1)*/
E:last-child{}          /* 选择器匹配属于其父元素的最后一个子元素的每个元素  p:last-child等同于p:nth-last-child(1) */
E:last-of-type{}        /* 选择器匹配属于其父元素的特定类型的最后一个子元素的每个元素   p:last-of-type等同于p:nth-last-of-type(1) */
E:only-child{}          /* 选择器匹配属于其父元素的唯一子元素的每个元素 */
E:only-of-type{}        /* 选择器匹配属于其父元素的特定类型的唯一子元素的每个元素 */
```

##### 4)其他伪类
###### target伪类
target伪类是用来匹配文档(页面)的URI中某个标志符的目标元素。具体来说，URI中的标志符通常会包含一个”#”字符，然后后面带有一个标志符名称，比如#respond，target就是用来匹配ID为respond的元素的。 <br>
target伪类的出现让我们可以用纯CSS实现tab切换效果，例如：  <br>
```HTML
<div class="tablist">
    <ul class="tabmenu">
        <li><a href="#tab1">tab1</a></li>
        <li><a href="#tab2">tab2</a></li>
        <li><a href="#tab3">tab3</a></li>
    </ul>
    <div id="tab1" class="tab_content">tab1</div>
    <div id="tab2" class="tab_content">tab2tab2</div>
    <div id="tab3" class="tab_content">tab3tab3tab3</div>
</div>
```
```CSS
#tab1:target, #tab2:target, #tab3:target {     /* 先根据target的特性锚链接到对应的div,再根据z-index的属性，改变div的层级关系，从而实现tab的切换效果 */
        z-index: 1;
}  
```
###### empty伪类
E:empty伪类选择器匹配没有任何子元素（包括text节点）的元素
```HTML
<p><strong>有内容</strong></p>
<div class="box">有内容</div>
<p><strong>无内容淡色背景</strong></p>
<div class="box"></div>
<p><strong>空格也算内容</strong></p>
<div class="box"> </div>
<p><strong>伪元素不算内容</strong></p>
<div class="box pseudo"></div>
```
```css
.box { width: 256px; height: 90px; padding: 10px; background-color: #cd0000; color: #fff; }
.box:empty { opacity: .1; }
.pseudo::after { content: '伪元素生成内容'; }
```
###### root伪类
root伪类选择文档树中的第一个元素， 它唯一真正的作用就是发生在XML文档添加样式表的时候。同时它在HTML中使用root也有一个小作用——为html元素赋予特殊度，在需要覆盖简单类型选择器的时候很有用。
###### not伪类
否定选择器“:not”主要用于定位不匹配该选择器的元素。在统一设置元素样式时过滤不需要的元素。其最大的作用是在统一设置某个元素样式时可以排除部分特定的元素，简化CSS样式代码。
```css
p{ font-style:italic; }
p:first-child{ font-style:normal; }
/* :not伪类可以把上诉两行代码简化成下面一行代码 */
p:not(:first-child){ font-style:italic; }     /* 传入not伪类的参数必须是一个简单的选择器，连结符（如+和>）以及伪类元素都是没效的 */
```
###### UI元素状态伪类
这种伪类的特点是指定的样式只有当元素处于某种状态时才起作用，在默认状态下不起作用。具体有：
```CSS
E:hover | E:active | E:focus | E:enable | E:disable
E:read-only | E:read-write | E:checked | E:default
E:indeterminate | E::selection | E:valid | E:invalid
E:required | E:optional | E:in-range
```
详细介绍可参考 [CSS3--UI元素状态伪类选择器](http://www.jianshu.com/p/69371a97d142)

##### 5)伪元素
很多时候我会把伪类和伪元素搞混，




























