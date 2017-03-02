#JSON 语法  
JSON 语法是 JavaScript 语法的子集。

###JSON 语法规则  
* 1.数据在名称/值对中
* 2.数据由逗号分隔
* 3.大括号保存对象
* 4.中括号保存数组

###JSON 名称/值对  
名称/值对包括字段名称（在双引号中），后面写一个冒号，然后是值：  
"name" : "菜鸟教程"

###JSON 值
* 1.数字（整数或浮点数）
* 2.字符串（在双引号中）
* 3.逻辑值（true 或 false）
* 4.数组（在中括号中）
* 5.对象（在大括号中）
* 6.null

###JSON 对象  
JSON 对象在大括号{}中书写：  
对象可以包含多个名称/值对：  
{ "name":"菜鸟教程" , "url":"www.runoob.com" }  

###JSON 数组
JSON 数组在中括号中书写：  
```
{
"sites": [
{ "name":"菜鸟教程" , "url":"www.runoob.com" }, 
{ "name":"google" , "url":"www.google.com" }, 
{ "name":"微博" , "url":"www.weibo.com" }
]
}
```
