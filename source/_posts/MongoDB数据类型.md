---
title: MongoDB数据类型
date: 2019-03-13 17:46:35
tags: MongoDB
categories: 数据库
---
MongoDB通过BSON（Binary JSON）来描述和存放数据。BSON是一种可进行二进制序列化的，类JSON格式的文档对象。通过BSON，MongoDB可以方便地存储无模式（scheme）数据。
|类型|编号|别名|说明|
| ------ | ------ | ------ |------|
|Double|	1|	“double”||	 
|String|	2|	“string”||	 
|Object|	3|	“object”||	 
|Array|	4|	“array”||	 
|Binary data|	5|	“binData”||	 
|Undefined|	6|	“undefined”|	Deprecated.|
|ObjectId|	7|	“objectId”||	 
|Boolean|	8|	“bool”||	 
|Date|	9|	“date”|| 
|Null|	10|	“null”||	 
|Regular Expression|	11|	“regex”||	 
|DBPointer|	12|	“dbPointer”|	Deprecated.|
|JavaScript|	13|	“javascript”||	 
|Symbol|	14|	“symbol”|	Deprecated.|
|JavaScript (with scope)|	15|	“javascriptWithScope”||	 
|32-bit integer|	16|	“int”||	 
|Timestamp|	17|	“timestamp”||	 
|64-bit integer|	18|	“long”||	 
|Decimal128|	19|	“decimal”|	New in version 3.4.|
|Min key|	-1|	“minKey”||	 
|Max key|	127|	“maxKey”||

###### ObjectId
具有12字节的唯一标识。MongoDB中，每条记录都有一个类似关系型数据库主键标识的_id字段来标识此条记录，如果没有设定文档记录的_id值，则通过ObjectId来生成_id。其具体组成如下：
- 4字节，Unix的时间戳
- 5字节，随机数值
- 3字节，计数器，从随机的起始值开始

###### String
BSON的字符串编码为UTF-8，UTF-8可以更好地支持各种语言文字，一般每种编程语言的MongoDB驱动会帮助处理字符串编码以符合要求。

###### Timestamps
BSON中的时间戳比较特别，他并不能直接对应到Date类型，而是通过如下方式组织64位数据：
- 前32位是从Unix纪元（1970.1.1）开始计算到现在时间的秒数
- 后32位是一秒内的操作序数
在一个单实例MongoDB中，时间戳是唯一的。如果对一个一级timestamp类型字段插入了空值，MongoDB会将空值替换成当前时间戳（从2.6版开始，之前的版本只会替换前两个字段）。

###### Date
BSON日期是一个64位的字段类型，存放了从Unix纪元（1970.1.1）开始计算的毫秒数计数时间。BSON日期是有符号类型的，负值代表1970.1.1之前的时间。