# 三层架构
三层架构是一种软件设计模式，将应用程序分为三个主要层次：

1. 表示层（Presentation Layer）：负责与用户交互，展示数据并接收用户输入。
2. 业务逻辑层（Business Logic Layer）：处理应用程序的核心业务逻辑和规则。
3. 数据访问层（Data Access Layer）：负责与数据库或其他持久化存储进行交互，读取和写入数据。
## 分层

DAO层：
	指的是数据访问对象（Data Access Object）所在层，也叫mapper层，持久层。
	无论多么复杂的查询，dao只是封装增删改查。至于增删查改如何去实现一个功能，dao是不管的。
	
	某个DAO一定是和数据库的某一张表对应映射的，其中封装了CRUD（增加Create、检索Retrieve、更新Update和删除Delete）基本操作，DAO只做原子操作。常用的持久层框架有：MyBatis,Hibernate,Spring Data


Service层：
	也叫业务层。粗略理解就是对一个或多个DAO进行再次封装，成为具体的一个服务。因此这里也就不会是一个原子操作了，需要事务控制，操作具体的某个功能。
	
	service包含了serviceImpl（service接口的实现类） 是提供给controller 使用的，针对于某些业务将 dao 的对于某些表的crud进行组合，也就是说间接的和数据库打交道。

 View层：
	Controller负责请求转发，接受页面传过来的参数，交给Service处理，接收到返回值，再传给页面。管理业务（Service）调度和管理跳转。

![[Pasted image 20250401095759.png]]

# MVC架构
![[Pasted image 20250401100326.png]]

**MVC**，即 **Model 模型、View 视图，及 Controller 控制器**。

**View：视图**，为用户提供使用界面，与用户直接进行交互。

**Model：模型**，承载数据，并对用户提交请求进行计算的模块。其分为两类：
一类称为数据承载 Bean：实体类，专门用户承载业务数据的，如 Student、User 等 
一类称为业务处理 Bean：指 Service 或 Dao 对象，专门用于处理用户提交请求的。

**Controller：控制器**，用于将用户请求转发给相应的 Model 进行处理，并根据 Model 的计算结果向用户提供相应响应。




# MVC与三层架构的关系



# 三层架构之外的分层

POJO层（实体类）：
	全称为“Plain Old Java Object”,直译为“简单老式Java对象”,指未实现特殊接口或继承特定框架类的普通Java类。是实体类层。也叫model、domain、dto。贯穿联通了三个分层。

util层：
	就是工具类，用来封装相应的方法，然后将其放在对应的util包下，使用的时候直接调用就可以了。比如，日期转换util，http请求等相关的工具类。获取properties文件属性等等都可能会被放进util



Filter层：
	Filter对用户请求进行预处理，接着将请求交给Servlet进行处理并生成响应，最后Filter再对服务器响应进行后处理。

VO层：
	vo层的存在就是方便前端获取数据，后端将前端的需要的数据做一个整合，打包成一个类。
