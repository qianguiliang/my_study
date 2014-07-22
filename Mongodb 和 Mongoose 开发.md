MongoDB 和 Mongoose
===================

MongoDB
-======
1. mongodb 介绍
2. mongodb 入门
3. 建立，修改和删除文档
4. 查询


1. mongodb 介绍
--------------
* 容易使用
    * 面向文档的数据库
    * 行 -> 文档，文档可以具有负载的层级关系
    * 没有固定的 Schema，keys 和 values 可变
* 容易伸缩
    * 伸缩可以向上或者横向
    * mongodb 采用横向
* 特点
    * 索引
    * 聚合
    * 特殊集合类型 time-to-live
    * 文件存储 （大文件和文件元数据）
* 高性能
  
2. mongodb 入门
--------------
### 文档 Documents
* mongodb 核心是文档：有序的键值对

    ```
    {"greeting": "hello, world!"}
    ```
    
* 多数文档有着复杂的键值对
    
    ```
    {"greeting" : "Hello, world!", "foo" : 3}
    ```
    
    * Document 中的 Key 是 字符串，UTF-8
        * 不包含 \0
        * . $ 特殊含义
    * 类型敏感，大小写敏感
    
        ```
        {"foo" : 3} != {"foo" : "3"}
        {"foo" : 3} != {"Foo" : 3}
        ```
    * 不能含有重复 Key
    * 键值对是有序的，但应当避免
        
        ```
        {"x" : 1, "y" : 2} != {"y" : 2, "x" : 1}
        ```
    
### 集合 Collection
* Collection 是一组文档
* 集合中的文档类型可以不同

    ```
    {"greeting" : "Hello, world!"}
    {"foo" : 5}
    ```
    
* 既然集合中的文档类型可以不同，为什么还要建立集合？不可以将所有文档放到同一个集合中吗？
    * 不同类型的文档在同一个集合中是程序员和管理员的恶梦
    * 在集合中抽取某一类型的列表要慢于从集合中获取列表
    * 将相同类型的文档放置到相同的集合允许数据集中存储
    * 索引需要结构
    
* 命名
    * utf-8
    * 不能是 ""
    * 不包含 \0
    * 不能以 system. 开头
    * 不包含保留 $
    
    * 子集合
        * 利用 . 作为命名空间
        * blog.posts, blog.authors
        * 仅是命名组织问题，不存在真正的父子关系，没有 blog 集合

### 数据库 Database
* 一组集合
* 一个 mongodb 实例可以存在多个数据库
* 每个数据库都有自己的权限和单独的存储文件
* 同一个应用应该存储在同一个数据库
* 数据库命名
    * UTF-8
    * 不能是 ""
    * 不包含 /, \, ., ", *, <, >, :, |, ?, $, (a single space), \0 
    * 大小写敏感，即使在大小写不敏感的系统上，推荐全部小写
    * 最大 64Bytes
    * 保留名
        * admin 根（root）数据库
        * local 不会被复制
        * config
* 全限定名：数据库.集合.子集合，全限定名不要超过100Bytes

### 获取和启动 mongodb
* www.mongodb.org
* mongod
    * 默认数据目录 /data/db
    * --dbpath 指定
    * 27017 port

### mongodb shell
* javascript shell
    ```
    mongo
    x = 200
    x / 5
    Math.sin(Math.PI / 2)
    new Date("2014/7/20")
    "Hello, World!".replace("World", "MongoDB")
    function factorial (n) {
    ... if (n <= 1) return 1;
    ... return n * factorial(n - 1); 
    ... }
    factorial(5)
    ```
    
* mongodb client

    ```
    db
    use foobar
    db
    ```

* 数据类型
    * JSON-like -> BSON
    * null
    * boolean
    * number
    * string
    * date
    * regular expression
    * array
    * embedded document
        
        ```
        {"x" : {"foo" : "bar"}}
        ```
        
    * object id
    
        ```
        {"x" : ObjectId()}
        ```
        
    * binary data
    * code
    
        ```
        {"x" : function() { /* ... */ }}
        ```
* _id 和 ObjectId
    * 每个文档都有一个 "_id" key
    * 可以是任何类型，缺省是 ObjectId
    * ObjectId 12 Bytes
    
        ```
        0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10  | 11 
        Timestamp     | Machine   | PID   | Increment
        ```
        
    * 文档中没有 _id，自动生成

### MongoDB Shell
* 连接方式

    ```
    mongo some-host:30000/myDB
    mongo --nodb
    conn = new Mongo("some-host:30000")
    db = conn.getDB("myDB")
    
    help
    db.help()
    db.foo.help()
    db.foo.update
    
    mongo script1.js script2.js script3.js
    mongo --quiet server-1:30000/foo script1.js script2.js script3.js
    load("script1.js")
    ```
    
3. 建立，修改和删除文档
-------------------
#### 插入和保存文档
* 插入文档
    
    ```
    db.foo.insert({"bar" : "baz"})
    ```
    
