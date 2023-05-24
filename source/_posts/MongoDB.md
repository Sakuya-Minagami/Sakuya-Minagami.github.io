---
title: MongoDB
---
数据库之一，来自陌生的后端领域
同时也兼笔记
# 安装
官网安装[MongoDB](https://www.mongodb.com/try/download/community)和[MongoDBshell](https://www.mongodb.com/try/download/shell)
# 启动
在powershell下
```shell
PS D:\Environment\MongoDB\mongodb-win32-x86_64-windows-6.0.4\bin> .\mongod.exe --dbpath=D:\review\nodejs\db
```
注意，打开的是mongod.exe，dbpath为数据库路径，全程不能有中文
打开MongoDBshell，输入默认端口号27017
```shell
Please enter a MongoDB connection string (Default: mongodb://localhost/): 27017
```
# 指令
所有指令在MongoDBshell中执行
## 基础操作
### help 帮助
很乱用不上
### show dbs 查询数据库
会显示所有已创建的数据库
```
admin   40.00 KiB
config  60.00 KiB
local   40.00 KiB
```
### use test 创建数据库，若已有则转入
```
27017> use test
switched to db test
```
### db 检测当前所在数据库
*其实在MongoDBshell里会默认显示当前数据库*
### db.createCollection("users") 创建集合
*括号与引号是指令*
```
test> db.createCollection("users")
{ ok: 1 }
```
### db.dropDatabase() 删库跑路
```
test> db.dropDatabase()
{ ok: 1, dropped: 'test' }
```
### db.getCollectionNames() 查询集合
```
test> db.getCollectionNames()
[ 'users' ]
```
### db.users.drop() 删除集合
```
test> db.users.drop()
true
```
## 增删改查
### db.users.insertOne({username:"kh",age:100}) 写入数据
```
test> db.users.insertOne({username:"kh",age:100})
{
  acknowledged: true,
  insertedId: ObjectId("64016693cf1bab53a72c8f9a")
}
```
*旧版本save方法已经弃用，只能insertOne*
### db.users.deleteOne({age:100}) 删除符合字段数据
```
test> db.users.deleteOne({age:100})
{ acknowledged: true, deletedCount: 0 }
```
