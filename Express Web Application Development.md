Express Web Application Development
===================================

内容
----
* 什么是 Express
* 开发 Express App
* 理解 Express 路由
* 服务器响应
* Froms，Cookies， Sessions
* 生产环境下的 Express

什么是 Express
-------------
* 什么是 Express
* 安装 Express
* Express 的组成
* Express 中的概念

### 什么是 Express
* Express 是基于 Node.js 平台的最小化，但是灵活而强大的 web 开发框架
    * 最小化：初始只支持非常基本的 Web 开发，甚至这些基本功能也不是默认打开的，可以按照需要选择。
    * 灵活：Express 的中间件和 Node 模块
    * 强大：Express 可以调用 Node 的全部核心 API

### Express 发展历史
* 2009.2 Ryan Dahl 组合 JS 和 Google V8 建立了 系统级的开发平台，Node
* Node 提供了低级的 Web API，作为 Web Server 需要大量的工作
* 2009.6 T.J. Holowaychuk 发布了 Express，让 Node下的 Web 开发简单化
    * Ruby 社区的 Sinatra 采用了 Express
* 2010.6 Sencha Lab 发布了 Connect， 解决 Node web server API 的 模块化和可扩展
    * Ruby 社区的 Rack server 采用了 Connect
    * Tim Caswell（Sencha 员工）和 T.J. Holowaychuk 共同领导此项目
* 2010.7 Holowaychuk 重新架构了 Express，使之构建在 Connect 之上，V1.0.0
* Express 是在 Connect 基础之上添加了一些功能的框架

### 安装 Express
* 安装 Node
* npm install express
* npm install -g express-generator

### Express 的组成
* application 对象

    ```JavaScript
    var express = require(‘express’);
    var app = express();
    ```
* Request 对象
* Response 对象

### Express 中的概念
* 异步 JS
* Node 模块

    ```
    MyModule.js
        exports.name = ‘hello’;
        exports.sayHello = function() {}
        
    app.js
        var myModule = require(‘myModule’);
        myModule.sayHello();
    或者
        module.exports = {
          name: ‘Hello’,
          sayHello: function() {}
        }
    或者
        module.exports = function(x) {
          …
        }
    ```
* Express 应用就是 Node 模块
* 中间件

    ```
    app.use(function(req, res, next) {
      console.log(‘Request from: ‘ + req.ip);
      next();
    };
    ```
    * Demo
    
        ```
        forbidder.js
            module.exports = function(forbidden_day) {
              var days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', ‘Thursday', 'Friday', 'Saturday'];
              return function(req, res, next) {
                var day = new Date().getDay();
                if (days[day] === forbidden_day) {
                  res.send('No visitors allowed on ' + forbidden_day + 's!');
                } else {
                  next(); 
                }
              } 
            };
        
        app.js
            var forbidder = require(‘./forbidder.js’);
            app.use(forbidder('Wednesday'));
        ```
* Request Flow
    * 一个 Http Request 穿过一系列的 Express 中间件，直到一个中间件或者路由处理它，并返回 Http Response
    * http request -> favicon -> logger -> bodyParser -> methodOverride -> cookieParser -> session -> router -> http response
* Router 中间件
    * 可以处理灵活的 URI，通过不同的 URI 来让不同的处理函数来进行处理
    * http request
        * / -> 显示主页
        * /hello -> Print 'hello'
        * /download/:file_id -> 发送文件
        
    ```
    var express = require('express');
    var app = express();
    app.get('/', function(req, res) {
         res.send('Welcome!');
    });
    app.get('/hello.text', function(req, res) {
         res.send('Hola!');
    });
    app.get('/contact', function(req, res) {
         res.render('contact');
    });
    app.listen(3000);
    ```
    * 分离路由处理
    
        ```
        routes.js
        module.exports = function(app) {
          app.get('/', function(req, res) {
             res.send('Welcome!');
          });
          app.get('/hello.text', function(req, res) {
             res.send('Hola!');
          });
          app.get('/contact', function(req, res) {
             res.render('contact');
          });
        }
        
        app.js
        var express = require(‘express’);
        var app = express();
        var routes = require(‘./routes.js’)(app);
        app.listen(3000);
        ```

