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