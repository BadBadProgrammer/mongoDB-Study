目录：C:\Program Files\MongoDB\Server\3.6\bin
启动 mongo.exe   mongod.exe

use testdb                          创建/切换数据库
show dbs                            展示所有数据库

db                                  查看当前的数据库
db.dropDatebase()                   删除数据库

db.test.drop()                      删除集合
db.createCollection("test")         创建集合
show collections                    查看已有集合

/************插入文档*****************/
db.test.insert({"name":"test"})     向一个集合里插入一个或多个文档（每一个文档都需要一个唯一的 _id 字段作为 primary_key。如果一个插入文档操作遗漏了``_id`` 字段，MongoDB驱动会自动为``_id``字段生成一个 ObjectId。）
db.test.insertOne()                 插入一个文档
db.test.insertMany([
     { name: "bob", age: 42, status: "A", },
     { name: "ahn", age: 22, status: "A", },
     { name: "xi", age: 34, status: "D", }
   ])                               插入多个文档

/***************查询文档****************/
db.test.find()                      查看已插入文档
db.users.find( { status: { $in: [ "P", "D" ] } } )          从集合中按照条件检索
db.users.find( { finished: { $elemMatch: { $gt: 15, $lt: 20 } } } )
$eleMatch:操作符为数组元素指定复合条件，以查询数组中至少一个元素满足所有指定条件的文档。

/******************更新文档************/
db.users.updateOne(
   { "favorites.artist": "Picasso" },
   {
     $set: { "favorites.food": "pie", type: 3 },
     $currentDate: { lastModified: true }
   }
)                                   更新文档。使用 $set 操作符更新 favorites.food 字段的值为 "Pisanello" 并更新 type 字段的值为 3,使用 $currentDate 操作符更新 lastModified 字段的值到当前日期。如果 lastModified 字段不存在， $currentDate 会创建该字段。使用 db.collection.update() 并包含 multi: true 选项来更新多个文档

db.users.replaceOne(
   { name: "abc" },
   { name: "amy", age: 34, type: 2, status: "P", favorites: { "artist": "Dali", food: "donuts" } }
)                                   对 users 集合使用 db.collection.replaceOne() 方法将通过过滤条件 name 等于 "abc" 匹配到的 第一个 文档替换为新文档

db.collection.update(
   { name: "xyz" },
   { name: "mee", age: 25, type: 1, status: "A", favorites: { "artist": "Matisse", food: "mango" } }
)                                   对 users 集合使用 db.collection.update() 方法将通过过滤条件 name 等于 "xyz" 匹配到的 第一个 文档替换为新文档

/********************删除文档***************/
db.users.deleteMany({})                         从 users 集合中删除了 所有 文档
db.users.deleteMany({ status : "A" })           从 users 集合中删除所有 status 字段等于 "A" 的文档
db.users.remove( { status : "P" } )             另一种选择如下例子使用 db.collection.remove() 从 users 集合中删除所有 status 字段等于 "A" 的文档

/*******************聚合管道******************/
$match()            筛选条件，过滤掉不满足条件的文档,
$group()分组        使用_id指定要分组的键，要分组的键也可以是多个，使用其他自定义的字段用于统计
$project()投射      用于包含、排除字段: 设置要查询或者要过滤掉的字段，0: 要过滤掉的字段，不显示，1：需要查询的字段
                    对字段重命名
                    在投射中使用一些【表达式】:数学表达式、日期表达式、字符串表达式、逻辑表达式(比较表达式、布尔表达式、控制语句)
$sort               排序：1升序，2降序          