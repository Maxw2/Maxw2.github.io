---
layout: post
title:  "浅谈JavaScript数据间的转换类型"
date:   2020-08-09 19:52:39 +0800
categories: javaScript 
tags: javaScript
---
最近在进行面试的时候遇到了这样几道题，结果没有回答出来，感觉对于一些数据类型还是停留在不知所以然的程度上，所以在此进行记录。

``` js
" 34" * 4 = ?           // 136
undefined * 4 = ?       // NaN
NaN * 4 = ?             // NaN
null * 4 = ?            // 0 
```
### String
对于第一个String类型与Number类型相乘的话，String应转化为Number，之后在与之进行运算，String转换的规则应为：
* 如果String的首个数据是空格 则会向后寻找，但是如果空格在字符串之中，如 "12 34" 则最后结果判定为 NaN
* 如果String的数据为非数字 最后的结果判定为NaN
* 如果String的数字之中夹带着字符串 如 "123ab" 则最后的判定结果为NaN
* 如果String的数据为数字 如 "456" 则会正常进行运算
* 总结起来就是如果String没办法是数字的话 则结果会全部变为NaN

### Undefined / NaN
第二个Undefind 对于任何数值的运算结果都为NaN
<br />
第三个NaN本身就为一个非数值类型的数值 typeof NaN === 'number' 所以与之运算的结果仍为NaN

### Null
Null是一个很特别的数据类型 null在 typeof 的时候结果为 object, 在与undefined的比较中 null == undefined 为 true。 
在阮一峰的 [JavaScript教程] 文档中是这样解释的:
> null与undefined都可以表示“没有”，含义非常相似。将一个变量赋值为undefined或null，老实说，语法效果几乎没区别。
``` js
Number(null) // 0
5 + null // 5
```
上面代码中，null转为数字时，自动变成0。
<br />
但是，JavaScript 的设计者 Brendan Eich，觉得这样做还不够。首先，第一版的 JavaScript 里面，null就像在 Java 里一样，被当成一个对象，Brendan Eich 觉得表示“无”的值最好不是对象。其次，那时的 JavaScript 不包括错误处理机制，Brendan Eich 觉得，如果null自动转为0，很不容易发现错误。
因此，他又设计了一个undefined。区别是这样的：null是一个表示“空”的对象，转为数值时为0；undefined是一个表示"此处无定义"的原始值，转为数值时为NaN。

所以这道问题的关键也就在此，Null在运算时会转化为0，所以 
``` js
null + 1    // 1    null + 1 = 0 + 1
null * 1    // 0    null * 1 = 0 * 1
```

[JavaScript教程]: https://wangdoc.com/javascript/types/null-undefined-boolean.html