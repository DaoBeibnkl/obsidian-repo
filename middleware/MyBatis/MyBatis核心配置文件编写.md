```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE configuration  
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  
        "https://mybatis.org/dtd/mybatis-3-config.dtd">  
  
<configuration>  
    <properties resource="Myproperty.properties"></properties>  
    <environments default="mysql">  
       <environment id="mysql">  
           <transactionManager type="JDBC"></transactionManager>  
           <dataSource type="POOLED">  
               <property name="driver" value="${driver}"/>  
               <property name="url" value="${url}"/>  
               <property name="username" value="${username}"/>  
               <property name="password" value="${password}"/>  
           </dataSource>       </environment>    </environments>    <mappers>        <mapper resource="StudentMapper.xml"></mapper>  
    </mappers></configuration>
```


## 标签顺序
在MyBatis的核心配置文件`mybatis-config.xml`中，标签的顺序是有严格规定的。必须按照以下顺序进行配置：

```
properties?, settings?, typeAliases?, typeHandlers?, objectFactory?, 
objectWrapperFactory?, reflectorFactory?, plugins?, environments?, 
databaseIdProvider?, mappers?
```

- **`properties`**：用于引入外部的properties文件，例如`jdbc.properties`。
- **`settings`**：配置MyBatis的一些运行时设置。
- **`typeAliases`**：为Java类型设置别名。
- **`typeHandlers`**：配置类型处理器。
- **`objectFactory`**：配置对象工厂。
- **`objectWrapperFactory`**：配置对象包装工厂。
- **`reflectorFactory`**：配置反射工厂。
- **`plugins`**：配置插件。
- **`environments`**：配置环境。
- **`databaseIdProvider`**：配置数据库厂商标识。
- **`mappers`**：配置映射器。

这些标签的顺序不能随意改变，否则可能会导致配置文件无法正确解析。