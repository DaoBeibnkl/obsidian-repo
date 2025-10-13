>docker run \[选项]   镜像  \[命令] \[参数]，
>
>命令和参数通常不用管，因此注意顺序把镜像放到最后。

### 🐳 ​**​Docker Run 核心参数全称解析​**​

#### ​**​1. 容器运行控制​**​

|参数|全称|作用|示例|
|---|---|---|---|
|`-d`|​**​Detached​**​|后台运行容器|`docker run -d nginx`|
|`-it`|​**​Interactive + TTY​**​|交互式终端（组合使用）|`docker run -it ubuntu bash`|
|`--rm`|​**​Remove​**​|容器退出时自动删除|`docker run --rm alpine echo "hello"`|
|`--restart`|​**​Restart Policy​**​|设置重启策略（always, on-failure）|`docker run --restart=always nginx`|

#### ​**​2. 命名与标识​**​

|参数|全称|作用|示例|
|---|---|---|---|
|`--name`|​**​Container Name​**​|指定容器名称|`docker run --name my-web nginx`|
|`-h`|​**​Hostname​**​|设置容器主机名|`docker run -h app-server nginx`|

#### ​**​3. 网络配置​**​

|参数|全称|作用|示例|
|---|---|---|---|
|`-p`|​**​Publish​**​|暴露容器端口到主机|`docker run -p 8080:80 nginx`|
|`-P`|​**​Publish all​**​|暴露所有声明端口（大写P）|`docker run -P nginx`|
|`--network`|​**​Network Mode​**​|指定网络模式（bridge/host/none）|`docker run --network=host nginx`|

#### ​**​4. 存储卷挂载​**​

|参数|全称|作用|示例|
|---|---|---|---|
|`-v`|​**​Volume​**​|挂载主机目录/卷到容器|`docker run -v /data:/app/data nginx`|
|`--mount`|​**​Mount​**​|更灵活的挂载语法（类型化参数）|`docker run --mount type=bind,src=/data,dst=/app nginx`|

#### ​**​5. 环境变量与权限​**​

|参数|全称|作用|示例|
|---|---|---|---|
|`-e`|​**​Environment​**​|设置环境变量|`docker run -e JAVA_HOME=/usr/lib/jvm nginx`|
|`--env-file`|​**​Environment File​**​|从文件加载环境变量|`docker run --env-file .env nginx`|
|`-u`|​**​User​**​|指定运行用户（UID:GID）|`docker run -u 1000:1000 nginx`|
|`--cap-add`|​**​Capability Add​**​|添加Linux权能|`docker run --cap-add NET_ADMIN nginx`|

#### ​**​6. 资源限制​**​

|参数|全称|作用|示例|
|---|---|---|---|
|`-m`|​**​Memory​**​|限制内存使用|`docker run -m 512m nginx`|
|`--cpus`|​**​CPUs​**​|限制CPU核心数|`docker run --cpus=1.5 nginx`|
|`--cpuset-cpus`|​**​CPU Set CPUs​**​|绑定到指定CPU核心|`docker run --cpuset-cpus="0-2" nginx`|

---

### 📌 ​**​经典组合场景​**​

#### 1. 开发调试容器

```
docker run -it --rm \
  -v $(pwd):/app \          # 挂载当前目录
  -p 3000:3000 \            # 暴露开发端口
  -e DEBUG=true \           # 设置调试模式
  node:18 bash              # 进入交互式终端
```

#### 2. 生产级Web服务

```
docker run -d \
  --name web-server \
  -p 80:80 \
  -v /etc/nginx:/etc/nginx \  # 挂载配置
  --restart=unless-stopped \
  --cpus=2 -m 1g \          # 资源限制
  nginx:alpine
```

---

### ⚠️ ​**​易混淆参数对比​**​

|参数|易混淆点|正确用法|
|---|---|---|
|`-p`vs `-P`|`-p`需手动指定端口映射，`-P`自动映射所有暴露端口|`-p 8080:80`vs `-P`|
|`-v`vs `--mount`|`-v`语法简洁，`--mount`支持高级选项（如只读挂载）|`-v /data:/app:ro`vs `--mount type=bind,src=/data,dst=/app,readonly`|
|`-it`vs `-i`/`-t`|`-it`是组合参数，单独使用 `-i`保持STDIN打开，`-t`分配伪终端|交互式命令必须组合使用：`-it`|

---

### 💡 ​**​设计哲学解析​**​

Docker 参数命名遵循 Unix 传统：

- ​**​单字母参数​**​（如 `-d`）用于高频操作
    
- ​**​双横杠长参数​**​（如 `--restart`）提高可读性
    
- ​**​组合参数​**​（如 `-it`）实现复杂行为
    

掌握这些全称和设计逻辑后，您不仅能更精准地使用命令，还能在阅读脚本时快速理解其意图。需要具体场景的深入解析，请随时告知！
