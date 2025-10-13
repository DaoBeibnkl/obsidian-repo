在 Linux 中，`mkdir`命令用于创建目录，其参数可以灵活控制目录的创建方式。以下是 `mkdir`的核心参数详解和实用示例：

---

### 📌 ​**​核心参数速查表​**​

|参数|全称|作用|示例|
|---|---|---|---|
|`-p`|​**​Parents​**​|自动创建父目录（多级目录）|`mkdir -p dir1/dir2`|
|`-m`|​**​Mode​**​|设置目录权限（无需后续`chmod`）|`mkdir -m 755 secure_dir`|
|`-v`|​**​Verbose​**​|显示创建过程的详细信息|`mkdir -v new_dir`|
|`--help`|-|显示帮助信息|`mkdir --help`|
|`--version`|-|显示版本信息|`mkdir --version`|

---

### 🔍 ​**​参数详解与场景示例​**​

#### 1. ​**​`-p`：自动创建父目录​**​

​**​场景​**​：需要创建多级嵌套目录时（最常用参数）

```
# 一次性创建多级目录（即使父目录不存在）
mkdir -p /project/{html/css,logs,backup}

# 创建后的结构：
# /project
# ├── html
# │   └── css
# ├── logs
# └── backup
```

​**​注意​**​：不加 `-p`时，若父目录不存在会报错：

```
mkdir a/b/c  # 报错：mkdir: cannot create directory 'a/b/c': No such file or directory
```

---

#### 2. ​**​`-m`：设置目录权限​**​

​**​场景​**​：创建目录时直接指定权限，避免后续`chmod`

```
# 创建仅允许用户自己读写的目录（700权限）
mkdir -m 700 private_data

# 创建Web服务器常用目录（755权限）
mkdir -m 755 public_html
```

​**​权限对照表​**​：

|数字|权限|含义|
|---|---|---|
|`755`|`rwxr-xr-x`|所有者可读写执行，其他人只读执行|
|`700`|`rwx------`|仅所有者可读写执行|
|`777`|`rwxrwxrwx`|所有人可读写执行（慎用）|

---

#### 3. ​**​`-v`：显示操作详情​**​

​**​场景​**​：脚本中需要记录目录创建日志

```
mkdir -pv /backup/{2023,2024}/{jan,feb,mar}
```

​**​输出示例​**​：

```
mkdir: created directory '/backup'
mkdir: created directory '/backup/2023'
mkdir: created directory '/backup/2023/jan'
mkdir: created directory '/backup/2023/feb'
...
```

---

### 💡 ​**​高级技巧​**​

#### 1. ​**​批量创建多个目录​**​

使用大括号扩展（Brace Expansion）：

```
# 一次性创建多个平级目录
mkdir -p client/{docs,src/{main,test},config}

# 生成结构：
# client/
# ├── docs
# ├── src
# │   ├── main
# │   └── test
# └── config
```

#### 2. ​**​结合变量创建动态目录​**​

```
# 使用变量定义目录名
YEAR=$(date +%Y)
mkdir -p "/data/logs/${YEAR}/"{quarter_1,quarter_2}
```

#### 3. ​**​防止目录已存在的错误​**​

```
# 先检查再创建（适合脚本）
[ -d "target_dir" ] || mkdir target_dir
```

---

### ⚠️ ​**​常见错误及解决​**​

1. ​**​权限不足​**​
    
    ```
    mkdir /sys/new_dir  # 报错：mkdir: cannot create directory '/sys/new_dir': Permission denied
    ```
    
    ​**​解决​**​：使用 `sudo`或选择用户有权限的路径
    
2. ​**​目录名包含特殊字符​**​
    
    ```
    mkdir "dir with space"
    mkdir 'test$dir'
    mkdir "line\nbreak"
    ```
    
3. ​**​误删已有目录​**​
    
    ```
    mkdir -p existing_dir/  # 不会报错，安全操作
    ```
    

---

### 📚 ​**​补充知识：目录权限继承​**​

新目录的默认权限由 ​**​umask​**​ 值决定：

```
umask 022       # 设置默认掩码（目录权限为755）
mkdir new_dir   # 实际权限：drwxr-xr-x
```

---

掌握这些参数后，您可以更高效地管理Linux目录结构！如需创建文件，记得配合使用 `touch`命令（如 `touch dir/file.txt`）。