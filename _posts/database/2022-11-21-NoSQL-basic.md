---
title:  "NoSQL Basic"
date:   2022-06-21 -0500
categories: database
tags: NoSQL
header:
    teaser: /assets/img/training-guidence.png
---

## MongoDB Introduction

Tutorial：http://www.vue5.com/mongodb/mongodb_1uxs37ih.html

### 用户角色 Roles

#### 数据库用户角色（Database User Roles)

- read : 授权User只读数据的权限，允许用户读取指定的数据库
- readWrite  授权User读/写数据的权限，允许用户读/写指定的数据库
数据库管理角色（Database Admininstration Roles)
- dbAdmin：在当前的数据库中执行管理操作，如索引的创建、删除、统计、查看等
- dbOwner：在当前的数据库中执行任意操作，增、删、改、查等
- userAdmin ：在当前的数据库中管理User，创建、删除和管理用户。
备份和还原角色（Backup and Restoration Roles)
- backup
- restore

#### 跨库角色（All-Database Roles)

- readAnyDatabase：授权在所有的数据库上读取数据的权限，只在admin 中可用
- readWriteAnyDatabase：授权在所有的数据库上读写数据的权限，只在admin 中可用
- userAdminAnyDatabase：授权在所有的数据库上管理User的权限，只在admin中可用
- dbAdminAnyDatabase： 授权管理所有数据库的权限，只在admin 中可用

#### 集群管理角色（Cluster Administration Roles)

- clusterAdmin：授权管理集群的最高权限，只在admin中可用
- clusterManager：授权管理和监控集群的权限
- clusterMonoitor：授权监控集群的权限，对监控工具具有readonly的权限
- hostManager：管理server
  超级角色（super master  Roles)
- root ：超级账户和权限，只在admin中可用

### 基本概念 Basic Concepts

- Collection：类似于RDBMS中的Table
- Document：类似于RDBMS中的Row

### 基本数据类型 Basic Data Structure

- ObjectId：自动生成的唯一标识符
- String：字符串
- Boolean：布尔值
- Integer：整数
- Double：浮点数
- Array：数组
- Object：字典
- null：空值
- Timestamp：时间戳
- date：日期

### 大文件存储 GridFS For Big Files

GridFS 用于存储和恢复那些超过16M（BSON文件限制）的文件(如：图片、音频、视频等)。
GridFS 也是文件存储的一种方式，但是它是存储在MonoDB的集合中。
GridFS 可以更好的存储大于16M的文件。
GridFS 会将大文件对象分割成多个小的chunk(文件片段),一般为256k/个,每个chunk将作为MongoDB的一个文档(document)被存储在chunks集合中。
GridFS 用两个集合来存储一个文件：fs.files与fs.chunks

GridFS存储的基本字段如下图：
[Image]
metadata可展开

使用GridFS作为文件存储的方式，除默认字段外，添加关系型数据库字段到metadata中用于检索
数据库基本结构如下：(每个Bucket包含files和chunks两个collection)

```
CourseFileLib
-ExperimentBucket
--metadata: {eid: "..."}
-AssignmentBucket
--metadata: {asid: "..."}
-SubmissionBucket
--metadata: {asid: "...", sid: "..."}
```



### Development Usage

MongoDB Compass下载：管理可视化管理MongoDB数据
https://www.mongodb.com/try/download/compass
demo代码：简单的RESTful API实现文件上传、下载、删除等操作
远程连接url：Compass/Springboot可用

```shell
# demo db
mongodb://demo-test:test@120.78.65.145:27017/demo?authSource=admin
# CourseFileLib db
mongodb://file-connector:connector@120.78.65.145:27017/CourseLib?authSource=admin

```



#### Demo Repo

https://github.com/Tptrix29/DB-demo

相关配置代码（已完成配置）

#### Docker Config

```dockerfile
# Local

docker run -itd --name mongo-data --restart=always -v ~/IdeaProjects/containers/mongodb/demo/data:/data/db -p 27017:27017 mongo --auth

# Server

docker run -itd --name mongo-data --restart=always -v /home/data/mongo_data:/data/db -p 27017:27017 mongo --auth

# Enter Container

docker exec -it mongo-data bash

# Change Mirror & Install Vim

sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list
sed -i s@/security.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list
apt clean
apt-get update -y
apt-get install vim 

# Change Config to Open port

vim /etc/mongod.conf.orig
```

#### DB Config

##### User

```c++
// 用户创建
use admin
// 创建root
db.createUser({ user:'root',pwd:'iamroot',roles:[ "root"]});
db.auth("root", "iamroot")
// 创建文件读写者
db.createUser({ user:'file-connector',pwd:'connector',roles:[ {role: "readWrite", db: "CourseFileLib"}]});
// 创建demo测试者
db.createUser({ user:'demo-test',pwd:'test',roles:[ {role: "readWrite", db: "demo"}]});
```

##### DB

```c++
// demo测试数据库
use demo
// 测试学生数据
db.students.insertMany( [
   { sid: 22001, name: "Alex", year: 1, score: 4.0 },
   { sid: 21001, name: "bernie", year: 2, score: 3.7 },
   { sid: 20010, name: "Chris", year: 3, score: 2.5 },
   { sid: 22021, name: "Drew", year: 1, score: 3.2 },
   { sid: 17301, name: "harley", year: 6, score: 3.1 },
   { sid: 21022, name: "Farmer", year: 1, score: 2.2 },
   { sid: 20020, name: "george", year: 3, score: 2.8 },
   { sid: 18020, name: "Harley", year: 5, score: 2.8 },
] )

// CrouseFileLib数据库：文件操作通过Java API实现
use CourseFileLib
```





## Redis Introduction

Key-value database

### Data Type

1. String
2. Lists
3. Sets
4. Hashes
5. Sorted sets
6. Streams
7. Geospatial
8. HyperLogLog
9. Bitmaps
10. Bitfields
