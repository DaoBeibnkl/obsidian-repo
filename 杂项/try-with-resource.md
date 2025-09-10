编译时转换：编译器会将 try-with-resources 转换为包含 finally 块的等效代码

关闭顺序：多个资源按声明顺序的逆序关闭

资源类必须实现 AutoCloseable 接口（Java 7+）或 Closeable 接口（Java 5+），来实现关闭的效果。



111


















一、基本概念  
try-with-resources 是 Java 7 引入的语法结构，用于自动管理资源（如文件流、数据库连接等需要关闭的对象）。

核心特点  
自动资源释放：无需手动调用 close()

简洁代码：减少 try-catch-finally 嵌套

安全可靠：即使发生异常也能保证资源关闭

二、语法结构  
java

try (资源声明) {  
// 使用资源的代码  
} catch (异常类型 e) {  
// 异常处理  
}  
使用条件  
资源类必须实现 AutoCloseable 接口（Java 7+）或 Closeable 接口（Java 5+）。

三、与传统写法的对比  
传统写法（Java 6及之前）  
java

InputStream in = null;  
try {  
in = new FileInputStream("file.txt");  
// 使用资源...  
} catch (IOException e) {  
e.printStackTrace();  
} finally {  
if (in != null) {  
try {  
in.close(); // 必须手动关闭  
} catch (IOException e) {  
e.printStackTrace();  
}  
}  
}  
try-with-resources 写法  
java

try (InputStream in = new FileInputStream("file.txt")) {  
// 使用资源...  
} catch (IOException e) {  
e.printStackTrace();  
}  
// 无需finally块，自动调用close()  
四、工作原理  
编译时转换：编译器会将 try-with-resources 转换为包含 finally 块的等效代码

关闭顺序：多个资源按声明顺序的逆序关闭

异常处理：

如果 try 块和 close() 都抛出异常，try 块的异常会被保留，close() 的异常被抑制（可通过 getSuppressed() 获取）

五、使用示例  
1. 单个资源  
java

try (FileOutputStream fos = new FileOutputStream("output.txt")) {  
fos.write("Hello".getBytes());  
}  
2. 多个资源  
java

//多个资源用；隔开  
try (InputStream in = new FileInputStream("src.txt");  
OutputStream out = new FileOutputStream("dest.txt")) {  
byte[] buffer = new byte[1024];  
int len;  
while ((len = in.read(buffer)) != -1) {  
out.write(buffer, 0, len);  
}  
}  
3. 自定义资源  
java

class MyResource implements AutoCloseable {  
@Override  
public void close() throws Exception {  
System.out.println("资源已释放");  
}  
}  
  
// 使用  
try (MyResource res = new MyResource()) {  
// 使用资源...  
}  
六、注意事项  
资源变量是隐式 final 的：不能在 try 块中重新赋值

java

try (InputStream in = new FileInputStream("a.txt")) {  
in = new FileInputStream("b.txt"); // 编译错误！  
}  
Java 9+ 增强：可以使用已存在的变量（必须是 final 或等效 final）

java

InputStream in = new FileInputStream("a.txt");  
try (in) { // Java 9+ 支持  
// ...  
}  
异常处理优先级：

try 块异常 > close() 异常

可通过 Throwable.getSuppressed() 获取被抑制的异常

七、最佳实践  
优先使用 try-with-resources：比手动 try-finally 更安全简洁

配合标准库类使用：所有 JDK I/O 流、JDBC 连接等都实现了 AutoCloseable

自定义资源类：记得实现 AutoCloseable 接口

八、记忆口诀  
"try 后加括号，资源里面放，  
用完自动关，代码更清爽，  
异常不用慌，抑制也能查，  
Java 7 引入，9 还能再增强。"

九、面试常见问题  
1. 为什么需要 try-with-resources？  
解决资源泄漏问题

减少样板代码（避免嵌套 try-finally）

提供更可靠的异常处理机制

2. 多个资源的关闭顺序？  
逆序关闭：最后声明的资源最先关闭

3. 如果 try 块和 close() 都抛出异常会怎样？  
try 块的异常被抛出

close() 的异常被抑制（可通过 getSuppressed() 获取）

4. 哪些类可以用在 try-with-resources 中？  
所有实现 AutoCloseable 或 Closeable 接口的类

包括：InputStream, OutputStream, Connection, Statement, ResultSet 等

try-with-resources 是 Java 资源管理的重要改进，掌握它能显著提升代码质量和安全性！