开发 Express Web 应用
--------------------
* 简单的 Express Web App
* 自动生成 Express Web App
* 为 Express  添加中间件
* 为 Express 添加 Node 模块
* 记录日志
* 使用配置文件
* 设置获取应用选项
* 不同环境下的 Express

### 第一个 Express App
* 建立项目目录

    ```
    mkdir express-app
    cd express-app
    ```
* Express manifest file
    * package.json
    
        ```
        npm install express --save
        ```
* 基本的 Express App

    ```
    /**
     *
     * Created by zhangjinglin on 14-7-15.
     */
    var express = require('express');
    var app = express();
    
    app.get('/', function(req, res) {
        res.send('hello, Express!\n');
    });
    
    app.listen(3000);
    ```
    
    * 监听3000端口，打印 Hello，Express，其他访问返回 404
    * 开始和就结束 App
    
    ```
    node app
    Ctrl + c 停止
    ```
    
    * 可以尝试采用 nodemon 来监视 app.js 变化，并自动重启
* 带有视图的 Express app

    ```
    mkdir views
    
    index.jade
        // Created by zhangjinglin on 14-7-16.
        doctype html
        html
          head
            title Welcome
          body Welcome!
    
    hello.jade
        //Created by zhangjinglin on 14-7-16
        doctype html
        html
          head
            title Hello
          body
            h1 hello
    
    npm install jade
    
    app.js
        /**
         *
         * Created by zhangjinglin on 14-7-15.
         */
        var express = require('express');
        var app = express();
        
        app.set('view engine', 'jade');
        app.set('views', './views');
        
        app.get('/', function(req, res) {
            res.render('index');
        });
        
        app.get('/say-hello', function(req, res) {
            res.render('hello');
        });
        
        app.get('/test', function(req, res) {
            res.send('this is a test');
        });
        
        
        app.listen(3000);
    ```
    
* app 中的 public 目录
   
    ```
    mkdir public
    mkdir public/css
    mkdir public/js
    
    main.css
        h1 {
            color: red;
        }
        
    main.js
        /**
         *
         * Created by zhangjinglin on 14-7-16.
         */
        window.onload = function() {
            document.getElementsByTagName('h1')[0].innerHTML = 'Hello, Express + JS';
        };
        
    hello.jade
        //   Created by zhangjinglin on 14-7-16
        doctype html
        html
          head
            title Hello
            script(src="js/main.js")
            link(href="css/main.css", rel="stylesheet")
          body
            h1 hello
            
    app.js
        /**
         *
         * Created by zhangjinglin on 14-7-15.
         */
        var express = require('express');
        var app = express();
        
        app.set('view engine', 'jade');
        app.set('views', './views');
        app.use(express.static('./public'));
        
        app.get('/', function(req, res) {
            res.render('index');
        });
        
        app.get('/say-hello', function(req, res) {
            res.render('hello');
        });
        
        app.get('/test', function(req, res) {
            res.send('this is a test');
        });
        
        
        app.listen(3000);
    ```
    
### 自动生成 Express Web App

    ```
    express -h
    express --session auto-express
    cd auto-express 
    npm install
    node www/bin
    ```

### 为 Express  添加中间件（Node 模块）
* **Express 4.x 开始，不在依赖 Connect，意味着不再含有中间件（除了 static )，所有 Express 以前版本中的中间件需要单独安装，在查阅以前版本的案例时需要注意**
* response-time
    
    ```
    npm install response-time
    
    var responseTime = require('response-time');
    app.use(responseTime());
    ```
    
