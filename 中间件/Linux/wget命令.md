wget 是 Linux 系统中一个非常强大和稳定的命令行下载工具，支持 HTTP、HTTPS 和 FTP 协议。它特别适合在服务器环境下从网络下载文件，具备​**​断点续传​**​、​**​后台下载​**​等实用功能。

下面这个表格汇总了它的核心选项和常见使用场景，方便你快速查阅。

|​**​功能需求​**​|​**​常用选项​**​|​**​命令示例​**​|
|---|---|---|
|​**​基本下载​**​|只需 URL|`wget https://example.com/file.zip`|
|​**​指定输出文件/目录​**​|`-O`(文件), `-P`(目录)|`wget -O latest.zip -P /tmp/ https://example.com/file.zip`|
|​**​断点续传​**​|`-c`|`wget -c https://example.com/large-file.iso`|
|​**​后台下载​**​|`-b`|`wget -b https://example.com/large-file.iso`|
|​**​限速下载​**​|`--limit-rate`|`wget --limit-rate=200k https://example.com/file.zip`|
|​**​递归下载（整站/目录）​**​|`-r`, `-l`(深度)|`wget -r -l 2 https://example.com/path/`|
|​**​批量下载​**​|`-i`(从文件读取URL)|`wget -i url_list.txt`|
|​**​静默/安静模式​**​|`-q`|`wget -q https://example.com/file.zip`|

### 💻 详细使用场景与技巧

#### ​**​1. 基础文件下载​**​

最基本的用法是直接使用 URL 下载文件。文件会以原始名称保存在当前目录。

```
wget http://nginx.org/download/nginx-1.24.0.tar.gz
```

- ​**​重命名文件​**​：使用 `-O`选项可以指定下载后的文件名。
    
    ```
    wget -O nginx-latest.tar.gz http://nginx.org/download/nginx-1.24.0.tar.gz
    ```
    
- ​**​下载到指定目录​**​：使用 `-P`选项可以将文件下载到特定目录。
    
    ```
    wget -P /opt/source/ http://nginx.org/download/nginx-1.24.0.tar.gz
    ```
    

#### ​**​2. 可靠下载：断点续传与后台运行​**​

当下载大文件或网络不稳定时，这些功能非常关键。

- ​**​断点续传 (`-c`)​**​：如果下载中途中断，重新使用带 `-c`参数的命令可以从断开的地方继续下载，而无需重新开始。
    
    ```
    wget -c https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-8.0.34-linux-glibc2.17-x86_64.tar.gz
    ```
    
- ​**​后台下载 (`-b`)​**​：下载大文件时，可以放入后台执行，避免占用当前终端。下载日志会写入 `wget-log`文件，可用 `tail -f wget-log`查看进度。
    
    ```
    wget -b https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-8.0.34-linux-glibc2-17-x86_64.tar.gz
    ```
    

#### ​**​3. 批量下载​**​

如果有多个文件需要下载，可以先将所有 URL 保存到一个文本文件中（例如 `url_list.txt`），然后使用 `-i`选项进行批量下载。

```
wget -i url_list.txt
```

#### ​**​4. 递归下载与网站镜像​**​

这是 wget 非常强大的功能，可以递归下载整个网站或特定目录。

- ​**​基本递归下载 (`-r`)​**​：会下载页面以及页面内链接的资源。
    
    ```
    wget -r https://example.com/path/
    ```
    
- ​**​控制递归深度 (`-l`)​**​：使用 `-l`选项限制向下遍历的层级，避免下载过多内容。
    
    ```
    wget -r -l 2 https://example.com/path/
    ```
    
- ​**​创建完整镜像 (`--mirror`)​**​：`-m`选项等效于 `-r -N -l inf --no-remove-listing`，适合做完整的站点镜像。
    

#### ​**​5. 实用技巧与注意事项​**​

- ​**​限速下载​**​：在不希望下载任务占满带宽时，可以使用 `--limit-rate`。
    
    ```
    wget --limit-rate=500k https://example.com/large-file.iso
    ```
    
- ​**​安静模式​**​：如果不希望 wget 输出详细信息，可以使用 `-q`选项进入安静模式。
    
- ​**​测试链接有效性​**​：使用 `--spider`选项可以检查链接是否有效，而无需实际下载文件。
    
    ```
    wget --spider https://example.com/file.zip
    ```
    
- ​**​规避限制​**​：某些网站可能根据 User-Agent 拒绝命令行工具的下载请求，此时可以使用 `-U`选项模拟浏览器。
    
    ```
    wget -U "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36..." https://example.com/file.zip
    ```
    

### 💎 重要提醒

递归下载 (`-r`) 功能非常强大，但请​**​务必谨慎使用​**​。请尊重目标网站的 `robots.txt`规则，并避免对他人网站造成不必要的流量压力。

希望这份指南能帮助你熟练掌握 wget！如果你对某个特定场景有更深入的问题，随时可以再问我。