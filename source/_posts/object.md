---
title: 面向对象的程序设计
date: 2018-04-09 17:05:32
updated : 2018-04-09
tags: js 自我笔记
---
# 属性类型
只有内部才用的特性(attribute)，描述了属性(property)的各种特征，这些特征是为了实现javascript引擎用的，因此在javascript中不能直接访问他们，为了表示特性是内部值，ES5规范把他们放在两对方括号里，ECMAScript中有两个属性：数据属性与访问器属性
## 数据属性
[[Configurable]] : 能否通过delete删除属性从而重新定义，默认值`true`
[[Enumberable]] : 能否通过for-in循环返回属性，默认值`true`
[[Writable]] : 是否可以修改属性的值，默认值`true`
[[Value]] : 包含属性的数据值，默认为`undefined`
要是想要修改属性默认的特性，必须使用ES5的`Object.defineProperty()`方法，三个参数：属性所在对象，属性名，描述对象(4个属性值中的一个或者多个)
{% codeblock lang:javascript %}
var person = {};
Object.defineProperty( person, 'name', {
	value : 'oldThree',
	writable : false,
	configurable : false
} );
person.name = 'oldSix';  //严格模式下报错
console.log( person.name );  //'oldThree'

delete person.name;  //严格模式下报错
console.log( person.name );  //'oldThree'  
{% endcodeblock %}
一旦把configurable设置为不可配置，那么就不能变回可配置了，此时若在调用`Object.defineProperty()`方法修改除`writable`之外的特性都将报错