* errorhandler

    ```
    app.get('/', function(req, res) {
      fail();
    }
    
    npm install errorhandler
    
    app.use(require('errorhandler')()); //放置在路由前，无效果
    
    app.use(require('errorhandler')()); //放置在路由后，正确
    ```

### 为 Express 添加 Node 模块

    ```
    npm install iniparser
    
    建立 config.ini
        title = My Awesome App
        port = 3000
        message = You are awesome!
    
    app.js
        var iniparser = require('iniparser');
        var config = iniparser.parseSync('./config.ini');
        ...
        app.get('/', function(req, res) {
            res.render('index', {title: config.title, message: config.message});
        });
        ...
        app.listen(config.port);
        
    index.jade
        //   Created by zhangjinglin on 14-7-16.
        doctype html
        html
          head
            title #{title}
          body
            p= message
    ```
    
### App 日志

    ```
    npm install morgan
    
    app.use(require('morgan')(':remote-addr :method :url'));
    
    app.use(morgan({format: 'tiny', stream: fs.createWriteStream('app.log', {flags: 'w'})}));
    ```

### 使用配置文件

    ```
    config.json
        {
          "development": {
            "db_host": "localhost",
            "db_user": "root",
            "db_pass": "root"    
          },
          
          "production": {
            "db_host": "192.168.1.9",
            "db_user": "myappdb",
            "db_pass": "xxxxxxxx"
          }
        }
        
    var config = require('./config.json')[app.get('env')];
    console.log(config.db_host);
    console.log(config.db_user);
    console.log(config.db_pass);
    ```
    
### 设置获取应用选项

    ```
    app.set(name, value)
    app.get(name)
    ```

### 不同环境下的 Express    

    ```
    app.get('env') //default: development
    $ export NODE_ENV=production
    $ NODE_ENV=production node app
    windows 环境下设置环境变量
    ```
    
理解 Express 路由
----------------
* 什么是路由
* 路由中间件
* 路由处理
* 组织路由

### 什么是路由（Routes）
* 路由是 URL 模式，描述应用请求的接口
* 每一个路由都有相应的处理程序，执行动作并发送响应

#### http verbs （Http 动词）
* GET
* POST
* PUT
* DELETE
* HEAD
* TRACE
* OPTIONS
* CONNECT
* PATCH
* M-SEARCH
* NOTIFY
* SUBSCRIBE
* UNSUBSCRIBE
    
### 路由中间件
* 路由中间件由 Express 本身实现

#### 定义路由
* Chrome plugin - postman

    ```
    app.get('/', ...);
    app.post('/', ...);
    app.put('/', ...);
    app.patch('/', ...);
    app.delete('/', ...);
    app.options('/', ...);
    ...
    // 放置在所有路由之前
    app.all('/', function(req, res, next) {
      res.set('X-Catch-All', 'true');
      next();
    }   
    ```
* 路由标识
    * 路由仅仅是定义了请求路径，GET 查询参数不是路由的一部分
   
        ```
        /abcd -> /abcd
        /ab?cd -> /acd /abcd
        /ab+cd -> /abcd /abbcd
        /ab*cd -> /acxyzcd
        /ab(cd)?e -> /abe /abcde
        ? + * () 正则表达式
        - . 字面含义
        /user/:id -> req.params.id
        /country/:country/state/:state -> req.params.country, req.params.state
        /score/:from-:to -> req.params.from, req.params.to
        /file/:name.:ext -> req.params.name + '.' + req.params.ext.toLowerCase()
        /feed/:format? -> if (req.params.format) {...}
        正则表达式路由，注意没有引号
        app.get(/.+app?le$/, ...) -> pineapple, redapple, redaple, aaple, NOT apple, apples
        app.get(/a/, ...) -> 任何带有 a 的路由
        ```
        
