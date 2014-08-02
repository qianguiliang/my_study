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
    
    
 

               
    
    
    