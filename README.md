# canvas 学习笔记

## Canvas definition
在 MDN 中是这样定义 `<canvas>`的：
> `<canvas>` 是 HTML5 新增的元素，可用于通过使用 JavaScript中的脚本来绘制图形。例如，它可以用于绘制图形，制作照片，创建动画，甚至可以进行实时视频处理或渲染。

tips: `<canvas>`只是一个画布，允许脚本语言动态渲染`位图像`，本身并不具备绘图的能力，绘图必须使用JvaScript等脚本语言(画笔)。

## Canvas 出现的背景，出于什么目的，解决了什么问题
为了解决只能在Web页面中显示静态图片的问题，出现了Canvas标签。它是一个绘图矩形，通过JavaScript API动态创建和操作图像及动画。

## 自己总结Canvas是什么
什么是 Canvas？Canvas 是为了解决 Web 页面中只能显示静态图片这个问题而提出的一个可以使用 JavaScript 等脚本语言向其中绘制图像的 HTML 标签。

## 游览器支持
Internet Explorer 9、Firefox、Opera、Chrome 以及 Safari 支持 `<canvas>` 及其属性和方法。
Tips：Internet Explorer 8 以及更早的版本不支持 `<canvas>` 元素。

## svg 和 Canvas 的区别

### 什么是svg
> svg(Scalable Vector Graphics) 可缩放矢量图形是基于可扩展标记语言(XML)(SGML标准通用标记语言的子集)，用于描述二维矢量图形的一种图形格式。
简单来说， `svg就是可以用来定义XML格式的矢量图形。`

因为其本质是 XML 文件，所以 svg 是使用 XML 文档描述来绘图的。和 HTML 一样，如果我们需要修改 svg 文件，可以直接使用记事本打开修改。

### canvas 和 svg 的区别
canvas 和 svg都允许您在浏览器中创建图形，但是它们在根本上是不同的。那么 canvas 和 svg 有什么根本区别呢？

就如刚刚介绍的那样，`svg 本质上是一种使用 XML 描述 2D 图形的语言。`

那么 svg 创建的每一个元素都是一个独立的 dom 元素，既然是独立的 dom 元素，那么我们就可以通过 css 和 JavaScript 来操控 dom。可以对每一个 dom 元素进行监听。

并且因为每一个元素都是一个 dom 元素，所以修改 svg 中的 dom 元素，系统会自动进行 dom 重绘。

`Canvas 通过 JavaScript 来绘制 2D 图形`，Canvas 只是一个 HTML 元素，其中的图形不会单独创建 dom 元素。所以我们不能通过 JavaScript 操控 Canvas 内单独的图形。不能对其中的具体图形进行监控。

在 Canvas中，一旦图形被绘制完成，它就不会继续得到浏览器的关注。如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象。

<table>
	<tr>
		<th>Canvas</th>
		<th>svg</th>
	</tr>
	<tr>
		<td>依赖分辨率（位图）</td>
		<td>不依赖分辨率（矢量图）</td>
	</tr>
	<tr>
		<td>单个 HTML 元素</td>
		<td>每一个图形都是一个 dom 元素</td>
	</tr>
	<tr>
		<td>只能通过脚本语言绘制图形</td>
		<td>可以通过 CSS 也可以通过脚本语言绘制</td>
	</tr>
	<tr>
		<td>不支持事件处理器</td>
		<td>支持事件处理器</td>
	</tr>
	<tr>
		<td>弱的文本渲染能力</td>
		<td>最适合带有大型渲染区域的应用程序（比如谷歌地图）</td>
	</tr>
	<tr>
		<td>图面较小，对象数量较大（> 10）时性能最佳</td>
		<td>对象数量较小(< 10)图面更大时性能更佳</td>
	</tr>
</table>

## Canvas应用场景
绘制图表 小游戏 活动页面 小特效 炫酷背景 等等

## 第一个Canvas画布
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        #canvas {
            background: #000;
        }
    </style>
</head>
<body>
<canvas id="canvas" width="400" height="400">

</canvas>
<script>
	var canvas = document.getElementById('canvas');
	var context = canvas.getContext('2d');
	var cx = canvas.width = 400;
	var cy = canvas.height = 400;

	context.beginPath(); // 起始一条路径，或重置当前路径
	context.arc(100,100,50,0,Math.PI*2,true); // 创建弧/曲线
	context.closePath(); // 创建从当前点回到起始点的路径
	context.strokeStyle = "#FFF";// 设置或返回用于描边绘画的颜色、渐变或模式
	context.stroke();// 描边当前绘图（路径）
</script>
</body>
</html>
```
### 如何设置Canvas高度、宽度
见代码，一般来说会在html标签canvas中直接设置，或者通过Javascript设置。
tips: 如果通过CSS设置的话画布会按照原始大小`300*150`的比例进行缩放，也就是会将`300*150`的页面显示在`400*400`的画布中，导致变形。

## <a href="http://www.w3school.com.cn/tags/html_ref_canvas.asp" target="_blank">Canvas方法与属性</a>
### `getContext(contextType, contextAttributes)方法`
可返回一个对象，该对象提供了用于在画布上绘图的方法和属性。
* 上下文类型（contextType）：
	* 2d（本小册所有的示例都是 2d 的）：代表一个二维渲染上下文
	* webgl（或"experimental-webgl"）：代表一个三维渲染上下文
	* webgl2（或"experimental-webgl2"）：代表一个三维渲染上下文；这种情况下只能在浏览器实现 WebGL 版本2 (OpenGL ES 3.0)。

### 绘制弧/曲线
arc() 方法创建弧/曲线（用于创建圆或部分圆）。
```
context.arc(x,y,r,sAngle,eAngle,counterclockwise);
```
* x:圆的中心的 x 坐标。
* y:圆的中心的 y 坐标。
* r:圆的半径
* sAngle:起始角，以弧度计。（弧的圆形的三点钟位置是 0 度）。
* eAngle:结束角，以弧度计。
* counterclockwise:可选。规定应该逆时针还是顺时针绘图。False = 顺时针，true = 逆时针。
<div style="text-align:center;cursor: default;">
	<img src="images/arc.png" title="图片来自 w3cschool" alt="arc()方法图">
</div>