###  MUI滑块美化
公司用MUI开发，碰巧今天要做一个滑块。  <br>
于是看了一下MUI的滑块，功能挺强大的，并且在手机上测试也很流畅。 可以点击[这里](http://www.dcloud.io/hellomui/examples/range.html)体验一下。  <br>

但是设计图需要的是： ![](./views/MUIrange.jpg)  <br>
明显range的样式差别挺大的。   <br>
其实滑块的样式，不是MUI的问题，所有range的原始样式都略丑。Range是HTML5中新出现的滑块控件，也是常见的控件的之一，不过Internet Explorer 9及更早IE版本并不支持这个控件。  <br>
首先来看看最原始的range样式：
```html
<input type="range" value="0">
```
Chrome： ![](./views/chrome_range.jpg)    <br>
Firefox： ![](./views/firefox_range.jpg)     <br>
IE 9+ ： ![](./views/IE9+_range.jpg)    <br>

##### 那么如何美化range呢？

* 通过css改造range的样式
* 将滑动条隐藏(设置opacity: 0)，通过自定义div实现
这里主要讲一下css改造range的样式：

###### 去除系统默认的样式
```css
input[type="range"]{
	  -webkit-appearance: none;
		overflow:hidden;     /* 范围 */
		outline : none;      /* 点选会有蓝线或虚线 */
		background:none;
}
input[type="range"]{
		background-image:-webkit-linear-gradient(left ,#fff 0%,#fff 15%,#A96413 0%, #A96413 100%);
}
```

###### 给滑动轨道(track)添加样式
```css
input[type="range"]::-webkit-slider-thumb{
		-webkit-appearance: none;
		width:16px;
		height:16px;
		background:#D3D3D3;
		border-radius:50%;      /* 将轨道设为圆角的 */
		transition:.2s;        /* 点选放大时候的渐变时间 */
}
```
###### 根据滑块所在位置填充进度条
```javascript
          //监听input事件，获取range的value值，也可以直接element.value获取该range的值
		    var inlineRange = document.getElementById("inline-range");
		    var inlineVal = document.getElementById("inline-range-val");
		    
		    var payTime = document.getElementById("payPeriods");
		    var p = 0;

		    inlineRange.addEventListener('input',function(){
		    	inlineVal.innerHTML = this.value;
		    	payTime.innerHTML = this.value;
		    	p = this.value /24 * 100;
		    	bg(p);
		    },false);
		    
		    
		    function bg(n){
		      inlineRange.style.backgroundImage = '-webkit-linear-gradient(left ,#fff 0%,#fff '+n+'%,#A96413  '+n+'%, #A96413  100%)';
		  	}
```
demo可以点击[这里](http://jsbin.com/cisayevaho/1/edit?html,css,js,output)
