你在 SpringConfig 配置类中只配置了 `@ComponentScan("com.cjj.service")` 来扫描 service 包，却依然能成功注入并使用 `UserDao` 和 `BookDao`，这确实可能与你的 MyBatis 配置有关。核心原因在于 ​**​MyBatis 的 `MapperScannerConfigurer` 帮你自动扫描并注册了 Dao 层的接口​**​。

为了帮你更直观地理解，我用一个表格对比这两种机制：

|特性|传统 Spring 组件扫描 (`@ComponentScan`)|MyBatis Mapper 扫描 (`MapperScannerConfigurer`)|
|---|---|---|
|​**​目标类型​**​|带有 `@Component`, `@Service`, `@Repository` 等注解的​**​类​**​|Mapper​**​接口​**​ (通常是 `com.cjj.dao` 包下的接口)|
|​**​注册的 Bean 类型​**​|组件类本身 (如 `BookDaoImpl`)|`MapperFactoryBean` (工厂 Bean, 其 `getObject()` 返回代理对象)|
|​**​依赖注入依据​**​|Bean 的类型是组件类的类型|Bean 的类型是 Mapper​**​接口​**​的类型 (如 `BookDao`)|
|​**​实现机制​**​|Spring 直接实例化并管理|MyBatis 创建​**​动态代理对象​**​, Spring 管理 `MapperFactoryBean`|
|​**​是否需要注解​**​|需要 `@Component` 等注解|无需在接口上添加注解 (但 `@Repository` 可增强异常转换)|

下面我们来详细看看这是如何发生的：

### 🔧 MyBatis 的 Mapper 扫描机制

1. ​**​`MapperScannerConfigurer` 的作用​**​  
    你在 `MybatisConfig` 中配置了 `MapperScannerConfigurer`，并指定了 `msc.setBasePackage("com.cjj.dao")`。这个配置器会在 Spring 容器初始化时​**​自动扫描 `com.cjj.dao` 包下的所有接口​**​（例如 `BookDao` 和 `UserDao`）。
    
2. ​**​自动注册为 Bean​**​  
    对于扫描到的每一个 Mapper 接口，`MapperScannerConfigurer` 并不会直接将接口实例化（因为接口无法直接实例化），而是会为每个接口​**​在 Spring 容器中注册一个 `MapperFactoryBean`​**​。`MapperFactoryBean` 是 Spring 提供的一个特殊工厂 Bean，它的 `getObject()` 方法会​**​返回一个 MyBatis 通过动态代理生成的该接口的实现对象​**​。
    
3. ​**​依赖注入的依据​**​  
    当你在 `BookServiceImpl` 中使用 `@Autowired` 注入 `BookDao` 时：
    
    ```
    @Autowired
    private BookDao bookDao;
    ```
    
    Spring 容器会查找类型为 `BookDao` 的 Bean。它会找到由 `MapperFactoryBean` 创建的​**​代理对象​**​（其类型恰好就是 `BookDao` 接口的类型），从而成功完成注入。`UserDao` 的注入过程同理。
    

### 📌 总结与对比

|扫描方式|要求|Bean 类型|实现方式|
|---|---|---|---|
|​**​`@ComponentScan`​**​|需注解 (`@Repository`)|`BookDaoImpl`|Spring 直接实例化|
|​**​`MapperScannerConfigurer`​**​|无需注解|`MapperFactoryBean` (代理)|MyBatis 动态代理|

所以，​**​你虽然没有在 `SpringConfig` 中扫描 dao 包，但通过 MyBatis 的 `MapperScannerConfigurer`，Dao 层的接口依然被注册到了 Spring 容器中​**​，从而能够被成功注入到 Service 层。

### 💡 额外提示
- 这种设计体现了 ​**​Spring 框架的“约定大于配置”​**​ 的理念和强大的扩展能力。通过 `MapperScannerConfigurer`，MyBatis 巧妙地与 Spring 容器集成，简化了开发者的配置工作。
