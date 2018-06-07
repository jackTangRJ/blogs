---
title: JSON对象
date: 2018-06-05 09:53:12
---
![image](http://www.wailian.work/images/2018/06/05/timg.jpg)

> #### JSON的优点

相比 XML 格式，JSON 格式有两个显著的优点：书写简单，一目了然；符合 JavaScript 原生语法，可以由解释引擎直接处理，不用另外添加解析代码。所以，JSON 迅速被接受，已经成为各大网站交换数据的标准格式，并被写入标准。

<!-- more -->

## 1.JSON 格式


> 每个 JSON 对象就是一个值，可能是一个数组或对象，也可能是一个原始类型的值。总之，只能是一个值，不能是两个或更多的值。

**JSON 对值的类型和格式有严格的规定**

1. 复合类型的值只能是数组或对象，不能是函数、正则表达式对象、日期对象。
2. 原始类型的值只有四种：字符串、数值（必须以十进制表示）、布尔值和<code>null</code>（不能使用<code>NaN</code>,<code>Infinity</code> , <code>-Infinity</code>和<code>undefined</code>）。
3. 字符串必须使用<code>双引号</code>表示，不能使用单引号。
4. 对象的键名必须放在双引号里面。
5. 数组或对象最后一个成员的后面，不能加逗号。


注意: <code>null</code> 、空数组和空对象都是合法的 JSON 值。

## 2.JSON 静态方法
JSON对象是 JavaScript 的原生对象，用来处理 JSON 格式数据。它有两个静态方法：<code>JSON.stringify()</code> <br/> <code>JSON.parse()</code> 

#### 2.1  &nbsp;&nbsp;JSON.stringify方法

**用于将一个值转为 JSON 字符串。该字符串符合JSON格式 ，并且可以被<code>JSON.parse</code>方法还原。**

1.如果对象的属性是<code>undefined</code>、<code>函数</code>、或<code>XML对象</code>，<code>Json.stringify</code>会过滤
2.如果数组的成员是<code>undefined</code>、<code>函数</code>、或<code>XML对象</code>，<code>Json.stringify</code>，则这些值会转为null
3.正则对象会被转化为空对象
4.<code>Json.stringify</code>会忽略对象不可遍历熟悉
5.对于原始类型的值，转换结果会自带双引号。
6.**<code>Json.stringify</code>**方法会忽略对象的不可遍历属性
<div class="code">
<b>
<pre>
	<span class="line">var <span class="keyword">obj</span> = {};</span>
	<span class="line">Object.defineProperties(obj, {</span></span>
		<span class="line"><span class="keyword">'foo'</span>: {</span>
		    <span class="line"><span class="keyword">value</span>: <span class="string">1</span></span>
			<span class="line"><span class="keyword">enumerable</span>: <span class="string">true</span></span>
		<span class="line">},</span>
		<span class="line"><span class="keyword">'bar'</span>: {</span>
		    <span class="line"><span class="keyword">value</span>: <span class="string">2</span></span>
			<span class="line"><span class="keyword">enumerable</span>: <span class="string">false</span></span>
		<span class="line">}</span>
	<span class="line">});</span>
	
	<span class="line">console.log(<span class="keyword">JSON.stringify(obj);</span>)</span>		// "{"foo":1}"
	
</pre>
</b>
</div>

**<code>Json.stringify</code>的第二个参数**

JSON.stringify方法还可以接受一个数组，作为第二个参数，指定需要转成字符串的属性。
	参数为数组： **只对对象的属性有效，对数组无效**
	参数为函数 **：用来更改JSON.stringify的返回值，中间过滤操作,如果处理函数返回undefined或没有返回值，则改属性会被忽略**

<div class="code">
<b>
<pre>
	<span class="line">console.log( <span class="keyword"> JSON.stringify(['a', 'b'], ['0'])</span> ) </span>
	// "["a","b"]"
	
	<span class="line">console.log( <span class="keyword"> JSON.stringify({0: 'a', 1: 'b'}, ['0']) </span> )</span>
	// "{"0":"a"}"
	
	<span class="line">function <span class="string">f</span>(key, value) {</span>
		<span class="line">if (typeof value === "number") </span>
			<span class="line"> value = 2 * value; </span>
		<span class="line">} </span>
		<span class="line"> <code>return</code>  value;</span>
	<span class="line">}</span>
	
	<span class="line"><span class="keyword">JSON.stringify({ a: 1, b: 2 }, f)</span></span> 		// '{"a": 2,"b": 4}'
	
</pre>
<b />
</div>

**<code>Json.stringify</code>的第三个参数**

<b>JSON.stringify</b>还可以接受第三个参数，用于增加返回的 JSON 字符串的可读性。如果是数字，表示每个属性前面添加的空格（最多不超过10个）；如果是字符串（不超过10个字符），则该字符串会添加在每行前面。


## 2.1.2 &nbsp;&nbsp;JSON.parse方法
用于将 JSON 字符串转换成对应的值。

<div class="tip">
	如果传入的字符串不是有效的 JSON 格式，JSON.parse方法将报错
</div>

为了处理解析错误，可以将JSON.parse方法放在try...catch代码块中。
<div class="code">
<b>
<pre>
	<span class="line">try {</span>
		<span class="line">JSON.parse("'String'");</span>
	<span class="line"> } catch(e) {</span>
		<span class="line">console.log('parsing error'); </span>
	<span class="line"> }</span>

</pre>
</b>
</div>

> JSON.parse方法可以接受一个处理函数，作为第二个参数，用法与JSON.stringify方法类似.

#### 3.参数对象的 toJSON 方法

<div class="tip">
	如果参数对象有自定义的toJSON方法，那么JSON.stringify会使用这个方法的返回值作为参数，而忽略原对象的其他属性。
</div>





			