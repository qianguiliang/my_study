Sencha Touch
============

第一个应用
--------
* 准备构建一个联系人项目
* 项目结构
    * app.html
    
        ```html
        <!-- Created by zhangjinglin on 14-7-25. -->
        <!DOCTYPE html>
        <html>
        <head>
          <meta charset="UTF-8">
          <title>Sencha Touch 演示</title>
          <link rel="stylesheet" href="css/sencha-touch.css"/>
          <script src="js/sencha-touch-all.js"></script>
        
          <script>
            Ext.Loader.setConfig({
              enabled: true,
              paths: {
                MyApp: 'MyApp'
              }
            })
        
            Ext.require(['MyApp.MainContainer'])
        
            Ext.application({
              launch: function() {
                Ext.create('MyApp.MainContainer', {
                  fullscreen: true
                })
              }
            })
          </script>
        
        </head>
        <body>
        
        </body>
        </html>
        ```
        
    * MyApp/
        * MainContainer.js
        
            ```js
            /**
             * Created by zhangjinglin on 14-7-25.
             */
            Ext.define('MyApp.MainContainer', {
              extend: 'Ext.Container',
              requires: ['MyApp.PersonList', 'MyApp.PersonDetail'],
              config: {
                layout: {type: 'hbox', aligh: 'stretch'},
                items: [
                  {
                    xtype: 'personlist',
                    itemId: 'list',
                    width: 200,
                    style: 'border-right: 1px solid #999'
                  },
                  {
                    xtype: 'persondetail',
                    itemId: 'detail',
                    flex: 1
                  }
                ],
                listeners: {
                  select: {fn: 'onListSelect', delegate: '> #list'}
                }
              },
              onListSelect: function(list, record) {
                console.log('click')
                this.getComponent('detail').setRecord(record)
              }
            })
            ```
            
        * PersonDetail.js
        
            ```js
            /**
             * Created by zhangjinglin on 14-7-25.
             */
            Ext.define('MyApp.PersonDetail', {
              extend: 'Ext.form.Panel',
              xtype: 'persondetail',
              config: {
                items: [
                  {
                    xtype: 'fieldset',
                    defaultType: 'textfield',
                    defaults: {labelWidth: 100},
                    items:[
                      {label: 'First', name: 'firstname'},
                      {label: 'Last', name: 'lastname'},
                      {label: 'Street', name: 'street'},
                      {label: 'City', name: 'city'},
                      {label: 'State', name: 'state'},
                      {label: 'Zip', name: 'zip'}
                    ]
                  },
                  {
                    xtype: 'toolbar',
                    title: 'Person Detail',
                    docked: 'top'
                  }
                ]
              }
            })
            ```
            
        * PersonList.js
        
            ```js
            /**
             * Created by zhangjinglin on 14-7-25.
             */
            Ext.define('MyApp.PersonList', {
              extend: 'Ext.List',
              xtype: 'personlist',
              requires: ['MyApp.PersonStore'],
              config: {
                allwDeselect: false,
                itemTpl: '{lastname}, {firstname}',
                store: {
                  type: ['personstore']
                },
                items: [{
                  xtype: 'toolbar',
                  title: 'People',
                  docked: 'top'
                }]
              }
            })
            ```
            
        * PersonModel.js
        
            ```js
            /**
             * Created by zhangjinglin on 14-7-25.
             */
            Ext.define('MyApp.PersonModel', {
              extend: 'Ext.data.Model',
              config: {
                fields: [
                  'city',
                  'firstname',
                  'lastname',
                  'middle',
                  'state',
                  'street',
                  'zip'
                ]
              }
            })
            ```
          
        * PersonStore.js
        
            ```js
            /**
             * Created by zhangjinglin on 14-7-25.
             */
            Ext.define('MyApp.PersonStore', {
              extend: 'Ext.data.Store',
              alias: 'store.personstore',
              requires: ['MyApp.PersonModel'],
              config: {
                autoLoad: true,
                model: 'MyApp.PersonModel',
                proxy: {
                  type: 'jsonp',
                  url: 'http://extjsinaction.com/dataQuery.php',
                  limit: 20,
                  reader: {
                    type: 'json',
                    rootProperty: 'records'
                  }
                }
              }
            })
            ```
 
布局
---
* fit
    
    ```coffee
    Ext.application
      launch: ->
        Ext.define 'Zjl.st.FitDemo',
          extend: 'Ext.Container'
          xtype: 'fitdemo'
          config:
            fullscreen: true
            style: 'background-color: darkgray'
            layout:
              type: 'fit'
            items: [
              xtype: 'component'
              margin: 10
              html: 'This componet is set to fit'
              style: 'background-color: lightgray'
            ]
        Ext.create 'Zjl.st.FitDemo'
    ```
    
