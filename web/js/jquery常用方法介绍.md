# jquery常用工具方法 #

$.trim方法用于移除字符串头部和尾部多余的空格。

	$.trim('   Hello   ') // Hello

$.contains方法返回一个布尔值，表示某个DOM元素（第二个参数）是否为另一个DOM元素（第一个参数）的下级元素。

	$.contains(document.documentElement, document.body); 
	// true
	$.contains(document.body, document.documentElement); 
	// false

$.each方法用于遍历数组和对象，然后返回原始对象。它接受两个参数，分别是数据集合和回调函数。
	
	$.each([ 52, 97 ], function( index, value ) {
	  	console.log( index + ": " + value );
	});
	// 0: 52 
	// 1: 97 
	var obj = {
	  	p1: "hello",
	  	p2: "world"
	};
	$.each( obj, function( key, value ) {
	  	console.log( key + ": " + value );
	});
	// p1: hello
	// p2: world

需要注意的，jQuery对象实例也有一个each方法（$.fn.each），两者的作用差不多。

$.map方法也是用来遍历数组和对象，但是会返回一个新对象。

	var a = ["a", "b", "c", "d", "e"];
	a = $.map(a, function (n, i){
	  	return (n.toUpperCase() + i);
	});
	console.log(a);
	// ["A0", "B1", "C2", "D3", "E4"]
$.inArray方法返回一个值在数组中的位置（从0开始）。如果该值不在数组中，则返回-1。

	var a = [1,2,3,4];
	$.inArray(4,a) // 3

$.extend方法用于将多个对象合并进第一个对象。

	var o1 = {p1:'a',p2:'b'};
	var o2 = {p1:'c'};
	$.extend(o1,o2);
	console.log(o1.p1);// "c"

$.extend的另一种用法是生成一个新对象，用来继承原有对象。这时，它的第一个参数应该是一个空对象。

	var o1 = {p1:'a',p2:'b'};
	var o2 = {p1:'c'};
	var o = $.extend({},o1,o2);
	// Object {p1: "c", p2: "b"}

默认情况下，extend方法生成的对象是“浅拷贝”，也就是说，如果某个属性是对象或数组，那么只会生成指向这个对象或数组的指针，而不会复制值。如果想要“深拷贝”，可以在extend方法的第一个参数传入布尔值true。

	var o1 = {p1:['a','b']};
	var o2 = $.extend({},o1);
	var o3 = $.extend(true,{},o1);
	o1.p1[0]='c';
	o2.p1 // ["c", "b"]
	o3.p1 // ["a", "b"] 

上面代码中，o2是浅拷贝，o3是深拷贝。结果，改变原始数组的属性，o2会跟着一起变，而o3不会。

$.proxy方法类似于ECMAScript 5的bind方法，可以绑定函数的上下文（也就是this对象）和参数，返回一个新函数。
jQuery.proxy()的主要用处是为回调函数绑定上下文对象。
复制代码 代码如下:

	var o = {
	    type: "object",
	    test: function(event) {
	        console.log(this.type);
	    }
	};
	$("#button").on("click", o.test) // 无输出
	$("#button").on("click", $.proxy(o.test, o)) // object

上面的代码中，第一个回调函数没有绑定上下文，所以结果为空，没有任何输出；第二个回调函数将上下文绑定为对象o，结果就为object。这个例子的另一种等价的写法是：

	$("#button").on( "click", $.proxy(o, test)) 

上面代码的$.proxy(o, test)的意思是，将o的方法test与o绑定。这个例子表明，proxy方法的写法主要有两种。jQuery.proxy(function, context)或者jQuery.proxy(context, name)。第一种写法是为函数（function）指定上下文对象（context），第二种写法是指定上下文对象（context）和它的某个方法名（name）。
再看一个例子。正常情况下，下面代码中的this对象指向发生click事件的DOM对象。

	$('#myElement').click(function() {
	    $(this).addClass('aNewClass');
	});

如果我们想让回调函数延迟运行，使用setTimeout方法，代码就会出错，因为setTimeout使得回调函数在全局环境运行，this将指向全局对象。

	$('#myElement').click(function() {
	    setTimeout(function() {
	        $(this).addClass('aNewClass');
	    }, 1000);
	});

