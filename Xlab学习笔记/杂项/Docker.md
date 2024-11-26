模拟本地开发环境（完全隔离的），即**容器**，避免使用虚拟机
# 概念
## Docker file
创建镜像的自动化脚本
## Image 镜像
虚拟机快照，包含不同的容器
- Dockerhub
## Container 容器
## Volume 卷
永久保留写入容器指定路径的数据
### 简单语法
- `FROM`：指定基础镜像
- `WORKDIR`：指定之后的命令工作目录，如 `/app`
- `COPY <local file> <image path>`：拷贝文件到 Docker 镜像
- `RUN`：创建镜像时运行的 Shell 命令
- `CMD`：容器开始运行后执行的命令
### 命令行
#### 创建镜像
```cmd
docker build -t my-fiance .
```
- `-t` 指定镜像名称
- `.` 指定 Dockerfile 路径
#### 运行镜像
直接看 t3-app 例子
```shell
docker run -d \
  --name $DB_CONTAINER_NAME \
  -e POSTGRES_USER="postgres" \
  -e POSTGRES_PASSWORD="$DB_PASSWORD" \
  -e POSTGRES_DB=chat-room-server \
  -p "$DB_PORT":5432 \
  docker.io/postgres
```
#### 容器操作
```shell
docker ps
docker stop <ID>
docker restart <ID>
docker rm <ID>
```