* hbox

    ```coffee
    Ext.application
      launch: ->
        Ext.define 'Zjl.st.HBoxDemo',
          extend: 'Ext.Container'
          xtype: 'hboxdemo'
          config:
            fullscreen: true
            defaults:
              margin: 10
            style: 'background-color: darkgray'
            layout:
              type: 'hbox'
            items: [
              xtype: 'component'
              flex: 1
              margin: 10
              html: 'Flex 1'
              style: 'background-color: pink'
            ,
              xtype: 'component'
              flex: 3
              margin: 10
              html: 'Flex 3'
              style: 'background-color: yellow'
            ]
        Ext.create 'Zjl.st.HBoxDemo'
    ```
    
* vbox

    ```coffee
    Ext.application
      launch: ->
        Ext.define 'Zjl.st.VBoxDemo',
          extend: 'Ext.Container'
          config:
            fullscreen: true
            defaults:
              margin: 10
            style: 'background-color: darkgray'
            layout:
              type: 'vbox'
            items: [
              xtype: 'component'
              flex: 1
              margin: 10
              html: 'Flex 1'
              style: 'background-color: pink'
            ,
              xtype: 'component'
              flex: 3
              margin: 10
              html: 'Flex 3'
              style: 'background-color: yellow'
            ]
        Ext.create 'Zjl.st.VBoxDemo'
    ```
    
* card

    ```coffee
    Ext.application
      launch: ->
        Ext.define 'Zjl.st.CardDemo',
          extend: 'Ext.Container'
          config:
            fullscreen: true
            defaults:
              margin: 10
            style: 'background-color: darkgray'
            layout:
              type: 'card'
            items: [
              xtype: 'segmentedbutton'
              docked: 'top'
              items: [
                text: 'First'
                pressed: true
              ,
                text: 'Second'
              ,
                text: 'Third'
              ]
              listeners:
                toggle: (segmentedbutton, button, isPressed, eOpts) ->
                  if isPressed
                    container = segmentedbutton.getParent()
                    txt = button.getText()
                    selectedComponent = container.getComponent button.getText()
                    container.setActiveItem selectedComponent
            ,
              xtype: 'component',
              itemId: 'First'
              html: 'First Component'
              style: 'background-color: pink'
            ,
              xtype: 'component',
              itemId: 'Second'
              html: 'Second Component'
              style: 'background-color: yellow'
            ,
              xtype: 'component',
              itemId: 'Third'
              html: 'Third Component'
              style: 'background-color: cyan'
            ]
        Ext.create 'Zjl.st.CardDemo'
    ```
    
* Docking
   
   ```coffee
   Ext.application
     launch: ->
       Ext.define 'Zjl.st.DockingDemo',
         extend: 'Ext.Container'
         config:
           fullscreen: true
           defaults:
             margin: 5
           layout:
             type: 'fit'
           items: [
             xtype: 'component'
             docked: 'top'
             html: 'Docking: Top'
             style: 'background-color: pink'
           ,
             xtype: 'component'
             docked: 'left'
             html: 'Docking: left'
             style: 'background-color: lightblue'
           ,
             xtype: 'component'
             docked: 'right'
             html: 'Docking: right'
             style: 'background-color: lightgreen'
           ,
             xtype: 'component'
             docked: 'bottom'
             html: 'Docking: bottom'
             style: 'background-color: cyan'
           ,
             xtype: 'component'
             docked: 'top'
             html: 'Centered'
             style: 'background-color: lightgray'
           ]
       Ext.create 'Zjl.st.DockingDemo'
   ```
   
* Template

    ```coffee
    Ext.application
      launch: ->
        Ext.define 'Zjl.st.TemplatesDemo',
          extend: 'Ext.Container'
          config:
            fullscreen: true
            scrollable: true
            data:
              countries: [
                country: 'Argentina'
                flagColors: [
                  'light blue'
                  'white'
                ]
              ,
                country: 'Switzerland'
                flagColors: [
                  'red'
                  'white'
                ]
              ,
                country: 'France'
                flagColors: [
                  'blue'
                  'white'
                  'red'
                ]
              ]
            tpl: Ext.create 'Ext.XTemplate',
              '<tpl for="countries"',
              '<p>{#}. {country}: {flagColors}</p>',
              '</tpl>'
        Ext.create 'Zjl.st.TemplatesDemo'
    ```
    
组件
----
* 按钮
    * Ext.Button
    * xtype: button

    ```coffee
    Ext.application
      launch: ->
        Ext.define 'Zjl.st.ButtonDemo',
          extend: 'Ext.Container'
          config:
            fullscreen: true
            items: [
              xtype: 'button'
              text: 'Replay'
              badgeText: '42'
              ui: 'normal'
              iconCls: 'reply'
              iconMask: true
              iconAlign: 'top'
              margin: '50 0 0 50'
              width: 100
              heigh: 100
              listeners:
                tap: (button, e, eOpts) ->
                  Ext.Msg.alert 'INFO', 'You clicked me'
            ]
    
        Ext.create 'Zjl.st.ButtonDemo'
    ```
    
