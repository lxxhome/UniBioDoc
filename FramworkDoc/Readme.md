## 1.   
本系统采用.net Core的MVC架构进行开发。官方技术文档：https://docs.microsoft.com/zh-cn/aspnet/core/getting-started/?view=aspnetcore-6.0&tabs=windows   

## 2.
数据库交互使用Dapper。官方技术文档：https://dapper-tutorial.net/   

## 3.
本系统采用插件化开发模式进行开发。所有公用的插件已封装为.dll包进行引入。所有以Eis.开头的插件皆为自己开发。在程序中可进行方便的引用。视图相关插件的引用一般会放入在各文件夹模块Views文件夹根目录下，作为分部视图引入。

## 4.
本系统分为三大模块：   
Eis.Web,   
Eis.Web.Business,   
Eis.Web.Model。   
### Eis.Web
此模块包括系统各个模块的基本功能与业务逻辑。        
结构上以.net Core 提倡的分主外层MVC模式+Areas文件夹中存放各功能组业务MVC模式。   
Areas功能技术文档参考：https://docs.microsoft.com/zh-cn/aspnet/core/mvc/controllers/areas?view=aspnetcore-6.0
### Eis.Web.Business
此模块用于与第三方数据库相关的业务逻辑编写。例如：排班功能，需要和err-ch库进行连接，对err-ch库与本系统库的数据同步。
### Eis.Web.Model
此模块用于与第三方库系统有关系的Model的创建。例如：排班表LongshineShift。

## 5.
本系统主要采用“服务器呈现的 UI”来完成数据的展示。具体优缺点请求参照文档：https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/choose-web-ui?view=aspnetcore-6.0   

## 6.本系统封装特性展示：   

### 一.数据基本的增删改查方法已封装到IRepository   
直接调用即可。 各方法均有同步和异步调用。

#### 异步调用写法
在同步的基础上加 async 后缀。例：_repository.Delete为同步删除，_repository.DeleteAsync为异步删除。   
程序中基本所有的方法均采用异步调用。   
调用异步方法前一般需要加上 await 关键字。   
await关键字作用：等待前当前方法异步执行完成并返回结果后，代码再依次向后执行。   
如果不加 await 关键字，则程序不会等待，会直接向后执行。此种方式一般用于无须考虑各请求是否成功执行完成。一般在加了transcation的，请求结果需要有先后顺序，各结果有依赖关系的时候不宜使用此种方式。

调用示例1：    
异步调用，不用等街结果立即返回：  
```
    public class TimeApiController
    {
        // 类中引入IRepository接口
        private readonly IRepository _repository;
        // 构造函数进行注入
        public TimeApiController(IRepository repository)
        {
            _repository = repository;
        }
        // 直接使用其中的DeleteAsync()方法，即可删除库中相应记录
        public void DeleteRecord(Student student)
        {
           _repository.DeleteAsync(student);
        }
    }
```
调用示例2：
异步调用，加 await,等待结果返回：
```
 public void HolidayAsync([FromForm] StudentModel studentModel)
        {
         // 先删除库表中指定数据
         await _repository.DeleteByPredicateAsync<Student>(t => t.id == studentModel.id);
          // 删除成功后再插入新数据
         await _repository.InsertAsync(list);
        }
```
 

