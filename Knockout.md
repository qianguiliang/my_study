Knockout
========

Knockout 入门
------------

### JavaScript 开发
* 关注分离
    * html - 语义
    * css - 表现
    * js - 行为
* 结构化 js
    * 关注分离
    * 数据和绑定
    * UI 模式
    
### Knockout 是什么
* 富客户端交互
* MVVM-like 模式
    * 分离行为和结构
    * 声明式绑定
    * 可观察 Observable
* 跨浏览器支持

### Knockout 关键概念
* Dependency Tracking via Observables
* 声明式绑定
* 模板支持

### Knockout 三部曲
1. 声明式绑定
    
    ```
    <input data-bind="value: firstName" />
    ```
    
2. 创建一个可观察对象

    ```
    var myViewModel = {
      firstName: ko.obeservable('Jinglin')
    };
    ```
    
3. 绑定 ViewModel 到视图    

    ```js
    ko.applyBindings(myViewModel);
    ```

* demo
    
    ```html
    <!-- Created by zhangjinglin on 14-7-23. -->
    <!DOCTYPE html>
    <html>
    <head lang="en">
      <meta charset="UTF-8">
      <title>Knockout 演示</title>
      <link rel="stylesheet" href="css/bootstrap.min.css"/>
    </head>
    <body>
    
    <div class="well">
      <div class="page-header"><h3>Knockout</h3></div>
      <div class="panel-body">
        <input class="form-control" type="text" data-bind="value: xingming"/>
        <span data-bind="text: firstName"></span>
      </div>
    </div>
    
    
    <script src="js/knockout.js"></script>
    <script>
      var myViewModel = {
        xingming: ko.observable('zhang jinglin')
      }
    
      ko.applyBindings(myViewModel)
    </script>
    </body>
    </html>
    ```
    
### 分离，组织，数据绑定
* MVVM
    * 一种分离模式
    * Model - View - ViewModel
    * 非特殊技术，广泛应用（XAML) 
    * 与数据绑定协同工作
    
* MVVM 组件
    * view - HTML
         * web page
         * 用户友好的界面以便表现信息 
    * Model - json
        * 数据
        * js 对象
    * ViewModel
        * 视图的行为和数据
        * 包含模型的属性和方法
        
        ```
        viewModel = {
          id: ko.observable('123'),
          salePrice: ko.observable(788),
          rating: ko.observable(4),
          isInStock: ko.observable(true),
          guitarModel: {
            code: ko.observable('314ce'),
            name: ko.observable('民谣 314  ce')
          },
          showDetails: function() {
            // ...
          }
        }
        ```
        
* demo

    ```html
    <div class="well">
      <div class="page-header"><h3>Knockout</h3></div>
      <div class="panel-body">
        <span data-bind="text: shortDesc"></span>
        <div data-bind="text: description"></div>
        <span data-bind="text: formatCurrency(salePrice)"></span>
      </div>
    </div>
    
    
    <script src="js/knockout.js"></script>
    <script>
      var data = {
        Id: 1001,
        SalePrice: 788.01,
        ListPrice: 1299,
        ShortDesc: '民谣 314 ce',
        Description: '民谣吉他，非常棒'
      }
    
      var viewModel = {
        id: ko.observable(data.Id),
        salePrice: ko.observable(data.SalePrice),
        listPrice: ko.observable(data.ListPrice),
        shortDesc: ko.observable(data.ShortDesc),
        description: ko.observable(data.Description),
        formatCurrency: function(val) {
          return '￥' + val().toFixed(2);
        }
      }
    
      ko.applyBindings(viewModel)
    </script>
    ```
    
* demo
    
    ```
    var viewModel = function() {
      this.id = ko.observable(data.Id),
      this.salePrice = ko.observable(data.SalePrice),
      this.listPrice = ko.observable(data.ListPrice),
      this.shortDesc = ko.observable(data.ShortDesc),
      this.description = ko.observable(data.Description),
      this.formatCurrency = function(val) {
        return '￥' + val().toFixed(2);
      }
    }
    
    ko.applyBindings(new viewModel())
    ```

Bindings & Observable
---------------------
### 使用 jquery 和 knockout 绑定数据

* jQuery

    ```
    <div class="well">
      <div class="page-header"><h1>No binding vs Using Knockout</h1></div>
      <div class="panel-body">
        <div class="panel panel-default">
          <div class="page-header"><h3>使用 jQuery</h3></div>
          <div class="panel-body">
            <div class="form-group">
              <label>项目编号:  </label>
              <span id="guitarItemNumber" class="help-block"></span>
            </div>
            <div class="form-group">
              <label>吉他型号: </label>
              <input id="guitarModel" type="text" class="form-control"/>
            </div>
            <div class="form-group">
              <label>售价: </label>
              <input id="guitarSalePrice" type="text" class="form-control"/>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <script src="js/jquery.js"></script>
    <script src="js/knockout.js"></script>
    <script>
      $(function() {
        var product = {
          id: 1001,
          itemNumber: 'T314CE',
          model: '民谣 314ce',
          salePrice: 1199.95
        }
    
        $('#guitarItemNumber').text(product.itemNumber)
        $('#guitarModel').val(product.model)
        $('#guitarSalePrice').val(product.salePrice)
      })
    </script>
    ```
    
