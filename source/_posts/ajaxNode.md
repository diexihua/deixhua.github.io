---
title: ajax学习笔记
date: 2018-05-11 22:32:23
updated : 2018-05-12
tags: js 自我笔记 ajax
---
ajax( Asynchronous Javascript And XML -- 异步的javascript和XML )，用javascript异步的去操作xml，主要用来实现客户端与服务器的异步通信效果，实现页面的局部刷新
可以理解为ajax替用户做了一系列的动作
1.打开浏览器
{% codeblock lang:javascript %}
var xhr = null;
// 创建一个ajax对象
if( window.XMLHttpRequest ){
	xhr = new XMLHttpRequest();
}else{
	// 兼容ie
	xhr = new ActiveXObject('Microsoft.XMLHTTP');
}
{% endcodeblock %}
2.输入网址
{% codeblock lang:javascript %}
// 请求方式 / 请求地址 / 是否异步
xhr.open('get', '1.get.php?username='+encodeURI('六六')+'&password=22', true);
{% endcodeblock %}
3.提交请求
{% codeblock lang:javascript %}
xhr.send();
{% endcodeblock %}
4.等待网页打开
{% codeblock lang:javascript %}
// 等待服务器返回内容
xhr.onreadystatechange = function(){
	if ( xhr.readyState === 4 ) {
		if ( xhr.status === 200) {
			alert(xhr.responseText);
		}else{
			alert( xhr.status );
		}
	};
};
{% endcodeblock %}
综合起来
{% codeblock lang:javascript %}
var xhr = null;
if( window.XMLHttpRequest ){
	xhr = new XMLHttpRequest();
}else{
	xhr = new ActiveXObject('Microsoft.XMLHTTP');
}
xhr.open('get', 'testAjax.php?username=magicalj', true);
xhr.send();
xhr.onreadystatechange = function(){
	if ( xhr.readyState === 4 ) {
		if ( xhr.status === 200) {
			alert(xhr.responseText);
		}else{
			alert( xhr.status );
		}
	};
};
{% endcodeblock %}
### 容错处理
当状态值改变时触发`onreadystatechange`函数,那么什么是状态呢，`readyState`的值代表了ajax的工作状态
0: (初始化)未调用open()方法
1: (载入)已经调用send(),正在发送请求
2: (载入完成)send()完成,已收到全部响应内容
3: (解析)正在解析响应内容
4: (完成)解析完成,可在客户端调用
也就是每次`readyState`值变化都会触发`onreadystatechange`函数，我们需要在响应内容解析完成时去对得到的数据进行处理才不会出错，所以进行第一次判断是对`readyState`
第二次判断是对`status`进行判断，`status`为服务器状态，常见的就是200(成功了)，404(找不到页面)等等，具体可查询{% link http状态码 https://baike.baidu.com/item/HTTP状态码 %}，针对性进行相应处理
### 注意事项
#### get方式存在的问题
1.缓存(请求地址一样的话)，post无缓存
解决办法：可以在url?后面接随机数,例如时间戳
2.乱码(存在中文)
解决办法：使用编码encodeURI
{% codeblock lang:javascript %}
xhr.open('get', 'testAjax.php?username='+encodeURI('六六'), true);
{% endcodeblock %}
#### post方式存在的问题
1.post方式不能在url后面直接接数据，数据放在`send()`里面作为参数传递，并且必须设置请求头，去申明发送的数据类型，因为设置请求头已经告诉了编码，所以post无需编码
{% codeblock lang:javascript %}
xhr.SetRequestHeader('content-type', 'application/X-www-form-urlencoded');
xhr.send(username=magicalj);
{% endcodeblock %}
### 跨域
协议、域名、端口都相同是同源，否则都是跨域
解决方案:
1.jsonp
在数据加载进来前定义好一个函数，这个函数接受一个参数(数据)，利用这个参数做相应操作
在需要的时候，使用script标签加载相应的远程文件资源，资源加载完毕后就执行上述定义好的函数，把数据当做参数传进去
{% codeblock lang:javascript %}
function cb(data){
	alert(data);
}

var scriptTest = document.createElement('script');
scriptTest.src='url?callback=cb';
document.body.appendChild(scriptTest);
{% endcodeblock %}
后端传过来的数据需要定义好一个函数名字与我们的一样，这样就可以使用了，所以我们把这个函数名当做参数传给后端，后端再操作完返回
2.document.domain+iframe的设置
3.window.name实现的跨域数据传输
4.使用HTML5 postMessage