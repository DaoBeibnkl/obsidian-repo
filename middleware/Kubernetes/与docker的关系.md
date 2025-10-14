容器 加一层抽象->pod
pod 加一层抽象->deployment



## docker：
* 容器 container，是独立的一套环境和配置，可以直接在其他操作系统上运行
* docker compose，基于多个container创建一整套服务
* docker swarm，调度、扩缩



## k8s：容器编排引擎
* 容器 container，本质上是服务进程
* pod，k8s中最小调度单位，一般一个pod放一个容器（符合最佳实践）
* 调度pod
