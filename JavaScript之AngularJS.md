# 第十二章 JavaScript之AngularJS

小猫杯AngularJS学习笔记

将a对象传递到b对象里面去，就叫做a注入b；b对象如果需要用到a对象，就叫b依赖于a。
例如：

	<script>
		var A = function(){
			this.getName = function(){
				return '张三';
			}
		}
	
		var B = function(aObj){
			//依赖
			this.aObj = aObj;
			document.write(aObj.getName());
		}
	
		var a = new A;
	
	              //注入
		var b = new B(a);
	</script>

angularjs表达式：{{}}
angularjs表达式与javascript表达式的不同：


angularjs中的controller其实就是在js中定义的一个类型为函数的变量。
例如：
	var firstController = function($scope){
		$scope.name = 'zhangsan';
	}

$scope 作用域

ng-bind用来替换angularjs表达式{{}}，以避免代码还没加载及执行完成时显示还未填入数据的表达式内容。

当在当前作用域中找不到某model时，会到父级元素的作用域中去找。



$digest()会触发所属scope及其所有子scope的脏检查，脏检查又会触发$watch()。但不建议直接调用$digest()，而应该使用$apply()。$apply()其实不能把信直接送给$digest()，其中间还有$eval门卫把关，如果$apply()带的表达式不合法，$eval会把错误送交给$exceptionHandler，合法时才会触发$digest()，所以更加安全。
例如：

	setInterval(function(){
		$scope.$apply(function(){
			$scope.date = new Date();
			//...这样才会触发脏检查，否则，如果不用$apply()方法的话，则值是会改变，但angularjs并不知道其值变化了，因此页面上的{{date}}自然也就不会跟着更新
		})
	},1000)


$watch()方法
$watch(watchFn,watchAction,deepWatch)
watchFn——是angular表达式或函数的字符串
watchAction(newValue,oldValue,scope)——watchFn发生变化时会被调用
deepWatch——可选的布尔值命令检查被监控的对象的每个属性是否发生变化

$watch()会返回一个函数，想要注销这个$watch()可以使用函数

	//$watch()用于监听一个model，该model每改变一次，都会触发一下$watch()中的第二个参数（watchAction，是一个函数）
	$scope.name = 'zhangsan';
	$scope.count = 0;
	$scope.data = {
		name:'李四'，
		count:20
	}
	$scope.$watch('name',function(newValue,oldValue){
		++$scope.count;
		if($scope.count > 30){
			$scope.name = '已经大于30次了！'；
		}
	});
	
	//$watch()的第三个
	$scope.$watch('data',function(newValue,oldValue){
		++$scope.count;
		if($scope.count > 30){
			$scope.name = '已经大于30次了！'；
		}
	},true);




ng-show:

	ng-show='cart.length';// 当购物车不为空的时候，显示购物车

	//这种方式定义的是全局的控制器
	function firstController($scope){
		$scope.name = 'zhangsan';
	}

	//而下面这种方式定义的是只属于某一模块的控制器，必需在ng-app='模块名'控制的范围内才可以使用该controller
	
	var myApp = angular.module('myApp',[]);
	myApp.controller('firstController',function($scope){
		$scope.name = '张三';
	});


factory
方法和service方法的区别：factory可以返回任何数据，而service方法只能返回对象类型的数据。

过滤器：

	//数字：每三个数字用逗号分隔
	{{1234567 | number}}  输出1,234,567
	//number参数指定保留几位小数输出
	{{123123.4567 | number:3}}  输出123,123.457
	// 货币，默认是$开头，每三位逗号分隔，保留两位小数点
	{{999999 | currency}} 输出$999,999.00
	//更换单位为rmb
	{{999999 | currency:'rmb'}} 输出rmb999,999.00
	//日期：
	{{ .... | date:'medium' }}
	{{ .... | date:'short' }}
	{{ .... | date:'fullDate' }}
	{{ .... | date:'longDate' }}
	{{ .... | date:'mediumDate' }}
	{{ .... | date:'shortDate' }}
	{{ .... | date:'mediumDate' }}
	{{ .... | date:'shortDate' }}
	{{ .... | date:'mediumTime' }}
	{{ .... | date:'shortTime' }}
	{{ .... | date:'y' }} //2014
	{{ .... | date:'yy' }} //14
	{{ .... | date:'yyyy' }} //2014
	{{ .... | date:'M' }} //1
	{{ .... | date:'MM' }} //01
	{{ .... | date:'MMM' }} //Jan
	{{ .... | date:'MMMM' }} // January
	{{ .... | date:'d' }} // 31
	{{ .... | date:'dd' }} // 31
	{{ .... | date:'EEEE' }} // Friday
	{{ .... | date:'EEE' }} // Fri
	{{ .... | date:'HH' }} //11
	{{ .... | date:'y-MM-d h:m:s }} // 2014-03-23 10:23:12
	{{ .... | date:'H' }} //11
	{{ .... | date:'hh' }} //11
	{{ .... | date:'h' }} //11
	{{ .... | date:'mm' }} //12
	{{ .... | date:'m' }} //12
	{{ .... | date:'ss' }} //38
	{{ .... | date:'s' }} //38
	{{ .... | date:'.sss' }} //.685

limitTo

	//参数为正数，显示从头数的几位
	{{[1,2,3,4,5,6,7,8] | limitTo:3 }}  输出[1,2,3]

	//参数为负数，显示从末尾开始数的几位
	{{[1,2,3,4,5,6,7,8] | limitTo:-3 }}  输出[6,7,8]
	
	{{... | lowercase }} 字母转小写
	{{... | uppercase }} 字母转大写
	
	{{ ... | filter:'上海' }}
	{{ ... | filter:{pingyin:'g'} }}  //获得待过滤对象中的pingyin的键值包含'g'的
	
	{{ ... | orderBy: 'pingyin' }} //按拼音进行升序排序
	{{ ... | orderBy: '-pingyin' }} //按拼音进行降序排序
	
	$filter('json')(....)  //将数据输出为json格式，主要在调试中用
	
	//自定义过滤器
	$scope.checkName = function(obj){
		if(obj.pingyin.indexOf('h') === -1)
			return false;
		return true;
	}
	
	{{ ... | filter: checkName }}


正确的使用controller:

controller不应该尝试做太多的事情，它应该仅仅包含单个视图所需要的业务逻辑。

保持controller的简单性，常见办法是抽出那些不属于controllder的工作到service中，在controllder通过依赖注入来使用这些service。

不要在controller中以下的事情：

1、任何类型的DOM操作——controller应该仅仅包含业务逻辑，任何表现逻辑放到controller中，大大地影响了应用逻辑的可测试性。angular为了自动操作（更新）DOM，提供的数据绑定。如果希望执行我们自定义的DOM操作，可以把表现逻辑抽取到directive中。

2、Input formating（输入格式化）——使用angular form controls代替

3、Output filtering（输出格式化）——使用angular filters代替

4、执行无状态或有状态的、controller共享的代码——使用angulat services代替

5、实例化或者管理其它组件的生命周期（例如创建一个服务实例）


	.config(function(APIKEY)){
		console.log(APIKEY);
		console.log('config');
	})
	.run(function(){
		console.log('run');
	});//run在config之后controller等其它服务之前执行
	
	.constant('APIKEY','xxx');//可以注入任何方法
	.value('version','1.0.0');//只能注入controller...service factory

