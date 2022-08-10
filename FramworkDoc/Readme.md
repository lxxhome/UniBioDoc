1.本系统采用.net Core的MVC架构进行开发。官方技术文档：https://docs.microsoft.com/zh-cn/aspnet/core/getting-started/?view=aspnetcore-6.0&tabs=windows   

2.数据库交互使用Dapper。官方技术文档：https://dapper-tutorial.net/   

3.本系统采用插件化开发模式进行开发。所有公用的插件已封装为.dll包进行引入。所有以Eis.开头的插件皆为自己开发。在程序中可进行方便的引用。视图相关插件的引用一般会放入在各文件夹模块Views文件夹根目录下，作为分部视图引入。

4.本系统分为三大模块：   
Eis.Web,   
Eis.Web.Business,   
Eis.Web.Model。   
### Eis.Web
此模块包括系统各个模块的基本功能与业务逻辑。        
结构上以.net Core 提倡的分主外层MVC模式+Areas文件夹中存放各功能组业务MVC模式。Areas功能技术文档参考：https://docs.microsoft.com/zh-cn/aspnet/core/mvc/controllers/areas?view=aspnetcore-6.0
### Eis.Web.Business
此模块用于与第三方数据库相关的业务逻辑编写。例如：排班功能，需要和err-ch库进行连接，对err-ch库与本系统库的数据同步。
### Eis.Web.Model
此模块用于与第三方库系统有关系的Model的创建。例如：排班表LongshineShift。   


 

