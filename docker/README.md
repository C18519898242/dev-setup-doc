# 如何在 Windows 11 上安装 WSL 和 Docker

本指南将引导您完成在 Windows 11 上安装适用于 Linux 的 Windows 子系统 (WSL) 和 Docker 的过程。通过结合使用 WSL 和 Docker，您可以在 Windows 上获得一个强大、高效的开发环境。

## 安装步骤

按照以下步骤安装 WSL：

### 1. 以管理员身份打开 PowerShell

您需要管理员权限才能启用 WSL 所需的 Windows 功能。

- 点击 **开始** 按钮或按 `Win` 键。
- 输入 `PowerShell`。
- 在搜索结果中，右键点击 **Windows PowerShell**。
- 从上下文菜单中选择 **以管理员身份运行**。
- 如果用户帐户控制 (UAC) 提示，请单击 **是**。

### 2. 运行安装命令

在打开的管理员 PowerShell 窗口中，复制并粘贴以下命令，然后按 **Enter** 键：

```powershell
wsl --install
```

此命令将自动执行以下操作：
- 启用所需的 WSL 和虚拟机平台可选组件。
- 下载并安装最新的 Linux 内核。
- 将 WSL 2 设置为默认值。
- 下载并安装 **Ubuntu** Linux 发行版（除非使用 `-d` 参数指定了其他发行版）。

### 3. 重新启动计算机

命令成功执行后，您需要重新启动计算机以完成 WSL 的安装和更新。

### 4. 初始化您的 Linux 发行版

重新启动后，您新安装的 Linux 发行版（默认为 Ubuntu）将自动启动。您需要为新的 Linux 环境创建一个用户帐户和密码。

- **输入用户名**：此用户名不必与您的 Windows 用户名相同。
- **输入密码**：输入密码时，您不会在屏幕上看到任何字符。这是正常的安全功能。
- **确认密码**：再次输入密码进行确认。

完成这些步骤后，您的 Linux 环境就准备就绪了！您可以通过在“开始”菜单中找到您的发行版（例如 Ubuntu）来启动它。

##如何在 WSL (Ubuntu) 中安装 Docker

安装 WSL 和 Ubuntu 后，您可以按照以下步骤在其中安装 Docker：

### 1. 更新您的包列表

打开您的 Ubuntu 终端并运行以下命令来更新您的包列表：

```bash
sudo apt-get update
```

### 2. 安装必要的包

安装一些允许 `apt` 通过 HTTPS 使用仓库的包：

```bash
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release
```

### 3. 添加 Docker 的 GPG 密钥

接下来，添加 Docker 的官方 GPG 密钥：

```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

### 4. 设置 Docker 仓库

现在，将 Docker 仓库添加到您的 APT 源中：

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 5. 再次更新包列表

使用新的 Docker 仓库再次更新您的包列表：

```bash
sudo apt-get update
```

### 6. 安装 Docker 引擎

运行以下命令安装最新版本的 Docker 引擎、CLI、containerd 和 Docker Compose：

```bash
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 7. 将您的用户添加到 `docker` 组

为了能够在没有 `sudo` 的情况下运行 `docker` 命令，请将您的用户添加到 `docker` 组：

```bash
sudo usermod -aG docker $USER
```

**重要提示：** 您需要关闭并重新打开您的 WSL 终端才能使此更改生效。您可以通过在 PowerShell 或 CMD 中运行 `wsl --shutdown` 然后再次启动您的发行版来完成此操作。

### 8. 验证 Docker 安装

重新启动终端后，通过运行 `hello-world` 镜像来验证 Docker 是否已正确安装：

```bash
docker run hello-world
```

如果一切设置正确，此命令将下载一个测试镜像并在容器中运行它。
