# 第九章 JavaScript之基础知识理解

《JavaScript深入浅出》学习笔记

1.1 数据类型

一、六种数据类型

object——包括Function、Array、Date……

number

string

boolean

null

undefined

JavaScript是一种弱类型语言：

	var num = 32;
	num = "this is a string";
	32+32       // 64
	"32"+32     // "3232"
	"32"-32     // 0

二、隐式转换

**最常用且容易疑惑的是加法运算，除了算数加的意义，还可表示着字符串拼接。**

（1）+、-的转换规则：

	num - 0;   //用于将num转换成对象

	num + '';  //用于将num转换成字符串

	"37" – 7  //30

	"37" + 7   //377

（2）==转换规则：

==的一边是数字，一边是字符串，则字符串隐式转换成数字

	"1.23"== 1.23       //true

==一边是数字，一边是boolean，则boolean隐式转换成数字

	0 == false          //true

	null == undefined   //true

	+0 = -0             //true

==一边是字符串或数字，一边是对象，则对象会转成基本数据类型

	3 == [3]        //true

==一边是boolean，另一边是string，则两边都转成数字进行比较，如

	true == '111'   //false

	true == '1'     //true

	true =='0'      //false

	false == '0'    //true

	'true'== true   //false

	'false'==false  //false

==的一边是boolean，另一边是undefined或者null，则值均为false。如：

	ture==undefined;   //false

	false==undefined;   //false

	true==null;   //false

	false==null  //false

均为false。

	0==null;   //false

	0==undefined;  //false

均为false

	'null'==null;   //false

	'undefined'==undefined;  //false

均为false

==一边是字符串或数字，一边是对象，则对象会转成基本数据类型。

	new Object() == new Object()   //false，因为是两个不同的对象

	[1, 2] == [1, 2]   //false，因为是两个不同的数组

**注意，JS中的对象在进行比较时，比较的是引用，而不是引用所指向的值。所以值相同的对象不代表==。**

类型相同，同===

类型不同，尝试类型转换和比较:

	null == undefined 相等

	number == string 转number     1 == “1.0" // true

	boolean == ?  转number       1 == true  // true

	object == number | string 尝试对象转为基本类型  new String('hi') == 'hi' // true

	其它：false


（3）===运算符首先比较的是左右两边类型是否相同，若不同，则为false，若相同再进行相当于==运算符所进行的比较

===两边类型不同，返回false；

类型相同则：

NaN ≠ NaN，NaN与任何东西均不等，包括与NaN也不等

new Object ≠ new Object

+0 === -0 以及 +0 == -0 均为true

null === null  以及 null== null 均为true

undefined === undefined undefined==undefined均为true

一段摘录

>
The Abstract Equality Comparison Algorithm
The comparison x == y, where x and y are values, produces true or false. Such a comparison is performed as
follows:
1. If Type(x) is the same as Type(y), then
a. If Type(x) is Undefined, return true.
b. If Type(x) is Null, return true.
c. If Type(x) is Number, then
i. If x is NaN, return false.
© Ecma International 2011 81
ii. If y is NaN, return false.
iii. If x is the same Number value as y, return true.
iv. If x is +0 and y is -0, return true.
v. If x is -0 and y is +0, return true.
vi. Return false.
d. If Type(x) is String, then return true if x and y are exactly the same sequence of characters (same
length and same characters in corresponding positions). Otherwise, return false.
e. If Type(x) is Boolean, return true if x and y are both true or both false. Otherwise, return false.
f. Return true if x and y refer to the same object. Otherwise, return false.
2. If x is null and y is undefined, return true.
3. If x is undefined and y is null, return true.
4. If Type(x) is Number and Type(y) is String,
return the result of the comparison x == ToNumber(y).
5. If Type(x) is String and Type(y) is Number,
return the result of the comparison ToNumber(x) == y.
6. If Type(x) is Boolean, return the result of the comparison ToNumber(x) == y.
7. If Type(y) is Boolean, return the result of the comparison x == ToNumber(y).
8. If Type(x) is either String or Number and Type(y) is Object,
return the result of the comparison x == ToPrimitive(y).
9. If Type(x) is Object and Type(y) is either String or Number,
return the result of the comparison ToPrimitive(x) == y.
10. Return false.

三、包装对象

	var a = "string";
	alert(a.length);
	a.t = 3;
	alert(a.t);//undefined

