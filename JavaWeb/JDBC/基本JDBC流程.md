

>基本连接执行流程：注册驱动 -->获取连接对象 -->创建执行SQL语句的对象（静态语句、动态语句）-->执行并接收操作结果 -->释放连接资源

```java
package com.cjj.JDBC;  
  
import java.sql.DriverManager;  
import java.sql.SQLException;  
import java.sql.Connection;  
import java.sql.Statement;  
  
public class TestJDBC {  
    public static void main(String[] args) throws ClassNotFoundException, SQLException {  
//        1、注册驱动 并且让它自己自动加载驱动  
        Class.forName("com.mysql.cj.jdbc.Driver");  
  
//        2、获得数据库连接  
        String url="jdbc:mysql://localhost:3306/companydb?useSSL=false";  
        String user="root";  
        String password="123456";  
        Connection conn=DriverManager.getConnection(url,user,password);  
  
        if(conn!=null)  
            System.out.println("数据库连接成功");  
        else  
            System.out.println("连接失败");  
  
//        3、创建执行SQL语句的对象，并执行DML语句获取返回值。静态语句才需要用statement,而带参数的预编译语句用PreparedStatement  
        Statement stmt=conn.createStatement();  
  
        String sql="INSERT t_jobs VALUES ('2','经理',5,99990);";  
  
        int ret=stmt.executeUpdate(sql);  
        System.out.println(ret);  
  
          
//        4、处理结果  
        if(ret>=0) System.out.println("成功");  
        else System.out.println("no");  
//        5、释放资源(先开启的后关闭）  
        stmt.close();  
        conn.close();  
    }  
  
}
```


	带参数的预编译语句类PreparedStatement，是Statement的子接口。
	需要先传入SQL语句进行预编译，之后赋值，最后才执行SQL语句


```java
//3、方法是prepareStatement,是准备，不是已经准备好了。
	PreparedStatement pstmt=conn.prepareStatement("SELECT * FROM user WHERE username=? AND password=?");
// ?是占位符，输入的文本填充到占位符位置上，不用考虑特殊文本，阻止了SQL注入

//给?占位符赋值。下标从1开始，为SQL语句中的参数赋值。
	pstmt.setString(1,uname);
	pstmt.setString(2,pwd);


//无需再传入sql语句
	ResultSet resultset=pstmt.excuteQuery();
	
```


封装工具类

	普通的重用性方案：
```java
import java.sql.DriverManager;  
import java.sql.Connection;  
import java.sql.ResultSet;  
import java.sql.SQLException;  
import java.sql.Statement;

public class DBUtils{

//注册驱动放到静态代码块中，以免多次调用时重复检查。
	static{
		try{
			Class.forName("com.mysql.jdbc.Driver");
		}catch (ClassNotFoundException e) {  
		    throw new RuntimeException(e);  
		}
	}

//静态函数，可以直接通过类名调用方法。
	public static Connection getConnection(){
		//...
	}
	public static void closeAll(...){
	
	}
}
```

跨平台的封装工具类方案：

	创建配置文件 (跟工具类放在同级文件夹下)
```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/companydb
user=root
password=123456
```

```java

//都是静态，配置文件加载一次就可以了。可以减少开销。
public class DBUtils{
	//设置为静态final常量。搭配static使用。并且始终指向同一个配置文件的对象
	private static final Properties PROPERTIES=new Properties();
	static{
		//try-with-resources自动关闭
		try(InputStream is=DBUtils.class.getResourceAsStream("/mydb.properties"))
		{
			//从输入流中加载数据，进入到当前配置文件对象中。
			PROPERTIES.load(is);
			//加载类 自己自动注册驱动
			Class.forName(PROPERTIES.getProperty("driver"));
		}catch(..)
	}
}
```