* Label
    * Ext.Label
    * xtype: label

    ```coffee
    Ext.application
      launch: ->
        Ext.define 'Zjl.st.LabelDemo',
          extend: 'Ext.Container'
          config:
            fullscreen: true
            items: [
              xtype: 'label'
              html: '<h1>Hello</h1><h3>Welcome to Sencha Touch</h3>'
              styleHtmlContent: true
            ]
    
        Ext.create 'Zjl.st.LabelDemo'
    ```
    
容器
----

* Carousel
    * Ext.carousel.Carousel
    * xtype: carousel

    ```coffee
    Ext.application
      launch: ->
        Ext.define 'Zjl.st.CarouselDemo',
          extend: 'Ext.carousel.Carousel'
          config:
            fullscreen: true
            direction: 'vertical'
            items: [
              html: 'red'
              style: 'background-color: pink'
            ,
              html: 'green'
              style: 'background-color: lightgreen'
            ,
              html: 'blue'
              style: 'background-color: cyan'
            ]
        Ext.create 'Zjl.st.CarouselDemo'
    ```
    
* Navigation View
    * Ext.NavigationView
    * xtype: navigationview
    
    ```coffee
    Ext.application
      launch: ->
        view = Ext.create 'Ext.NavigationView',
          fullscreen: true
          items: [
            title: 'First'
            items: [
              xtype: 'button'
              text: 'Push a new View'
              handler: ->
                view.push
                  title: 'Second'
                  html: 'Second View'
            ]
          ]
    
        Ext.Viewport.add view
    ```
    
* SegmentButtion
    * Ext.SegmentedButton
    * xtype: segmentedbutton

    ```coffee
    Ext.application
      launch: ->
        sb = Ext.create 'Ext.SegmentedButton',
          height: 100
          items: [
            text: 'Option 1'
          ,
            text: 'Option 2'
            pressed: true
          ,
            text: 'Option 3'
          ]
          listeners:
            toggle: (container, button, pressed) ->
              Ext.Msg.alert("#{button.getText()} button: #{pressed}")
    
        Ext.Viewport.add sb
    ```
    
* TabPanel
    * Ext.tab.Panel
    * xtype: tabpanel
    
    ```coffee
    Ext.application
      launch: ->
        tabpanel = Ext.create 'Ext.tab.Panel',
          layout:
            animation: 'flip'
          tabBarPosition: 'bottom'
          items: [
            title: 'Profile'
            iconCls: 'user'
            style: 'background-color: #e17467; color: white'
            html: 'Content is here'
          ,
            title: 'Favorites'
            iconCls: 'favorites'
            style: 'background-color: #f6eb69; color: white'
            html: 'Content is here'
          ,
            title: 'Search'
            iconCls: 'search'
            style: 'background-color: #74e796; color: white'
            html: 'Content is here'
          ,
            title: 'Action'
            iconCls: 'action'
            style: 'background-color: #688ee2; color: white'
            html: 'Content is here'
          ,
            title: 'More'
            iconCls: 'more'
            style: 'background-color: lightgray; color: white'
            html: 'Content is here'
          ]

    
        Ext.Viewport.add tabpanel
    ```
    
* Title Bar
    * Ext.TitleBar
    * xtype: titlebar
* Tool Bar
    * Ext.Toolbar
    * xtype: toolbar
    
    ```coffee
    Ext.application
      launch: ->
        titlebar = Ext.create 'Ext.TitleBar',
          docked: 'top'
          title: 'Title Bar'
          items: [
            iconCls: 'action'
            align: 'left'
            text: 'Action'
          ,
            iconCls: 'user'
            align: 'right'
            text: 'Profile'
          ]
          
        toolbar = Ext.create 'Ext.Toolbar',
          docked: 'bottom'
          items: [
            iconCls: 'favorites'
          ,
            xtype: 'spacer'
          ,
            iconCls: 'search'
          ]
    
        Ext.Viewport.add
          xtype: 'container'
          items: [
            titlebar
          ,
            html: 'This sample shows title bar and toolbar.'
          ,
            toolbar
          ]
    ```
  
* Ext.Panel panel

    ```coffee
    Ext.application
      launch: ->
        panelbutton = Ext.create 'Ext.Button',
          height: 100
          width: 300
          text: 'Touch to open panel'
          itemId: 'openPanel'
          listeners:
            tap: (button, e, eOpts) ->
              if !button.panel
                button.panel = Ext.widget 'panel',
                  html: 'This is a panel'
                  padding: 20
                  width: 280
                  modal: true
                  hideOnMaskTap: true
                  hidden: true
                  hideAnimation: 'fadeOut'
                  showAnimation: 'fadeIn'
              button.panel.showBy(button)
    
        Ext.Viewport.add panelbutton
    ```
    