对于string、boolean、number类型进行属性或方法操作时，会临时转变成String、Boolean、Number对象，这个临时对象就是包装对象，待该属性或方法操作完毕，这个临时对象立即被销毁，所以如果对string、boolean、number类型的值进行新的属性或方法定义时，定义完后再调用时，因为包装对象已被销毁，所以结果是undefined。

四、类型检测

typeof —— 可以准确判断基本数据类型以及function

instanceof—— 可以判断左操作数的原型链上是否有右操作数的prototype属性

注意：不同window或iframe间的对象类型检测不能使用instanceof！

MDN上关于跨越iframe的instanceof的解释：

instanceof and multiple context (e.g. frames or windows)

Different scope have different execution environments. This means that they have different built-ins (different global object, different constructors, etc.). This may result in unexpected results. For instance, [] instanceof window.frames[0].Array will return false, because Array.prototype !== window.frames[0].Array and arrays inherit from the former. This may not make sense at first but when you start dealing with multiple frames or windows in your script and pass objects from one context to another via functions, this will be a valid and strong issue. For instance, you can securely check if a given object is in fact an Array using Array.isArray(myObj)


Object.prototype.toString

constructor

duck type：关于鸭子类型（Duck Type）：http://zh.wikipedia.org/wiki/%E9%B8%AD%E5%AD%90%E7%B1%BB%E5%9E%8B

	typeof 100 === "number"
	typeof true === "boolean"
	typeof function() {} === "function"
	typeof(undefined)) === "undefined"
	typeof(new Object()) === "object"
	typeof( [1,2]) === "object"
	typeof(NaN) === "number"
	typeof(null) === "object"

	[1, 2] instanceof Array === true
	new Object() instanceof Array === false

	Object.prototype.toString.apply([]); === "[object Array]";
	Object.prototype.toString.apply(function(){}); === "[object Function]";

	Object.prototype.toString.apply(undefined); === "[object Undefined]"
	Object.prototype.toString.apply(null); === "[object Null]"

但是，在IE6/7/8中，

	Object.prototype.toString.apply(undefined)

以及

	Object.prototype.toString.apply(null)

均返回"[object Object]"

类型检测小结:

typeof

适合基本类型及function检测，遇到null失效（typeof  null 返回的结果是"object"）。

[[Class]]

通过{}.toString拿到，适合内置对象和基元类型，遇到null和undefined失效(IE6\7\8等返回"[object Object]")。

instanceof

适合自定义对象，也可以用来检测原生对象，在不同iframe和window间检测时失效

五、实践

	1 == '1'                            //true
	1 === '1'                           //false
	1 + '2' === '1' + 2                 //true
	 1 + '2' === '1' + new Number(2)    //true
	1 + true === false + 2              //true
	1 + null == undefined  + 1          //false
	'a' - 'b' == 'b' - 'a'              //false
	typeof(typeof('string'))            //true
	[null] instanceof Object            //false
	"test".substring(0 ,1)              // 't'
	{}.toString.apply(new String('str'));
	{}.toString.apply('str');

	5 – "4"                             //1
	5 + "4"                             //54
	+!{}[true]                          //1   ???
	+[1]                                //1   ???
	+[1, 2]                             //NaN
	7-"a"                               //NaN
	7 / 0;                              // Infinity

>
Interaction -> solving problems
Chrome21.0.1180.60m自带V8引擎下的结果：1, 54, 1, 1, NaN, NaN, Infinity
ECMA-262-5 Document reference:
4.3.2 NOTE A primitive value is a datum that is represented directly at the lowest level of the language implementation.
4.3.3 NOTE An object is a collection of properties and has a single prototype object. The prototype may be the null value.
11.6.1 The Addition Operator (+)
7. If Type(lprim) is String or Type(rprim) is String, then
a. Return the String that is the result of concatenating ToString(lprim) followed by ToString(rprim)
8. Return the result of applying the addition operation to ToNumber( lprim) and ToNumber(rprim). See the
Note below 11.6.3.
11.6.2 The Subtracktion Operator (-)
5. Let lnum be ToNumber(lval).
6. Let rnum be ToNumber(rval).

