---
title: js学习笔记
date: 2018-03-24 13:54:01
updated : 2018-03-24
tags: js 自我笔记
---
# 简介
一个完整的Javascript实现应由下列三个不同的部分组成
1. 核心(ECMAScript)，ECMAScript与Web浏览器没有依赖关系，web浏览器只是ECMAScript实现可能的'宿主环境'之一，宿主环境不仅提供基本的ECMAScript实现，同时也会提供该语言的扩展 - （如DOM，则利用ECMAScript的核心类型和语法提供更多具体的功能，以便实现对环境的操作)，其他宿主环境包括node和Adobe Flash。
ECMAScript规定了这门语言的下列组成部分：
语法、 类型、 语句、 关键字、 保留字、操作符、 对象
2. 文档对象模型(DOM Document Object Model)，DOM是针对XML但经过扩展用于HTML的'应用程序编程接口'(API application Programming Interface)
3.  浏览器对象模型(BOM)

# 数据类型
5种简单数据类型: `Number`, `String`, `Boolean`, `undefined`, `null`
1种复杂数据类型: `Object`
## typeof操作符
返回值:
`undefined` - 这个值未定义，未初始化和未声明的变量执行typeof均返回undefined
`object` -  是对象或者null
`function`  -  函数
{% codeblock lang:javascript %}
console.log( error );  //未声明,产生错误
console.log( typeof error );  //undefined
console.log( null == undefined );  //true
console.log( null === undefined );  //false
{% endcodeblock %}
## Number
保存浮点数值需要的存储空间是整数的两倍
{% codeblock lang:javascript %}
console.log( 0/0 );   //NaN
console.log( 200/0 ); //Infinity
console.log( -200/0 );    //-Infinity
{% endcodeblock %}
### 数值转换Number() / parseInt()
parseIntd对8进制解析存在困惑，所以使用parseInt一定要带上第二个参数(进制基数)，第二个参数为: 2, 8, 10, 16
{% codeblock lang:javascript %}
console.log( Number("") );  //0
console.log( parseInt("") );    //NaN

console.log( Number( "1234abcd" ) );  //NaN
console.log( parseInt( "1234abcd" ) );    //1234

console.log( Number( "q123" ) );  //NaN
console.log( parseInt( "q123" ) );    //NaN
{% endcodeblock %}
## String
ECMAScript5为所有字符串定义了`trim()`方法，创建一个字符串副本并且删除前后所有空格，然后返回
### toString
null与undefined的toString会报错
{% codeblock lang:javascript %}
try{
    console.log( typeNull.toString() ); //报错
}catch(e){
    console.log(e.name + ": " + e.message);
}
try{
    console.log( undef.toString() );    //报错
}catch(e){
    console.log(e.name + ": " + e.message);
}
{% endcodeblock %}
# 操作符
## 位操作符
{% blockquote %}
这套运算符针对的是整数，所以对javascript无用，因为javascript内部所有数字都保存为双精度浮点数，若使用位运算符，javascript不得不将运算数先转化为整数后再运算，降低了效率
{% endblockquote %}
按位非(NOT)`~`,使用方法,位运算NOT是三步的处理过程:
1.把运算数转换成32位数字
2.把二进制数转换成它的二进制反码（0->1, 1->0）
3.把二进制数转换成浮点数
{% codeblock lang:javascript %}
//对于浮点数，~~value可以代替parseInt(value)，而且前者效率更高些
~~(-6.6) //-6
parseint(-6.6) //-6
{% endcodeblock %}
## 布尔操作符
* 逻辑非(!) 将值变为布尔值再取反，两个逻辑非(!!) 可以模拟Boolean()函数，将值转化为布尔值
* 逻辑与(&&) 两者都是true返回true，有一个是false则返回false，第一个参数是对象，则返回第二个参数，第一个参数为true才会返回第二个参数，若有null、NaN、undefined则返回这些本身,若有未定义变量，则报错，短路操作，第一个参数是false就不会对第二个参数求值了
* 逻辑或(||) 两者有一个true就返回true，也是短路操作,可以利用这一特性来防止给变量赋null或undefined值

{% codeblock lang:javascript %}
var myObject = initialObject || reserveObject;
{% endcodeblock %}
# 函数
* 推荐要让函数始终都有一个返回值，要么永远不要有返回值，时有时无会给调试代码带来不便
* ECMAScript函数参数不介意传多少个参数，也不在乎传什么数据类型，参数会用一个类数组表示，arguments是一个类数组对象(不是Array的实例)，包含着传入函数中的所有参数，length属性可以确定参数的多少
* ECMAScript中的所有参数传递的都是值，不可能通过引用传递参数

