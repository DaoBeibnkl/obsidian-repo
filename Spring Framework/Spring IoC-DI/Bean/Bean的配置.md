## 1. xml的bean标签
`<bean class="" name=""></bean>`

## 2. @Component注解

* @Service
* @Respository
* @Configuration
* ......


## 3. @Bean注解
	a. 是放在方法上面的
	b. 通常放在配置类里面
		i. 用@Component搭配，不会去容器里面查找获取，变成了普通方法的调用。必须要@Configuration注解才会通过springboot的代理来调用。
		ii. 一个bean调用另一个bean，在spring底层依赖关系处理机制的基础上，会从容器中拿被依赖的bean，而不会多次实例化bean。（参考spring的依赖解析）
		
	c. 使用该注解，手动干预bean的实例化过程，可以让我们把jar包中无法修改的代码进行重新配置
	d. @Bean方法的参数spring会为我们自动注入


## 4. @Import注解
用于把实例导入spring容器中，可以起到批量注册bean的作用。（比如我们使用构造函数自动注入，但有很多参数需要配置为bean，如果不一个个手动标注@Component，我们可以用@Import的两个接口方法来强制显式注册bean）

>ImportSelector和ImportBeanDefinitionRegistrar不用配置为bean。他们的作用类似xml中的bean标签。而且后者可以进一步操控bean的定义信息。

	a. @Import注解必须写在类上
	b. 注解标记的类首先要是一个bean，否则不会生效
	c. 直接导入普通类，会把普通类注册为bean
	d. importSelector，根据完整限定名批量注册bean
		i. 定义一个实现ImportSelector接口的类
		ii. @Import导入(importSelector.class)
	e. ImportBeanDefinitionRegistrar
		i. 定义一个实现ImportBeanDefinitionRegistrar接口的类
		ii. BeanDefintion负责配置bean的信息，registry注册bean
		
	
```java
public class MyImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {  
    @Override  
    public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry, BeanNameGenerator importBeanNameGenerator) {  
  
        RootBeanDefinition rootBeanDefinition = new RootBeanDefinition();  
        rootBeanDefinition.setBeanClassName("com.cjj.springioc.service.UserService");  
        registry.registerBeanDefinition("UserService",rootBeanDefinition);  
    }  
}
```