>
关于操作符优先级的参考：
https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Operators/Operator_Precedence
Interaction -> solving problems -> type coercion in equality operators
Chrome21.0.1180.60m自带V8引擎下的结果：54, 5, true, false, true, true, false, false
11.9.3 The Abstract Equality Comparison Algorithm
1. If Type(x) is the same as Type(y), then
a. If Type(x) is Undefined, return true.
b. If Type(x) is Null, return true.
c. If Type(x) is Number, then
i. If x is NaN, return false.
© Ecma International 2011 81
ii. If y is NaN, return false.
iii. If x is the same Number value as y, return true.
iv. If x is +0 and y is -0, return true.
v. If x is -0 and y is +0, return true.
vi. Return false.
d. If Type(x) is String, then return true if x and y are exactly the same sequence of characters (same
length and same characters in corresponding positions). Otherwise, return false.
e. If Type(x) is Boolean, return true if x and y are both true or both false. Otherwise, return false.
f. Return true if x and y refer to the same object. Otherwise, return false.
2. If x is null and y is undefined, return true.
3. If x is undefined and y is null, return true.
4. If Type(x) is Number and Type(y) is String,
return the result of the comparison x == ToNumber(y).
5. If Type(x) is String and Type(y) is Number,
return the result of the comparison ToNumber(x) == y.
6. If Type(x) is Boolean, return the result of the comparison ToNumber(x) == y.
7. If Type(y) is Boolean, return the result of the comparison x == ToNumber(y).
8. If Type(x) is either String or Number and Type(y) is Object,
9.3 ToNumber Conversions
Undefined => NaN
Null => +0
Boolean => 1 | 0


5 + "4"  //9
5 + null  //5
4 == "4.00" //true
4 === "4.00" //false
null == undefined //true
0 == false //true
0 == null  //false
null == false  //false


**offsetWidth,offsetHeight的值包含了content,padding,border的值，而不仅仅是content的值。**

这是非常容易出错的地方。要获得content的宽高值，需要用.style.width或style.height来获取。

**函数声明和变量声明会被提前，而函数表达式中的函数不会。**

例如，

	var add = function(a,b){

	}
中的var add会前置，而其中的function不会前置。

若去掉var add=,则这个function就成了一个函数声明，故而而也会被前置。

那些踩过的坑：

（1）误以为对象有length属性，其实对象是没有length属性的
所以在获取json对象的长度的时候，不能直接用length，而要用

	for(var key in obj){
		if(obj.hasOwnProperty)
	}

（2）动态添加的元素或动态修改的class上绑定事件失效的问题
即错误地采用.click(function(){}) 、.on('click',function(){}这样的方式来对动态生成的元素或动态生成的class上绑定元素。这样的绑定方法只对页面中一开始就有的元素的事件的绑定是有效的。
还有人采用.live(click,function(){})的方式，这在老版本的jQuery中是有效的，但是live() 方法在 jQuery 版本 1.7 中被废弃了,在版本 1.9 中被移除了。所以，最新版本的jQuery显然是无法继续使用live了。
正确的做法应该是采用delegate：

	$('.foldAndUnfoldWrap').delegate('.fold','click',function(){
		//do something
	})
	$('.foldAndUnfoldWrap').delegate('.unfold','click',function(){
		//do something
	})

（3）使用ajaxfileupload插件时，

	$.ajaxFileUpload({
		....
		success : function(data, status){
			var result = JSON.parse(data),
		}
		....
	)
JSON.parse语句在Mac系统的firefox 37.0.2版本中报错说data格式不对，最后发现是因为该data 的末尾被迅雷的插件附加了一个div元素的 html，导致它已不再是json格式，从而JSON.parse(data)时解析出错。最后把这个插件删除了，重启一下该浏览器，就好了。

### DOM详解：

HTML标签并不等同于DOM，它需要经过浏览器的解析才能生成DOM。当页面中所有的HTML标签都转换成DOM元素的时候，就叫做DOM树构建完毕，这个状态就叫domReady。

浏览器引擎的基本渲染流程：解析HTML,构建DOM树（把HTML解析成DOM节点组成DOM树）;解析样式信息，构建渲染树（解析行内样式和外部样式）；布局渲染树（确定DOM节点在屏幕中的位置）；绘制渲染树（把DOM节点绘制到屏幕上）。

WEBKIT主要渲染流程：

HTML经解析并依据DOM规则，生成DOM树，层叠样式表经解析生成样式规则（Style Rules），WEBKIT通过Attachment来连接DOM树和样式规则，生成渲染树，渲染树通过Layout来表示元素的布局，接着遍历渲染树，在页面上绘制渲染树。

window.onload是在页面所有的资源加载完成后才执行，也就是说如果页面中有大量的图片等资源，都需要等到这些资源加载完后才会触发。

DOMReady实现策略：DOMContentLoaded事件是在DOM树构建完成的时候即触发，相比于window.onload事件来说，显然更早。不过，早期的IE对它不兼容。要使用hack办法来兼容之。