{% codeblock lang:javascript %}
var one = 1;
var changeOne = function( args ){
    args = 3;
};
changeOne( one );
console.log( one ); //1
{% endcodeblock %}
ECMAScript函数不能重载,相同函数名,后者覆盖前者

# 变量、作用域和内存问题
## 基本类型和引用类型
引用类型(object),只有引用数据类型可以添加属性,基本类型添加后访问为undefined
复制变量值,基本类型的复制就是值的复制,这两个变量可以参与任何操作而不互相影响
{% codeblock lang:javascript %}
var num1 = 5;
var num2 = num1;
num2 = 10;
console.log( num1 );  //5

var setName = function( obj ){
	obj.name = "oldName";
	obj = new Object();
	obj.name = "newName";
};
var person = new Object();
setName( person );
console.log( person.name );  //'oldName'
{% endcodeblock %}
### 检测类型
{% codeblock lang:javascript %}
obj1 instanceof Object
obj1 instanceof Array
obj1 instanceof RegExp
{% endcodeblock %}
## 执行环境与作用域
每个函数都有自己的执行环境，当代码在一个环境执行时，会创建变量对象的作用域链，作用域链用途是保证执行环境对有权访问所有的变量和函数的有序访问，标识符的解析是沿着作用域链一级一级的搜索标识符的过程，全局变量始终是作用域链中的最后一个对象
### 浏览器js解析器
预解析，两步，第一步，先找一些东西，第二步，逐行解析，全局和局部范围都遵循此规则
+ 第一步会先找`var`和`function`还有参数，找到`var`后，会直接存储变量名，不会读取变量值，变量值初始存为`undefined`，函数声明(function)保存为函数块，如果变量名和函数声明的名字相同，二者取一，保留函数声明，两个函数声明重名了，那么后者覆盖前者
+ 第二步逐行解析，遇到表达式( + - * / = ++ -- % ! 等 还有‘参数‘ )，会改变相应存储变量的值( 函数声明不改变值 )，参数相当于赋值
`script`标签是自上而下的，一个`script`标签内部全部执行完才会进入下一个
`if`和`for`等不属于作用域，但是firefox对无法对这些里面的函数预解析，所以尽量不要在这些里面声明全局变量

