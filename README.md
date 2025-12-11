# RustDesk 服务端 Docker Compose 快速部署

## 项目介绍
这是一个使用 Docker Compose 快速部署 RustDesk 服务端的项目。RustDesk 是一款开源的远程桌面软件，可以让你安全地远程访问和控制其他设备。

## 部署步骤

### 1. 安装 Docker 和 Docker Compose
请确保你的系统已经安装了 Docker 和 Docker Compose。如果没有安装，可以参考以下链接进行安装：
- Docker 官方文档：https://docs.docker.com/get-docker/
- Docker Compose 官方文档：https://docs.docker.com/compose/install/

### 2. 下载项目文件
将本项目的 `docker-compose.yaml` 文件下载到你的服务器上。

### 3. 启动服务
在项目目录下执行以下命令启动服务：
```bash
docker-compose up -d
```

### 4. 验证服务
服务启动后，可以通过以下命令查看服务状态：
```bash
docker-compose ps
```

如果看到 `hbbs` 和 `hbbr` 两个容器状态为 `Up`，则表示服务启动成功。

## 端口说明

确保在防火墙中打开这些端口：

### hbbs:
- `21114` (TCP): 用于网页控制台，仅在 Pro 版本中可用。
- `21115` (TCP): 用于 NAT 类型测试。
- `21116` (TCP/UDP): 请注意 21116 应该同时为 TCP 和 UDP 启用。21116/UDP 用于 ID 注册和心跳服务。21116/TCP 用于 TCP 打洞和连接服务。
- `21118` (TCP): 用于支持网页客户端。

### hbbr:
- `21117` (TCP): 用于中继服务。
- `21119` (TCP): 用于支持网页客户端。

如果你不需要网页客户端支持，可以禁用相应的端口 21118、21119。

请确保你的防火墙已经开放了这些端口。

## 配置说明

### 修改服务端地址
如果你需要修改服务端地址，可以编辑 `docker-compose.yaml` 文件中的 `command` 参数：
```yaml
command: hbbs -r your-server-ip:21117
```

将 `your-server-ip` 替换为你的服务器 IP 地址。

### 数据持久化
项目中使用了 `./data` 目录来持久化 RustDesk 服务端的数据。如果你需要修改数据存储目录，可以编辑 `docker-compose.yaml` 文件中的 `volumes` 参数：
```yaml
volumes:
  - ./data:/root
```

将 `./data` 替换为你想要的目录路径。

## 使用客户端

### 1. 下载客户端
你可以从 RustDesk 官方网站下载客户端：https://rustdesk.com/

### 2. 配置客户端
打开 RustDesk 客户端，点击 "设置" 按钮，然后选择 "网络" 选项卡。在 "ID/中继服务器" 部分，输入你的服务端 IP 地址和端口号：
- ID 服务器：`your-server-ip:21116`
- 中继服务器：`your-server-ip:21117`

### 3. 连接设备
配置完成后，你就可以使用 RustDesk 客户端连接其他设备了。

## 常见问题

### 客户端无法连接到服务端
- 请确保你的服务器防火墙已经开放了所需的端口。
- 请确保你的服务端已经启动。
- 请确保你的客户端配置的服务端地址正确。

### 服务端启动失败
- 请检查你的 Docker 和 Docker Compose 是否安装正确。
- 请检查你的 `docker-compose.yaml` 文件是否配置正确。
- 请查看服务端日志，了解具体的错误信息：
```bash
docker-compose logs hbbs
docker-compose logs hbbr
```

