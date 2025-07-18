# 在 WSL 中使用 Docker 安装 PostgreSQL

本指南将引导您在 WSL (Windows Subsystem for Linux) 环境下，通过 Docker 安装和运行 PostgreSQL。

## 步骤 1: 拉取 PostgreSQL Docker 镜像

首先，打开您的 WSL 终端，执行以下命令来从 Docker Hub 拉取最新的 PostgreSQL 官方镜像。

```bash
docker pull postgres
```

## 步骤 2: 运行 PostgreSQL 容器

拉取镜像后，您可以使用以下命令来创建并运行一个新的 PostgreSQL 容器。

```bash
docker run --name postgres -e POSTGRES_PASSWORD=P@ssw0rd12345678 -p 5432:5432 -d postgres
```

命令详解：
- `docker run`: 运行一个新容器。
- `--name my-postgres`: 为您的容器指定一个名称，方便后续管理。您可以替换 `my-postgres` 为您喜欢的任何名称。
- `-e POSTGRES_PASSWORD=mysecretpassword`: 设置 PostgreSQL 数据库的超级用户密码。**请务必将 `mysecretpassword` 替换为您自己的安全密码。**
- `-p 5432:5432`: 将主机的 5432 端口映射到容器的 5432 端口。这样您就可以通过主机的 `localhost:5432` 来访问数据库。
- `-d`: 以分离模式（detached mode）在后台运行容器。
- `postgres`: 指定要使用的镜像。

## 步骤 3: 连接到 PostgreSQL 数据库

容器运行后，您可以使用任何 PostgreSQL 客户端工具连接到数据库。docker pull registry.cn-hangzhou.aliyuncs.com/library/ubuntu:latest

### 使用 `psql` 命令行工具连接

您可以使用 `docker exec` 命令进入正在运行的容器，并使用 `psql` 连接到数据库。

```bash
docker exec -it postgres psql -U postgres
```

命令详解：
- `docker exec -it my-postgres`: 在名为 `my-postgres` 的容器中以交互模式执行一个命令。
- `psql -U postgres`: 使用 `psql` 客户端，以 `postgres` 用户身份登录。`postgres` 是默认的超级用户名。

成功连接后，您将看到 `psql` 的提示符，可以在此执行 SQL 命令。

### 使用图形化客户端工具连接

您也可以使用图形化的数据库管理工具（如 DBeaver, pgAdmin, DataGrip 等）连接。连接信息如下：

- **Host**: `localhost`
- **Port**: `5432`
- **Database**: `postgres` (默认数据库)
- **Username**: `postgres`
- **Password**: 您在步骤 2 中设置的密码 (例如 `mysecretpassword`)

## 管理容器

- **查看正在运行的容器**:
  ```bash
  docker ps
  ```
- **停止容器**:
  ```bash
  docker stop my-postgres
  ```
- **启动容器**:
  ```bash
  docker start my-postgres
  ```
- **查看容器日志**:
  ```bash
  docker logs my-postgres
  ```

现在您已经成功在 WSL 的 Docker 环境中部署了 PostgreSQL。
