# 1、
使用顺序语句、循环语句、条件语句三个控制语句，来实现灵活的SQL语句编写。

```xml
'''核心配置文件中起别名
<configuration>  
    <properties resource="Myproperty.properties"></properties>  
    <typeAliases>        
	    <typeAlias type="bean.Student" alias="student"></typeAlias>  
    </typeAliases>
</configuration>
'''

<typeAlias>
	<typeAlia>
</typeAlias>

<select id="selectById" resultType=student 
```

# 2、foreach标签中的属性
在 MyBatis 中，`parameterType` 和 `collection` 属性的作用机制不同，它们的角色和用法需要明确区分。以下是详细解释：

---

### 🔧 ​**​1. `parameterType` 的本质：参数类型的声明​**​

`parameterType` 用于 ​**​声明 SQL 语句的输入参数类型​**​，它本身不是变量，而是告诉 MyBatis：

- 传入的 Java 参数是什么类型（如 `List`、`Map`、`String` 等）；
- 如何解析参数中的属性或值。

​**​示例场景​**​：

```
<select id="selectDynamic" parameterType="list" ...>
```

此处 `parameterType="list"` 表示方法参数是 `List` 类型（MyBatis 对 `java.util.List` 的默认别名）。

---

### 🔄 ​**​2. `collection` 属性的作用：引用集合变量​**​

`<foreach>` 标签中的 `collection` 属性 ​**​指向一个具体的集合变量名​**​，该变量必须是已存在的参数或参数中的属性：

- ​**​当 `parameterType="list"` 时​**​：  
    `collection="list"` 表示遍历 ​**​方法参数本身​**​（即传入的 `List` 对象）。
- ​**​当 `parameterType` 是 POJO 或 Map 时​**​：  
    `collection` 需指向该对象内的某个集合属性（如 `collection="userList"` 表示遍历参数对象中的 `userList` 属性）。

​**​用户代码解析​**​：

```
<foreach collection="list" item="id" ...>
```

- `collection="list"`：  
    因为 `parameterType="list"`，此处 `list` 指代方法参数（即 `List<Integer> ids`），而非 `parameterType` 类型名。
- `#{id}`：  
    是 `item="id"` 定义的循环变量，表示当前遍历到的元素值。

---

### 💎 ​**​3. 核心区别总结​**​

| ​**​属性​**​      | ​**​角色​**​      | ​**​示例场景​**​                                              |
| --------------- | --------------- | --------------------------------------------------------- |
| `parameterType` | ​**​声明参数类型​**​  | `parameterType="list"` 表示方法参数是 `List` 类型。                 |
| `collection`    | ​**​引用集合变量名​**​ | `collection="list"` 指向方法参数本身（需与 `parameterType` 定义的类型匹配）。 |

---

### ⚠️ ​**​4. 常见误区与纠正​**​

- ​**​误区​**​：  
    “`parameterType` 是变量名，所以 `collection` 直接用它”。
- ​**​纠正​**​：  
    `collection` 引用的是 ​**​参数名​**​（如 `list`、`array` 或 `@Param` 注解定义的名称），而非 `parameterType` 的类型名。  
    例如，若方法参数用 `@Param("ids")` 标注，需改为 `collection="ids"`，此时 `parameterType` 可省略或仍为 `list`。

---

### 🌰 ​**​5. 对比示例​**​

#### 场景 1：单参数 `List`（无 `@Param`）

```
List<Student> selectDynamic(List<Integer> ids); // 参数默认名称为 list
```

```
<select parameterType="list"> <!-- 声明参数类型 -->
    <foreach collection="list" ...> <!-- 引用参数变量名 -->
```

#### 场景 2：多参数（用 `@Param`）

```
List<Student> selectDynamic(@Param("ids") List<Integer> idList);
```

```
<select> <!-- parameterType 可省略 -->
    <foreach collection="ids" ...> <!-- 引用 @Param 定义的变量名 -->
```

---


## 3、foreach的使用
```xml
<select id="selectDynamic" resultType="bean.Student" parameterType="java.util.List">  
    SELECT * FROM student  
    <where>  
        <foreach collection="list" open="id IN(" item="id" close=")" separator=",">  
            #{id}  
        </foreach>  
    </where>
</select>
```

# 3、sql语句抽取，重复利用
```xml
<sql id="Basic"> SELECT * FROM student</sql>
```