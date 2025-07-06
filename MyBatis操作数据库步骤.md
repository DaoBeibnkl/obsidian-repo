

	1. 加载MyBatis配置文件，并且使用SqlSessionFactoryBuilder的build方法，创建对应的工厂。
	2. 通过SqlSessionFactory开启数据库连接，默认手动提交事务。
	3. 用`SqlSession`来执行数据库操作
	4. 提交事务（如果需要）
	5. 关闭连接，关闭输入流


## 1、先写MyBatis核心配置文件

## 2、写xxxMapper接口，规定要做的数据库操作

## 3、写xxxMapper.xml配置文件，具体实现接口里面的操作语句

## 4、service类具体实现数据库连接，数据库操作

## 5、controller类调用服务类