上面代码中的this，将指向全局对象window，导致出错。
这时，就可以用proxy方法，将this对象绑定到myElement对象。

	$('#myElement').click(function() {
	    setTimeout($.proxy(function() {
	        $(this).addClass('aNewClass'); 
	    }, this), 1000);
	});

$.data方法可以用来在DOM节点上储存数据。

	// 存入数据
	$.data(document.body, "foo", 52 );
	// 读取数据
	$.data(document.body, "foo");
	// 读取所有数据
	$.data(document.body);

上面代码在网页元素body上储存了一个键值对，键名为“foo”，键值为52。

$.removeData方法用于移除$.data方法所储存的数据。

	$.data(div, "test1", "VALUE-1");
	$.removeData(div, "test1");

$.parseHTML方法用于将字符串解析为DOM对象。

	var html = $.parseHTML("hello, <b>my name is</b> jQuery.");
$.parseJSON方法用于将JSON字符串解析为JavaScript对象，作用与原生的JSON.parse()类似。但是，jQuery没有提供类似JSON.stringify()的方法，即不提供将JavaScript对象转为JSON对象的方法。

	var obj = $.parseJSON('{"name": "John"}');

$.parseXML方法用于将字符串解析为XML对象。

	var xml = "<rss version='2.0'><channel><title>RSS Title</title></channel></rss>";
	var xmlDoc = $.parseXML(xml);

$.makeArray方法将一个类似数组的对象，转化为真正的数组。

	var a = $.makeArray(document.getElementsByTagName("div"));

$.merge方法用于将一个数组（第二个参数）合并到另一个数组（第一个参数）之中。

	var a1 = [0,1,2];
	var a2 = [2,3,4];
	$.merge(a1, a2);
	a1
	// [0, 1, 2, 2, 3, 4]

$.now方法返回当前时间距离1970年1月1日00:00:00 UTC对应的毫秒数，等同于(new Date).getTime()。
	
	$.now()
	// 1388212221489

> 判断数据类型的方法

jQuery提供一系列工具方法，用来判断数据类型，以弥补JavaScript原生的typeof运算符的不足。以下方法对参数进行判断，返回一个布尔值。

jQuery.isArray()：是否为数组。

jQuery.isEmptyObject()：是否为空对象（不含可枚举的属性）。

jQuery.isFunction()：是否为函数。

jQuery.isNumeric()：是否为数组。

jQuery.isPlainObject()：是否为使用“{}”或“new Object”生成的对象，而不是浏览器原生提供的对象。

jQuery.isWindow()：是否为window对象。

jQuery.isXMLDoc()：判断一个DOM节点是否处于XML文档之中。

	下面是一些例子。
	
	$.isEmptyObject({}) // true
	$.isPlainObject(document.location) // false
	$.isWindow(window) // true
	$.isXMLDoc(document.body) // false

除了上面这些方法以外，还有一个$.type方法，可以返回一个变量的数据类型。它的实质是用Object.prototype.toString方法读取对象内部的[[Class]]属性（参见《标准库》的Object对象一节）。

	$.type(/test/) // "regexp"

> Ajax操作

$.ajax jQuery对象上面还定义了Ajax方法（$.ajax()），用来处理Ajax操作。调用该方法后，浏览器就会向服务器发出一个HTTP请求。
$.ajax()的用法有多种，最常见的是提供一个对象参数。

	$.ajax({
	  async: true,
	  url: '/url/to/json',
	  type: 'GET',
	  data : { id : 123 },
	  dataType: 'json',
	  timeout: 30000,
	  success: successCallback,
	  error: errorCallback,
	  complete: completeCallback
	})
	function successCallback(json) {
	    $('<h1/>').text(json.title).appendTo('body');
	}
	function errorCallback(xhr, status){
	    console.log('出问题了！');
	}
	function completeCallback(xhr, status){
	    console.log('Ajax请求已结束。');
	}



> 上面代码的对象参数有多个属性，含义如下：

