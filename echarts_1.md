###### ECharts画圆使用小结

[ECharts](http://echarts.baidu.com/)开源来自百度商业前端数据可视化团队，基于html5 Canvas，是一个纯Javascript图表库，提供直观，生动，可交互，可个性化定制的数据可视化图表。 <br>
用一张图来了解ECharts：   <br>
![](./views/40e161e7435e316b26ed0bf5dd5315e3.png)   <br>
设计师给的图是： ![](./views/echarts.jpg)

只需要在html中添加如下代码：
```javascript
<div id="ECharts"></div>
```
参考官方实例，最像设计图的就是 ![](./views/echarts_1.jpg)   <br>
或者点击[这里](http://echarts.baidu.com/demo.html#pie-doughnut)  <br>
但是可以看出设计图的图例组件是有一定的空白间隔的，而且显示的数据不单纯只是data的数值，还有就是控制饼图的每一项位置是在饼图的右方。  <br>
所以我们接下来就来对照ECharts [配置项手册](http://echarts.baidu.com/option.html#title) 来做出我们想要的饼图

