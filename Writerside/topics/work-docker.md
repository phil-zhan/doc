# work-docker

### 导出镜像

```Shell
# 首先，列出你当前的 Docker 镜像，以确认你要导出的镜像名称或 ID。
docker images

# 使用 docker save 命令导出镜像。指定镜像名称或 ID 以及导出的 tar 文件名称。
docker save -o my-image.tar my-image:latest

# 导出完成后，可以使用 ls 命令确认 tar 文件已经创建。
ls -lh my-image.tar
```

### 导入镜像

```Shell
docker load -i my-app.tar
```

### Docker设置语言环境


```Shell
RUN localedef -c -f UTF-8 -i zh_CN zh_CN.UTF-8 || true
# 设置环境变量来更改编码
ENV LANG=zh_CN.UTF-8
ENV LC_ALL=zh_CN.UTF-8
# 复制当前目录的内容到容器的/app目录
COPY das_ai.py db_module.py process.py server.py test_sql.py /app/
# 配置Python的编码
ENV PYTHONIOENCODING=utf-8
```

