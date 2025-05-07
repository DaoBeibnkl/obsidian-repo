>创建好的bean，它的名字首字母小写（≠类名）。getbean时要注意。在bean标签中配置了name，那么bean的名字就是name，而不是默认的首字母小写类名。


在 Spring 框架中，`getBean()` 的查找机制并非简单的“先按名字找，找不到再按类型找”，而是 严格区分名称匹配与类型匹配两种独立模式。以下是具体解析：

---

​**​一、名称查找与类型查找的独立性​**​

1. 按名称查找（`getBean(String name)`）  
    • 机制：严格匹配容器中注册的 Bean 名称（包括 `id` 或 `name` 属性），完全区分大小写。
    
    • 结果：
    
    ◦ 名称匹配成功 → 返回 Bean 实例；
    
    ◦ 名称不匹配 → 直接抛出 `NoSuchBeanDefinitionException`，不会触发类型查找。
    
2. 按类型查找（`getBean(Class<T> type)`）  
    • 机制：基于 Bean 的 类型匹配，要求容器中该类型的 Bean 唯一。
    
    • 结果：
    
    ◦ 类型唯一 → 返回 Bean 实例；
    
    ◦ 存在多个同类型 Bean → 抛出 `NoUniqueBeanDefinitionException`。
    

---

​**​二、名称查找与类型查找的关系​**​

1. 两种模式独立运行  
    • 名称查找和类型查找是两种不同的检索逻辑，彼此之间不会自动切换。
    
    • 例如：
    
    java
    
    复制
    
    ```java
    // 假设容器中存在类型为 UserService 的 Bean，但注册名为 "userServiceImpl"
    app.getBean("UserService"); // 严格匹配名称 → 抛出异常（名称不存在）
    app.getBean(UserService.class); // 按类型查找 → 成功（假设类型唯一）
    ```
    
2. 组合使用（`getBean(String name, Class<T> type)`）  
    • 机制：先按名称匹配，再验证类型是否兼容。
    
    • 优势：
    
    ◦ 避免类型强转错误（如名称匹配但类型不兼容时抛出 `BeanNotOfRequiredTypeException`）。
    

---

​**​三、常见误区与验证​**​  
​**​误区​**​  
认为 `getBean("UserService")` 会依次执行以下操作：

1. 查找名为 `"UserService"` 的 Bean → 若失败；
2. 查找类型为 `UserService` 的 Bean → 若存在则返回。

​**​实际验证​**​  
通过以下代码可明确验证：

java

复制

```java
// 注册一个类型为 UserService 的 Bean，但未显式命名（默认名称为 "userServiceImpl"）
@Bean
public UserService userServiceImpl() {
    return new UserServiceImpl();
}

// 测试代码
try {
    app.getBean("UserService"); // 严格按名称查找 → 抛出 NoSuchBeanDefinitionException
} catch (NoSuchBeanDefinitionException e) {
    System.out.println("名称查找失败，未触发类型查找！");
}

UserService service = app.getBean(UserService.class); // 成功（按类型查找）
```

---

​**​四、企业级开发建议​**​

1. 优先使用类型安全的方式  
    • 推荐使用 `getBean(Class<T> type)` 或 `getBean(String name, Class<T> type)`，避免类型强转错误。
    
2. 处理多实例场景  
    • 若存在多个同类型 Bean，需通过 `@Primary` 指定默认 Bean 或 `@Qualifier` 指定名称。
    
3. 命名规范  
    • 显式定义 Bean 名称（如 `@Service("userService")`），保持名称与类型语义一致。
    

---

​**​总结​**​

|查找方式|触发逻辑|异常场景|
|---|---|---|
|按名称|严格匹配注册名|`NoSuchBeanDefinitionException`（名称不存在）|
|按类型|匹配唯一类型|`NoUniqueBeanDefinitionException`（多实例）|
|名称+类型|名称匹配 + 类型校验|`BeanNotOfRequiredTypeException`（类型不兼容）|

正确理解这两种模式的独立性，是避免 `NoSuchBeanDefinitionException` 的关键。

## 1. 默认使用无参构造器实例化

* 有多个构造函数，依然会调用无参构造
* 如果只有一个有参构造，则spring会调用它，并且会自动装配参数
* 如果有参构造有多个但没有无参构造，会报错


## 2. 使用@Bean自由选择构造函数进行实例化
（也叫实例工厂方法）


## 3. FactoryBean进行实例化

接口的两个方法：
* ioc.getBean("orderService")不会获取OrderService对应的bean，而是getObject()中返回的类型对应的bean。
* public Class\<?> getObjectType()方法可以指定工厂绑定的bean类型，
	不然无法直接用ioc.getbean(UserService.class)来获取到这个bean。

接口实现类也要是bean，但根据接口实现类名字获取的bean，被替换为了getObject()返回的bean。可以用


```java
@Component  
public class OrderService implements FactoryBean {  
    @Override  
    public Object getObject() throws Exception {  
        return new UserService();  
    }  
  
    @Override  
    public Class<?> getObjectType() {  
        return UserService.class;  
    }  
}
```

```java
@SpringBootApplication  
public class SpringiocApplication {  
    public static void main(String[] args) {  
        ConfigurableApplicationContext ioc = SpringApplication.run(SpringiocApplication.class, args);  
        //通过类名获取bean，会被替换成方法内返回的bean
        System.out.println(ioc.getBean("orderService"));  
        
        //要获取工厂接口实例化类的bean，可以在类名前加地址符，或者从对应的的类型来访问
        System.out.println(ioc.getBean("&orderService"));  
        System.out.println(ioc.getBean(OrderService.class));  
    }  
}
```
