

response对象输出中文乱码：
	设置服务器端响应编码的格式为utf-8
	再设置客户端响应内容的头文件，文件类型与编码格式

或设置同时设置服务器端响应时编码格式，以及客户端响应时的文件类型与文件编码格式
```java
public class HttpServlet extends javax.servlet.http.HttpServlet {  
  
    @Override  
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {  
        
        //这是服务器端设置接收编码的格式
        req.setCharacterEncoding("UTF-8");  

       
        //这是服务器端设置响应编码的格式
        //resp.setCharacterEncoding("UTF-8");  
        //resp.setHeader("Content-type","text/html;charset=utf-8");  
        
		resp.setContentType("text/html;charset=utf-8");
        
        PrintWriter pw=resp.getWriter();
        pw.println("注册成功！");  
    }  
}
```