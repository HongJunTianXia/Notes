# Freemaker
* 模板+数据模型=输出
* 注释 <#-- and --> 
### 基本指令
* if 指令:有条件地跳过模板的一些片段
```aidl

  <h1>
    Welcome ${user}<#if user == "Big Joe">, our beloved leader</#if>!
  </h1>
```
```aidl
<#if animals.python.price < animals.elephant.price>
  Pythons are cheaper than elephants today.
<#else>
  Pythons are not cheaper than elephants today.
</#if>
```
* list指令:当需要列表显示内容时，list指令是必须的
```aidl
<p>We have these animals:
<table border=1>
  <#list animals as animal>
    <tr><td>${animal.name}<td>${animal.price} Euros
  </#list>
</table>
```
```aidl
<#list misc.fruits>
  <ul>
    <#items as fruit>
      <li>${fruit}
    </#items>
  </ul>
</#list>
```
此时， list 指令将列表视为一个整体， 在 items 指令中的部分才会为每个水果重复。 如果我们有0个水果，那么在 list 中的所有东西都被略过了， 因此就不会有 ul 标签了。
```aidl
<p>Fruits: <#list misc.fruits as fruit>${fruit}<#sep>, </#list>
```
被 sep 覆盖的部分(我们也可以这么来写： ...<#sep>, </#sep></#list>) 只有当还有下一项时才会被执行。 因此最后一个水果后面不会有逗号。
```aidl
<p>Fruits: <#list misc.fruits as fruit>${fruit}<#sep>, <#else>None</#list>
```
再次回到这个话题，如果我们有0个水果，会怎么样？只是打印 "Fruits:" 也没有什么不方便。 list 指令，也像 if 指令那样，可以有 else 部分，如果列表中有0个元素时就会被执行.
* include 指令:使用 include 指令， 我们可以在模板中插入其他文件的内容
```aidl
<html>
<head>
  <title>Test page</title>
</head>
<body>
  <h1>Test page</h1>
  <p>Blah blah...
  <#include "/copyright_footer.html">
</body>
</html>
```
* 联合使用指令
```aidl
<#list animals as animal>
      <div<#if animal.protected> class="protected"</#if>>
        ${animal.name} for ${animal.price} Euros
      </div>
</#list>
```
* 使用内建函数
```aidl
内建函数很像子变量(如果了解Java术语的话，也可以说像方法)， 它们并不是数据模型中的东西，是 FreeMarker 在数值上添加的。 为了清晰子变量是哪部分，使用 ?(问号)代替 .(点)来访问它们。常用内建函数的示例：

user?html 给出 user 的HTML转义版本， 比如 & 会由 &amp; 来代替。

user?upper_case 给出 user 值的大写版本 (比如 "JOHN DOE" 来替代 "John Doe")

animal.name?cap_first 给出 animal.name 的首字母大写版本(比如 "Mouse" 来替代 "mouse")

user?length 给出 user 值中 字符的数量(对于 "John Doe" 来说就是8)

animals?size 给出 animals 序列中 项目 的个数(我们示例数据模型中是3个)

如果在 <#list animals as animal> 和对应的 </#list> 标签中：

animal?index 给出了在 animals 中基于0开始的 animal的索引值

animal?counter 也像 index， 但是给出的是基于1的索引值

animal?item_parity 基于当前计数的奇偶性，给出字符串 "odd" 或 "even"。在给不同行着色时非常有用，比如在 <td class="${animal?item_parity}Row">中。

一些内建函数需要参数来指定行为，比如：

animal.protected?string("Y", "N") 基于 animal.protected 的布尔值来返回字符串 "Y" 或 "N"。

animal?item_cycle('lightRow','darkRow') 是之前介绍的 item_parity 更为常用的变体形式。

fruits?join(", ") 通过连接所有项，将列表转换为字符串， 在每个项之间插入参数分隔符(比如 "orange,banana")

user?starts_with("J") 根据 user 的首字母是否是 "J" 返回布尔值true或false。

内建函数应用可以链式操作，比如user?upper_case?html 会先转换用户名到大写形式，之后再进行HTML转义。(这就像可以链式使用 .(点)一样)
```
* 处理不存在的变量
数据模型中经常会有可选的变量(也就是说有时并不存在)。 除了一些典型的人为原因导致失误外，FreeMarker 绝不能容忍引用不存在的变量， 除非明确地告诉它当变量不存在时如何处理。这里来介绍两种典型的处理方法。

这部分对程序员而言: 一个不存在的变量和一个是 null 值的变量， 对于FreeMarker来说是一样的，所以这里所指的"丢失"包含这两种情况。

不论在哪里引用变量，都可以指定一个默认值来避免变量丢失这种情况， 通过在变量名后面跟着一个 !(叹号，译者注)和默认值。 就像下面的这个例子，当 user 不存在于数据模型时, 模板将会将 user 的值表示为字符串 "visitor"。(当 user 存在时， 模板就会表现出 ${user} 的值)：
```aidl
<h1>Welcome ${user!"visitor"}!</h1>
```
也可以在变量名后面通过放置 ?? 来询问一个变量是否存在。将它和 if 指令合并， 那么如果 user 变量不存在的话将会忽略整个问候的代码段：
```aidl
<#if user??><h1>Welcome ${user}!</h1></#if>
```