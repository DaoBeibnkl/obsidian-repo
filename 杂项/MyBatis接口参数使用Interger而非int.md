在 MyBatis 的 Mapper 接口方法中，参数使用 `Integer` 而非 `int` 的原因主要涉及 ​**​`null` 值处理​**​和 ​**​MyBatis 类型映射机制​**​的设计考量。以下是具体分析：

---

### ⚙️ ​**​1. `null` 值的兼容性​**​

- ​**​`int` 是基本类型​**​，不能为 `null`。若数据库字段允许 `NULL` 且查询条件可能缺失，传递 `null` 会导致错误：
    
    ```
    // 错误示例：无法传递 null
    List<Student> selectByCid(int cid); 
    ```
    
    ```
    mapper.selectByCid(null); // 编译错误或运行时 NullPointerException
    ```
    
- ​**​`Integer` 是包装类型​**​，支持 `null` 值，更符合实际业务场景（如可选参数查询）：
    
    ```
    // 正确示例：可安全处理 null
    List<Student> selectByCid(Integer cid);
    ```
    
    ```
    mapper.selectByCid(null); // 可正常执行，返回空列表或全量数据
    ```
    

---

### 🔍 ​**​2. MyBatis 参数绑定的要求​**​

MyBatis 通过 ​**​动态代理​**​ 实现 Mapper 接口，其底层依赖 ​**​反射机制​**​ 解析方法签名：

- ​**​反射限制​**​：反射调用方法时，基本类型参数（如 `int`）会被强转为默认值（`0`），若传入 `null` 会触发 `IllegalArgumentException`。
- ​**​类型映射逻辑​**​：MyBatis 的 `TypeHandler` 在处理参数时，优先匹配包装类型（如 `Integer`），以确保 `null` 值能正确映射到 SQL 的 `NULL`。

---

### 🛡️ ​**​3. 类型安全性与一致性​**​

- ​**​避免歧义​**​：数据库中的 `NULL` 与 `0` 语义不同（例如 `cid=0` 可能是有效值）。使用 `Integer` 能明确区分 `null`（无参数）和 `0`（具体值）。
- ​**​ORM 框架规范​**​：MyBatis 官方示例及最佳实践中，​**​推荐对可能为空的参数使用包装类型​**​，以保持类型系统的一致性。

---

### 💎 ​**​4. 实际开发中的建议​**​

|​**​场景​**​|​**​参数类型​**​|​**​说明​**​|
|---|---|---|
|​**​参数必填​**​|`int`|仅当业务逻辑强制要求参数非空时可用（需额外校验）|
|​**​参数可选（可能为空）​**​|`Integer`|✅ 首选方案，兼容 `null` 值，避免运行时异常|
|​**​批量查询​**​|`List<Integer>`|直接支持集合参数，无需转为基本类型数组|

---

### ✅ ​**​正确用法示例​**​

```
// Mapper 接口
public interface StudentMapper {
    // 使用 Integer 支持 cid 为 null 的查询
    @Select("SELECT * FROM student WHERE class_id = #{cid}")
    List<Student> selectByCid(Integer cid);
}

// Service 层调用
public List<Student> getStudents(Integer classId) {
    // classId 可能为 null（查询所有学生）
    return studentMapper.selectByCid(classId);
}
```

---

### ⚠️ ​**​例外情况​**​

若确定参数 ​**​绝对不会为空​**​（如主键查询），理论上可使用 `int`，但仍需在调用前校验非空：

```
// 不推荐：需调用方确保 cid != null
List<Student> selectByCid(int cid);

// 调用前需显式校验
if (cid != null) {
    mapper.selectByCid(cid); // 自动拆箱，cid=null 时抛 NullPointerException
}
```

> ​**​风险​**​：增加调用方负担，且易遗漏校验，导致生产环境异常。

---

### 💎 ​**​总结​**​

- ​**​优先使用 `Integer`​**​：适应 MyBatis 的 `null` 值处理机制，避免运行时异常，提升代码健壮性。
- ​**​保留 `int` 的场景​**​：仅限性能敏感且参数绝对非空的场景（需谨慎评估）。
- ​**​设计原则​**​：ORM 框架中，​**​包装类型优先于基本类型​**​是通用最佳实践，与数据库的 `NULL` 语义天然兼容。