在 MyBatis 的注解开发中，​**​方法参数名是否必须与 `@Select` 等注解中的 SQL 参数名一致​**​，取决于参数的数量和是否使用 `@Param` 注解。以下是详细规则：

### 📌 1. ​**​单参数方法（无 `@Param` 注解）​**​

当接口方法​**​只有一个参数​**​且未使用 `@Param` 注解时，​**​SQL 中的参数名可以任意指定​**​，与实际参数名无关。  
​**​示例​**​：

```
@Select("SELECT * FROM student WHERE cid = #{任意名称}")
List<Student> selectByCid(Integer id); // SQL 中的 #{xxx} 可为 #{cid}、#{id} 或 #{abc}
```

​**​原因​**​：  
MyBatis 会直接使用该参数值，不依赖名称匹配。

---

### 🔢 2. ​**​多参数方法（无 `@Param` 注解）​**​

当方法有​**​多个参数​**​且未用 `@Param` 注解时，必须通过 ​**​默认占位符​**​ 访问参数：

- ​**​`#{arg0}`、`#{arg1}`​**​（参数索引，从 0 开始）
- ​**​`#{param1}`、`#{param2}`​**​（自动编号，从 1 开始）  
    ​**​示例​**​：

```
@Select("SELECT * FROM student WHERE cid = #{arg0} AND name = #{arg1}")
List<Student> selectByCidAndName(Integer cid, String name);
```

---

### 🏷️ 3. ​**​使用 `@Param` 注解时​**​

若方法参数标注了 `@Param("别名")`，​**​SQL 中的参数名必须与别名一致​**​。  
​**​示例​**​：

```
@Select("SELECT * FROM student WHERE cid = #{cidAlias}")
List<Student> selectByCid(@Param("cidAlias") Integer id); // 必须用 #{cidAlias}
```

> ⚠️ 注意：`@Param` 注解的优先级最高，会覆盖默认参数名。

---

### ✅ 总结你的代码场景

```
public interface StudentMapper {
    @Select("SELECT * FROM student WHERE cid = #{cid}") // SQL 参数名 #{cid}
    List<Student> selectByCid(Integer id); // 方法参数名 id
}
```

- ​**​是否必须一致？​**​ ❌ ​**​不需要​**​。
- ​**​原因​**​：单参数方法未用 `@Param`，SQL 中的 `#{cid}` 是自定义名称，与参数 `id` 无关，实际会正确绑定到 `id` 的值。

---

### ⚡ 最佳实践建议

1. ​**​单参数​**​：可自由命名 SQL 参数（如 `#{cid}`），无需匹配方法参数名。
2. ​**​多参数​**​：
    - 优先使用 `@Param` 显式命名（增强可读性）；
    - 避免依赖 `arg0/param1`（易混淆且难维护）。
3. ​**​避免混用​**​：
    - 不要同时使用 `@Param` 和默认占位符（如 `#{param1}`），易导致错误。

> 💡 ​**​扩展提示​**​：若需强制方法参数名与 SQL 参数名一致，需在编译时添加 `-parameters` 参数（Java 8+），但 MyBatis 中单参数场景无需此操作。