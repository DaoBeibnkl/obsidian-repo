dnf-repolist，列出仓库
如果有docker官方的仓库，需要先删除，再添加阿里云的docker仓库源

### 🛠️ 快速配置阿里云镜像源

为 CentOS Stream 9 系统配置阿里云镜像源，主要有以下几种方法，您可以选择一种进行操作。

|方法|特点|推荐度|
|---|---|---|
|​**​方法一：直接创建配置文件​**​|一步到位，清晰明确，不易出错。|⭐⭐⭐⭐⭐|
|​**​方法二：使用 `curl`命令下载​**​|快速便捷，适合喜欢命令行的用户。|⭐⭐⭐⭐|

#### 方法一：直接创建配置文件

这是最推荐的方法，手动创建一个新的配置文件。

1. ​**​创建并编辑仓库文件​**​：
    
    ```
    sudo vi /etc/yum.repos.d/aliyun-centos-stream-9.repo
    ```
    
2. ​**​写入配置内容​**​：将以下配置粘贴到文件中。它包含了最核心的 `BaseOS`和 `AppStream`仓库。
    
    ```
    [aliyun-baseos]
    name=CentOS Stream 9 - BaseOS (Aliyun)
    baseurl=https://mirrors.aliyun.com/centos-stream/9-stream/BaseOS/$basearch/os/
    enabled=1
    gpgcheck=1
    gpgkey=https://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-Official
    
    [aliyun-appstream]
    name=CentOS Stream 9 - AppStream (Aliyun)
    baseurl=https://mirrors.aliyun.com/centos-stream/9-stream/AppStream/$basearch/os/
    enabled=1
    gpgcheck=1
    gpgkey=https://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-Official
    ```
    

#### 方法二：使用 `curl`命令下载（没有9）

阿里云也提供了一个快速配置脚本，您可以尝试运行以下命令：

```
sudo curl -o /etc/yum.repos.d/CentOS-Stream-9-Aliyun.repo http://mirrors.aliyun.com/repo/Centos-9.repo
```

​**​注意​**​：此方法可能因阿里云仓库页面结构调整而存在不确定性。如果执行后无效，建议使用方法一。

### ⚙️ 完成配置与验证

无论使用哪种方法，完成配置后都需要执行以下步骤：

1. ​**​清理并重建缓存​**​：让 DNF 包管理器识别新的镜像源。
    
    ```
    sudo dnf clean all
    sudo dnf makecache
    ```
    
2. ​**​验证仓库状态​**​：查看阿里云源是否已成功添加并启用。
    
    ```
    sudo dnf repolist
    ```
    
    在输出的仓库列表中，您应该能看到 `aliyun-baseos`和 `aliyun-appstream`仓库。
    

### 💡 实用补充

- ​**​关于GPG检查​**​：配置中的 `gpgcheck=1`是安全设置，用于验证软件包的完整性和来源，​**​强烈建议保持开启​**​以确保安全。
    
- ​**​安装EPEL源​**​：为了获得更丰富的第三方软件包，您还可以配置阿里云提供的 EPEL（Extra Packages for Enterprise Linux）源，这能进一步扩展可安装软件的范围。
    
    ```
    sudo dnf install epel-release
    ```
    
    安装后，您也可以类似地将其配置文件中的源地址替换为阿里云的镜像地址。
    

希望这份指南能帮助您顺利完成配置！享受飞快的软件更新和安装体验吧。如果过程中遇到任何问题，随时可以再问我。