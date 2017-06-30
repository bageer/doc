# JS常用方法总结 #

**9个常规函数**

alert函数：显示一个警告对话框，包括一个OK按钮。

confirm函数：显示一个确认对话框，包括OK、Cancel按钮。

eval函数：计算表达式的结果。
	
	 var str = "{'name':"123"}";str.name; var json = eval(str); json.name;

isNaN函数：测试是(true)否(false)不是一个数字。

parseFloat函数：将字符串转换成符点数字形式。

parseInt函数：将符串转换成整数数字形式(可指定几进制)。

prompt函数：显示一个输入对话框，提示等待用户输入。

escape函数：将字符转换成Unicode码。

unescape函数：解码由escape函数编码的字符。

**Array对象4个数组函数**

> 常用属性

length属性：返回数组的长度。

> 常用方法

push和pop

push函数：把某一个元素放到数组里；pop函数：把某一个元素移除数组。
	
	var arr = ["Lily","lucy","Tom"];
	var count = arr.push("Jack","Sean");
	console.log(count); // 5
	console.log(arr); // ["Lily", "lucy", "Tom", "Jack", "Sean"]
	var item = arr.pop();
	console.log(item); // Sean
	console.log(arr); // ["Lily", "lucy", "Tom", "Jack"]

indexOf和lastIndexOf

indexOf：接收两个参数：要查找的项和（可选的）表示查找起点位置的索引。其中， 从数组的开头（位置 0）开始向后查找。 
lastIndexOf：接收两个参数：要查找的项和（可选的）表示查找起点位置的索引。其中， 从数组的末尾开始向前查找。这两个方法都返回要查找的项在数组中的位置，或者在没找到的情况下返回-1。在比较第一个参数与数组中的每一项时，会使用全等操作符。

	var arr = [1,3,5,7,7,5,3,1];
	console.log(arr.indexOf(5)); //2
	console.log(arr.lastIndexOf(5)); //5
	console.log(arr.indexOf(5,2)); //2
	console.log(arr.lastIndexOf(5,4)); //2
	console.log(arr.indexOf("5")); //-1

forEach：对数组进行遍历循环，对数组中的每一项运行给定函数。这个方法没有返回值。参数都是function类型，默认有传参，参数分别为：遍历的数组内容；第对应的数组索引，数组本身。

	var arr = [1, 2, 3, 4, 5];
	arr.forEach(function(x, index, a){
		console.log(x + '|' + index + '|' + (a === arr));
	});
	// 输出为：
	// 1|0|true
	// 2|1|true
	// 3|2|true
	// 4|3|true
	// 5|4|true

join函数：设置分隔符连接数组元素为一个字符串。

	var arr = [1,2,3];
	console.log(arr.join(",")); // 1,2,3
	console.log(arr.join("-")); // 1-2-3
	console.log(arr); // [1, 2, 3]（原数组不变）

shift 和 unshift

shift()：删除原数组第一项，并返回删除元素的值；如果数组为空则返回undefined ；unshift:将参数添加到原数组开头，并返回数组的长度 。

	var arr = ["Lily","lucy","Tom"];
	var count = arr.unshift("Jack","Sean");
	console.log(count); // 5
	console.log(arr); //["Jack", "Sean", "Lily", "lucy", "Tom"]
	var item = arr.shift();
	console.log(item); // Jack
	console.log(arr); // ["Sean", "Lily", "lucy", "Tom"]
slice：返回从原数组中指定开始下标到结束下标之间的项组成的新数组。slice()方法可以接受一或两个参数，即要返回项的起始和结束位置。在只有一个参数的情况下， slice()方法返回从该参数指定位置开始到当前数组末尾的所有项。如果有两个参数，该方法返回起始和结束位置之间的项——但不包括结束位置的项。

	var arr = [1,3,5,7,9,11];
	var arrCopy = arr.slice(1);
	var arrCopy2 = arr.slice(1,4);
	var arrCopy3 = arr.slice(1,-2);
	var arrCopy4 = arr.slice(-4,-1);
	console.log(arr); //[1, 3, 5, 7, 9, 11](原数组没变)
	console.log(arrCopy); //[3, 5, 7, 9, 11]
	console.log(arrCopy2); //[3, 5, 7]
	console.log(arrCopy3); //[3, 5, 7]
	console.log(arrCopy4); //[5, 7, 9]