* 批量插入：多个文档到一个集合

    ```
    db.foo.batchInsert([{"_id" : 0}, {"_id" : 1}, {"_id" : 2}])
    db.foo.find()
    ```
    
    * 插入大量的原始数据（从其他数据库导出的数据）请使用 mongoimport 
    * 不能插入大于 48MB 的文档，如果插入大于 48MB 的文档，驱动程序可能会将文档拆分
    * 中间失败前面部分完成，后面取消
    * option：continueOnError，继续插入失败之后的文档
* 插入验证
    * 验证文档大小
    * 添加 _id
    
### 移除文档
* 移除集合中的全部文档

    ```
    db.foo.remove() //移除集合中的全部文档，保留集合
    db.mailing.list.remove({"opt-out" : true})
    db.foo.drop() //清除整个集合最好是 drop ，不是 remove
    ```

### 修改文档
* 参数：要修改的文档，修改之后的文档
    * 文档替换
    
        ```
        db.user.insert({
           "_id" : ObjectId("4b2b9f67a1f631733d917a7a"),
           "name" : "joe",
           "friends" : 32,
           "enemies" : 2
        }
        
        var joe = db.users.findOne({"name" : "joe"});
        joe.relationships = {"friends" : joe.friends, "enemies" : joe.enemies};
        joe.username = joe.name;
        delete joe.friends
        delete joe.enemies
        delete joe.name
        
        db.users.update({"name" : "joe"}, joe)
        ```
    
    * 最好利用 _id 作为查找方式
    
        ```
        有以下数据
        {"_id" : ObjectId("4b2b9f67a1f631733d917a7b"), "name" : "joe", "age" : 65},
        {"_id" : ObjectId("4b2b9f67a1f631733d917a7c"), "name" : "joe", "age" : 20},
        {"_id" : ObjectId("4b2b9f67a1f631733d917a7d"), "name" : "joe", "age" : 49},
        
        joe = db.people.findOne({"name" : "joe", "age" : 20}；
        joe.age++;
        db.people.update({"name" : "joe"}, joe);
        
        ** E11001 duplicate key on update **
        
        db.people.update({"_id" : ObjectId("4b2b9f67a1f631733d917a7c")}, joe)
        ```

#### 使用修改器 Modifiers
* $set 设置某一字段，不存在则建立 $unset 移除某一字段

    ```
    db.users.findOne()
        {
            "_id" : ObjectId("4b253b067525f35f94b60a31"),
            "name" : "joe",
            "age" : 30,
            "sex" : "male",
            "location" : "Wisconsin"
        }
        
    db.users.update({"_id" : ObjectId("4b253b067525f35f94b60a31")},
        {"$set" : {"favorite book" : "War and Peace"}})
        
        
    db.users.update({"name" : "joe"},
        {"$set" : {"favorite book" : ["Cat's Cradle", "Foundation Trilogy", "Ender's Game"]}})
        
    db.users.update({"name" : "joe"},
        {"$unset" : {"favorite book" : 1}})    

    {
        "_id" : ObjectId("4b253b067525f35f94b60a31"),
        "title" : "A Blog Post",
        "content" : "...",
        "author" : {
            "name" : "joe",
            "email" : "joe@example.com"
        }
    }
    
    db.blog.posts.update({"author.name" : "joe"},
        {"$set" : {"author.name" : "joe schmoe"}})
    ```
    
* 必须总是使用 $-modifier 来添加、更改、或者移除 Key，不要犯下面的错误

    ```    
    db.coll.update(criteria, {"foo" : "bar"}) //全文档替换    
    ```
    
* $inc 用于 number

    ```
    db.games.update({"game" : "pinball", "user" : "joe"},
        {"$inc" : {"score" : 50}})
    ```
        
