
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
```
## 属性namespace
	在MyBatis中，`namespace` 是映射文件（Mapper XML文件）中的一个重要属性。它主要用于以下几个方面：
	
	### 1. **唯一标识映射文件**
	
	`namespace` 是映射文件的唯一标识符。在MyBatis中，每个映射文件都需要有一个唯一的`namespace`，这样MyBatis才能正确地识别和管理不同的映射文件。
	
	### 2. **关联接口和映射文件**
	
	`namespace` 通常与`Mapper`接口的全限定名一致。这样可以确保`Mapper`接口中的方法和映射文件中的SQL语句能够正确地关联起来。
	
	例如，假设你有一个`StudentMapper`接口，其全限定名为`com.example.mapper.StudentMapper`，那么在`studentMapper.xml`文件中，`namespace`应该设置为`com.example.mapper.StudentMapper`。
	```xml
	<mapper namespace="com.example.mapper.StudentMapper">
	    <!-- SQL语句 -->
	</mapper>
	```
	
	这样，MyBatis就可以通过`namespace`将`StudentMapper`接口中的方法和`studentMapper.xml`文件中的SQL语句进行匹配。
	
	### 3. **避免SQL语句ID冲突**
	
	在MyBatis中，每个SQL语句都有一个唯一的`id`。如果多个映射文件中有相同的`id`，MyBatis将无法区分它们。通过`namespace`，可以确保每个映射文件中的SQL语句`id`在全局范围内是唯一的。

## 与接口类对应一致
映射文件的`id`要跟接口里面的方法一致（小驼峰）
同理，映射文件的`ResultType`等返回类型和`parameterType`参数类型也要一致