* knockout

    ```  
    <div class="panel panel-default">
      <div class="page-header"><h3>使用 Knockout</h3></div>
      <div class="panel-body">
        <div class="form-group">
          <label>项目编号:  </label>
          <span data-bind="text: itemNumber" class="help-block"></span>
        </div>
        <div class="form-group">
          <label>吉他型号: </label>
          <input data-bind="value: model" type="text" class="form-control"/>
        </div>
        <div class="form-group">
          <label>售价: </label>
          <input data-bind="value: salePrice" type="text" class="form-control"/>
        </div>
      </div>
    </div>
        
    ko.applyBindings(product)
    ```
 
### Observable
* 使用 ko.observable 建立绑定
* 双向绑定

    ```
    <div class="well">
      <div class="page-header"><h1>Knockout: Observable</div>
      <div class="panel-body">
        <div class="panel panel-default">
          <div class="page-header"><h3>对象 Object Literal</h3></div>
          <div class="panel-body">
            <div class="form-group">
              <label>项目编号:  </label>
              <span data-bind="text: product1.itemNumber" class="help-block"></span>
            </div>
            <div class="form-group">
              <label>吉他型号: </label>
              <input data-bind="value: product1.model" type="text" class="form-control"/>
            </div>
            <div class="form-group">
              <label>售价: </label>
              <input data-bind="value: product1.salePrice" type="text" class="form-control"/>
            </div>
    
            <div class="panel panel-default">
              <div class="panel-header"><h4>底层原始对象</h4>
                项目编号: <span data-bind="text: product1.itemNumber"></span>
                <br/>
                吉他型号: <sapn data-bind="text: product1.model"></sapn>
                <br/>
                售价: <sapn data-bind="text: product1.salePrice"></sapn>
              </div>
            </div>
          </div>
        </div>
    
        <div class="panel panel-default">
          <div class="page-header"><h3>对象 Object Literal</h3></div>
          <div class="panel-body">
            <div class="form-group">
              <label>项目编号:  </label>
              <span data-bind="text: product2.itemNumber" class="help-block"></span>
            </div>
            <div class="form-group">
              <label>吉他型号: </label>
              <input data-bind="value: product2.model" type="text" class="form-control"/>
            </div>
            <div class="form-group">
              <label>售价: </label>
              <input data-bind="value: product2.salePrice" type="text" class="form-control"/>
            </div>
    
            <div class="panel panel-default">
              <div class="panel-header"><h4>底层原始对象</h4>
                项目编号: <span data-bind="text: product2.itemNumber"></span>
                <br/>
                吉他型号: <sapn data-bind="text: product2.model"></sapn>
                <br/>
                售价: <sapn data-bind="text: product2.salePrice"></sapn>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <script src="js/jquery.js"></script>
    <script src="js/knockout.js"></script>
    <script>
      $(function() {
        var data = {
          product1: {
            id: 1002,
            itemNumber: 'T110',
            model: '古典 110',
            salePrice: 699.75
          },
          product2: {
            id: ko.observable(1001),
            itemNumber: ko.observable('T314CE'),
            model: ko.observable('民谣 314ce'),
            salePrice: ko.observable(1199.95)
          }
        }
    
        ko.applyBindings(data)
      })
    </script>
    ```
    
### Computed Observable
* 创建一个绑定表达式，绑定到一个函数
* 当观察者发现修改，通知改变，重新计算
* 管理 "this"
    * 如果需要的话，传入拥有者 this 指针
* 又被称为 computed observables

    ```
    vm = {
      id: ko.observable(1),
      salePrice: ko.observable(1499),
      qty: ko.observable(2)
    }
    
    vm.total = ko.computed(function() {
      return this.salePrice() * this.qty()
    }, vm);
    
    this.salePrice -> this -> vm
    ```
  
