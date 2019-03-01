---
title: css笔记
date: 2018-04-26 23:37:32
updated : 2018-04-27
tags: css 自我笔记
---
`margin`与`padding`的值为百分比时，分别是如何计算的？
`margin`：相对最近的父级块级元素的width( `margin-top`与`margin-bottom`也是根据width )
`padding`：相对最近的父级块级元素的width( `padding-top`与`padding-bottom`也是根据width )

css同级别样式会找到最后一条, 与类名排序无关，最后引入的css最新覆盖,`style`样式一样看顺序 例如:
{% codeblock lang:css %}
<style>
.dd3{
  background: yellow;
}
</style>
{% endcodeblock %}
{% codeblock one.css lang:css %}
.dd1{ background: black; }
{% endcodeblock %}
{% codeblock two.css lang:css %}
.dd2{ background: white; }
{% endcodeblock %}
{% codeblock lang:html %}
<div class="dd1 dd2"></div>
<div class="dd2 dd1"></div>
{% endcodeblock %}
上述两个div均为白色

完善中...