* Ext.ActionSheet actionsheet

    ```coffee
    Ext.application
      launch: ->
        actionSheet = Ext.create 'Ext.ActionSheet',
          items: [
              text: 'Delete draft'
              ui  : 'decline'
            ,
              text: 'Save draft'
            ,
              text: 'Cancel'
              ui  : 'confirm'
          ]
    
        Ext.Viewport.add actionSheet
        actionSheet.show()
 
* Message box

    ```coffee
    Ext.application
      launch: ->
        Ext.Viewport.add
          xtype: 'container'
          defaults:
            margin: '50 50 0 50'
          items:[
            xtype: 'button'
            text: 'alert'
            listeners:
              tap: ->
                Ext.Msg.alert 'INFO', 'this is an alert box'
          ,
            xtype: 'button'
            text: 'confirm'
            listeners:
              tap: ->
                Ext.Msg.confirm 'INFO', 'This is a confirm box', (answer) ->
                  Ext.Msg.alert 'Answer', "Your are answer is #{answer}"
          ,
            xtype: 'button'
            text: 'proptm'
            listeners:
              tap: ->
                Ext.Msg.prompt 'INFO', 'This is a prompt box', (answer, text) ->
                  Ext.Msg.alert 'INFO', "Your are answer is #{answer}, the text is #{text}"
          ]
    ```
    
* Ext.picker.Picker picker

    ```coffee
    Ext.application
      launch: ->
        Ext.Viewport.add
          xtype: 'container'
          defaults:
            margin: '50 50 0 50'
          items:[
            xtype: 'button'
            text: 'touch to show pickers'
            listeners:
              tap: (button, e, eOpts) ->
                if not button.picker
                  button.picker = Ext.widget 'picker',
                    useTitles: true
                    slots:[
                      name: 'xingming'
                      title: '姓名'
                      data: [
                        text: '张景林'
                        value: 'zjl'
                      ,
                        text: '赵四'
                        value: 'zs'
                      ,
                        text: '吴莫愁'
                        value: 'momo wu'
                      ]
                    ,
                      name: 'job'
                      title: '职业'
                      data: [
                        text: '演员'
                        value: 'actor'
                      ,
                        text: '教授'
                        value: 'professor'
                      ,
                        text: '歌手'
                        value: 'singer'
                      ]
                    ]
                    listeners:
                      cancel: ->
                        Ext.Msg.alert 'Cancel', 'Picker has been canceled'
                      change: (picker, value, opts) ->
                        Ext.Msg.alert 'Change', "Picker has value: #{Ext.JSON.encode(value)}"
                  Ext.Viewport.add button.picker
                button.picker.show()
          ]
    ```
    
* Ext.Sheet sheet

    ```coffee
    Ext.application
      launch: ->
        Ext.Viewport.add
          xtype: 'container'
          defaults:
            margin: '50 50 0 50'
          items:[
            xtype: 'button'
            text: 'touch to show sheet'
            listeners:
              tap: (button, e, eOpts) ->
                if not button.sheet
                  button.sheet = Ext.widget 'sheet',
                    html: 'Some text inside of the sheet'
                    style: 'color: white; font-weight: bold'
                    hideOnMaskTap: true
                  Ext.Viewport.add button.sheet
                button.sheet.show()
          ]
    ```
    
* Ext.Audio audio

    ```coffee
    Ext.application
      launch: ->
        Ext.Viewport.add
          xtype: 'container'
          layout:
            type : 'vbox'
            pack : 'center'
            align: 'stretch'
          items: [
            xtype : 'toolbar',
            docked: 'top',
            title : 'Ext.Audio'
          ,
            xtype: 'audio',
            url  : 'http://docs.sencha.com/touch/2.3.1/touch-build/examples/audio/crash.mp3'
        ]
    ```
    
* Ext.Video video

    ```
    Ext.application
      launch: ->
        Ext.Viewport.add
          xtype: 'container'
          items:[
            xtype: 'video'
            url: 'http://docs.sencha.com/touch/2.3.1/touch-build/examples/video/resources/media/BigBuck.m4v'
            posterUrl: 'http://docs.sencha.com/touch/2.3.1/touch-build/examples/video/resources/images/cover.jpg'
          ]
    ```
    
* 百度地图

    ```coffee
    <script src="http://api.map.baidu.com/api?key=&v=1.1&services=true" type="text/javascript"></script>
    
    Ext.application
      launch: ->
        Ext.Viewport.add
          xtype: 'container'
          layout:
            type : 'vbox'
            pack : 'center'
            align: 'stretch'
          items: [
            xtype : 'titlebar'
            docked: 'top'
            title : '调用百度地图'
          ,
            xtype: 'panel'
            id: 'bmap'
            flex: 1
        ]
    
    
        map = new BMap.Map 'bmap'
        point = new BMap.Point 124.370181,40.183184
        map.centerAndZoom point, 18
    ```
    
st 视图组件
---------
* Container

    ```coffee
    Ext.application
      launch: ->
        loginContainer = Ext.create 'Ext.Container',
          itemId    : 'loginContainer'
          fullscreen: true
          items     : [
            xtype      : 'textfield'
            label      : 'Login'
            placeHolder: 'Enter Username Here'
          ,
            xtype      : 'textfield'
            label      : 'Organization'
            placeHolder: 'Enter Your Organization Here'
          ,
            xtype      : 'textfield'
            label      : 'Password'
            placeHolder: 'Enter Password Here'
          ]
    
    ```
    
* add 和 insert

    ```coffee
    Ext.application
      launch: ->
        loginContainer = Ext.create 'Ext.Container',
          itemId    : 'loginContainer'
          fullscreen: true
          items     : [
            xtype      : 'textfield'
            label      : 'Login'
            placeHolder: 'Enter Username Here'
          ,
            xtype      : 'textfield'
            label      : 'Organization'
            placeHolder: 'Enter Your Organization Here'
          ,
            xtype      : 'textfield'
            label      : 'Password'
            placeHolder: 'Enter Password Here'
          ]
    
        root = (Ext.ComponentQuery.query '#loginContainer')[0]
        root.add
          xtype: 'checkboxfield'
          label: 'Remember'
    
        root.insert 0, {html: '<h3>Please enter your credentials</h3>', styleHtmlContent: true}
    
    ```
    
* remove

    ```coffee
    Ext.application
      launch: ->
        rootContainer = Ext.create 'Ext.Container',
          fullscreen: true
          defaults:
            style: 'margin-bottom: 30px'
          items     : [
            itemId: 'panel1'
            items : [
              xtype      : 'textfield'
              label      : 'Login'
              itemId     : 'loginField'
              placeHolder: 'Enter Username Here'
            ,
              xtype      : 'textfield'
              label      : 'Password'
              placeHolder: 'Enter PassWord Here'
            ]
          ,
            itemId: 'panel2'
            items : [
              xtype      : 'textfield'
              label      : 'Organization'
              placeHolder: 'Enter Your Organization here'
            ,
              xtype: 'checkboxfield'
              label: 'Remember Login'
            ]
          ]
    
        Ext.Function.defer ->
          logField = rootContainer.down '#loginField'
          rootContainer.down '#panel1'
            .remove logField, false # true 销毁
          rootContainer.down '#panel2'
            .add logField
        , 2000, this
    ```
     
* 缺省布局

    ```coffee
    Ext.application
      launch: ->
        rootContainer = Ext.create 'Ext.Container',
          fullscreen: true
          defaults  :
            style: 'border: 1px solid blue; margin: 5px 0'
          items     : [
            xtype : 'toolbar'
            docked: 'top'
            title : 'Default Layout'
          ,
            xtype : 'toolbar'
            docked: 'bottom'
            items : [
              text   : 'Add Child'
              handler: ->
                rootContainer.add
                  xtype: 'container'
                  style: 'border: 1px solid red'
                  html : 'Child'
            ,
              text   : 'Add Fixed Width Child'
              handler: ->
                rootContainer.add
                  xtype: 'container'
                  style: 'border: 1px solid green'
                  width: 100
                  html : 'Fixed Child'
            ]
          ,
            html: 'First Child'
          ,
            html : 'Fixed Width Child'
            width: 100
          ,
            html: 'Child'
          ]
    ```
  
* fit布局

    ```coffee
    Ext.application
      launch: ->
        rootContainer = Ext.create 'Ext.Container',
          fullscreen: true
          layout: 'fit'
          items: [
            xtype: 'toolbar'
            docked: 'top'
            title: 'Fit Layout'
          ,
            xtype: 'container'
            style: 'background-color: pink; padding: 20px'
            html: 'I Fit in my Parent'
          ]
    ```
    
* Card布局

    ```coffee
    Ext.application
      launch: ->
        handleNavigation = (btn) ->
          currentContainer = rootContainer.getActiveItem()
          innerItems = rootContainer.getInnerItems()
          totalItems = innerItems.length
          currentIndex = innerItems.indexOf(currentContainer)
          newIndex = null
          direction = null
    
          if btn.getText() == 'Back'
            direction = 'right'
            newIndex = if currentIndex > 0 then currentIndex - 1 else totalItems - 1
          else
            direction = 'left'
            newIndex = if currentIndex < totalItems - 1 then currentIndex + 1 else 0
    
          #rootContainer.setActiveItem innerItems[newIndex]
          rootContainer.animateActiveItem newIndex,
            type: 'slide'
            direction: direction
            
        rootContainer = Ext.create 'Ext.Container',
          fullscreen: true
          layout:
            type: 'card'
            animation: 'slide'
          items: [
            xtype: 'toolbar'
            docked: 'top'
            title: 'Card Layout'
            items: [
              text: 'Back'
              ui: 'back'
              handler: handleNavigation
            ,
              xtype: 'spacer'
            ,
              text: 'Forward'
              ui: 'forward'
              handler: handleNavigation
            ]
          ,
            html: 'Card 1'
            style: 'background-color: pink'
          ,
            html: 'Card 2'
            style: 'background-color: cyan'
          ,
            html: 'Card 3'
            style: 'background-color: yellow'
          ]
    
    ```
    
* hbox vbox

    ```coffee
    Ext.application
      launch: ->
        rootContainer = Ext.create 'Ext.Container',
          fullscreen: true
          layout    :
            type : 'hbox'
            pack : 'start' #center end justify
            align: 'start' #center end stretch
          defaults  :
            style: 'border: 1px solid red'
          items     : [
            html  : 'item 1'
            height: 100
          ,
            html  : 'item 2'
            height: 75
            width : 100
          ,
            html  : 'item 3'
            height: 200
          ]
    ```
    
    ```coffee
    Ext.application
      launch: ->
        rootContainer = Ext.create 'Ext.Container',
          fullscreen: true
          layout    :
            type : 'hbox'
            pack : 'start' #center end justify
            align: 'start' #center end stretch
          defaults  :
            style: 'border: 1px solid red'
          items     : [
            html: 'item 1'
            flex: 1
          ,
            html: 'item 2'
            flex: 1
          ,
            html: 'item 3'
            flex: 2
          ]
    ```
    
* 嵌套布局

    ```coffee
    Ext.application
      launch: ->
        rootContainer = Ext.create 'Ext.Container',
          fullscreen: true
          layout    : 'vbox'
          defaults  :
            width: '100%'
            flex : 1
          items     : [
            itemId: 'cardContainer'
            layout:
              type: 'card'
              animation: 'slide'
            activeItem: 0
            items: [
              xtype: 'toolbar'
              docked: 'top'
              title: 'Nested Layout'
              items: [
                text: 'Back'
                ui: 'back'
              ,
                xtype: 'spacer'
              ,
                text: 'Forward'
                ui: 'forward'
              ]
            ,
              html: 'Card 1'
            ,
              html: 'Card 2'
            ,
              html: 'Card 3'
            ]
          ,
            layout: 'hbox'
            style: 'border: 1px solid blue'
            defaults:
              style: 'border: 1px solid red'
              flex: 1
            items: [
              html: 'Panel 0, 0'
            ,
              html: 'Panel 0, 1'
            ,
              html: 'Panel 0, 2'
            ]
          ,
            layout: 'vbox'
            style: 'border: 1px solid blue'
            defaults:
              style: 'border: 1px solid red'
              flex: 1
            items: [
              html: 'Panel 1, 0'
            ,
              html: 'Panel 1, 1'
            ,
              html: 'Panel 1, 2'
            ]
          ]
    ```
        
数据和视图
--------

* local store

    ```coffee
    Ext.application
      launch: ->
        Ext.define 'MyModel',
          extend: 'Ext.data.Model'
          config:
            fields: [
              'name'
            ,
              name: 'age', type: 'int'
            ]
    
        store = Ext.create 'Ext.data.Store',
          model: 'MyModel'
          data: [
            name: '张景林'
            age: 41
          ,
            name: '赵四'
            age: 45
          ,
            name: '吴莫愁'
            age: 23
          ]
    ```
    
* remote store

    ```
    Ext.application
      launch: ->
        Ext.define 'MyModel',
          extend: 'Ext.data.Model'
          config:
            fields: [
              'name'
            ,
              name: 'age', type: 'int'
            ]
    
        store = Ext.create 'Ext.data.Store',
          model   : 'MyModel'
          autoLoad: true
          proxy   :
            type  : 'ajax'
            url   : 'http://localhost/user'
            reader:
              type         : 'json'
              rootProperty : 'users'
              totalProperty: 'totalNumber'
    ```
    
* 模板
    
    ```coffee
    Ext.application
      launch: ->
        template = Ext.create 'Ext.XTemplate',
          'Welcome : {data}'
        html = template.apply
          data: '张景林'
        Ext.getBody().setHtml html
    ```
    
* 模板：遍历数组

    ```coffee
    Ext.application
      launch: ->
        template = Ext.create 'Ext.XTemplate',
          '成员 : ',
          '{data}',
          '<tpl for="user">',
          '<p style="padding-left: 2em;">{name}</p>',
          '</tpl>'
    
    
        html = template.apply
          data: '移动项目小组',
          user: [
            name: '张景林'
          ,
            name: '赵四'
          ,
            name: '王五'
          ]
        Ext.getBody().setHtml html
    ```
 
* 模板：格式化数据

    ```coffee
    Ext.application
      launch: ->
        template = Ext.create 'Ext.XTemplate',
          '<p>Ext.util.Format: ',
          '{dateVal:date("Y-m-d")}</p>', # dateVal:date("Y-m-d") 之间不可有空格
          '<p>Member function: ',
          '{[this.formatDate(values.dateVal)]}</p>',
          {
            formatDate: (date) ->
              Ext.util.Format.date(date, "Y年m月d日")
          }
    
    
        html = template.apply
          dateVal: new Date()
    
        Ext.getBody().setHtml html
    ```
    
* dataview: local store

    ```coffee
    Ext.application
      launch: ->
        Ext.define 'User',
          extend: 'Ext.data.Model'
          config:
            fields: [
              'name'
            ,
              name: 'age', type: 'int'
            ]
    
        store = Ext.create 'Ext.data.Store',
          model: 'User'
          data: [
            name: '张景林'
            age: 41
          ,
            name: '赵四'
            age: 45
          ,
            name: '吴莫愁'
            age: 23
          ]
    
        Ext.create 'Ext.dataview.DataView',
          fullscreen: true
          store: store
          itemTpl: [
            '欢迎 {name}, ',
            '他的年龄是 ',
            '{age}'
          ]
    ```
    
* dataview: remote store
    * express - app.coffee
        
        ```coffee
        express = require 'express'
        cors = require 'cors'
        app = express()
        
        app.use cors()
        
        app.get '/user', (req, res) ->
          res.json
            success: true
            totalNumber: 40
            users: [
              name: '张景林'
              age: 41
            ,
              name: '赵四'
              age: 45
            ]
        
        app.listen 8080
        ```
        
    * sencha touch - app.coffee
    
        ```coffee
        Ext.application
          launch: ->
            Ext.define 'User',
              extend: 'Ext.data.Model'
              config:
                fields: [
                  'name'
                ,
                  name: 'age', type: 'int'
                ]
        
            store = Ext.create 'Ext.data.Store',
              model: 'User'
              autoLoad: true
              proxy:
                type: 'ajax'
                url: 'http://localhost:8080/user'
                reader:
                  type: 'json'
                  rootProperty: 'users'
                  totalPeoperty: 'totalNumber'
        
            Ext.create 'Ext.dataview.DataView',
              fullscreen: true
              store: store
              itemTpl: [
                '欢迎 {name}, ',
                '他的年龄是 ',
                '{age}'
              ]
        ```
    
    * 简化
      
        ```coffee
        express = require 'express'
        cors = require 'cors'
        app = express()
        
        app.use cors()
        
        app.get '/user', (req, res) ->
          res.json [
            name: '张景林'
            age: 41
          ,
            name: '赵四'
            age: 45
          ]
                
        app.listen 8080
        
        ----------------
        
        Ext.application
          launch: ->
            Ext.define 'User',
              extend: 'Ext.data.Model'
              config:
                fields: [
                  'name'
                ,
                  name: 'age', type: 'int'
                ]
        
            store = Ext.create 'Ext.data.Store',
              model: 'User'
              autoLoad: true
              proxy:
                type: 'ajax'
                url: 'http://localhost:8080/user'
        
        
            Ext.create 'Ext.dataview.DataView',
              fullscreen: true
              store: store
              itemTpl: [
                '欢迎 {name}, ',
                '他的年龄是 ',
                '{age}'
              ]
        ```

* list

    ```coffee
    Ext.application
      launch: ->
        Ext.define 'User',
          extend: 'Ext.data.Model'
          config:
            fields: [
              'name'
            ,
              name: 'age', type: 'int'
            ]
    
        store = Ext.create 'Ext.data.Store',
          model: 'User'
          data: [
            name: '张景林'
            age: 41
          ,
            name: '赵四'
            age: 45
          ,
            name: '吴莫愁'
            age: 23
          ]
    
        Ext.create 'Ext.dataview.List',
          fullscreen: true
          store: store
          itemTpl: [
            '欢迎 {name}, ',
            '他的年龄是 ',
            '{age}'
          ]
    ```
    
    * infinite
    
        ```coffee
        express = require 'express'
        cors = require 'cors'
        app = express()
        
        app.use cors()
        
        
        userArray = []
        
        for i in [1..1000]
          userArray.push
            name: '张景林'
            age: i
        
        app.get '/user', (req, res) ->
          res.json userArray
        
        
        app.listen 8080
        
        ---------------------
        
        Ext.application
          launch: ->
            Ext.define 'User',
              extend: 'Ext.data.Model'
              config:
                fields: [
                  'name'
                ,
                  name: 'age', type: 'int'
                ]
        
            store = Ext.create 'Ext.data.Store',
              model: 'User'
              autoLoad: true
              proxy:
                type: 'ajax'
                url: 'http://localhost:8080/user'
        
            Ext.create 'Ext.dataview.List',
              fullscreen: true
              infinite: true #设置 true or false，通过web 开发工具查看生成的数量， 
              store: store
              itemTpl: [
                '欢迎 {name}, ',
                '他的年龄是 ',
                '{age}'
              ]
        ```
           
    
    * 在 list 底部添加动作按钮
    
        ```
        express = require 'express'
        cors = require 'cors'
        app = express()
        
        app.use cors()
        
        
        userArray = []
        
        for i in [1..1000]
          userArray.push
            name: '张景林'
            age: i
        
        app.get '/user', (req, res) ->
          result = []
          start = (req.query.page - 1) * 20
          end = start + 20
          for i in [start..end]
            result.push userArray[i]
          res.json result
        
        
        app.listen 8080
        
        -----------------------
        
        Ext.application
          launch: ->
            pageIndex = 1
            Ext.define 'User',
              extend: 'Ext.data.Model'
              config:
                fields: [
                  'name'
                ,
                  name: 'age', type: 'int'
                ]
        
            store = Ext.create 'Ext.data.Store',
              model: 'User'
              autoLoad: true
              page: 1
              proxy:
                type: 'ajax'
                url: 'http://localhost:8080/user'
                pageParam: 'page'
        
            myList = Ext.create 'Ext.dataview.List',
              fullscreen: true
              store: store
              itemTpl: [
                '欢迎 {name}, ',
                '他的年龄是 ',
                '{age}'
              ]
              items: [
                xtype: 'button'
                docked: 'bottom'
                scrollDock: 'bottom'
                text: 'Load next'
                ui: 'confirm'
                handler: ->
                  pageIndex++
                  store.loadPage pageIndex
              ]
        ```
        
    * 使用 indexBar， grouping和disclosures
    
        ```coffee
        express = require 'express'
        cors = require 'cors'
        app = express()
        
        app.use cors()
        
        
        userArray = []
        
        for i in [1..1000]
          userArray.push
            name: i + '张景林'
            age: i
        
        app.get '/user', (req, res) ->
          result = []
          start = (req.query.page - 1) * 20
          end = start + 20
          for i in [start..end]
            result.push userArray[i]
          res.json result
        
        
        app.listen 8080
        
        ----------------------
        
        Ext.application
          launch: ->
            pageIndex = 1
            Ext.define 'User',
              extend: 'Ext.data.Model'
              config:
                fields: [
                  'name'
                ,
                  name: 'age', type: 'int'
                ]
        
            store = Ext.create 'Ext.data.Store',
              model: 'User'
              autoLoad: true
              page: 1
              grouper:
                groupFn: (record) ->
                  record.get('name')[0]
              proxy:
                type: 'ajax'
                url: 'http://localhost:8080/user'
                pageParam: 'page'
        
            myList = Ext.create 'Ext.dataview.List',
              fullscreen: true
              store: store
              indexBar: true
              grouped: true
              onItemDisclosure: true
              itemTpl: [
                '欢迎 {name}, ',
                '他的年龄是 ',
                '{age}'
              ]
              items: [
                xtype: 'button'
                docked: 'bottom'
                scrollDock: 'bottom'
                text: 'Load next'
                ui: 'confirm'
                handler: ->
                  pageIndex++
                  store.loadPage pageIndex
              ]
              listeners:
                disclose: (list, record) ->
                  Ext.Msg.alert record.get('name'), record.get('age')
        
        ```
        
    * masked
        
        ```coffee
        express = require 'express'
        cors = require 'cors'
        app = express()
        
        app.use cors()
        
        
        userArray = []
        
        for i in [1..1000]
          userArray.push
            name: i + '张景林'
            age: i
        
        app.get '/user', (req, res) ->
          result = []
          start = (req.query.page - 1) * 20
          end = start + 20
          for i in [start..end]
            result.push userArray[i]
        
          setTimeout ->
            res.json result
          , 2000
        
        
        app.listen 8080
        
        ---------------
        
        Ext.application
          launch: ->
            pageIndex = 1
            Ext.define 'User',
              extend: 'Ext.data.Model'
              config:
                fields: [
                  'name'
                ,
                  name: 'age', type: 'int'
                ]
        
            store = Ext.create 'Ext.data.Store',
              model: 'User'
              autoLoad: true
              page: 1
              grouper:
                groupFn: (record) ->
                  record.get('name')[0]
              proxy:
                type: 'ajax'
                url: 'http://localhost:8080/user'
                pageParam: 'page'
        
            myList = Ext.create 'Ext.dataview.List',
              fullscreen: true
              store: store
              indexBar: true
              grouped: true
              onItemDisclosure: true
              masked:
                xtype: 'loadmask'
                message: '正在装载数据...'
              itemTpl: [
                '欢迎 {name}, ',
                '他的年龄是 ',
                '{age}'
              ]
              items: [
                xtype: 'button'
                docked: 'bottom'
                scrollDock: 'bottom'
                text: 'Load next'
                ui: 'confirm'
                handler: ->
                  pageIndex++
                  store.loadPage pageIndex
              ]
              listeners:
                disclose: (list, record) ->
                  Ext.Msg.alert record.get('name'), record.get('age')
        ```
        