splice：很强大的数组方法，它有很多种用法，可以实现删除、插入和替换。
删除：可以删除任意数量的项，只需指定 2 个参数：要删除的第一项的位置和要删除的项数。例如， splice(0,2)会删除数组中的前两项。
插入：可以向指定位置插入任意数量的项，只需提供 3 个参数：起始位置、 0（要删除的项数）和要插入的项。例如，splice(2,0,4,6)会从当前数组的位置 2 开始插入4和6。
替换：可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定 3 个参数：起始位置、要删除的项数和要插入的任意数量的项。插入的项数不必与删除的项数相等。例如，splice (2,1,4,6)会删除当前数组位置 2 的项，然后再从位置 2 开始插入4和6。

	var arr = [1,3,5,7,9,11];
	var arrRemoved = arr.splice(0,2);
	console.log(arr); //[5, 7, 9, 11]
	console.log(arrRemoved); //[1, 3]
	var arrRemoved2 = arr.splice(2,0,4,6);
	console.log(arr); // [5, 7, 4, 6, 9, 11]
	console.log(arrRemoved2); // []
	var arrRemoved3 = arr.splice(1,1,2,4);
	console.log(arr); // [5, 2, 4, 4, 6, 9, 11]
	console.log(arrRemoved3); //[7]


reverse函数：将数组元素顺序颠倒。

filter：“过滤”功能，数组中的每一项运行给定函数，返回满足过滤条件组成的数组。
	
	var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
	var arr2 = arr.filter(function(x, index) {
		return index % 3 === 0 || x >= 8;
	}); 
	console.log(arr2); //[1, 4, 7, 8, 9, 10]

map：指“映射”，对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。

	var arr = [1, 2, 3, 4, 5];
	var arr2 = arr.map(function(item){
		return item*item;
	});
	console.log(arr2); //[1, 4, 9, 16, 25]

sort函数：将数组元素重新排序，在排序时，sort()方法会调用每个数组项的 toString()转型方法，然后比较得到的字符串，以确定如何排序。即使数组中的每一项都是数值， sort()方法比较的也是字符串，因此会出现以下的这种情况：

	var arr1 = ["a", "d", "c", "b"];
	console.log(arr1.sort()); // ["a", "b", "c", "d"]
	arr2 = [13, 24, 51, 3];
	console.log(arr2.sort()); // [13, 24, 3, 51]
	console.log(arr2); // [13, 24, 3, 51](元数组被改变)

为了解决上述问题，sort()方法可以接收一个比较函数作为参数，以便我们指定哪个值位于哪个值的前面。比较函数接收两个参数，如果第一个参数应该位于第二个之前则返回一个负数，如果两个参数相等则返回 0，如果第一个参数应该位于第二个之后则返回一个正数。以下就是一个简单的比较函数：

	function compare(value1, value2) {
		if (value1 < value2) {
			return -1;
		} else if (value1 > value2) {
			return 1;
		} else {
			return 0;
		}
	}
	arr2 = [13, 24, 51, 3];
	console.log(arr2.sort(compare)); // [3, 13, 24, 51]

every：判断数组中每一项都是否满足条件，只有所有项都满足条件，才会返回true。

	var arr = [1, 2, 3, 4, 5];
		var arr2 = arr.every(function(x) {
	return x < 10;
	}); 
	console.log(arr2); //true
	var arr3 = arr.every(function(x) {
		return x < 3;
	}); 
	console.log(arr3); // false

some：判断数组中是否存在满足条件的项，只要有一项满足条件，就会返回true。
	
	var arr = [1, 2, 3, 4, 5];
	var arr2 = arr.some(function(x) {
		return x < 3;
	}); 
	console.log(arr2); //true
	var arr3 = arr.some(function(x) {
		return x < 1;
	}); 
	console.log(arr3); // false

reduce和 reduceRight

这两个方法都会实现迭代数组的所有项，然后构建一个最终返回的值。reduce()方法从数组的第一项开始，逐个遍历到最后。而 reduceRight()则从数组的最后一项开始，向前遍历到第一项。
这两个方法都接收两个参数：一个在每一项上调用的函数和（可选的）作为归并基础的初始值。
传给 reduce()和 reduceRight()的函数接收 4 个参数：前一个值、当前值、项的索引和数组对象。这个函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代发生在数组的第二项上，因此第一个参数是数组的第一项，第二个参数就是数组的第二项。

	下面代码用reduce()实现数组求和，数组一开始加了一个初始值10。
	var values = [1,2,3,4,5];
	var sum = values.reduceRight(function(prev, cur, index, array){
		return prev + cur;
	},10);
	console.log(sum); //25