### 没有块级作用域
{% codeblock lang:javascript %}
if( true ){
	var i = 1;
};
console.log( i );  //1,i为全局变量
for( var m=0, m < 5, m++ ){
	m = m;
};
console.log( m );  //5,m为全局变量
{% endcodeblock %}
## 垃圾回收
javascript具有自动垃圾收集机制，原理就是找到不再继续使用的变量，然后释放其占用的内存，垃圾收集器会按照固定时间间隔(或代码执行中预定的收集时间)，周期性执行这一操作
### 管理内存
分给web浏览器的可用内存数量通常比分配给桌面应用的少，确保占用最少的内存可以让页面获得更好地性能，优化内存占用的最佳方式就是执行中的代码只保存必要数据，一旦不用的，将其设置为`null`来释放其引用(解除引用)，适用于大多数全局变量和全局变量的属性
解除并不意味着自动回收该值占用的内存，解除内存的真正作用是让值脱离执行环境，以便垃圾回收器下次运行时将其回收
# 引用类型
尽管ECMAScript从技术上讲是一门面向对象的语言，但它不具备传统面向对象语言所支持的类和接口等基本结构，引用类型有时候也被称为对象定义
## Object 类型
我们用到的大多数引用类型都是`Object`类型的实例，创建Object实例方式有两种，第一种是new操作符后面跟Object构造函数，另一种方式是**对象字面量**表示法
{% codeblock lang:javascript %}
// 第一种方式
var person = new Object();
person.name = 'wuming';
// 第二种方式
var person2 = {
	name : 'meiming';
};
{% endcodeblock %}
在通过*对象字面量*定义对象时，实际上不会调用Object构造函数
访问对象属性时使用的都是点表示法，Javascript中还可以使用方括号表示法来访问，属性以字符串形式放在括号中
{% codeblock lang:javascript %}
console.log( person.name );
console.log( person['name'] );
{% endcodeblock %}
通常，除非必须使用变量来访问属性，否则建议使用点表示法
## Array类型
与对象一样，在使用数组字面量表示法时，也不会调用Array构造函数
数组的`length`属性很有特点-不止是可读的，可以通过设置`length`来从数组末尾移除或者添加新项
### 检测数组
`instanof Array`,instance操作符问题在于，它假设只有一个全局执行环境，如果网页中包含多个框架，那实际就存在两个以上不同的全局执行环境，从而存在两个不同版本的Array构造函数，如果你从一个框架向另一个框架传递一个数组，那么传入的数组与第二个框架中原生创建的数组分别具有各自不同的构造函数。未解决此问题，ECMAScript5新增了`Array.isArray()`方法，这个方法的目的是最终确定某个值到底是不是数组，想要在未实现这个方法的浏览器中检测数组，办法都一样，大家知道，在任何值上调用Object原生的toString方法，都会返回一个[Object  NativeConstructorName]格式的字符串，每个类在内部都有一个[Class]属性，这个属性就指定了上述字符串中的构造函数名
{% codeblock lang:javascript %}
var isArray = function( value ){
	return Object.prototype.toString.call( value ) == '[Object Array]';
};
var isFunction = function( value ){
	return Object.prototype.toString.call( value ) == '[Object Function]';
};
var isRegExp = function( value ){
	return Object.prototype.toString.call( value ) == '[Object RegExp]';
};
{% endcodeblock %}
这一技巧也广泛用于检测原生JSON对象，Object的toString()方法不能检测非原生构造函数的构造函数名，因此，开发人员定义的任何构造函数都将返回[Object Object]
{% codeblock lang:javascript %}
var isNativeJSON = window.JSON && Object.prototype.toString.call(JSON) == '[Object JSON]';
{% endcodeblock %}
请注意，Object.prototype.toString()本身也可能被修改，上述是假设Object.prototype.toString()是未被修改的原生版本
### Array对象方法
`reverse()` : 反转数组
`sort()` : 默认升序(前小后大)，默认会调用每一项的toString()方法，以字符串形势比较，可以传入一个比较函数，若第一个参数要位于第二个参数前则返回一个负数，反之则返回一个正数，相等返回0
{% codeblock lang:javascript %}
var compare = function( value1, value2 ){
	if( value1 < value2 ){
		return -1;
	}else if( value1 > value2 ){
		return 1;
	}else{
		return 0;
	}
};
var testArr = [ 0, 3, 1, 5, 4, 7 ];
testArr.sort( compare );
{% endcodeblock %}
只想反转数组，`reverse()`更快些，对于数值类型，可以用如下方法，更简单
{% codeblock lang:javascript %}
var compare = function( value1, value2 ){
	return value1 - valiue2;
};
{% endcodeblock %}
#### 迭代方法与归并方法
ECMAScript5为数组定义了5个迭代方法
ps: 迭代：迭代算法是用计算机解决问题的一种基本方法。它利用计算机运算速度快、适合做重复性操作的特点，让计算机对一组指令（或一定步骤）进行重复执行，在每次执行这组指令（或这些步骤）时，都从变量的原值推出它的一个新值。有时候，迭代也会指循环执行，反复执行的意思。
↓对数组中的每一项运行给定参数
`every()` : 若每一项都返回true，则返回true
`filter()` : 返回是true的项目组成的数组
`forEach()` : 仅运行，无返回值
`map()` : 返回每次调用结果组成的数组
`some()` : 任一项返回true，则返回true
ECMAScript5新增了两个归并数组的方法,这两个方法都会迭代数组的所有项，然后构建一个最终返回值，区别就是迭代顺序
`reduce()` : 从数组第一项遍历到最后
`reduceRight()` : 从最后一项，向前遍历到第一项
参数接受4个值，前一个值，当前值，项的索引和数组对象，函数返回的任何值都会作为第一个参数自动传递给下一项，`reduce()`第一次迭代发生在数组第二项上，此时第一个参数是数组第一项，第二个参数是数组第二项，`reduceRight()`从倒数第二项开始，一个道理
{% codeblock lang:javascript %}
var testArr = [ 1, 2, 3, 4, 5 ];
var num = testArr.reduce( prev, cur, index, array ){
	return prev + cur;
};
console.log( num );  //15
//过程: 1+2=3 → 3+3=6 → 6+4=10 → 10+5=15 
{% endcodeblock %}
支持这些方法的浏览器: IE9+、FireFox 2+、Safari 3+、Opera 9.5+和Chrome
## RegExp 类型
{% codeblock lang:javascript %}
var expression = / pattern / flags;
{% endcodeblock %}
`pattern`可以使任何**正则表达式**，`flags`表明正则表达式的行为，正则表达式匹配模式支持下列3个标志
`g` : 全局(global)模式，应用于所有字符串，而非在发现第一个匹配项时立即停止
`i` : 不区分大小写( case-insensitive )模式
`m` : 多行(multiline)模式，到达一行文本末尾时还会继续查找下一行中是否存在与模式匹配的项
使用**正则表达式字面量**和使用`RegExp`构造函数创建的正则表达式不一样，ECMAScript3中，正则表达式字面量始终会共享一个实例，而使用构造函数`RegExp`每一次都创建一个新的实例
ECMAScript5中明确规定，两者统一，每次都创建新的实例。IE9+、FireFox4+和chrome都据此做了修改
### 实例方法
`exec()`
`test()` : 接受一个字符串，模式与该参数匹配返回true，否则返回false
## Function 类型
ECMAScript中最有意思的莫过于函数了，有意思的根源是 - 函数实际上是对象。每个函数都是Function类型的实例，而且与其他引用类型一样具有属性和方法，由于函数是对象，因此函数名实际上是一个指向函数对象的指针，不会与某个函数绑定。
### 函数声明和函数表达式
解析器会率先解析函数声明，并使其在执行任何代码前可用(可以访问)，而函数表达式则必须等到解析器解析到它所在的代码行
### 作为值的函数
因为ECMAScript中的函数名本身就是变量，所以函数也可以当做值来使用，不仅可以像传递参数一样把一个函数传递给另一个函数，还可以将一个函数作为另一个函数的返回结果
{% codeblock lang:javascript %}
var callSomeFunction = function( someFunction, args ){
	return someFunction( args );
};
{% endcodeblock %}
可以从一个函数中返回另一个函数也是极为有用的一个技术，例如我们有一个对象数组，要根据某个属性排序，`sort()`方法需要两个要比较的值好，我们还需要指明按照哪个属性来排序，要解决此问题，我们可以定义一个函数，接受一个属性名，然后返回根据这个属性名创建的比较函数
{% codeblock lang:javascript %}
var createCompare = function( propertyName ){
	return function( arg1, arg2 ){

		var value1 = arg1[propertyName],
			value2 = arg2[propertyName];
		if( value1 < value2 ){
			return -1;
		}else if( value1 > value2 ){
			return 1;
		}else{
			return 0;
		}

	};
};
var data = [
	{ name : 'ada', age : '12' },
	{ name : 'aer', age : '11' }
];
data.sort( createCompare( 'age' ) );
{% endcodeblock %}
### 函数内部属性
禁止使用`arguments.caller`和`arguments.callee`，未来会被弃用， ECMAScript5禁止使用`arguments.callee`和`arguments.caller`
### 函数属性和方法
每个函数都包含两个属性，`length`和`prototype`，ECMAScript5中`prototype`不可枚举，用`for-in`无法发现
每个函数都有两个非继承而来的方法：`apply()`和`call()`，这两个方法的用途都是在特定作用域中调用函数，实际上等于设置函数内部`this`对象的值，`apply()`接收两个参数，一个是在其中运行函数的作用域，另一个是参数数组，第二个参数可以是Array实例也可以是arguments对象
{% codeblock lang:javascript %}
var sum = function( num1, num2 ){
	return num1 + num2;
};
var callSum1 = function( num1, num2 ){
	return sum.apply( this, arguments );
};
var callSum2 = function( num1, num2 ){
	return sum.apply( this, [num1, num2] );
};
console.log( callSum1( 2, 2 ) );  //4
console.log( callSum2( 2, 2 ) );  //4
{% endcodeblock %}
`call()`方法第一个参数也是this的值，区别在于其余参数都直接传递给函数，传递的参数必须逐个列举出来
{% codeblock lang:javascript %}
var callSum3 = function( num1, num2 ){
	return sum.call( this, num1, num2 );
};
{% endcodeblock %}
他们真正强大的地方是能够扩充函数赖以运行的作用域,使用`apply()`和`call()`来扩充作用域最大的好处，就是对象不需要和方法有任何耦合关系
{% codeblock lang:javascript %}
window.color = 'red';
var c = {
	color : 'orange'
};
var sayColor = function(){
	console.log( this.color );
};
sayColor();  //red
sayColor.call( c );  //orange
{% endcodeblock %}
## 基本包装类型
`Boolean`类型，`Number`类型，`String`类型
## 单体内置对象
`Global`,`Math` 