- async：该项默认为true，如果设为false，则表示发出的是同步请求。
- cache: 该项默认为true，如果设为false，则浏览器不缓存返回服务器返回的数据。注意，浏览器本身就不会缓存POST请求返回的数据，所以即使设为false，也只对HEAD和GET请求有效。
- url：服务器端网址。这是唯一必需的一个属性，其他属性都可以省略。
- type：向服务器发送信息所使用的HTTP动词，默认为GET，其他动词有POST、PUT、DELETE。
- dataType：向服务器请求的数据类型，可以设为text、html、script、json、jsonp和xml。
- data：向服务器发送的数据，如果使用GET方法，此项将转为查询字符串，附在网址的最后。
- success：请求成功时的回调函数，函数参数为服务器传回的数据、状态信息、发出请求的原始对象。
- timeout: 等待的最长毫秒数。如果过了这个时间，请求还没有返回，则自动将请求状态改为失败。
- error：请求失败时的回调函数，函数参数为发出请求的原始对象以及返回的状态信息。
- complete：不管请求成功或失败，都会执行的回调函数，函数参数为发出请求的原始对象以及返回的状态信息。

这些参数之中，url可以独立出来，作为ajax方法的第一个参数。也就是说，上面代码还可以写成下面这样。


	$.ajax('/url/to/json',{
	  	type: 'GET',
	  	dataType: 'json',
	  	success: successCallback,
	  	error: errorCallback
	})

简便写法

- ajax方法还有一些简便写法。
- $.get()：发出GET请求。
- $.getScript()：读取一个JavaScript脚本文件并执行。
- $.getJSON()：发出GET请求，读取一个JSON文件。
- $.post()：发出POST请求。
- $.fn.load()：读取一个html文件，并将其放入当前元素之中。


一般来说，这些简便方法依次接受三个参数：url、数据、成功时的回调函数。

$.get()，$.post()这两个方法分别对应HTTP的GET方法和POST方法。

	$.get('/data/people.html', function(html){
	  	$('#target').html(html);
	});
	$.post('/data/save', {name: 'Rebecca'}, function (resp){
	  	console.log(JSON.parse(resp));
	});

get方法接受两个参数，分别为服务器端网址和请求成功后的回调函数。post方法在这两个参数中间，还有一个参数，表示发给服务器的数据。
上面的post方法对应的ajax写法如下。

	$.ajax({
	    type: 'POST',
	    url: '/data/save',
	    data: {name: 'Rebecca'},
	    dataType: 'json',
	    success: function (resp){
	      console.log(JSON.parse(resp));
	    }
	});

$.getJSON() ajax方法的另一个简便写法是getJSON方法。当服务器端返回JSON格式的数据，可以用这个方法代替$.ajax方法。

	$.getJSON('url/to/json', {'a': 1}, function(data){
	    console.log(data);
	});

上面的代码等同于下面的写法。

	$.ajax({
	  dataType: "json",
	  url: '/url/to/data',
	  data: {'a': 1},
	  success: function(data){
	    console.log(data);
	  }
	});
（3）$.getScript()
$.getScript方法用于从服务器端加载一个脚本文件。

	$.getScript('/static/js/myScript.js', function() {
	    functionFromMyScript();
	});

上面代码先从服务器加载myScript.js脚本，然后在回调函数中执行该脚本提供的函数。
getScript的回调函数接受三个参数，分别是脚本文件的内容，HTTP响应的状态信息和ajax对象实例。

$.getScript( "ajax/test.js", function (data, textStatus, jqxhr){
  console.log( data ); // test.js的内容
  console.log( textStatus ); // Success
  console.log( jqxhr.status ); // 200
});

getScript是ajax方法的简便写法，因此返回的是一个deferred对象，可以使用deferred接口。
复制代码 代码如下:

jQuery.getScript("/path/to/myscript.js")
    .done(function() {
        // ...
    })
    .fail(function() {
        // ...
});

$.fn.load不是jQuery的工具方法，而是定义在jQuery对象实例上的方法，用于获取服务器端的HTML文件，将其放入当前元素。由于该方法也属于ajax操作，所以放在这里一起讲。

	$('#newContent').load('/foo.html');

$.fn.load方法还可以指定一个选择器，将远程文件中匹配选择器的部分，放入当前元素，并指定操作完成时的回调函数。

	$('#newContent').load('/foo.html #myDiv h1:first',
	    function(html) {
	        console.log('内容更新！');
        }
	});
	上面代码只加载foo.html中匹配“#myDiv h1:first”的部分，加载完成后会运行指定的回调函数。



