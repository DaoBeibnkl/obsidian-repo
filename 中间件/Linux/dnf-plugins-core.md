> 它提供了管理软件源所需的 `dnf config-manager`命令。
​

下面这个表格清晰地说明了它的必要性：

|场景|是否有必要安装 `dnf-plugins-core`？|原因说明|
|---|---|---|
|​**​通过 DNF 从官方仓库安装 Docker​**​|​**​非常有必要（强烈推荐）​**​|它提供 `dnf config-manager`命令，这是​**​安全、方便地添加 Docker 官方仓库​**​的关键工具。|
|​**​使用 RPM 包手动离线安装​**​|不必要|手动下载所有 RPM 依赖包并直接用 `dnf install ./package.rpm`安装，不需要配置仓库。|
|​**​使用自动化脚本（如 get.docker.com）​**​|不必要|官方安装脚本会自动处理所有依赖和仓库配置，无需手动干预。|
|​**​日常普通软件安装​**​|​**​很有用（推荐安装）​**​|它提供的工具（尤其是 `config-manager`）对管理第三方仓库、启用/禁用仓库等操作非常方便。|

---

### 为什么需要 `dnf-plugins-core`？

核心原因在于它提供了一个名为 ​**​`dnf config-manager`​**​ 的关键工具。我们来对比一下有它和没有它的安装流程差异：

#### 1. 不安装 `dnf-plugins-core`的复杂流程

如果你想添加 Docker 的官方 YUM/DNF 仓库，你需要手动创建一个 `.repo`文件：

```
# 1. 手动创建仓库配置文件
sudo vi /etc/yum.repos.d/docker-ce.repo

# 2. 在文件中手动输入以下内容（容易出错）
[docker-ce-stable]
name=Docker CE Stable - $basearch
baseurl=https://download.docker.com/linux/centos/$releasever/$basearch/stable
enabled=1
gpgcheck=1
gpgkey=https://download.docker.com/linux/centos/gpg
```

这个过程​**​繁琐且容易出错​**​（比如写错 URL 或 `$basearch`这样的变量）。

#### 2. 安装 `dnf-plugins-core`后的简单流程

安装了该插件后，你可以用一行命令轻松、可靠地添加仓库：

```
# 1. 安装 dnf-plugins-core
sudo dnf install dnf-plugins-core

# 2. 一键添加 Docker 的官方仓库（推荐方式）
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# 3. 直接安装 Docker
sudo dnf install docker-ce docker-ce-cli containerd.io
```

​**​`dnf config-manager --add-repo`的优势：​**​

- ​**​自动可靠​**​：它会自动从给定的 URL 下载正确的 `.repo`文件并放到正确的位置。
    
- ​**​避免错误​**​：无需你手动输入容易出错的内容。
    
- ​**​功能强大​**​：还提供了 `--enable-repo`、`--disable-repo`等命令来轻松管理仓库。
    

---

### `dnf-plugins-core`还提供了哪些其他有用工具？

除了 `config-manager`，这个元数据包还包含其他实用的插件：

- `dnf copr`: 用于管理 Copr（个人仓库），这是 Fedora/RHEL 生态中非常流行的软件分发方式。
    
- `dnf debug`: 用于调试 DNF 问题。
    
- `dnf builddep`: 可以一键安装编译某个软件所需的所有依赖包，对开发者非常有用。
    

### 💎 总结与建议

​**​结论：对于通过 DNF 安装 Docker 的场景，安装 `dnf-plugins-core`是非常有必要的。​**​ 它不仅是 Docker 官方文档中推荐的步骤，也能让你的安装过程更简单、更规范、更不容易出错。

即使不为安装 Docker，作为一个系统管理的好习惯，也建议你安装这个工具集：

```
sudo dnf install dnf-plugins-core
```

它会为你日后管理软件仓库和安装其他第三方软件带来极大的便利。