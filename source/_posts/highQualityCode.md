---
title: 编写高质量的代码
date: 2018-05-15 12:45:10
updated : 2018-05-15
tags: js
---
## 防止浮点数溢出
{% codeblock lang:javascript %}
var num = 0.1 + 0.2; //0.30000000000000004
{% endcodeblock %}
这是JsvaScript中最经常报告的bug，这是遵循二进制浮点数算术标准(IEEE 754)而导致的，幸运的是，浮点数中的整数运算是精确的，所以小数表现出来的问题可以通过指定精度来避免
例如，针对上面的相加可以这样处理
{% codeblock lang:javascript %}
var num = (1+2)/10； //0.3
{% endcodeblock %}
## 正确检测数据类型
使用`typeof`运算符检测`null`值时，返回的是“object”，而不是“null”。更好的检测方式其实很简单。下面定义了一个检测值类型的一般方法
{% codeblock lang:javascript %}
var type = function(o){
	return (o === null) ? "null" : (typeof o);
}
{% endcodeblock %}
这样可以避开因为`null`值影响基本数据的监测。
**注意：typeof不能够检测复杂的数据类型，以及各种特殊用途的对象，如正则表达式、日期对象、数学对象等**
下面是一个比较完整的数据类型安全检测方法
{% codeblock lang:javascript %}
//安全检测javascript基本数据类型和内置对象
//参数：o 表示检测的值
//返回值: 返回字符串"undefined"、"number"、"boolean"、"string"、"function"、
//"regexp"、"array"、"date"、"error"、"object"、或"null"
var typeof = function(o){
  var _toString = Object.prototype.toString;
  //获取对象的toString()方法引用
  //列举基本数据类型和内置对象类型，可以进一步补充该数组的检测数据类型范围
  var _type = {
    "undefined" : "undefined",
    "number" : "number",
    "boolean" : "boolean",
    "string" : "string",
    "[object Function]" : "function",
    "[object RegExp]" : "regexp",
    "[object Array]" : "array",
    "[object Date]" : "date",
    "[object Error]" : "error"
  }
  return _type[typeof o] || _type[_toString.call(o)] || (o? "object" : "null");
}
{% endcodeblock %}
应用示例:
{% codeblock lang:javascript %}
var s = Math.abs;
alert( typeof(s) );
{% endcodeblock %}
## 避免误用parseInt
建议使用parseInt时，一定要带基数参数
{% codeblock lang:javascript %}
parseInt("08"); //0
parseInt("08", 10); //8
{% endcodeblock %}
## 使用查表法提高条件检测的性能
例如，在下面代码中，使用switch检测value值。
{% codeblock lang:javascript %}
switch(value){
	case 0:
		return result0;
	case 1:
		return result1;
	case 2:
		return result2;
	case 3:
		return result3;
	case 4:
		return result4;
	case 5:
		return result5;
	case 6:
		return result6;
	case 7:
		return result7;
	case 8:
		return result8;
	case 9:
		return result9;
	default :
		return result10;
}
{% endcodeblock %}
使用switch结构检测value值的代码所占的空间可能与switch重要性不成比例，代码很笨重，整个结构可以使用一个数组查询代替：
{% codeblock lang:javascript %}
var results = [result0, result1, result2, result3, result4, result5, result6, result7, result8, result9];
return results[value] ? results[value] : result10;
{% endcodeblock %}