**Data对象20个日期函数**

getDate函数：返回日期的“日”部分，值为1～31。

getDay函数：返回星期，值为0～6，0表示星期日。

getHours函数：返回日期的“小时”部分，值为0～23。

getMinutes函数：返回日期的“分钟”部分，值为0～59。

getMonth函数：返回日期的“月”部分，值为0～11。

getSeconds函数：返回日期的“秒”部分，值为0～59。

getTime函数：返回系统时间。

getTimezoneOffset函数：返回此地区的时差(当地时间与GMT格林威治标准时间的地区时差)，单位为分钟。

getYear函数：返回日期的“年”部分。返回值以1900年为基数，如1999年为99。

parse函数：返回从1970年1月1日零时整算起的毫秒数(当地时间)。

setDate函数：设定日期的“日”部分，值为0～31。

setHours函数：设定日期的“小时”部分，值为0～23。

setMinutes函数：设定日期的“分钟”部分，值为0～59。

setMonth函数：设定日期的“月”部分，值为0～11。其中0表示1月，...，11表示12月。

setSeconds函数：设定日期的“秒”部分，值为0～59。

setTime函数：设定时间。时间数值为1970年1月1日零时整算起的毫秒数。

setYear函数：设定日期的“年”部分。

toGMTString函数：转换日期成为字符串，为GMT格林威治标准时间。

setLocaleString函数：转换日期成为字符串，为当地时间。

UTC函数：返回从1970年1月1日零时整算起的毫秒数(GMT)。

**Math对象的属性和函数**

主要属性有

Math.e(e自然对数)

Math.LN2（2的自然对数)

Math.LN10(10的自然对数)

Math.LOG2E(e的对数，底数为2)

Math.LOG10E(e的对数，底数为10)

Math.PI(π圆周率)

Math.SQRT1_2(1/2的平方根值)

Math.SQRT2(2的平方根值)。

函数有18个：

abs函数：Math.abs(以下同)，返回一个数字的绝对值。

floor函数：返回一个数字的最大整数值(小于或等于)。

max函数：返回两个数的最大值。

min函数：返回两个数的最小值。

pow函数：返回一个数字的乘方值。

random函数：返回一个0～1的随机数值。

round函数：返回一个数字的四舍五入值，类型是整数。

cos函数：返回一个数字的反余弦值，结果为0～π弧度(radians)。

asin函数：返回一个数字的反正弦值，结果为-π/2～π/2弧度。

atan函数：返回一个数字的反正切值，结果为-π/2～π/2弧度。

atan2函数：返回一个坐标的极坐标角度值。

ceil函数：返回一个数字的最小整数值(大于或等于)。

cos函数：返回一个数字的余弦值，结果为-1～1。

exp函数：返回e(自然对数)的乘方值。

log函数：自然对数函数，返回一个数字的自然对数(e)值。

sin函数：返回一个数字的正弦值，结果为-1～1。

qrt函数：返回一个数字的平方根值。

tan函数：返回一个数字的正切值。

**String对象20个字符串函数**
concat将两个或多个字符的文本组合起来，返回一个新的字符串。

	var a = "hello";
	var b = ",world";
	var c = a.concat(b);
	alert(c);
	//c = "hello,world"

charAt函数：返回字符串中指定的某个字符。

	var get_char = a.charAt(0);
	//get_char = "h"

indexOf函数：返回字符串中第一个查找到的下标index，从左边开始查找,查找不到返回-1。

	var index1 = a.indexOf("l");
	//index1 = 2
	var index2 = a.indexOf("l",3);
	//index2 = 3

lastIndexOf函数：返回字符串中第一个查找到的下标index，从右边开始查找。

	var a = "hell0";
	var index1 = lastIndexOf('l');
	//index1 = 3
	var index2 = lastIndexOf('l',2)
	//index2 = 2
