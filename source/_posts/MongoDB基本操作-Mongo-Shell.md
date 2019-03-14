---
title: MongoDB基本操作 - Mongo Shell
date: 2019-03-13 17:42:07
tags: MongoDB
categories: 数据库
---
Mongo Shell提供了交互式的MongoDB操作体验，通过Mongo Shell我们可以方便地进行查询验证及学习。本篇我们将通过Mongo Shell来进行MongoDB基本操作介绍。
###### 文档插入
``` js
db.inventory.insertOne(
   { item: "canvas", qty: 100, tags: ["cotton"], size: { h: 28, w: 35.5, uom: "cm" } }
) // 单条插入
db.inventory.insertMany([
   { item: "journal", qty: 25, tags: ["blank", "red"], size: { h: 14, w: 21, uom: "cm" } },
   { item: "mat", qty: 85, tags: ["gray"], size: { h: 27.9, w: 35.5, uom: "cm" } },
   { item: "mousepad", qty: 25, tags: ["gel", "blue"], size: { h: 19, w: 22.85, uom: "cm" } }
]) //多条插入
```
###### 文档查询 - 基础
初始化查询记录
```js
 db.inventory.insertMany([
   { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "A" },
   { item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
   { item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
   { item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" }
]); 
```
查询语句
``` js
db.inventory.find( {} ) //所有记录
db.inventory.find( { status: "D" } ) //status是“D”的记录
db.inventory.find( { status: { $in: [ "A", "D" ] } } ) //status是“A”或“D”的记录
db.inventory.find( { status: "A", qty: { $lt: 30 } } ) //status是“A”并且qty小于30的记录
db.inventory.find( { $or: [ { status: "A" }, { qty: { $lt: 30 } } ] } ) //status是“A”或者qty小于30的记录
db.inventory.find( {
     status: "A",
     $or: [ { qty: { $lt: 30 } }, { item: /^p/ } ]
} ) //status是“A”，并且qty小于30或item以p开头
db.inventory.find( { "size.h": { $lt: 15 }, "size.uom": "in", status: "D" } ) //size.h小于15，size.uom是“in”并且status是“D”
```
###### 文档查询 - 数组
初始化查询记录
```js
db.inventory.insertMany([
   { item: "journal", qty: 25, tags: ["blank", "red"], dim_cm: [ 14, 21 ] },
   { item: "notebook", qty: 50, tags: ["red", "blank"], dim_cm: [ 14, 21 ] },
   { item: "paper", qty: 100, tags: ["red", "blank", "plain"], dim_cm: [ 14, 21 ] },
   { item: "planner", qty: 75, tags: ["blank", "red"], dim_cm: [ 22.85, 30 ] },
   { item: "postcard", qty: 45, tags: ["blue"], dim_cm: [ 10, 15.25 ] }
]);
```
查询语句
```js
db.inventory.find( { tags: ["red", "blank"] } ) //tags数组为 ["red", "blank"] 的记录
db.inventory.find( { tags: { $all: ["red", "blank"] } } ) //tags数组包含“red”和“blank”的记录
db.inventory.find( { tags: "red" } ) //tags包含“red”的记录
db.inventory.find( { dim_cm: { $gt: 25 } } ) //dim_cm包含大于25的元素的记录
db.inventory.find( { dim_cm: { $gt: 15, $lt: 20 } } ) //dim_cm包含大于15的元素，并且包含小于20的元素的记录
db.inventory.find( { dim_cm: { $elemMatch: { $gt: 22, $lt: 30 } } } ) //dim_cm包含大于22同时小于30的元素的记录
db.inventory.find( { "dim_cm.1": { $gt: 25 } } ) //dim_cm第二个元素大于25的记录
db.inventory.find( { "tags": { $size: 3 } } ) //tags有三个元素的记录
```
###### 文档查询 - 文档元素数组
初始化查询记录
```js
db.inventory.insertMany( [
   { item: "journal", instock: [ { warehouse: "A", qty: 5 }, { warehouse: "C", qty: 15 } ] },
   { item: "notebook", instock: [ { warehouse: "C", qty: 5 } ] },
   { item: "paper", instock: [ { warehouse: "A", qty: 60 }, { warehouse: "B", qty: 15 } ] },
   { item: "planner", instock: [ { warehouse: "A", qty: 40 }, { warehouse: "B", qty: 5 } ] },
   { item: "postcard", instock: [ { warehouse: "B", qty: 15 }, { warehouse: "C", qty: 35 } ] }
]);
```
查询语句
```js
db.inventory.find( { "instock": { warehouse: "A", qty: 5 } } ) //instock中包含元素为 { warehouse: "A", qty: 5 }并且顺序相同的记录
db.inventory.find( { "instock": { qty: 5, warehouse: "A" } } ) //顺序不相同，不能查询到记录！！！
db.inventory.find( { 'instock.qty': { $lte: 20 } } ) //instock中存在一个元素的qty属性小于等于20的记录
db.inventory.find( { 'instock.0.qty': { $lte: 20 } } ) //instock中第一个元素的qty属性小于等于20的记录
db.inventory.find( { "instock": { $elemMatch: { qty: 5, warehouse: "A" } } } ) //instock存在一个元素包含属性qty、warehouse，qty等于5并且warehouse是“A”的记录
db.inventory.find( { "instock": { $elemMatch: { qty: { $gt: 10, $lte: 20 } } } } ) //instock存在一个元素包含qty，qty大于10并且小于等于20
db.inventory.find( { "instock.qty": { $gt: 10,  $lte: 20 } } ) //instock中任意一个元素的qty属性大于10并且任意一个元素的属性小于等于20
db.inventory.find( { "instock.qty": 5, "instock.warehouse": "A" } ) //instock中存在元素的qty属性等于5并且instock中存在元素的属性warehouse为‘A’
```
###### 文档查询 - 投影
初始化查询记录
```js
db.inventory.insertMany( [
  { item: "journal", status: "A", size: { h: 14, w: 21, uom: "cm" }, instock: [ { warehouse: "A", qty: 5 } ] },
  { item: "notebook", status: "A",  size: { h: 8.5, w: 11, uom: "in" }, instock: [ { warehouse: "C", qty: 5 } ] },
  { item: "paper", status: "D", size: { h: 8.5, w: 11, uom: "in" }, instock: [ { warehouse: "A", qty: 60 } ] },
  { item: "planner", status: "D", size: { h: 22.85, w: 30, uom: "cm" }, instock: [ { warehouse: "A", qty: 40 } ] },
  { item: "postcard", status: "A", size: { h: 10, w: 15.25, uom: "cm" }, instock: [ { warehouse: "B", qty: 15 }, { warehouse: "C", qty: 35 } ] }
]);
```
查询语句
```js
db.inventory.find( { status: "A" }, { item: 1, status: 1 } ) //status是“A”的记录，返回字段_id、item、status
db.inventory.find( { status: "A" }, { item: 1, status: 1, _id: 0 } ) //status是“A”的记录，返回字段item、status
db.inventory.find( { status: "A" }, { status: 0, instock: 0 } ) //status是“A”的记录，返回status、instock以外的所有字段
db.inventory.find(
   { status: "A" },
   { item: 1, status: 1, "size.uom": 1 }
) //status是“A”的纪录，返回字段_id、item、status、uom(size下面的属性)
db.inventory.find(
   { status: "A" },
   { "size.uom": 0 }
) //status是“A”的记录，返回uom(size下面的属性)以外的所有字段
db.inventory.find( { status: "A" }, { item: 1, status: 1, "instock.qty": 1 } ) //status是“A”的纪录，返回字段_id、item、status、qty(instock数组下面的元素的属性)
db.inventory.find( { status: "A" }, { item: 1, status: 1, instock: { $slice: -1 } } ) //status是“A”的纪录，返回字段_id、item、status、instock数组下最后一条记录。数组投影只能使用$elemMatch、$slice、$。
```
###### 文档查询 - 空值
初始化查询记录
```js
db.inventory.insertMany([
   { _id: 1, item: null },
   { _id: 2 }
])
```
查询语句
```js
db.inventory.find( { item: null } ) //查询item为null或item不存在的记录
db.inventory.find( { item : { $type: 10 } } ) //查询item为BSON null的记录
db.inventory.find( { item : { $exists: false } } ) //查询item不存在的记录
```
###### 文档更新
初始化查询记录
```js
db.inventory.insertMany( [
   { item: "canvas", qty: 100, size: { h: 28, w: 35.5, uom: "cm" }, status: "A" },
   { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "mat", qty: 85, size: { h: 27.9, w: 35.5, uom: "cm" }, status: "A" },
   { item: "mousepad", qty: 25, size: { h: 19, w: 22.85, uom: "cm" }, status: "P" },
   { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "P" },
   { item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
   { item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
   { item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" },
   { item: "sketchbook", qty: 80, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "sketch pad", qty: 95, size: { h: 22.85, w: 30.5, uom: "cm" }, status: "A" }
] );
```
更新语句
```js
db.inventory.updateOne(
   { item: "paper" },
   {
     $set: { "size.uom": "cm", status: "P" },
     $currentDate: { lastModified: true }
   }
) //更新item为“paper”的单条记录，$set更新对应字段值，$currentDate用于更新lastModified字段（不存在则创建）
db.inventory.updateMany(
   { "qty": { $lt: 50 } },
   {
     $set: { "size.uom": "in", status: "P" },
     $currentDate: { lastModified: true }
   }
) //3.2开始支持，更新qty小于50的多条记录，$set更新对应字段值，$currentDate用于更新lastModified字段（不存在则创建）
db.inventory.replaceOne(
   { item: "paper" },
   { item: "paper", instock: [ { warehouse: "A", qty: 60 }, { warehouse: "B", qty: 40 } ] }
) //用第二个文档对象替换item是“paper”的对象
```
###### 文档删除
初始化查询记录
```js
db.inventory.insertMany( [
   { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "P" },
   { item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
   { item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
   { item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" },
] );
```
删除语句
```js
db.inventory.deleteMany({}) //删除所有记录
db.inventory.deleteMany({ status : "A" }) //删除status是“A”的多条记录
db.inventory.deleteOne( { status: "D" } ) //删除status是“D”的第一条记录
```
###### 聚合 - 管道
管道方式聚合时，多个聚合运算分布于管道上。每一步聚合运算完成后将结果交给下一步的聚合运算继续处理，以此类推。聚合运算的基本过程如下图所示。
![聚合运算示意](aggregate.png)

通过以上操作，我们看到可以通过Mongo Shell方便地对MongoDB进行日常维护操作。MongoDB的查询主要通过查询方法加上查询文档体来实现，这对于理解MongoDB第三方客户端的使用具有重要的指导意义。