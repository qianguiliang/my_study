Developer Tips
==============
ASP.NET Web API
---------------
### Cors
* Install-Package Microsoft.AspNet.WebApi.Cors 
* WebApiConfig.cs
```CSharp
var cors = new EnableCorsAttribute("*", "*", "*");
config.EnableCors(cors);
```


AngularJS
---------
### Filter: Format Date String
* {{item.Fabushijian | date: "yyyy年mm月dd日"}}