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
    * MyModule.js
    ```
    exports.name = ‘hello’;
    exports.sayHello = function() {}
    ```
    * app.js
    ```
    var myModule = require(‘myModule’);
    myModule.sayHello();
    ```
    * 或者
    ```
    module.exports = {
      name: ‘Hello’,
      sayHello: function() {}
    }
    ```
    * 或者
    ```
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

