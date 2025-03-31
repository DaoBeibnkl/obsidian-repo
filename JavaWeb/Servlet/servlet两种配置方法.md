
1. 使用web.xml文件
	其中路径匹配的格式有精确匹配，后缀匹配，通配符匹配
	
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"  
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
         xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"  
         version="5.0">  
         
  <!--配置名称，以及它的包名+类名-->
    <servlet>        
	    <servlet-name>my</servlet-name>  
        <servlet-class>com.cjj.servlet.HelloServlet</servlet-class>  
    </servlet>  
    
  <!--根据名称来映射，以及配置访问路径-->
    <servlet-mapping>        
	    <servlet-name>my</servlet-name>  
	    
        <url-pattern>/myservlet</url-pattern>
        <url-pattern>*.servlet</url-pattern>
        <url-pattern>/*</url-pattern>
        <url-pattern>/</url-pattern>
        
    </servlet-mapping>  
/*
	精确匹配：/名字
	后缀匹配：*.xxx
	通配符匹配：/*，匹配所有请求，包含了服务器所有资源
	通配符匹配：/,匹配所有请求，包含所有资源，不包括jsp
	
*/
    </web-app>
```


2. 使用注解(servlet3.0后支持)
	注解跟xml文件不冲突，可以同时使用
	* loadOnStarUp：配置servlet的创建时机。
		默认为负数，表示访问时创建。如果为0或正数，则在启动程序时创建
```java
//@WebServlet(urlPattern={"/hs","/hs2"},loadOnStartup = -1)  
  
@WebServlet(value = {"/hs","/1234hs"},loadOnStartup = 0)  
public class HttpServlet extends javax.servlet.http.HttpServlet {  
  
    @Override  
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        System.out.println("这是get内容");  
    }  
}
```