# 闭包
上面写道作用域，变量的作用域就是全局变量和局部变量，函数内部声明变量，使用var命令，声明的是一个局部变量，如果不用的话，声明的是全局变量
{% codeblock lang:javascript %}
var f1 = function(){
	var localVariable = 1;  //局部变量
	globalvariable = 1;  //全局变量
};
{% endcodeblock %}
我们想要在函数外部访问函数内部的局部变量，正常访问不到，函数内部的函数可以访问到，那么我们把函数内部的函数作为返回值，就可以访问到函数内部的属性了
{% codeblock lang:javascript %}
var f2 = function(){
	var localVariable = 1;

	//这个匿名函数本身也是闭包，相当于setter，可以在函数外部对函数内部的局部变量进行操作
	add = function(){
		localVariable += 1;
	}


	var visit = function(){
		alert( localVariable );
	}
	return visit;
};
var print = f2();
print();  //1
add();
print();  //2
{% endcodeblock %}
`print`实际上就是闭包函数`visit`,`f2`是`visit`的父函数，而`visit`被赋给了一个全局变量`print`，这导致`visit`始终在内存中，而`visit`的存在依赖于f2，因此f2也始终在内存中，不会在调用结束后，被垃圾回收机制（garbage collection）回收。
## 用法
可以模块化代码，减少全局变量污染
{% codeblock lang:javascript %}
var plus = (function(){
	var num = 1;
	return function(){
		num++;
		console.log(num);
	}
})();
plus();  //2
plus();  //3
{% endcodeblock %}
私有成员的存在
{% codeblock lang:javascript %}
var calculate = (function(){
	var num = 1;
	var plus = function(){
		num++;
		console.log(num);
	};
	var substract = function(){
		num--;
		console.log(num);
	};
	return {
		plus : plus,
		substract : substract
	}
})();
calculate.plus();  //2
calculate.plus();  //3
calculate.substract();  //2
{% endcodeblock %}
for循环中直接找到对应元素的索引
{% codeblock lang:javascript %}
for( var i=0; i<5 ; i++ ){
	setTimeout( function(){
		console.log(i);
	}, 0 );
}  //5 5 5 5 5
//想要顺序输出，除了去掉定时器，可以传参，定时器是可以传递更多参数的
for( var i=0; i<5 ; i++ ){
	setTimeout( function(i){
		console.log(i);
	}, 0, i );
}  //0 1 2 3 4
//还有就是闭包了
for( var i=0; i<5 ; i++ ){
	(function(i){
		setTimeout( function(){
			console.log(i);
		}, 0 );
	})(i);
}  //0 1 2 3 4
//或者这样
for( var i=0; i<5 ; i++ ){
	setTimeout( (function(i){
		return function(){
			console.log(i);
		}
	})(i), 0 );
}  //0 1 2 3 4
{% endcodeblock %}
## 使用闭包的注意点
1）由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。
2）闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。

