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
    




