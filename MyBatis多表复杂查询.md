>当返回查询类型比较复杂时，无法直接使用resultType标签和java内置类型来指明，因此需要我们使用resultMap来手动指明返回类型

## 复杂返回类型resultMap

 一个表关联一个表（association用来映射单个对象）

	  resultMap需要两个属性，用来标识的id，和需要映射的类型（可以在核心配置中起别名）  
    <id>标签用来映射主键，需要两个属性，字段名column，属性名property  
    <result>标签可以映射普通字段  
    
    <association>标签用来映射外键，需要两个属性，属性名property，类型或者类名javaType  
    里面的标签同理。

```xml
<!--  
    resultMap需要两个属性，用来标识的id，和需要映射的类型（可以在核心配置中起别名）  
    <id>标签用来映射主键，需要两个属性，字段名column，属性名property  
    <result>标签可以映射普通字段  
    
    <association>标签用来映射外键，需要两个属性，属性名property，类型或者类名javaType  
    里面的标签同理。  
-->  
<resultMap id="OneToOne" type="bean.Card">  
    <id column="cid" property="id"></id>  
    <result column="number" property="number"></result>  
    
    <association property="person" javaType="bean.Person">  
        <id column="pid" property="id"/>  
        <result column="phone" property="phone"/>  
        <result column="name" property="name"/>  
    </association>
</resultMap>
```

一个表关联多个表（collection标签用来映射对象集合，即多个同类型对象）