* 数组
    
    ```
    db.blog.posts.update({"title" : "A blog post"},
        {"$push" : {"comments" :
            {"name" : "joe", "email" : "joe@example.com", "content" : "nice post."}}})
            
    db.stock.ticker.update({"_id" : "GOOG"},
        {"$push" : {"hourly" : {"$each" : [562.776, 562.790, 559.123]}}})
        
    db.users.update({"_id" : ObjectId("4b2d75476cc613d5ee930164")},
        {"$addToSet" : {"emails" : "joe@gmail.com"}}) //避免重复
        
    {"$pop" : {"key" : 1}}  //数组末端
    {"$pop" : {"key" : -1}} //数组顶端
    db.lists.update({}, {"$pull" : {"todo" : "laundry"}}) //特定数组元素
    
    {
        "_id" : ObjectId("4b329a216cc613d5ee930192"),
        "content" : "...",
        "comments" : [
            {
                "comment" : "good post",
                "author" : "John",
                "votes" : 0
            },
            {
                "comment" : "i thought it was too short",
                 "author" : "Claire",
                 "votes" : 3
            }, 
            {
                "comment" : "free watches",
                "author" : "Alice",
                "votes" : -1
            } 
        ]
    }
    
    // $ 数组中元素的 index
    db.blog.update({"comments.author" : "John"},
        {"$set" : {"comments.$.author" : "Jim"}})
        
* 修改器速度
    * $inc 不修改文档大小，效率高
    * 改变文档大小将引起文档移动
    
        ```
        > db.coll.insert({"x" :"a"})
        > db.coll.insert({"x" :"b"})
        > db.coll.insert({"x" :"c"})
        > db.coll.find()
        { "_id" : ObjectId("507c3581d87d6a342e1c81d3"), "x" : "a" }
        { "_id" : ObjectId("507c3583d87d6a342e1c81d4"), "x" : "b" }
        { "_id" : ObjectId("507c3585d87d6a342e1c81d5"), "x" : "c" }
        > db.coll.update({"x" : "b"}, {$set: {"x" : "bbb"}})
        > db.coll.find()
        { "_id" : ObjectId("507c3581d87d6a342e1c81d3"), "x" : "a" }
        { "_id" : ObjectId("507c3585d87d6a342e1c81d5"), "x" : "c" }
        { "_id" : ObjectId("507c3583d87d6a342e1c81d4"), "x" : "bbb" }
        ```
 
#### Upserts
* 特殊类型的 update，如果文档没有找到，组合查询条件建立新文档

    ```
    db.analytics.update({"url" : "/blog"}, {"$inc" : {"pageviews" : 1}}, true)
    
    //仅仅在创建文档时应用，再次执行不会修改
    db.users.update({}, {"$setOnInsert" : {"createdAt" : new Date()}}, true)
    ```
                
* 默认修改仅修改第一个匹配的文档，设置第四个参数为 true，修改全部匹配文档
    
    ```
    db.users.update({"birthday" : "10/13/1978"},
    ... {"$set" : {"gift" : "Happy Birthday!"}}, false, true)
    ```
    
4. 查询
------
### 查询入门

    ```
    db.c.find()
    db.users.find({"age" : 27})
    db.users.find({"username" : "joe"})
    db.users.find({"username" : "joe", "age" : 27})
    ```
    
### 指定返回字段

    ```
    db.users.find({}, {"username" : 1, "email" : 1})
    db.users.find({}, {"fatal_weakness" : 0})
    ```
    
### 查询条件
             
    ```
    db.users.find({"age" : {"$gte" : 18, "$lte" : 30}})
    start = new Date("01/01/2007")
    db.users.find({"registered" : {"$lt" : start}})
    db.users.find({"username" : {"$ne" : "joe"}})
    db.raffle.find({"ticket_no" : {"$in" : [725, 542, 390]}})
    db.raffle.find({"ticket_no" : {"$nin" : [725, 542, 390]}})
    db.raffle.find({"$or" : [{"ticket_no" : 725}, {"winner" : true}]})
    db.raffle.find({"$or" : [{"ticket_no" : {"$in" : [725, 542, 390]}}, {"winner" : true}]})
    db.users.find({"id_num" : {"$mod" : [5, 1]}}) // 1, 6, 11, 16...
    db.users.find({"id_num" : {"$not" : {"$mod" : [5, 1]}}}) //2, 3, 4, 5, 7, 8, ...
    db.users.find({"age" : {"$lt" : 30, "$gt" : 20}})
    ```
    
### 类型查询
    
    ```
    db.c.find({"y" : null}) //不存在 y key 的文档也被匹配
    db.c.find({"z" : {"$in" : [null], "$exists" : true}}) //不能使用 $eq
    db.users.find({"name" : /joe/i})
    db.users.find({"name" : /joey?/i})
    
    db.food.insert({"fruit" : ["apple", "banana", "peach"]})
    db.food.find({"fruit" : "banana"})

    > db.food.insert({"_id" : 1, "fruit" : ["apple", "banana", "peach"]})
    > db.food.insert({"_id" : 2, "fruit" : ["apple", "kumquat", "orange"]})
    > db.food.insert({"_id" : 3, "fruit" : ["cherry", "banana", "apple"]})    
    db.food.find({fruit : {$all : ["apple", "banana"]}})
    
    db.food.find({"fruit" : ["apple", "banana", "peach"]}) //第一个
    db.food.find({"fruit" : ["apple", "banana"]}) //no
    db.food.find({"fruit" : ["banana", "apple", "peach"]}) //no
    db.food.find({"fruit.2" : "peach"})
    db.food.find({"fruit" : {"$size" : 3}})
    
    db.blog.posts.findOne(criteria, {"comments" : {"$slice" : 10}}) //返回符合文档的前 10 个 comments
    db.blog.posts.findOne(criteria, {"comments" : {"$slice" : -10}})
    db.blog.posts.findOne(criteria, {"comments" : {"$slice" : [23, 10]}})
    db.blog.posts.find({"comments.name" : "bob"}, {"comments.$" : 1})
    ```
    
#### 查询嵌入文档

    ```
    {
        "name" : {
            "first" : "Joe",
            "last" : "Schmoe"
        },
        "age" : 45 
    }
    
    db.people.find({"name" : {"first" : "Joe", "last" : "Schmoe"}})
    db.people.find({"name.first" : "Joe", "name.last" : "Schmoe"})
    
    {
        "content" : "...",
        "comments" : [
            {
                "author" : "joe",
                "score" : 3,
                "comment" : "nice post"
            }, 
            {
                "author" : "mary",
                "score" : 6,
                "comment" : "terrible post"
            } 
        ]
    }
    
    //不可以，嵌入文档必须匹配子文档，下面缺少 comment key
    db.blog.find({"comments" : {"author" : "joe", "score" : {"$gte" : 5}}})
    // 可以使用
    db.blog.find({"comments" : {"$elemMatch" : {"author" : "joe", "score" : {"$gte" : 5}}}})
    ```
    
### $Where

    ```
    db.foo.find({"$where" : function () {...});
    ```
 
### 游标

    ```
    var cursor = db.collection.find()；
    while (cursor.hasNext()) {
        obj = cursor.next(); 
        // do stuff
    }
    
    cursor.forEach(function(x) { 
        ...
    }
    
    var cursor = db.foo.find().sort({"x" : 1}).limit(1).skip(10);
    var cursor = db.foo.find().limit(1).sort({"x" : 1}).skip(10);
    var cursor = db.foo.find().skip(10).limit(1).sort({"x" : 1});
    // 查询没有执行，直到 cursor.next()
    
    db.c.find().limit(3)
    db.c.find().skip(3)
    db.c.find().sort({username : 1, age : -1}) //1 升序 -1 降序
    db.stock.find({"desc" : "mp3"}).limit(50).sort({"price" : -1})
    db.stock.find({"desc" : "mp3"}).limit(50).skip(50).sort({"price" : -1})
    ```
    
### 数据库命令
    
    ```
    db.runCommand({"drop" : "test"}); == db.test.drop()
    
    db.count.update({x : 1}, {$inc : {x : 1}}, false, true)
    db.runCommand({getLastError : 1})
    ```
    
Mongoose
========
Mongoose 入门
------------
* mongoose 是运行在 node 下的 mongodb 的对象模型工具

### mongoose 概念
* Schemas：描述文档的数据结构

    ```
    var userSchema = new mongoose.Schema({
         name: String,
         email: String,
         createdOn: Date,
         verified: Boolean
    });
    ```
    
* Models：一个 model 是 schema 的编译版本，一个 model 实例对应一个数据库中的文档

    ```
    var User = mongoose.model('User', userSchema);
    ```
    
* 安装 mongoose

    ```
    npm install mongoose --save
    ```
    
连接到数据库
----------
### 缺省连接

    ```
    var dbURI = 'mongodb://localhost/mydatabase';
    mongoose.connect(dbURI);
    ```
    
### 多个连接

    ```
    var dbURI = 'mongodb://localhost/myadmindatabase';
    var adminConnection = mongoose.createConnection(dbURI);
    ```

### 连接字符串

    ```
    var dbURI = 'mongodb://localhost:27018/mydatabase';
    var dbURI = 'mongodb://username:password@localhost/mydatabase';
    ```
    
### 连接选项

    ```
    var dbURI = 'mongodb://localhost/mydatabase';
    var dbOptions = {'user':'db_username','pass':'db_password'};
    mongoose.connect(dbURI, dbOptions);
    ```
    
### 关闭连接

    ```
    mongoose.connection.close(function () {
         console.log('Mongoose default connection closed');
    });
    adminConnection.close(function () {
         console.log('Mongoose connection adminConnection closed');
    });
    
    // Node 进程结束时关闭
    process.on('SIGINT', function() {
         mongoose.connection.close(function () {
           console.log('Mongoose disconnected through app termination');
           process.exit(0);
         });
    });
    ```
    
### 连接事件

    ```
    mongoose.connection.on('error',function (err) {
         console.log('Mongoose connection error: ' + err);
    });
    ```
    
Schemas & Models
----------------
### Schema 简介

* schema 描述数据结构的一种方法

    ```
    var userSchema = new mongoose.Schema({
         name: String,
         email: String,
         createdOn: Date
    });
    ```

### schema 中允许的数据类型
* String
    * utf-8
* Number
* Date
* Boolean
* Buffer
    * 用于存储二进制信息
* ObjectId
* Mixed
    * 可以包含任意类型
    
        ```
        vardjSchema= new mongoose.Schema({
          mixedUp: {}
        });
        vardjSchema= new mongoose.Schema({
          mixedUp: Schema.Types.Mixed
        });
        ```
    
    * 对此类型的修改，mongoose 不能自动检测并保存
    
        ```
        dj.mixedUp = { valueone: "a new value" };
        dj.markModified('mixedUp');
        dj.save()
        ```
         
* Array
    * 空数组被认为是 mixed
    

### 什么时候定义 Schema
* schema 应该在引入 mongoose 之后

### 定义 schema
* 定义基本结构

    ```
    var userSchema = new mongoose.Schema({
         name: String,
         email: String,
         createdOn: Date
    });
    
* 修改 schema 不需要重构数据库

    ```
    var userSchema = new mongoose.Schema({
         name: String,
         email: String,
         createdOn: Date,
         modifiedOn: Date,
         lastLogin: Date
    });

* 设置缺省值

    ```
    createdOn: Date
    等于
    createdOn: { type: Date }
    修改为
    createdOn: { type: Date, default: Date.now }
    defautl 为 js 保留字，最好
    createdOn: { type: Date, 'default': Date.now }
    ```
    
* 唯一值

    ```
    email: {type: String, unique:true}
    ```
    
    * 插入不唯一值时，mongodb 返回 E11000 错误
 
* 最后结果

    ```
    var userSchema = new mongoose.Schema({
      name: String,
      email: {type: String, unique:true},
      createdOn: { type: Date, default: Date.now },
      modifiedOn: Date,
      lastLogin: Date
    });
    ```
    
    * 一个结果例子
    
        ```
        { 
          "__v" : 0,
          "_id" : ObjectId("5126b7a1f8a44d1e32000001"),
          "createdOn" : ISODate("2013-02-22T00:11:13.436Z"),
          "email" : "simon@theholmesoffice.com",
          "lastLogin" : ISODate("2013-04-03T12:54:42.734Z"),
          "modifiedOn" : ISODate("2013-04-03T12:56:26.009Z"),
          "name" : "Simon Holmes" 
        }
        ```
        
* "__v" 什么东西？
    * mongoose 内部在建立文档时设置的版本号
    * 为什么需要？假设需要存取一个数组元素，myArray[3]，但同时有人删除了 2，这时原始数据存储在 2 中，保存将会覆盖在新的3也就是原先的4中。__V 避免此情况出现。
    
* 定义项目 schema

    ```
    var projectSchema = new mongoose.Schema({
         projectName: String,
         createdOn: Date,
         modifiedOn: { type: Date, default: Date.now },
         createdBy: String,
         tasks: String
    });
    ```
 
### 定义模型
* 模型是数据库中文档的1:1映射
* 使用连接建立模型

    ```
    mongoose.model( 'User', userSchema );
    adminConnection.model( 'User', userSchema );
    ```

* 模型实例

    ```
    var userOne = new User({ name: 'Simon' });
    var userTwo = new User({ name: 'Sally' });
    ```
    
* 同模型交互

    ```
    console.log(userOne.name); // 'Simon'
    userOne.name = 'Simon Holmes';
    console.log(userOne.name); // 'Simon Holmes'
    
    userTwo.save(function (err) {
      if (err) return handleError(err);
      // userTwo (Sally) is saved!
    });
    userOne.save(function (err) {
      if (err) return handleError(err);
      // userOne (Simon Holmes) is saved!
    });
    ```
    
* 查找单实例

    ```
    User.findOne({'name' : 'Sally', function(err,user) {
       if(!err){
         console.log(user);
       } 
    });
    ```
    
* 查找多实例

    ```
    User.find({}, function(err, users) {
      if(!err){
        console.log(users);
      }
    });
    ```
    
### 设置集合名字
* 默认集合是模型的小写复数名

    ```
    mongoose.model( 'User', userSchema );
    Collection： users
    ```

* 在 schema 中定义集合名

    ```
    var userSchema = new mongoose.Schema({
         name: String,
         email: {
           type: String, unique:true
         }
    }, {
         collection: 'myuserlist'
    });
    ```
    
* 在模型中定义集合名
    
    ```
    mongoose.model( 'User', userSchema, 'myuserlist' );
    ```
    
* model/db.js

    ```
    /**
     *
     * Created by zhangjinglin on 14-7-22.
     */
    
    var mongoose = require('mongoose')
    var dbURI = 'mongodb://localhost/MongoosePM'
    mongoose.connect(dbURI)
       
    /* ********************************************
     USER SCHEMA
     ******************************************** */
    var userSchema = new mongoose.Schema({
      name: String,
      email: {type: String, unique: true},
      createOn: {type: Date, defaults: Date.now},
      modifiedOn: Date,
      lastLogin: Date
    })
    
    mongoose.model('User', userSchema)
    
    /* ********************************************
     PROJECT SCHEMA
     ******************************************** */
    var projectSchema = new mongoose.Schema({
      projectName: String,
      createOn: {type: Date, default: Date.now},
      modifiedOn: Date,
      createBy: String,
      contributors: String,
      tasks: String
    })
    
    mongoose.model('Project', projectSchema)
    ```
        
数据交互 - 入门
-------------
### 模型方法和实例方法
* CRUD(Create, Read, Update, Delete)

    ```
    User.create
    User.find
    User.update
    User.remove
    ```

### 设置项目
* 代码结构
    * app.js
        
        ```
        /**
         *
         * Created by zhangjinglin on 14-7-22.
         */
        
        var express = require('express')
          , bodyParser = require('body-parser')
          , cookieSession = require('cookie-session')
          , db = require('./model/db')
          , user = require('./route/user')
          , project = require('./route/project')
        
        
        var app = express()
        
        app.set('view engine', 'jade')
        app.set('views', './views')
        app.use(express.static('./public'))
        
        app.use(bodyParser.urlencoded({extended: true}))
        app.use(cookieSession({secret: 'zjl'}))
        
        var userRoute = express.Router()
        userRoute.get('/', user.index)
        userRoute.route('/new').get(user.create).post(user.doCreate)
        userRoute.route('/edit').get(user.edit).post(user.doEdit)
        userRoute.route('/delete').get(user.confirmDelete).post(user.doDelete)
        
        app.use('/user', userRoute)
        
        app.route('/login').get(user.login).post(user.doLogin)
        app.get('/logout', user.doLogout)
        
        var projectRoute = express.Router()
        projectRoute.route('/new').get(project.create).post(project.doCreate)
        projectRoute.get('/:id', project.displayInfo)
        projectRoute.route('/edit/:id').get(project.edit).post(project.doEdit)
        projectRoute.route('/delete/:id').get(project.confirmDelete).post(project.doDelete)
        
        app.use('/project', projectRoute)
        
        app.listen(3000)
        ```
        
    * route/user.js
    
        ```
        /**
         *
         * Created by zhangjinglin on 14-7-22.
         */
        
        var mongoose = require('mongoose')
        var User = mongoose.model('User')
        
        exports.index = function(req, res) {
        
        }
        
        exports.create = function(req, res) {
          
        }
        
        exports.doCreate = function(req, res) {
        
        }
        
        exports.edit = function(req, res) {
        
        }
        
        exports.doEdit = function(req, res) {
        
        }
        
        exports.confirmDelete = function(req, res) {
        
        }
        
        exports.doDelete = function(req, res) {
        
        }
        
        exports.login = function(req, res) {
        
        }
        
        exports.doLogin = function(req, res) {
        
        }
        
        exports.doLogout = function(req, res) {
        
        }
        ```
        
    * route/project.js
    
        ```
        /**
         *
         * Created by zhangjinglin on 14-7-22.
         */
        
        var mongoose = require('mongoose')
        var Project = mongoose.model('Project')
        
        exports.create = function(req, res) {
          
        }
        
        exports.doCreate = function(req, res) {
          
        }
        
        exports.displayInfo = function(req, res) {
          
        }
        
        exports.edit = function(req, res) {
          
        }
        
        exports.doEdit = function(req, res) {
        
        }
        
        exports.confirmDelete = function(req, res) {
          
        }
        
        exports.doDelete = function(req, res) {
          
        }
        ```
        
    * views/layout.jade
    
        ```
        //   Created by zhangjinglin on 14-7-22.
        doctype html
        html
          head
            title= title
            link(rel='stylesheet', href='/css/style.css')
          body
            block content
        ```
        
数据交互 - 新建
-------------
### 新建实例
* 新建并设置数据

    ```
    var newUser = new User();
    newUser.name = 'Simon Holmes';
    
    var newUser = new User({
         name: 'Simon Holmes',
         email: 'simon@theholmesoffice.com',
         lastLogin : Date.now()
    });

    var newUser = new User({
      email: 'simon@theholmesoffice.com',
      lastLogin : Date.now()
    });
    newUser.name = 'Simon Holmes';
    ```
    
### 保存实例

    ```
    newUser.save( function( err ){
         if(!err){
           console.log('User saved!');
         }
    });
    ```
  
* 使用保存的数据

    ```
    newUser.save( function( err, user ){
         if(!err){
           console.log('Saved user name: ' + user.name);
           console.log('_id of saved user: ' + user._id);
         }
    });
    ```
    
### 建立同时保存数据

    ```
    var newUser = new User({
         name: 'Simon Holmes',
         email: 'simon@theholmesoffice.com',
         lastLogin : Date.now()
       }).save( function( err ){
         if(!err){
           console.log('User saved!');
         }
    });
    
    User.create({
         name: 'Simon Holmes',
         email: 'simon@theholmesoffice.com',
         lastLogin : Date.now()
       }, function( err, user ){
         if(!err){
           console.log('User saved!');
           console.log('Saved user name: ' + user.name);
           console.log('_id of saved user: ' + user._id);
    } });
    
    
### CRUD - Create
* 添加 user-form.jade

    ```
    //    Created by zhangjinglin on 14-7-22.
    extends layout
    
    block content
      h1= title
      form(id='frmUserProfile', method='post', action='')
        lable(for='name') Name
          input(name='name')
        label(for='email') Email
          input(name='email')
        input(type='submit', value='Create')
    ```
    
* 连接路由和视图

    ```
    exports.create = function(req, res) {
      res.render('user-form', {title: 'Create user'})
    }
    
    exports.doCreate = function(req, res) {
      User.create({
        name: req.body.name,
        email: req.body.email,
        modifiedOn: Date.now(),
        lastLogin: Date.now()
      }, function(err, user) {
        if (!err) {
          console.log('User created and saved: ' + user)
        }
      })
    }
    ```
    
    * 错误处理：例如，电子邮件地址存在
    
        ```
        exports.doCreate = function(req, res) {
          User.create({
            name: req.body.name,
            email: req.body.email,
            modifiedOn: Date.now(),
            lastLogin: Date.now()
          }, function(err, user) {
            if (err) {
              console.log(err)
              if (err.code == 11000) {
                res.redirect('/user/new?exists=true')
              } else {
                res.redirect('/user/error=true')
              }
            } else {
              console.log('User created and saved: ' + user)
            }
          })
        }
        ```
        
    * 建立用户 session
    
        ```
        exports.doCreate = function(req, res) {
          User.create({
            name: req.body.name,
            email: req.body.email,
            modifiedOn: Date.now(),
            lastLogin: Date.now()
          }, function(err, user) {
            if (err) {
              console.log(err)
              if (err.code == 11000) {
                res.redirect('/user/new?exists=true')
              } else {
                res.redirect('/user/error=true')
              }
            } else {
              console.log('User created and saved: ' + user)
              req.session.user = {'name': user.name, 'email': user.email, '_id': user._id}
              req.session.loggedIn = true
              res.redirect('/user')
            }
          })
        }
        ```

* user-page.jade

    ```
    //    Created by zhangjinglin on 14-7-22.
    
    extends layout
    
    block content
      h1 Mongoose Project Management
      h2= name
      p Email: #{email}
      h3 Actions
      ul
        li
          a(href='/project/new') Create a new project
        li
          a(href='/user/edit') Edit user info
        li
          a(href='/user/delete') Delete #{name}
        li
          a(href='/logout') Logout #{name}
    ```

* 对 Project 的访问必须是登录用户，修改 app.js

    ```
    projectRoute.use(function(req, res, next) {
      if (req.session.loggedIn === true) {
        next();
      } else {
        res.redirect('/login')
      }
    })
    ```

* project.js

    ```
    exports.create = function(req, res) {
      res.render('project-form', {
        title: 'Create Project',
        userid: req.session.user._id,
        userName: req.session.user.name
      });
    }
    ```
        
* project-form.jade

    ```
    //   Created by zhangjinglin on 14-7-22.
    
    extends layout
    
    block content
      h1= title
      form(method="post", action="")
        p Project owner: #{userName}
        input(type="hidden", name="userid", value="#{userid}")
        label(for="projectName") Project name
          input(type="text", name="projectName")
        br
        label(for="tasks") Tasks
          textarea(name="tasks")
        br
        input(type="submit", value="Create Project")
    ```
    
* project.js
    
    ```
    exports.doCreate = function(req, res) {
      Project.create({
        projectName: req.body.projectName,
        createBy: req.body.userid,
        createOn: Date.now(),
        tasks: req.body.tasks
      }, function(err, project) {
        if (err) {
          console.log(err);
          res.redirect('/user?error=project');
        } else {
          console.log('Project created and saved: ' + project)
          console.log('Project._id = ' + project._id)
          res.redirect('/project/' + project._id)
        }
      })
    }
    
    exports.displayInfo = function(req, res) {
      if (req.params.id) {
        Project.findById(req.params.id, function(err, project) {
          if (err) {
            console.log(err);
            res.redirect('/user?404=project')
          } else {
            res.render('project-page', {
              title: project.projectName,
              projectName: project.projectName,
              tasks: project.tasks,
              createBy: project.createBy,
              projectId: req.param.id
            })
          }
        })
      } else {
        res.redirect('/user')
      }
    }
    ```
    
* project-page.jade

    ```
    //   Created by zhangjinglin on 14-7-22.
    extends layout
    
    block content
      h1 Mongoose Project Management
      h2= projectName
      p Created by: #{createBy}
      p Tasks: #{tasks}
      h3 Actions
      ul
        li
          a(href="/project/new") Create a new project
        li
          a(href="/project/edit/#{projectID}") Edit project info
        li
          a(href="/project/delete/#{projectID}") Delete #{projectName}
        li
          a(href="/user") My profile
    ```
    
* index
   
   ```
   exports.index = function(req, res) {
     if (req.session.loggedIn === true) {
       res.render('user-page', {
         title: req.session.user.name,
         name: req.session.user.name,
         email: req.session.user.email,
         userId: req.session.user._id
       })
     } else {
       res.redirect('/login')
     }
   }
   ```
   
数据 - 读取，查询和查找
-------------------
### 查找数据
* 使用 QueryBuilder

    ```
    var myQuery = User.find({'name' : 'Simon Holmes'});
    myQuery.where('age').gt(18);
    myQuery.sort('-lastLogin');
    myQuery.select('_id name email');
    myQuery.exec(function (err, users){
       if (!err){
         console.log(users); // output array of users found
       } 
    });
    
    ** chain method **
    
    User.find({'name' : 'Simon Holmes'})
       .where('age').gt(18)
       .sort('-lastLogin')
       .select('_id name email')
       .exec(function (err, users){
         if (!err){
           console.log(users); // output array of users found
    ￼    } 
    });
    
    组合也行
    
    var myQuery = User.find({'name' : 'Simon Holmes'})
       .where('age').gt(18)
       .sort('-lastLogin')
       .select('_id name email');
       // do some other operations
       // and then...
    myQuery.exec(function (err, users){
      if (!err){
        console.log(users); // output array of users found
      }
    });
    
    ```

* 单查询操作

    ```
    Model.find(conditions, [fields], [options], [callback])
    
    User.find(
      {'name' : 'Simon Holmes'}, // users called Simon Holmes
      function (err, users){
        if (!err){console.log(users);}
    });
    
    User.find(
      {'name' : 'Simon Holmes'}, // users called Simon Holmes
      'name email', // returning just the name and email fields
      function (err, users){
        if (!err){console.log(users);}
    });
       
    User.find(
      {'name' : 'Simon Holmes'}, // users called Simon Holmes
      null, // returning all fields in model
      {sort : {lastLogin : -1}}, // sorted by lastLogin descending
      function (err, users){
        if (!err){console.log(users);}
    });
    ```
    
* 辅助方法
    * Model.find(query) 返回匹配的数组
    * Model.findOne(query) 第一个匹配的文档
    * Model.findById(ObjectID) 返回指定的 OjectID 的文档
    

### CRUD - 读取数据
* login-form.jade

    ```
    //   Created by zhangjinglin on 14-7-22.
    extends layout
    
    block content
      h1= title
      form(method="post", action="")
        label(for="Email") Email
          input(type="email", name="Email")
        input(type="submit", value="Login")
        p Or new users can
          a(href="/user/new") sign up
    
    
    exports.login = function(req, res) {
      res.render('login-form', {title: 'Log in'})
    }
    
    exports.doLogin = function(req, res) {
      if (req.body.email) {
        User.findOne(
          {email: req.body.email},
          '_id name email',
          function(err, user) {
            if (!err) {
              if (!user) {
                res.redirect('/login?404=user')
              } else {
                req.session.user = {
                  'name': user.name,
                  'email': user.email,
                  '_id': user._id
                };
                req.session.loggedIn = true;
                console.log('Logged in user: ' + user)
                res.redirect('/user')
              }
            } else {
              res.redirect('/login?404=error')
            }
          }
        )
      } else {
        res.redirect('/login?404=error')
      }
    }
    ```
    
* index.jade

    ```
    //   Created by zhangjinglin on 14-7-22.
    extends layout
    
    block content
      ul
        li
          a(href='/login') Login
        li
          a(href='/user/new') Sign up

    app.get('/', function(req, res) {
      res.render('index')
    })
          
* 返回 JSON to AJAX
    * db.js
    
        ```
        projectSchema.statics.findByUserId = function(userid, callback) {
          this.find(
            {createBy: userid},
            '_id projectName',
            {sort: 'modifiedOn'},
            callback
          )
        }
        ```
        
        
    * app.js
    
        ```
        projectRoute.get('/byuser/:userid', project.byUser)
        ```
    
    *  project.js
    
        ```
        exports.byUser = function(req, res) {
          if (req.params.userid) {
            Project.findByUserId(req.params.userid, function(err, projects) {
                if (!err) {
                  res.json(projects)
                } else {
                  res.json({status: 'error', error: 'Error finding projects'})
                }
            })
          } else {
            res.json({"status":"error", "error":"No user id supplied"});
          }
        }
        ```
    
    * user-page.jade
    
        ```
        //    Created by zhangjinglin on 14-7-22.
        
        extends layout
        
        block content
          h1 Mongoose Project Management
          h2= name
          p Email: #{email}
          h3 Actions
          ul
            li
              a(href='/project/new') Create a new project
            li
              a(href='/user/edit') Edit user info
            li
              a(href='/user/delete') Delete #{name}
            li
              a(href='/logout') Logout #{name}
        
          h3 My projects
          ul#myprojects
            li loading....
        
          script(src='/js/jquery.js')
          script.
            var userId = "#{userId}";
          script(src='/js/user.js')
          ```
          
    * public/js/user.js
    
        ```
        /**
         *
         * Created by zhangjinglin on 14-7-22.
         */
        
        $(function() {
          var output = '';
          $.ajax('/project/byuser/' + userId, {
            dataType: 'json',
            error: function() {
              console.log('ajax error :(')
            },
            success: function(data) {
              if (data.length > 0) {
                if (data.status && data.status === 'error') {
                  output = '<li>Error: ' + data.error + '</li>'
                } else {
                  $(data).each(function(index, item) {
                    output += '<li>'
                    output += '<a href="/project/'  + item._id + '">' + item.projectName + '</a>'
                    output += '</li>'
                  })
                  $('#myprojects').html(output)
                }
              }
            }
          })
        })
        ```
        
    
    

        
    
    