* 路由处理顺序
    * 按照定义顺序，第一个匹配的路由执行
        ```
        app.get('/abcd', ...);
        app.get('/abc*', ...);
        
        --------------------
        app.get('/abc*', function(req, res, next) {
             if (req.path == '/abcd') { next(); }
             else { res.send('abc*'); }
        });
           app.get('/abcd', function(req, res) {
             res.send('abcd');
        });
        ```
       
### 路由处理
* 一个路由可以有多个处理，每一个都可以返回 HTTP response，或者处理之后交给下一个处理程序 

    ```
    app.get('/abcd',
      function(req, res, next) {
        res.send('one');
      },
      function(req, res, next) {
        res.send('tow');
      },
      function(req, res) {
        res.send('three');
      }
    );
    只有 One 可以输出，res.send() 或者 res.render() 会终止 request 流程
    
    app.get('/abcd',
      function(req, res, next) {
        res.send('one');
        next();
      },
      function(req, res, next) {
        res.send('tow');
        next();
      },
      function(req, res) {
        res.send('three');
      }
    );
    错误
    
    app.get('/abcd',
      function(req, res, next) {
        res.set('X-ONE', 'one');
        next();
      },
      function(req, res, next) {
        res.set('X-TOW', 'tow');
        next();
      },
      function(req, res) {
        res.send('three');
      }
    );
    查看响应头
    
    利用数组
    var one = function(req, res, next) {
      res.set('X-One', 'hey!');
      next();
    };
    var two = function(req, res, next) {
      res.set('X-Two', 'ho!');
      next(); 
    };
    app.get('/abcd', [one, two], function(req, res) {
         res.send("Let's go!");
    });
    
    或者
    app.get('/abcd', function(req, res, next) {
         res.set('X-One', 'hey!');
         next();
    });
       app.get('/abcd', function(req, res, next) {
         res.set('X-Two', 'ho!');
         next();
    });
       app.get('/abcd', function(req, res) {
         res.send('three');
    });
    ```
    
### 组织路由
* 作为 Node 模块

    ```
    routes.js
        /**
         * Created by zhangjinglin on 14-7-16.
         */
        exports.index = function(req, res) {
          res.send('welcome');
        };
        
        exports.list = function(req, res) {
          res.send('One, Two, Three');
        };
        
    app.js
        var routes = require('./routes.js');
        app.get('/', routes.index);
        app.get('/list', routes.list);
    ```
    
    ```
    appRoutes.js
        var routes = require('./routes.js');
        module.exports = function(app) {
          app.get('/', routes.index);
          app.get('/list', routes.list);
        }
    
    app.js
        var app = ...
        var routes = require('./appRoutes.js')(app);
    ```
     
服务器响应
--------
* HTTP Response 入门
* Express 中的 HTTP Response

### HTTP Response 入门
* HTTP Server 对一个 request 的响应叫做 HTTP Response 消息，包含状态码、头部和可选的关联数据（技术上称作消息体）
* 消息体可以是普通文本，HTML，图像，下载文件等等。

#### HTTP status code 状态码
* 1xx Informational 信息
* 2xx Success 成功
* 3xx Redirect 重定向
* 4xx Client error 客户端错误
    * 404 Not found 没找到
* 5xx Server Error 服务器错误
    
#### HTTP response header 响应头
* key-value pair 键值对，对普通用户不可见

    ```
    X-Powered-By: Express
    Accept-Ranges: bytes
    ETag: "819254-1356021445000"
    Date: Tus, 17 Jul 2014 21:19:05 GMT
    Cache-Control: public, max-age=0
    Last-Modified: Thu, 22 Dec 2012 16:37:25 GMT
    Content-Type: image/gif
    Content-Length: 819254
    Connection: keep-alive
    ```
    
#### Media types 媒体类型
* 描述了是什么类型的数据, 媒体类型也被称为 MIME Type 或者 Content Type
    * text/html
    * multipart/form-data
    * text/plain

