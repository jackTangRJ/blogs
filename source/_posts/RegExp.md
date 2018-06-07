---
title: RegExp正则表达式
date: 2018-06-05 16:44:26
tags: "javascript"
---
![image](http://www.wailian.work/images/2018/06/07/timg.jpg)

> 正则表达式主要是针对字符串进行操作，可以简化对字符串的复杂操作，其主要功能有匹配、切割、替换、获取

<!-- more -->
## 定义
> 所谓正则表达式，就是一种描述字符串结构模式的形式化表达方法。

### 创建方式
在JavaScript中，我们可以通过<b><code>RegExp()构造函数</code></b>或者<b><code>RegExp()字面量</code></b>两种方式去创建正则表达式。

<div class="code"><b><pre>
	<span class="line">var <span class="keyword">regex</span> = new <span class="keyword">RegExp</span>('xyz', 'i')</span>
	// 等价于
	<span class="line">var <span class="keyword">regex</span> = /xyz/i </span>
</pre></b>
</div>

### 匹配规则
#### 1. 预定义模式

字符 | 匹配
---|---
\d |0-9之间的任意数字
\D |  匹配所有除0-9之间的任意数
\w |  匹配任意字母，数字，下划线
\W | 匹配所有除了数字、字母、下划线的所有字符
\s | 匹配空格（制表符，回车键、换行符）
\b | 匹配词的边界 不连续的英文字母，词首必须独立
\B | 匹配非词的边界（咋内部）


<div class="tip">
	通常情况下，正则表达式碰到换行符(\n)就会停止匹配
</div>

#### 2. 量词符 
量词符用来设定某个模式出现的次数
	
字符 | 匹配
---|---	
? | 表示匹配0次或者1次，相当于 {0,1}
*  | 表示某个模式出现0次或者多次
+ | 表示某个模式出现1次或者多次 等同于 {1,}

#### 3. 转义符 \
>正则表达式中，需要反斜杠转义的，一共有12个字符：^、.、[、$、(、)、|、*、+、?、{和\\。需要特别注意的是，如果使用RegExp方法生成正则对象，转义需要使用两个斜杠，因为字符串内部会先转义一次。

#### 4. 特殊字符
1. \n 换行键
2. \r 回车键
3. \t 制表符

#### 5. 字符类
1) 脱字符 ^ 只有在第一个位置才有效果
<div class="code"><b><pre>
	<span class="line"><span class="keyword">方括号中第一个字符是^,则表示除了这字符类，其他都匹配，如果除了^没有别的，则表示匹配所有</span></span>
	
</pre><b /></div>
    
    
2) 连字符
<div class="code"><b><pre>
	<span class="line"><span class="keyword"> 用-来表示字符的连续范围,当连字符不出现在方括号中时，则不表示连续范围</span></span>
	
</pre><b /></div>

#### 6. 重复字符类

字符 | 匹配
---|---
{n,m} |	匹配前一项至少n次，但不能超过m次
{n,} |	匹配前一项n次或多次
{n}	| 匹配前一项n次
? |	匹配前一项0次或1次，也就是说前一项是可选的，等价于{0,1}
+ | 匹配前一项1次或多次，等价于{1,}
* | 匹配前一项0次或多次，等价于{0,}


<div class="tip">
	javascript默认是贪婪匹配，也就是说匹配重复字符是尽可能多地匹配，而且允许后续的正则表达式继续匹配。而进行非贪婪匹配，只需要在待匹配的字符后面跟随一个问号即可：“??”、“+?”、“*?”、“{1,5}?”。比如：/a+/可以匹配一个或多个连续的字母a。当使用“aaa”作为匹配字符串时，/a+/会匹配它的三个字母。但是/a+?/会尽可能少的匹配，只能匹配第一个哦~
</div>

#### 7. 元字符
1.点字符 .

	匹配除了回车\r 换行\n 行分隔符 段分割付之外的所有字符
	
2.位置分隔符 ^ $

	^表示字符串开始的位置
	 $表示字符串结束的位置
	
3.选择符 |

	竖线选择符表示或的关系

#### 8. 修饰符
字符 |	匹配
---|---
i | 执行不区分大小写的匹配
g |	执行一个全局匹配，简而言之，即找到所有的匹配，而不是在找到第一个之后就停止
m  |	多行匹配模式，^匹配一行的开头和字符串的开头，$匹配行的结束和字符串的结束

#### 9.组匹配
正则表达式的()表示分组匹配，括号中的模式用来匹配分组的内容
     
非捕获组  
	<code>(?:x) </code> 表示不返回改组匹配的内容
     
先行断言
   <code>X(?=Y)</code>  x只有在y前面才会被匹配，y不计入匹配结果
    
先行否断言
    <code>x(?!=y)</code> x只有不在y前面才匹配



### 用于模式匹配的String方法

<code>String.search()</code> 参数：一个正则表达式。返回：第一个与参数匹配的子串的起始位置，如果找不到，返回-1。不支持全局搜索，如果参数是字符串，会先通过RegExp构造函数转换成正则表达式。

<code>String.replace()</code>	检索和替换。第一个参数：正则表达式，第二个参数：要进行替换的字符串，也可以是函数。设置了g修饰符，则替换所有匹配的子串，否则只替换第一个子串。通过在替换字符串中使用“$n”，可以使用子表达式相匹配的文本来替换字符。

<code>String.match()</code> 参数：一个正则表达式。返回：一个由匹配结果组成的数组。设置g则返回所有匹配结果，否则数组的第一个元素是匹配的字符串，剩下的是圆括号中的子表达式，即a[n]中存放的是$n的内容。

<code>String.split()</code> 参数：正则表达式或字符串。返回：子串组成的数组。

### 用于模式匹配的RegExp方法

<code>exec()</code>  参数：字符串。在一个字符串中执行匹配检索，与String.macth()非全局检索类似，返回一个数组或null。

<code>test()</code> 参数：字符串。返回true or false



## 案例实战
1.替换掉字符串首尾空格

<div class="code"><b><pre>
	<span class="line">let<span class="keyword"> str</span> = '    aab_abbbb_bgabb45+ef     '; </span>
	<span class="line">let<span class="keyword"> trim </span> =  /^\s+|\s+$/g</span>
	<span class="line"><span class="keyword">console</span>.log(<span class="keyword">str</span>.replace(<span class="keyword">trim</span>, ''));</span>
	
</pre><b /></div>

2.验证邮箱

<div class="code"><b><pre>

	<span class="line">console.log(/^\w+@[0-9a-zA-Z]+\.\w+$/.test("45gdg4ge@qq.com"))</span>
	
</pre><b /></div>

3.匹配电话号码
	
	```
	/^1[3|4|5|8][0-9]\d{4,8}$/
	```
	
	
####  [常用正则表达式](https://github.com/jaywcjlove/handbook/blob/master/Javascript/%E4%B8%80%E4%BA%9B%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F%E9%9A%8F%E8%AE%B0.md#%E7%94%A8%E6%88%B7%E5%90%8D%E6%AD%A3%E5%88%99)



>还不懂？没关系，拿着正则表达式去上面套公式

#### [阮一峰正则](http://javascript.ruanyifeng.com/stdlib/regexp.html)