上述内容注意点2)尚未理解



# 面向对象

{% codeblock lang:javascript %}
var person1 = new Object();
person1.name = '老三';
person1.say = function(){
	alert( this.name );
}
person1.say();  //老三

var person2 = new Object();
person2.name = '老六';
person2.say = function(){
	alert( this.name );
}
person2.say();  //老六
{% endcodeblock %}
如果这样创建多个对象，会有非常多的重复代码，那么就可以封装称为一个函数，不同的元素作为参数传进去，那么就会使用`工厂方式`，就是面向对象中的封装函数
{% codeblock lang:javascript %}
function createPerson( name ){
	var obj = new Object();
	obj.name = name;
	obj.say = function(){
		alert( this.name );
	};

	return obj;
}
var person1 = createPerson('小明');
var person2 = createPerson('小强');
person1.say();
person2.say();
{% endcodeblock %}
我们要模仿系统对象的创建方式
1. 首字母大写(直接改为大写即可)
2. 用`new`创建( 用`new`去调用一个函数，这个时候函数中的this就是创建出来的对象，而且函数的返回值直接就是`this`(隐式返回)，new后面调用的函数：叫做构造函数 )

{% codeblock lang:javascript %}
function CreatePerson( name ){
	this.name = name;
	this.say = function(){
		alert( this.name );
	};
}
var person1 =  new CreatePerson('小明');
var person2 =  new CreatePerson('小强');
person1.say();
person2.say();
{% endcodeblock %}
上述还有问题就是CreatePerson的say方法是相同的，但是我们new一个CreatePerson就会创建一个say方法，我们需要把公用方法或者属性存在内存中，只创建一个，然后实例分别去调用，提高性能，要使用到`原型`
{% codeblock lang:javascript %}
function CreatePerson( name ){
	this.name = name;
}
CreatePerson.prototype.say = function(){
	alert( this.name );
}
{% endcodeblock %}
待续...
