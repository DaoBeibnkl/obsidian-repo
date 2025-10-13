每使用一次，会去掉末尾的文件或者目录

```bash
$(dirname $(dirname $(readlink -f $(which java))))

```