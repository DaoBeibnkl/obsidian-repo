1. 清理旧版本
```shell
# 更新dnf
sudo dnf update

#清理docker残余
sudo dnf remove docker-ce \
docker-ce-cli \
containerd.io \
docker-buildx-plugin \
docker-compose

#清理残余文件
sudo rm -rf /var/lib/docker    # 核心数据目录，包含镜像、容器、卷等
sudo rm -rf /var/lib/containerd # containerd运行时数据
sudo rm -rf /etc/docker         # Docker配置文件，如daemon.json
sudo rm -rf /var/run/docker.sock # Docker套接字文件  
```

2. 安装必要依赖软件包
```shell
#它提供了管理软件源所需的 `dnf config-manager`命令。
sudo dnf -y install dnf-plugins-core
```

3. 添加 Docker 下载源​
dnf|系统基础仓库 (BaseOS/AppStream)里面没有docker引擎，需要额外添加仓库。

为了获得更快的下载速度，建议添加国内镜像源。
(要使用config-manager自动处理仓库文件repo，必须先安装dnf-plugins-core)

| 源名称             | 命令                                                                                                              |
| --------------- | --------------------------------------------------------------------------------------------------------------- |
| ​**​阿里云（推荐）​**​ | `sudo dnf config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo`           |
| 清华大学            | `sudo dnf config-manager --add-repo https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/docker-ce.repo` |
| Docker官方源       | `sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo`                    |
|                 |                                                                                                                 |

4. ​安装 Docker 引擎​
安装 Docker CE（社区版）及其核心组件。
```shell
sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```


5. 配置加速器
加速拉取镜像
```shell
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": [
    "https://docker.m.daocloud.io"
  ]
}
EOF
```