> Ajax事件

jQuery提供以下一些方法，用于指定特定的AJAX事件的回调函数。

- ajaxComplete()：ajax请求完成。
- ajaxError()：ajax请求出错。
- ajaxSend()：ajax请求发出之前。
- ajaxStart()：第一个ajax请求开始发出，即没有还未完成ajax请求。
- ajaxStop()：所有ajax请求完成之后。
- ajaxSuccess()：ajax请求成功之后。
下面是示例。
	
	$('#loading_indicator')
	.ajaxStart(function (){$(this).show();})
	.ajaxStop(function (){$(this).hide();});

返回值
ajax方法返回的是一个deferred对象，可以用then方法为该对象指定回调函数。

	$.ajax({
	  url: '/data/people.json',
	  dataType: 'json'
	}).then(function (resp){
	  console.log(resp.people);
	})

> JSONP
由于浏览器存在“同域限制”，ajax方法只能向当前网页所在的域名发出HTTP请求。但是，通过在当前网页中插入script元素,可以向不同的域名发出GET请求，这种变通方法叫做JSONP（JSON with Padding）。ajax方法可以发出JSONP请求，方法是在对象参数中指定dataType为JSONP。

	$.ajax({
	  url: '/data/search.jsonp',
	  data: {q: 'a'},
	  dataType: 'jsonp',
	  success: function(resp) {
	    $('#target').html('Results: ' + resp.results.length);
	  }
	});

JSONP的通常做法是，在所要请求的URL后面加在回调函数的名称。ajax方法规定，如果所请求的网址以类似“callback=?”的形式结尾，则自动采用JSONP形式。所以，上面的代码还可以写成下面这样。

	$.getJSON('/data/search.jsonp?q=a&callback=?',
	  function(resp) {
	    $('#target').html('Results: ' + resp.results.length);
	  }
	);

# jquery对象常用方法： #

$(”p”).addClass(css中定义的样式类型); 给某个元素添加样式  

$(”img”).attr({src:”test.jpg”,alt:”test Image”}); 给某个元素添加属性/值，参数是map  

$(”img”).attr(”src”,”test.jpg”); 给某个元素添加属性/值  

$(”img”).attr(”title”, function() { return this.src }); 给某个元素添加属性/值  

$(”元素名称”).html(); 获得该元素内的内容（元素，文本等）  

$(”元素名称”).html(”<b>new stuff</b>”); 给某元素设置内容  

$(”元素名称”).removeAttr(”属性名称”) 给某元素删除指定的属性以及该属性的值 
 
$(”元素名称”).removeClass(”class”) 给某元素删除指定的样式  

$(”元素名称”).text(); 获得该元素的文本  

$(”元素名称”).text(value); 设置该元素的文本值为value  

$(”元素名称”).toggleClass(class) 当元素存在参数中的样式的时候取消,如果不存在就设置此样式 
 
$(”input元素名称”).val(); 获取input元素的值  

$(”input元素名称”).val(value); 设置input元素的值为value  
  
$(”元素名称”).after(content); 在匹配元素后面添加内容  

$(”元素名称”).append(content); 将content作为元素的内容插入到该元素的后面  

$(”元素名称”).appendTo(content); 在content后接元素  

$(”元素名称”).before(content); 与after方法相反  

$(”元素名称”).clone(布尔表达式) 当布尔表达式为真时，克隆元素（无参时，当作true处理）  

$(”元素名称”).empty() 将该元素的内容设置为空  

$(”元素名称”).insertAfter(content); 将该元素插入到content之后  

$(”元素名称”).insertBefore(content); 将该元素插入到content之前  

$(”元素”).prepend(content); 将content作为该元素的一部分，放到该元素的最前面 
 
$(”元素”).prependTo(content); 将该元素作为content的一部分，放content的最前面  

$(”元素”).remove(); 删除所有的指定元素  

$(”元素”).remove(”exp”); 删除所有含有exp的元素  

$(”元素”).wrap(”html”); 用html来包围该元素  

$(”元素”).wrap(element); 用element来包围该元素  

# jquery事件 #

$(document).ready(fu)页面元素加载完毕以后执行的函数，按照按照fn定义的顺序自上而下顺序来执行。  

	$(document).ready(function(){
		console.log("页面初始化完成啦！");
	});

