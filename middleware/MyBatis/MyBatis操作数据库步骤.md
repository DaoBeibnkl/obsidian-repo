

	1. 加载编写的核心配置文件
	2. 使用SqlSessionFactoryBuilder的build方法，根据配置文件的字节流来创建对应的工厂。
	3. 通过SqlSessionFactory开启数据库session连接(默认手动提交事务)。
	4. 用`SqlSession`来执行数据库操作
	5. 提交事务（如果需要）
	6. 关闭连接，关闭输入流

>加载核心配置文件

	使用ibatis下Resources的getResourceAsStream方法来加载。底层是调用getClassLoader类加载器把配置文件转换为输入字节流。
```java
InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");  
//Student.class.getClassLoader().getResourceAsStream("MyBatisConfig.xml");
```


1、先写MyBatis核心配置文件
2、写xxxMapper接口类，规定要做的数据库操作
3、写xxxMapper.xml配置文件，具体实现接口里面的操作语句
4、service类具体实现数据库连接，数据库操作
5、controller类调用服务类