### Express 中的 HTTP Response
* res.send
* res.json
* res.jsonp
* res.sendfile
* res.download
* res.render
* res.redirect
* 如果上述的任一方法都不调用，request 将等待到超时位为止
* 多个方法调用，只有第一个执行

    ```
    app.get('/', function(req, res) {
         res.send('welcome');
    });
    
    检查响应头
    ```
 
#### 设置 HTTP 状态码
* res.status

    ```
    app.get('/', function(req, res) {
         res.status(404); // 如果不设置，Express 将发送 200
         res.send('forced 404');
    });
    
    app.get('/', function(req, res) {
         res.status(500);
         res.send('forced 500');
    });
    
    app.get('/', function(req, res) {
         res.status(404).send('not found');
    });
    ```

* res.send() 等方法，无需描述状态码，默认发送 200
* res.send(404), 单独的数字被认为是状态码
* res.send(404, 'not found') 也可以

#### 设置 HTTP 头
* res.set(name, value)

    ```
    app.get('/', function(req, res) {
         // status is optional, it defaults to 200
         res.status(200);
         res.set('Content-Type', 'text/plain; charset=us-ascii');
         res.set('X-Secret-Message', 'not really secret');
         res.set('X-Test', 'OK');
         res.send('welcome');
    });
    
    app.get('/', function(req, res) {
         res.set({
           'Content-Type': 'text/plain; charset=us-ascii',
           'X-Secret-Message': 'not really secret',
           'X-Test': 'OK'
         });
         res.send('welcome');
       });
    
#### 发送数据
* 普通文本

    ```
    app.get('/', function(req, res) {
        res.set('Content-Type', 'text/plain');
        res.send('<h1>welcome</h1>');
    });
    ```
    
* HTML

    ```
    app.get('/', function(req, res) {
         res.send('<h1>welcome</h1>');
    });
    
    res.render('index', {title:'Express'});
    ```
    
* json

    ```
    app.get('/', function(req, res) {
         res.json({message: 'welcome'});
    });
    
    res.json(404, {error: 'not found'});
    ```
       
* jsonp JSON with Padding
   
    ```
    app.get('/', function(req, res) {
         res.jsonp({message: 'welcome'});
    });
    ```

* 普通文件

    ```
    app.use(express.static('./content'));
    ```
    
* 程序化普通文件
    * res.sendfile() 与浏览器请求普通文件的方式一样，Content-Type 自动依据文件扩展名设定
        
        ```
        app.get('/file', function(req, res) {
             res.sendfile('./secret-file.png', function(err) {
               if (err) { condole.log(err); }
               else { console.log('file sent'); }
             });
        });
        
        app.get('/file.html', function(req, res) {
             console.log('HTML file is an image?');
             res.sendfile('./secret-file.png');
         });
        ```
        
    * res.download() 通知浏览器下载而不是渲染它
    
        ```
        app.get('/download', function(req, res) {
             res.download('./secret-file.png', 'open-secret.png', function(err) {
               if (err) { condole.log(err); }
               else { console.log('file downloaded'); }
             });
        });
        ```
      
* 内容协商
    * 是描述用户代理能够处理的数据类型的一种机制
    * 用户代理在 HTTP request 头部添加 accept 标识
    * Express 使用 res.format() 处理内容协商
    
        ```
        app.get('/', function(req, res) {
             res.format({
               'text/plain': function() {
                 res.send('welcome');
                },
               'text/html': function() {
                 res.send('<b>welcome</b>');
               },
            
               'application/json': function() {
                 res.json({ message: 'welcome' });
               },
              
               'default': function() {
                 res.send(406, 'Not Acceptable');
               } 
             });
        });
        
        res.format({
             text: function() {
               res.send('welcome');
             },
             html: function() {
               res.send('<b>welcome</b>');
             },
             json: function() {
               res.json({ message: 'welcome' });
             },
             default: function() {
               res.send(406, 'Not Acceptable');
             }
        });
        ```
        
* 重定向请求
    * res.redirect()
      

   

    
    
        

    