# 在 Ubuntu 上安装 Docker Engine

这是在 Ubuntu 上安装 Docker Engine 的详细步骤。

## 卸载旧版本

首先，卸载所有可能冲突的旧版本软件包：

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

## 设置 Docker 的 APT 软件源

1.  更新 `apt` 包索引并安装必要的软件包，以允许 `apt` 通过 HTTPS 使用软件源：

    ```bash
    sudo apt-get update
    sudo apt-get install \
        ca-certificates \
        curl \
        gnupg \
        lsb-release
    ```

2.  添加 Docker 的官方 GPG 密钥：

    ```bash
    sudo install -m 0755 -d /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    sudo chmod a+r /etc/apt/keyrings/docker.gpg
    ```

3.  设置 Docker 的稳定版软件源：

    ```bash
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```

## 安装 Docker Engine

1.  再次更新 `apt` 包索引：

    ```bash
    sudo apt-get update
    ```

2.  安装最新版本的 Docker Engine, containerd, 和 Docker Compose:

    ```bash
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```

## 验证安装

通过运行 `hello-world` 镜像来验证 Docker Engine 是否安装成功：

```bash
sudo docker run hello-world
```

这个命令会下载一个测试镜像并在容器中运行它。如果一切正常，它会打印一条信息。

## (推荐) 配置国内镜像加速器

由于 Docker Hub 服务器在国外，直接拉取镜像可能会很慢。配置国内的镜像加速器可以显著提升下载速度。

下面提供了3个目前（2025-07-19）实测可用的国内 Docker 镜像加速器地址，任选其一或全部组合都可以。直接执行以下命令即可完成配置：

```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": [
    "https://docker.xuanyuan.me",
    "https://docker.1ms.run",
    "https://hub-mirror.c.163.com"
  ]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

- **docker.xuanyuan.me**：知乎、腾讯云等多方推荐，稳定且带宽足。
- **docker.1ms.run**：毫秒镜像，免费、带搜索页面，实测可跑满家用带宽。
- **hub-mirror.c.163.com**：网易 163 老牌镜像，长期维护，适合备用。

配置完成后，可以再次运行 `docker pull hello-world` 来测试加速器是否生效。如果能秒下，即代表配置成功。

## (可选) 作为非 root 用户管理 Docker

默认情况下，`docker` 命令需要 `sudo`。如果你想避免每次都输入 `sudo`，可以将你的用户添加到 `docker` 用户组。

1.  创建 `docker` 用户组（如果它还不存在）：

    ```bash
    sudo groupadd docker
    ```

2.  将你的用户添加到 `docker` 组：

    ```bash
    sudo usermod -aG docker $USER
    ```

3.  **注销并重新登录**，这样你的组成员身份才会生效。

之后，你就可以直接使用 `docker` 命令，无需 `sudo`。
