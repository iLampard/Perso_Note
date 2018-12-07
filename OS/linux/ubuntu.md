# Ubuntu Snippets

### 将路径加入命令行启动

```bash
gedit ~/.bashrc
export PATH="/home/.../workarea/softwares/conda3/bin:"$PATH
source ~/.bashrc
```



### 强制关闭端口
```bash
lsof -i:8000    # 找到端口是8000的进程
kill -9 XXXX
```