match 检查一个字符串匹配一个正则表达式内容，如果么有匹配返回 null。

	var a = "hell0";
	var re = new RegExp(/^/w+$/);
	var is_alpha1 = a.match(re);
	//is_alpha1 = "hello"
	var is_alpha2 = b.match(re);
	//is_alpha2 = null

length函数：返回字符串的长度。

	var len = a.length();
	//len = 5

substring函数：返回字符串的一个子串，传入参数是起始位置和结束位置。

	var a = "hell0";
	var sub_string1 = a.substring(1);
	//sub_string1 = "ello"
	var sub_string2 = a.substring(1,4);
	//sub_string2 = "ell"

substr返回字符串的一个子串，传入参数是起始位置和长度

	var sub_string1 = a.substr(1);
	//sub_string1 = "ello"
	var sub_string2 = a.substr(1,4);
	//sub_string2 = "ello"

replace用来查找匹配一个正则表达式的字符串，然后使用新字符串代替匹配的字符串。
	
	var result1 = a.replace(re,"Hello");
	//result1 = "Hello"
	var result2 = b.replace(re,"Hello");
	//result2 = ",world"

search执行一个正则表达式匹配查找。如果查找成功，返回字符串中匹配的索引值。否则返回 -1 。

	var index1 = a.search(re);
	//index1 = 0
	var index2 = b.search(re);
	//index2 = -1

slice提取字符串的一部分，并返回一个新字符串（与 substring 相同）。

	var sub_string1 = a.slice(1);
	//sub_string1 = "ello"
	var sub_string2 = a.slice(1,4);
	//sub_string2 = "ell"

split通过将字符串划分成子串，将一个字符串做成一个字符串数组。

	var arr1 = a.split("");
	//arr1 = [h,e,l,l,o]

toLowerCase函数：将字符串转换为小写。

	var lower_string = a.toLowerCase();
	//lower_string = "hello"

toUpperCase函数：将字符串转换为大写。

	var upper_string = a.toUpperCase();
	//upper_string = "HELLO"

**原生JS操作dom元素**

JS选取DOM元素的方法

注意：原生JS选取DOM元素比使用jQuery类库选取要快很多

通过ID选取元素

	document.getElementById('myid');

通过CLASS选取元素

	document.getElementsByClassName('myclass')[0];

通过标签选取元素

	document.getElementsByTagName('mydiv')[0];

通过NAME属性选取元素（常用于表单）

	document.getElementsByName('myname')[0];

JS修改CSS样式

	document.getElementById('myid').style.display = 'none';

JS修改CLASS属性

	document.getElementById('myid').className = 'active';
	如果有多个CLASS属性，即用空格隔开
	document.getElementById('myid').className = 'active div-1';

> 移除该元素上的所有CLASS

	document.getElementById('myid').className = '';

注意：使用classList会优于使用className

	document.getElementById('myid').classList.item(0);//item为类名的索引
	
	document.getElementById('myid').classList.length;//只读属性
	
	document.getElementById('myid').classList.add('newClass');//添加class
	
	document.getElementById('myid').classList.remove('newClass');//移除class
	document.getElementById('myid').classList.toggle('newClass');//切换，有则移除，没有则添加
	
	document.getElementById('myid').classList.contains('newClass');//判断是否存在该class

JS修改文本

	document.getElementById('myid').innerHTML = '123';

JS创建元素并向其中追加文本

	var newdiv = document.createElement('div');
	
	var newtext = document.createTextNode('123');
	
	newdiv.appendChild(newtext);
	
	document.body.appendChild(newdiv);

同理：removeChild()移除节点，并返回节点

cloneNode()复制节点

insertBefore()插入节点（父节点内容的最前面）

注意：insertBefore()有两个参数，第一个是插入的节点，第二个是插入的位置

例子：

	var list = document.getElementById('myList');
	
	list.insertBefore(newItem,list.childNodes[1]);

//插入新节点newItem到list的第二个子节点

JS返回所有子节点对象childNodes

	var mylist = document.getElementById('myid');

	for(var i=0,i<mylist.childNodes.length;i++){

		console.log(mylist.childNodes[i]);

	}
firstChild返回第一个子节点

lastChild返回最后一个子节点

parentNode返回父节点对象

nextSibling返回下一个兄弟节点对象

previousSibling返回前一个兄弟节点对象

nodeName返回节点的HTML标记名称
