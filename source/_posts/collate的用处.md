---
title: collate的用处
date: 2019-03-13 10:08:42
tags: TSQL
categories: SQL
---
在多记录集Union时，发生了错误，由于排序集的不同而产生冲突。

一般说来数据库均会采用默认的排序集，因此不会产生这种问题，但自行建立的临时表则可能出现例外。今天正好遇到的就是这个问题。为了让各种记录集之间的排序统一，可以在创建临时表的同时指定数据库默认的排序规则：
>CREATE TABLE #Mytable
(
&nbsp;&nbsp;&nbsp;&nbsp;MyField COLLATE DATABASE_DEFAULT nvarchar（150）
)

这样为临时表指定了数据库的默认排序规则，在SQL中需要用到排序规则来处理的地方就不会由于口径不一而发生错误了。
