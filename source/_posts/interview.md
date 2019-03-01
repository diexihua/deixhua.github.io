---
title: 杂项
date: 2018-04-11 22:43:27
updated : 2018-04-12
tags: js html css 自我笔记
---
### 将类数组转化为数组
{% codeblock lang:javascript %}
Array.prototype.slice.call();
[].slice.call();
// ES6:
Array.from();
{% endcodeblock %}
### 判断undefined
undefined能够在低版本浏览器被赋值,为了避免判断错误，可以使用`testValue === void 0`来判断
### js自定义事件

记得写！！！！！



### 一个未知宽高的div如何在未知宽高的div中居中
{% codeblock lang:html %}
<div id="father">
	<div id="child"></div>
</div>
{% endcodeblock %}
1.使用table-cell渲染为表格，把子元素变为inline-block，若是`img`就不用变了
{% codeblock lang:css %}
#father{
	width: 300px;
	height: 300px;
	background: #000;

	display: table-cell;
	text-align: center;
	vertical-align: middle;
}
#child{
	width: 100px;
	height: 100px;
	background: #f00;

	display: inline-block;
}
{% endcodeblock %}

2.使用flex布局，也就是弹性盒子，还有一个`display: table`，据说是flex前身，兼容以前版本，具体内容还不清楚
display:box; 是老语法，display:flex;是新语法，二者还有以下不同：
与子元素display方式的相关性不同display:box; 作用下，如果子元素是block的，竖着排，如果子元素是 inline、inline-block的，横着排。display:flex; 作用下，是横着排还是竖着排，只取决于 flex-direction的值是row还是column。
float等属性是否受影响的情况不同display:box;作用下，float、clear、text-align、vertical-align 仍起作用。 display:flex;作用下，上述四属性失效。
横向排列时，子元素之间空白字符（空格、换行等）处理不同display:box;作用下，不会被忽略。display:flex; 作用下，忽略

{% codeblock lang:css %}
#father{
	width: 300px;
	height: 300px;
	background: #000;

	display: flex;
	justify-content: center;  /* 是在主轴上的对齐方式，即X轴 */
	align-items: center;  /* 是在Y轴上的对齐方式，针对于在只有一条轴的情况 */
}
#child{
	width: 100px;
	height: 100px;
	background: #f00;
}
{% endcodeblock %}

3.使用transform与绝对定位

{% codeblock lang:css %}
#father{
	width: 300px;
	height: 300px;
	background: #000;

	position: relative;
}
#child{
	width: 100px;
	height: 100px;
	background: #f00;

	position: absolute;
	top: 50%;
	left: 50%;
	transform: translate( -50%, -50% );
}
{% endcodeblock %}