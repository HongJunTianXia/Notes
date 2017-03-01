#Ajax简介
###1.
AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。
AJAX 不是新的编程语言，而是一种使用现有标准的新方法。
AJAX 最大的优点是在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容。
AJAX 不需要任何浏览器插件，但需要用户允许JavaScript在浏览器上执行。
###2.Ajax应用
* 1.运用XHTML+CSS来表达资讯；
* 2.运用JavaScript操作DOM（Document Object Model）来执行动态效果；
* 3.运用XML和XSLT操作资料;
* 4.运用XMLHttpRequest或新的Fetch API与网页服务器进行异步资料交换；  
*注意：AJAX与Flash、Silverlight和Java Applet等RIA技术是有区分的。*
```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script>
function loadXMLDoc()
{
	var xmlhttp;
	if (window.XMLHttpRequest)
	{
		//  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
		xmlhttp=new XMLHttpRequest();
	}
	else
	{
		// IE6, IE5 浏览器执行代码
		xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
	}
	xmlhttp.onreadystatechange=function()
	{
		if (xmlhttp.readyState==4 && xmlhttp.status==200)
		{
			document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
		}
	}
	xmlhttp.open("GET","/try/ajax/ajax_info.txt",true);
	xmlhttp.send();
}
</script>
</head>
<body>

<div id="myDiv"><h2>使用 AJAX 修改该文本内容</h2></div>
<button type="button" onclick="loadXMLDoc()">修改内容</button>

</body>
</html>
```
###3.XMLHttpRequest 对象
XMLHttpRequest 对象用于在后台与服务器交换数据。  
XMLHttpRequest 对象是开发者的梦想，因为您能够：
* 1. 在不重新加载页面的情况下更新网页
* 2.在页面已加载后从服务器请求数据
* 3.在页面已加载后从服务器接收数据
* 4.在后台向服务器发送数据
* 5.所有现代的浏览器都支持 XMLHttpRequest 对象。
#####创建 XMLHttpRequest 对象
所有现代浏览器 (IE7+、Firefox、Chrome、Safari 以及 Opera) 都内建了 XMLHttpRequest 对象。
通过一行简单的 JavaScript 代码，我们就可以创建 XMLHttpRequest 对象。
#####创建 XMLHttpRequest 对象的语法：
xmlhttp=new XMLHttpRequest();
老版本的 Internet Explorer （IE5 和 IE6）使用 ActiveX 对象：
xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
