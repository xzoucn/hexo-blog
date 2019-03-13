---
title: PHP 学习手记 - 字符串
date: 2019-03-13 10:05:54
tags: PHP
---
初始化字符串主要有两种方式：
>$myString = 'my text';
$myOutput = "$myString more text.";

单引号和双引号的区别在于单引号不发生interpolation，而双引号则需要:
>$myOutput = '$myString more text.' 将输出： $myString more text.
$myOutput = “$myString more text.” 将输出 ：my text  more text.

字符串可以使用‘.’来做链接操作：
>$CombinedString = 'my text' 等价于 $CombinedString = 'my'.'text'

还有一种创建多行字符串的方法，称为heredoc
>echo <<<
line one
line two
line three
theEnd

<<<后接定义的结束标记，在输入完内容后接结束标记，需要留心结束标记不能出现在文本内容中，否则会导致系统语法匹配第一个结束位置。以上的例子创建了一个三行字符串。
