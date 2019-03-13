---
title: PHP 学习手记 - 变量和常量
date: 2019-03-13 10:06:50
tags: PHP
categories: PHP
---
PHP是弱类型语言，并不需要在给变量初始化值前定义变量类型。解析器会由变量具体的赋值来判断变量的具体类型。
PHP基本数据类型包括：
>* Integer
>* Float
>* String
>* Boolean
>* Array
>* Object

两个特殊类型为：
>* NULL
>* resource

PHP特别提供了可变变量，也就是变量名是一个变量的变量，一般定义如：
>$varname = 'myvar';
$$varname = 10;
等价于$myvar = 10；

常量的定义类似于 C，常量一般用大写且不需要$符号，定义一个常量
>define('NUMBER',100);

使用这个常量
>echo NUMBER;

PHP中重要的全局变量包括：
>$GLOBALS
$_SERVER
$_GET
$_POST
$_COOKIE
$_FILES
$_ENV
$_REQUEST 包括 $_GET，$_POST，$_COOKIE 的输入内容
$_SESSION
