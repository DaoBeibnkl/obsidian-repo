# 以插件的形式添加到IDEA的maven
```xml
<build>  
    <plugins>        
	    <plugin>            
		    <groupId>org.apache.tomcat.maven</groupId>  
            <artifactId>tomcat7-maven-plugin</artifactId>  
            <version>2.2</version>  
            <configuration>                
	            <port>8080</port>  <!--配置端口号为8080-->
                <path>/</path>
            </configuration>        
        </plugin>    
    </plugins>        
</build>
```

注意项目的maven打包方式要是war包。

# 报错信息：A child container failed during start

tomcat服务器里面也有这个jar包，打包部署的时候冲突了。

```xml
<dependency>  
    <groupId>javax.servlet</groupId>  
    <artifactId>javax.servlet-api</artifactId>  
    <version>3.1.0</version>  
    <scope>provided</scope><!--添加这个，在编译和测试的时候有效-->
</dependency>
```


# Tomcat热部署要用2.2版本
要不然报错
```xml
<build>  
    <plugins>        
	    <plugin>            
		    <groupId>org.apache.tomcat.maven</groupId>  
            <artifactId>tomcat7-maven-plugin</artifactId>  
            <version>2.2</version>  
            <configuration>                
	            <port>8080</port>  <!--配置端口号为8080-->
                <path>/</path>
            </configuration>        
        </plugin>    
    </plugins>        
</build>
```


# ### 父模块定义版本，子模块引用即可

```xml
<!-- 父模块 pom.xml -->
<build>
    <pluginManagement>
        <plugins>
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <version>2.2</version>
                <configuration>
                    <port>8080</port>
                    <path>/</path>
                    <update>true</update>
                </configuration>
            </plugin>
        </plugins>
    </pluginManagement>
</build>
<!-- 子模块 pom.xml -->
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```


# Tomcat插件在快速配置没有选项

	根据你的描述和问题分析，​**​命令行和IDEA无法识别 `tomcat7:run` 命令​**​ 的核心原因在于 ​**​Maven插件前缀解析失败​**​ 或 ​**​IDEA配置缺失​**​。以下是具体原因和解决方案：

---

### 一、命令行报错 `No plugin found for prefix 'tomcat7'` 的解决

#### 1. ​**​插件前缀未正确注册​**​

Maven通过 `pluginGroups` 配置解析插件前缀（如 `tomcat7`）。若未将插件组添加到全局配置，需手动指定完整插件坐标：

`mvn org.apache.tomcat.maven:tomcat7-maven-plugin:2.2:run`

​**​验证是否配置插件组​**​：  
检查 `~/.m2/settings.xml`，添加以下配置：

`<settings>       <pluginGroups>           <pluginGroup>org.apache.tomcat.maven</pluginGroup>       </pluginGroups>   </settings>`

#### 2. ​**​本地仓库插件损坏​**​

执行以下命令清理并重新下载插件：

`mvn clean install -U   rm -rf ~/.m2/repository/org/apache/tomcat/maven/`

---

### 二、IDEA无法识别 `tomcat7:run` 的解决

#### 1. ​**​手动配置Maven运行命令​**​

- 打开 `Run/Debug Configurations` → 点击 `+` → 选择 `Maven`
- ​**​Name​**​：自定义名称（如 `Run Tomcat7`）
- ​**​Command line​**​：输入 `tomcat7:run`
- ​**​Working directory​**​：选择项目根目录
- ​**​勾选​**​ `Resolve Workspace artifacts`

#### 2. ​**​验证插件配置有效性​**​

确保 `pom.xml` 中插件声明无拼写错误：

`<plugin>       <groupId>org.apache.tomcat.maven</groupId>       <artifactId>tomcat7-maven-plugin</artifactId>       <version>2.2</version>       <configuration>           <port>8080</port>           <path>/</path>       </configuration>   </plugin>`

​**​关键点​**​：`groupId` 和 `artifactId` 必须与官方一致。

---

### 三、其他潜在问题排查

#### 1. ​**​IDEA缓存导致识别失败​**​

执行 `File > Invalidate Caches / Restart...` → 选择 `Invalidate and Restart`。

#### 2. ​**​多模块项目配置问题​**​

若项目为多模块结构，需在主 `pom.xml` 的 `<build><pluginManagement>` 中声明插件：

`<pluginManagement>       <plugins>           <plugin>               <groupId>org.apache.tomcat.maven</groupId>               <artifactId>tomcat7-maven-plugin</artifactId>               <version>2.2</version>           </plugin>       </plugins>   </pluginManagement>`

#### 3. ​**​Maven版本兼容性​**​

确认使用 ​**​Maven 3.0+​**​（旧版本可能不支持插件前缀自动解析）。

---

### 四、快速验证步骤

1. ​**​命令行验证​**​：
    
    `mvn help:describe -Dplugin=org.apache.tomcat.maven:tomcat7-maven-plugin`
    
    若输出插件信息，则证明插件已正确安装。
    
2. ​**​IDEA调试​**​：  
    在IDEA的Maven面板中展开 `Plugins > tomcat7`，右键 `tomcat7:run` → 选择 `Run`，观察是否正常启动。
    

---

### 总结

通过上述配置，命令行和IDEA应能正常识别 `tomcat7:run`。若仍失败，建议对比官方示例项目配置，或检查网络代理是否拦截了Maven仓库请求。