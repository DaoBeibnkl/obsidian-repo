### 1. singleton（单例）

#### 解释

这是 Spring 默认的 Bean 作用域。当一个 Bean 被定义为单例作用域时，Spring 容器在整个应用的生命周期里只会创建该 Bean 的一个实例，并且所有对该 Bean 的请求都会返回这个唯一的实例。

#### 示例代码

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {
    @Bean
    public MyBean myBean() {
        return new MyBean();
    }
}

class MyBean {
    public MyBean() {
        System.out.println("MyBean 实例被创建");
    }
}

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        MyBean bean1 = context.getBean(MyBean.class);
        MyBean bean2 = context.getBean(MyBean.class);
        System.out.println(bean1 == bean2); // 输出 true
        context.close();
    }
}
```

#### 适用场景

适用于那些无状态或者状态可以共享的 Bean，像服务层组件、数据访问对象（DAO）等。由于单例作用域只创建一个实例，所以能减少内存开销。

#### 可能存在的问题及解决方案

- **线程安全问题**：若单例 Bean 是有状态的，多个线程同时访问该 Bean 时可能会出现线程安全问题。解决方案是保证 Bean 无状态，或者使用线程安全的类和同步机制。

### 2. prototype（原型）

#### 解释

当一个 Bean 被定义为原型作用域时，每次从 Spring 容器请求该 Bean 时，容器都会创建一个新的实例。

#### 示例代码

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Scope;

@Configuration
public class AppConfig {
    @Bean
    @Scope("prototype")
    public MyBean myBean() {
        return new MyBean();
    }
}

class MyBean {
    public MyBean() {
        System.out.println("MyBean 实例被创建");
    }
}

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        MyBean bean1 = context.getBean(MyBean.class);
        MyBean bean2 = context.getBean(MyBean.class);
        System.out.println(bean1 == bean2); // 输出 false
        context.close();
    }
}
```

#### 适用场景

适用于有状态且状态不能共享的 Bean，比如需要为每个请求创建独立实例的场景。

#### 可能存在的问题及解决方案

- **内存开销问题**：由于每次请求都会创建新实例，若频繁请求可能会导致内存开销增大。解决方案是合理控制原型 Bean 的使用，避免不必要的创建。