* demo

    ```
    <div class="well">
      <div class="page-header"><h1>Knockout: Computed</h1></div>
      <div class="panel-body">
        <div class="row">
          <div class="col-xs-8">
            <div class="form-group">
              <label>产品:  </label>
              <span data-bind="text: product().shortDesc" class="help-block"></span>
            </div>
            <div class="form-group">
              <label>单价:  </label>
              <input type="text" class="form-control" data-bind="value: product().salePrice, valueUpdate: 'afterkeydown'"/>
            </div>
            <div class="form-group">
              <label>数量:  </label>
              <input type="text" class="form-control" data-bind="value: quantity, valueUpdate: 'afterkeydown'"/>
            </div>
            <div class="form-group">
              <label>总价:  </label>
              <input type="text" class="form-control" data-bind="value: totalPrice"/>
            </div>
          </div>
          <div class="col-xs-4">
            <img data-bind="attr: {src: product().photoUrl}" class="img-thumbnail" height="150"/>
          </div>
        </div>
    
      </div>
    </div>
    
    <script src="js/jquery.js"></script>
    <script src="js/knockout.js"></script>
    <script>
      $(function() {
        var photoPath = '/img/'
    
        var my = {}
    
        my.Product = function() {
          this.id = ko.observable()
          this.salePrice = ko.observable()
          this.photo = ko.observable()
          this.shortDesc = ko.observable()
          this.photoUrl = ko.computed(function() {
            return photoPath + this.photo()
          }, this)
        }
    
        my.vm = {
          product: ko.observable(
              new my.Product()
                  .shortDesc('民谣吉他，又称弹唱吉他')
                  .salePrice(799)
                  .photo('1.jpg')
          ),
          quantity: ko.observable(2)
        }
    
        my.vm.totalPrice = ko.computed(function() {
          return this.product().salePrice() * this.quantity()
        }, my.vm)
    
        ko.applyBindings(my.vm)
      })
    </script>
    ```
    
### Writeable computed observable
* 计算成员可以定义读写方法
* 非常容易定制

    ```
    vm.computedMember = ko.computed({
        read: function() {
            // 返回通过计算的值
        },
        write: function(value) {
            // 解析 value，并存入 Observable
        },
        owner: // ViewModel，可选
    }
    ```
    
* demo

    ```
    <div class="form-group">
      <label>总价:  </label>
      <input type="text" class="form-control" data-bind="value: totalPrice, valueUpdate: 'afterkeydown'"/>
    </div>
    
    my.vm.totalPrice = ko.computed({
      read: function() {
        return this.product().salePrice() * this.quantity()
      },
      write: function(value) {
        var unitPrice = value / this.quantity();
        this.product().salePrice(unitPrice);
      },
      owner: my.vm
    })
    ```
 
### Observable Array
* 跟踪数组中的对象（not their state）
* 当下列情况产生时发出通知
    * 添加
    * 移除
    
* demo

    ```
    <div class="well">
      <div class="page-header"><h1>Knockout: Observable Array</h1></div>
      <div class="panel-body">
        <span class="help-block"><span data-bind="text: products().length"></span> Products</span>
        <select class="form-control" multiple
                data-bind="options: products, selectedOptions: selectedProducts, optionsText: 'desc'"></select>
        <div class="btn-group">
          <button class="btn btn-default" data-bind="click: removeSelected, enable: selectedProducts().length > 0">移除</button>
          <button class="btn btn-default" data-bind="click: sortProducts, enable: products().length > 1">排序</button>
        </div>
      </div>
    </div>
    
    <script src="js/jquery.js"></script>
    <script src="js/knockout.js"></script>
    <script>
      $(function() {
        var photoPath = '/img/'
    
        var sampleData = [
          {ModelId: 1, SalePrice: 1699, Photo: '1.jpg', ItemNumber: 'CE314', Description: '民谣吉他'},
          {ModelId: 2, SalePrice: 2699, Photo: '2.jpg', ItemNumber: 'TY004', Description: '古典吉他'},
          {ModelId: 3, SalePrice: 1499, Photo: '3.jpg', ItemNumber: 'PM345', Description: '电吉他'}
        ];
    
        var Product = function() {
          var self = this;
          self.id = ko.observable()
          self.salePrice = ko.observable()
          self.photo = ko.observable()
          self.itemNumber = ko.observable()
          self.desc = ko.observable()
          self.photoUrl = ko.computed(function() {
            return photoPath + self.photo()
          })
        }
    
        var vm = {
          products: ko.observableArray([]),
    
          selectedProducts: ko.observableArray([]),
    
          removeSelected: function() {
            vm.products.removeAll(vm.selectedProducts())
            vm.selectedProducts([])
          },
    
          sortProducts: function() {
            vm.products.sort(function(left, right) {
              return left.desc() === right.desc() ? 0 : (left.desc() < right.desc() ? 1 : -1)
            })
          },
    
          load: function() {
            $.each(sampleData, function(index, item) {
              vm.products.push(
                  new Product()
                      .id(item.ModelId)
                      .salePrice(item.SalePrice)
                      .photo(item.Photo)
                      .itemNumber(item.ItemNumber)
                      .desc(item.Description)
              )
            })
          }
    
        }
    
        vm.load()
        ko.applyBindings(vm)
      })
    </script>
    ```
    
### Observable Array Functions
* destroy
* destroyAll
* indexOf
* pop
* push
* remove
* removeAll
* reverse
* shift
* slice
* sort
* splice
* unshift

### Subscribing to changes
* 注册修改通知
* 当属性修改时执行一些动作

    ```
    vm.xxxxx.subscribe(function() {
        // Do something
    }
    ```
    
