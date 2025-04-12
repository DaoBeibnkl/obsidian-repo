>创建好的bean，它的名字首字母小写（≠类名）。getbean时要注意。


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
