Sencha Touch
============

* 备注
    * st = Sencha Touch
    * js = JavaScript
    


1. st 入门
-------------------
* 移动开发解决方案
    * iOS - Objective C
    * Android - Java
    
* 第三方工具
    * st 开发一次，多平台使用
        
    * 需要的知识
        * HTML
        * CSS
        * js
        
### 1.1 st 是什么
* 前身是 ExtJS
* 使用 HTML5 和 CSS3 构建跨平台的移动应用
* st 目标平台是 WebKit-based，使用在 iPhone，iPad 和 Android 上
* 案例
    * http://checkout.github.io
    * http:// sencha.com/apps
    
#### 1.1.1 st 不是用于开发桌面 Web 应用的，在 IE 和 FireFox 工作不好
* Sencha Touch ！== ExtJS

#### 1.1.2 st 底层有大量的组件
* gesture library 手势库
    * tap 点击
    * pinch 双指缩放
    * swipe 滑动
* 原生的滚动
    * 硬件加速 CSS3 Transitions
    * slide 摩擦和跳动效果
     
#### 1.1.3 硬件兼容
#### 1.1.4 全屏兼容

### 1.2 st 组件
* Platform ExtJS 和 st 共享
* Layout 
* Utilities
* Data
* Style
* MVC
* UI widgets

### 1.3 st UI
#### 1.3.1 Containers
* 可配置和灵活
    * 允许子组件停靠
#### 1.3.2 使用 Tab 控件控制 UI 
* Sheets
* Views
* Misc


2. 首次使用 st
-------------
### 2.1 下载 st
### 2.2 st 包含什么
### 2.3 打招呼
* demo - Hello World
* 项目结构
    * css/sencha-touch.css
    * js/sencha-touch-all.js
    
    ```
    <!-- Created by zhangjinglin on 14-7-25. -->
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="UTF-8">
      <link rel="stylesheet" href="css/sencha-touch.css"/>
      <title>Sencha Touch 演示</title>
    </head>
    <body>
      <script src="js/sencha-touch-all.js"></script>
      <script>
        Ext.application({
          launch: function() {
            Ext.Msg.alert('你好', '欢迎进入 Sencha Touch 的世界')
          }
        })
      </script>
    </body>
    </html>
    ```
* 如果在桌面浏览器模拟，必须使用 Safari 或者 Chrome
  
### 2.4 第一个应用
* 准备构建一个联系人项目

