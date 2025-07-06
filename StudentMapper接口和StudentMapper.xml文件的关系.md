MyBatis配置文件是核心，它配置了MyBatis运行环境，并注册了`studentMapper`接口。`studentMapper`接口定义了操作方法，而`studentMapper.xml`文件定义了这些方法对应的SQL语句和映射关系。


映射文件的id要跟接口里面的方法一致（小驼峰）[[MyBatis映射文件编写]]
同理，映射文件的`ResultType`等返回类型和`parameterType`参数类型也要一致