bind( type, [data], fn ) 为每一个匹配元素的特定事件（像click）绑定一个或多个事件处理器函数。可能的事件属性有：blur, focus, load, resize, scroll, unload, click, dblclick, mousedown, mouseup, mousemove,  
mouseover, mouseout, mouseenter, mouseleave, change, select, submit, keydown, keypress,  
keyup, error  

	$("button").bind("click",function(){
		  alert("1");
	});

one( type, [data], fn ) 为每一个匹配元素的特定事件（像click）绑定一个或多个事件处理器函数。在每个对  
象上，这个事件处理函数只会被执行一次。其他规则与bind()函数相同。  

  	// 执行后one()会立即移除绑定的事件处理函数
	$("#btn").one("click", function(){
	    alert("只弹出一次提示框!");     
	});

	// 在n1元素上为所有后代p元素的click事件绑定事件处理函数
	$("#n1").one("click", "p", function(event){
	    alert( $(this).text() );
	});
  
trigger( type, [data] ) 在每一个匹配的元素上触发某类事件。

	 $("button").click(function(){
	  	$("input").trigger("select");
	});
  
triggerHandler( type, [data] ) 方法触发被选元素的指定事件类型。但不会执行浏览器默认动作，也不会产生事件冒泡

	$("button").click(function(){
	  	$("input").triggerHandler("select");
	});

与 trigger() 方法相比的不同之处：

- 它不会引起事件（比如表单提交）的默认行为
- .trigger() 会操作 jQuery 对象匹配的所有元素，而 .triggerHandler() 只影响第一个匹配元素。
- 由 .triggerHandler() 创建的事件不会在 DOM 树中冒泡；如果目标元素不直接处理它们，则不会发生任何事情。
- 该方法的返回的是事件处理函数的返回值，而不是具有可链性的 jQuery 对象。此外，如果没有处理程序被触发，则这个方法返回 undefined。

unbind( [type], [data] ) 反绑定，从每一个匹配的元素中删除绑定的事件。  

	$(”p”).unbind() 移除所有段落上的所有绑定的事件  
	$(”p”).unbind( “click” ) 移除所有段落上的click事件  

hover( over, out ) over,out都是方法, 当鼠标移动到一个匹配的元素上面时，会触发指定的第一个函数。当鼠标移出这个元素时，会触发指定的第二个函数。 
 
	$(”p”).hover(function(){  
			$(this).addClass(”over”);  
		},  
		function(){  
			$(this).addClass(”out”);  
		}  
	);  
  
  
toggle( fn, fn ) 如果点击了一个匹配的元素，则触发指定的第一个函数，当再次点击同一元素时，则触发指定的第二个函数。  

	$(”p”).toggle(function(){  
			$(this).addClass(”selected”);  
		},  
		function(){  
			$(this).removeClass(”selected”);  
		}  
	);  

jQuery不支持form元素的reset事件

blur( ) 元素失去焦点 a, input, textarea, button, select, label, map, area  

change( ) 用户改变域的内容 input, textarea, select  

click( ) 鼠标点击某个对象 几乎所有元素  

dblclick( ) 鼠标双击某个对象 几乎所有元素  

error( ) 当加载文档或图像时发生某个错误 window, img  

focus( ) 元素获得焦点 a, input, textarea, button, select, label, map, area  

keydown( ) 某个键盘的键被按下 几乎所有元素  

keypress( ) 某个键盘的键被按下或按住 几乎所有元素  

keyup( ) 某个键盘的键被松开 几乎所有元素  

load( fn ) 某个页面或图像被完成加载 window, img  

mousedown( fn ) 某个鼠标按键被按下 几乎所有元素  

mousemove( fn ) 鼠标被移动 几乎所有元素  

mouseout( fn ) 鼠标从某元素移开 几乎所有元素  

mouseover( fn ) 鼠标被移到某元素之上 几乎所有元素  

mouseup( fn ) 某个鼠标按键被松开 几乎所有元素  

resize( fn ) 窗口或框架被调整尺寸 window, iframe, frame  

scroll( fn ) 滚动文档的可视部分时 window  

select( ) 文本被选定 document, input, textarea  

submit( ) 提交按钮被点击 form  

unload( fn ) 用户退出页面 window  