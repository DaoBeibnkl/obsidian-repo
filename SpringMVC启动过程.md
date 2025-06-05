1、tomcat服务器启动，会执行ServletContainerInitConfig类。初始化web容器。
```java
public class ServletContainerInitConfig extends AbstractDispatcherServletInitializer(){
@Override  
protected WebApplicationContext createServletApplicationContext() {  
    AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();  
    ctx.register(SpringMvcConfig.class);  
    return ctx;  
}  
  
@Override  
protected String[] getServletMappings() {  
    return new String[]{"/"};  
}  
  
@Override  
protected WebApplicationContext createRootApplicationContext() {  
    return null;  
}
}
```

2、然后才执行类中的创建容器的方法，创建了webApplicationContext容器（注册了SpringMvcConfig.class）

3、Spring根据配置类，加载了@Controller下的bean，每个@RequestMapping的名称对应一个具体方法

4、过滤：执行getServletMappings，执行过滤规则。

简化初始化的类AbstractAnnotationConfigDispatcherServletInitializer（是子类）。

```java
public class ServletContainerInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {  
  
    @Override  
    protected Class<?>[] getRootConfigClasses() {  
        return new Class[]{SpringConfig.class};  
    }  
  
    @Override  
    protected Class<?>[] getServletConfigClasses() {  
        return new Class[]{SpringMvcConfig.class};  
    }  
  
    @Override  
    protected String[] getServletMappings() {  
        return new String[]{"/"};  
    }  
}
```