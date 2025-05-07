>隐式包扫描需要遍历指定路径下的所有类文件，而 `@Bean` 显式配置方法直接定位具体逻辑，避免了类路径扫描的开销。这在大型项目中可显著提升启动速度。当然也可以混合使用，即可以便捷注册bean，也能控制bean的初始化逻辑。

​​
​​基于Java类的配置​​
​​无需包扫描​​：通过@Configuration类中的@Bean方法显式定义Bean（如 @Bean public UserService userService() { return new UserServiceImpl(); }），依赖通过方法参数或调用其他@Bean方法注入。
​​


## 1. 基于XML文档实现

需在beans中逐一声明每个Bean的id和class属性。显式依赖注入​​：通过constructor-arg或property标签手动注入依赖。(有参构造函数注入，与设值注入)


```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xmlns:context="http://www.springframework.org/schema/context"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">  
  
    <bean class="com.cjj.service.UserService">  
    <!--使用property的设值注入时，需要提供setter方法，区别于autowired，它无法直接注入字段。-->
        <property name="UserDaoImpl" ref="UserDaoImpl"></property>  
    </bean>  
    <bean class="com.cjj.dao.UserDaoImpl" name="UserDaoImpl"></bean>   
    
    </beans>
```


>要通过spring实现依赖注入，就要从spring的IoC容器中获取创建的bean。

>ioc.getbean()方法最好传Class对象，这样方便并且可以保证唯一。而不是传类名（可以按bean中配置的id或name来匹配查找），不然需要对返回的Object类型进行转换。
```java
@Test  
public void show(){  
    ClassPathXmlApplicationContext ioc =new ClassPathXmlApplicationContext("spring.xml");  
    IUserService bean = ioc.getBean(IUserService.class);  
    // IUserService bean = (IUserService)ioc.getBean("IUserService")
    bean.getUser();  
}
```


## 2. 基于注解实现

**依赖于包扫描​**​：需要在xml文档中添加<context:component-scan base-package="com.cjj"></context:component-scan>标签，或使用@ComponentScan注解，指定基础扫描包。隐式依赖注入​​：通过@Autowired实现自动装配，无需手动声明依赖关系。



```java
@Component  
@ComponentScan("com.cjj")  
public class UserService implements IUserService {  
  
    @Autowired  
    IUserDao userDao;  
  
    @Override  
    public void getUser() {  
        System.out.println(userDao);  
    }  
  
//    public void setUserDaoImpl(IUserDao userDaoImpl) {  
//        this.userDao=userDaoImpl;  
//  
//    }  
  
}

    @Test  
    public void show(){  
//        ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("spring1.xml");  
//        IUserService bean = ioc.getBean(IUserService.class);  
//        bean.getUser();  
  
        AnnotationConfigApplicationContext ioc = new AnnotationConfigApplicationContext(UserService.class);  
        IUserService bean = ioc.getBean(IUserService.class);  
        bean.getUser();  
  
    }
```







## 3. 基于java配置类实现
>创建一个config包，放置我们的spring核心配置类（替换掉了XML配置文件）。此时不能再用xml的容器，要换成注解配置容器AnnotationConfigApplicationContext。


>@Configuration与@Bean搭配使用，显式指定要注册的bean。但通常跟ComponentScan混用

```java
@Configuration  
public class MySpringConfig {  
    @Bean  
    public IUserDao userDao(){  
        return new UserDao();  
    }  
    @Bean  
    public IUserService userService(){  
        return new UserService();  
    }  
  
}
```

```java
    @Test  
    public void show(){  
//        ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("spring1.xml");  
//        IUserService bean = ioc.getBean(IUserService.class);  
//        bean.getUser();  
  
        AnnotationConfigApplicationContext ioc = new AnnotationConfigApplicationContext(MySpringConfig.class);  
        IUserService bean = ioc.getBean(IUserService.class);  
        bean.getUser();  
  
    }
```


## 4. springboot实现

搭配component注解实现。

springboot测试无需手动创建容器，可以直接配置，然后运行
```java
//该注解包含了扫描包componentscan和配置类configuration注解

@SpringBootApplication  
public class Springboot3Application {  
    public static void main(String[] args) {  
  
        ConfigurableApplicationContext ioc = SpringApplication.run(Springboot3Application.class, args);  
        IUserService bean = ioc.getBean(IUserService.class);  
        bean.getUser();  
    }  
}
```
