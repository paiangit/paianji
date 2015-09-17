# 第五章 CSS之常见兼容性问题总结

踩到的兼容性问题坑：

（1）当父元素的直接子元素或者下级子元素的样式拥有position:relative属性时，在ie7下面，该父元素的overflow:hidden属性就会失效。

解决方法：

我们在IE7内发现子元素会超出父元素设定的高度，即使父元素设置了overflow:hidden。

解决这个bug很简单，在父元素中使用position:relative，即可解决该bug。

（2）IE 6-8不支持event.preventDefault()的问题

解决办法：

	prevDeft: function(ev){
		/*
		 * @ev: event
		 * */
		if ( ev && ev.preventDefault ){
			// 注意:此处的判断条件是ev && ev.preventDefault，而不是ev.preventDefault，因为后者在ie6-8下会报错，因为在这几个浏览器中，preventDefault对象不存在
			ev.preventDefault();
		}
		else {
			window.event.returnValue = false;
		};
	}

（3）使用了jQuery的$('#anode').attr('checked','checked')进行按钮选中设置可以成功更改checked属性，却无法在表单提交时正确地发送该属性的值，服务端无法正确取得其正确value。更换成$('#anode').prop('checked',true)后问题解决。此问题详见 http://stackoverflow.com/questions/15266533/jquery-attrchecked-checked-works-only-once
Use prop('checked', true / false) instead of removeAttr

$('input[name=foo]').prop('checked', true);
$('input[name=foo]').prop('checked', false);

（4）在firefox中，隐藏表单域在首次写入了值之后刷新，发现该隐藏表单域的值仍然是有值的（本来应该是要清空才对），怎么解决？
最后是给该表单隐藏域加了个autocomplete="off" 属性。

（5）关于overflow的兼容性问题

overflow-x和overflow-y只在ie8+有效。

**overflow-x和overflow-y如果值相同，那么和只设置overflow的效果一样。如果overflow-x和overflow-y的值不相同，其中一个是visible，另一个是auto／hidden/scroll,那么visible会被重置为auto。**

**IE7下设置某元素宽度100%，父元素overflow:auto，并固定width:400px;时会出现滚动条。而它在IE8下不会出现滚动条。是因为IE7和IE8宽度是否把滚动条包含在内的机制不同所导致的。**

**让overflow起作用的前提：**

第一，display是非inline

第二，对应方位的尺寸有限制，如width/height/max-width/max-height/absolute拉伸

第三，对于单元格td等，还需要把table设置称为 table-layout:fixed才能让overflow起作用

overflow:visible妙用：

ie7浏览器下，文字越多，按钮两侧padding留白就越大。当给所有按钮添加css样式overflow:visible后，该bug被修复

（6）ie6不支持position:fixed兼容方案

	position:absolute;
	_position:absolute;
	top:50%;
	_top:~"expression(eval(documentElement.scrollTop+documentElement.clientHeight/2))";  //对于不需要编译的myless的属性值，给它加~""

（7）拼接的边线在Chrome中显示正常，而Firefox和IE等浏览器中出现了一像素的偏移

最后发现，问题在于设置了奇数的高度，并对其内部的元素使用了50%，这样就导致算出来的结果在不同浏览器中有1像素的差距。

碰到少量像素偏移问题，可以循着以下路径查找解决办法：

首先看是否有浮动没有清除；

第二检查reset样式有无问题；

第三看是否有元素内容超出了其本来的边界，导致把其它元素挤偏；

第四就是查一查这里提到的这个奇数宽高的里面使用了相对该奇数宽高的50%的问题。

（8）让IE7飙泪的浮动问题

a.含clear样式的浮动元素，在IE7下会出现包裹不正确的问题。
——所以应该避免clear与float样式同时用在一个元素上

b.一系列浮动元素（要最少有四个浮动元素，每个浮动元素的宽度都超出祖先元素的宽度而不得不换行的情况下）的最后两个之间在IE7下会莫名其妙地出现间距。

c.浮动元素最后一个字符重复问题。

d.浮动元素楼梯排列问题

e.浮动元素和非浮动的文本不在同一行的问题
——解决办法，给右对齐的元素添加右浮动，给左对齐的文本添加一个单独的包裹元素，并给该元素添加左浮动


### CSS Hack的使用

了解几个基本的CSS Hack是必要的。虽然使用它们之前，最好先尝试别的办法，实在不行再使用浏览器检测和CSS Hack。下面是几个最基本的：

_property:value; —— (for IE6)
*property:value; —— (for IE6 和 IE7)
property:value\9; ——(for IE6、IE7、IE8和IE9)

CSS Hack书写顺序为：先写非IE浏览器所需样式，其次写IE8/9所需样式，接着是IE7的，再接着才是IE6的。
使用示例：

	.container{
		width:300px;
	    height:32px;
	    background-color:#aaa;/*所有浏览器识别*/
	    background-color:#bbb\9; /*IE6、7、8、9识别*/
	    *background-color:#ccc;/*IE6、7识别*/
	    _background-color:#ddd;/*IE6识别*/
	}

值得注意的是，随着浏览器版本的变化，曾经可用的一些Hack也在失效。所以，建议大家要用的话就只用*和_这两个目前来说比较稳定的Hack，其它的就尽可能不要用了。实在有不好解决的兼容问题，就用如下浏览器条件注释来判断，然后引入对应的CSS来解决吧。

	<!--[if IE]> 所有的IE可识别 <![endif]-->

	<!--[if IE 9]> 仅IE9可识别 <![endif]-->

	<!--[if IE 8]> 仅IE8可识别 <![endif]-->

	<!--[if lt IE 8]> IE8以下版本(不含IE8)可识别 <![endif]-->

	<!--[if lte IE 7]> IE7以及IE7以下版本可识别 <![endif]-->

	<!--[if IE 7]> 仅IE7可识别 <![endif]-->

	<!--[if IE 6]> 仅IE6可识别 <![endif]-->

对于非IE浏览器，基本上所有的兼容问题，都是不应该用Hack的方式来解决的。

### CSS中的单冒号（:）和双冒号（::）的区别

在CSS3中，有:与::的写法经常让人“傻傻分不清楚”。比如，像:before与::before，它们之间到底有什么区别呢？
一言以蔽之，单冒号(:)用于CSS3伪类，双冒号(::)用于CSS3伪元素。其中，双冒号是在当前规范中引入的，用于区分伪类和伪元素。不过浏览器需要同时支持旧的已经存在的伪元素写法（即单冒号的写法），比如:first-line、:first-letter、:before、:after等。也就是说，对于CSS2之前已有的伪元素，比如:before，单冒号和双冒号的写法::before作用是一样的。

所以，如果你的网站只需要兼容webkit、firefox、opera等浏览器，建议对于伪元素采用双冒号的写法，因为它是最新标准。但如果不得不兼容IE浏览器，还是用CSS2的单冒号写法比较安全。
