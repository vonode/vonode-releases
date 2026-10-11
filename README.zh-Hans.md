<p align="right"><a href="README.md">English</a> · <b>简体中文</b> · <a href="README.zh-Hant.md">繁體中文</a></p>

<p align="center">
  <img src="assets/logo.png" width="112" height="112" alt="Vonode 标志">
</p>

<h1 align="center">Vonode 节点</h1>

<p align="center">
  <b>SIM 卡留在家里，号码随身带走。</b><br>
  Vonode 节点软件的发布下载、安装指南和 Wiki。
</p>

<p align="center">
  <a href="https://github.com/vonode/vonode-releases/releases"><img alt="下载" src="https://img.shields.io/github/v/release/vonode/vonode-releases?style=flat-square&label=release&color=2a9468"></a>
  <a href="https://vonode.cc/zh-hans/install/"><img alt="安装指南" src="https://img.shields.io/badge/docs-install%20guide-2475c2?style=flat-square"></a>
  <img alt="平台" src="https://img.shields.io/badge/node-Linux%20amd64-141b26?style=flat-square">
  <img alt="App" src="https://img.shields.io/badge/app-iOS%20%26%20iPadOS%2017%2B-141b26?style=flat-square">
</p>

Vonode 把装了蜂窝模组的 Linux 电脑变成你自己的私人节点。用 iPhone 和 iPad 上的 Vonode App
收发短信、拨打和接听电话、切换 eSIM 配置文件、开启 Wi-Fi 通话，全部通过你自己运行的硬件完成。

本仓库**只存放发布文件和文档**，不包含源代码。节点软件是 VONODE LLC 的专有软件。

## 目录

- [功能](#功能)
- [硬件和系统要求](#硬件和系统要求)
- [下载与校验](#下载与校验)
- [用 systemd 安装包安装（推荐）](#用-systemd-安装包安装推荐)
- [用 Docker Compose 安装](#用-docker-compose-安装)
- [配对 App](#配对-app)
- [免费方案或订阅](#免费方案或订阅)
- [升级](#升级)
- [卸载](#卸载)
- [故障排查](#故障排查)
- [GPL 与 LGPL 组件的源代码](#gpl-与-lgpl-组件的源代码)
- [支持与法律信息](#支持与法律信息)

## 功能

| 功能 | 说明 |
|---|---|
| **短信** | 收发节点上所有号码的短信。垃圾短信自动归类，回复时默认使用收到这条会话的号码。 |
| **电话** | 通过你的节点拨打和接听电话。来电通过 CallKit 在 iPhone 上响铃，App 在后台时也一样。 |
| **Wi-Fi 通话** | 每张 SIM 卡都可以在蜂窝网络和 Wi-Fi 通话（VoWiFi）之间切换。节点通过互联网连接运营商，需要你的套餐支持。 |
| **SIM 与 eSIM** | 在可插拔 eUICC 卡上下载、切换、重命名和删除 eSIM 配置文件。实体 SIM 卡插上即可使用。 |
| **检查网络** | 用通俗的话检查 SIM 卡、运营商、移动数据、短信和通话服务；需要时，技术细节一点即看。 |
| **通知** | 通过 Apple 推送接收短信和来电提醒。可选的短信预览由节点加密，只在你的手机上解密。 |
| **还有更多** | 运营商代码（USSD）、自动任务、代理、备份与恢复、换绑到新手机，都在同一个 App 里。 |

你和你的号码之间没有 Vonode 云服务。App 通过加密 SSH 直接连接你的节点，并固定节点的主机密钥；
短信、通话记录和设置都保存在你运行的节点上。VONODE LLC 运营的中继负责投递 Apple 通知和确认订阅，不存储明文短信内容；详见[隐私政策](https://vonode.cc/zh-hans/privacy/)。

节点没有网页界面：装好、和 App 配对，之后的一切都在 App 里管理。

## 硬件和系统要求

购买硬件前，请先在 **[硬件页面](https://vonode.cc/zh-hans/hardware/)** 查看完整清单。

| 项目 | 要求 |
|---|---|
| **主机** | 64 位 x86（amd64）Linux：Debian 12、Ubuntu 22.04 及以上，或 Fedora；安装包方式需要 systemd，Docker 方式需要 Docker Engine 和 Compose v2。需要 root 权限；systemd 安装包还需要完整的 `iproute2` 软件包（Docker 镜像自带）。Intel N100 迷你主机、二手瘦客户机或 NUC 都合适；节点本身只需 2 GB 内存和 4 GB 磁盘。已验证平台：Ubuntu 22.04，amd64。 |
| **蜂窝模组** | 带 USB 串口（AT）和 QMI 接口、基于高通平台的 Quectel 模组。已实测：DJI Cellular 模块（内置 Quectel EG25-G，需按硬件页面的说明一次性切换 USB 标识）。EG25-G、EC25、EC20、EC21/EG21、EM05/EM06/EG06 和 RM500Q 已在代码中支持，但尚未实测。每个节点最多连接五个模组。 |
| **天线和供电** | 主天线和分集天线都要接上。连接多个模组时，请使用带供电的 USB 集线器。 |
| **SIM 卡与网络** | 已关闭 PIN 码的 SIM 卡；如需 eSIM，请使用可插拔 eUICC 卡。运营商已在套餐中开通 Wi-Fi 通话。手机能访问到 TCP 2222 端口：在路由器上做端口转发，或使用 WireGuard、Tailscale 等 VPN。 |
| **App** | iPhone 和 iPad 上的 Vonode，需要 iOS 或 iPadOS 17 及以上。App Store 免费下载。 |

本版本不支持：树莓派等 ARM（arm64）主机（计划在 1.3.1 提供）、OpenWrt 路由器、macOS 或 Windows
上的 Docker Desktop、没有 USB 直通的虚拟机，以及只提供网卡功能的 USB 上网卡（HiLink、仅 RNDIS 或仅
ECM 固件）。蜂窝网络通话目前还没有在任何模组上得到确认；通话可以通过 Wi-Fi 通话进行。

Vonode App 在中国大陆以外的 App Store 国家和地区提供；中国大陆的 App Store 目前不提供。

## 下载与校验

每个版本都发布在 **[最新版本](https://github.com/vonode/vonode-releases/releases/latest)** 页面。
把下面的 `VERSION` 设为该页面上显示的版本标签（文件名使用同一标签）。

| 文件 | 说明 |
|---|---|
| `vonode_<version>_linux_amd64_commercial.tar.gz` | 安装包：节点程序、`install.sh`、systemd 单元、`docker/` 部署文件、指南和许可证 |
| `vonode_<version>_linux_amd64_commercial.tar.gz.sha256` | 安装包的 SHA-256 校验值 |
| `docker-compose.yml` 和 `docker-compose.yml.sha256` | 本版本的 Docker Compose 文件（按 digest 固定镜像）及其校验值 |
| `vonode_<version>_linux_amd64_commercial` 和 `.sig` | 单文件形式的节点程序及其发布签名，供 App 内软件更新使用 |
| `latest-commercial.json` | 带签名的更新清单，节点据此在 App 中提供更新 |
| `vonode_<version>_linux_amd64_commercial.sources.tar` | GPL 和 LGPL 组件的对应源代码 |
| `vonode_<version>_linux_amd64_commercial.spdx.json` | 软件物料清单（SPDX SBOM） |
| `vonode_<version>_linux_amd64_commercial.THIRD_PARTY_NOTICES.txt` | 第三方组件及其许可证 |

### 校验 SHA-256

```sh
VERSION=vX.Y.Z        # 换成版本标签
BASE=https://github.com/vonode/vonode-releases/releases/download/$VERSION
curl -fLO $BASE/vonode_${VERSION}_linux_amd64_commercial.tar.gz
curl -fLO $BASE/vonode_${VERSION}_linux_amd64_commercial.tar.gz.sha256
sha256sum -c vonode_${VERSION}_linux_amd64_commercial.tar.gz.sha256
```

`sha256sum -c` 必须输出 `OK`。校验不通过的安装包请不要安装。

### 验证发布签名（可选）

节点程序用 Vonode 更新密钥（Ed25519）签名。节点在启动时、以及安装 App 内更新之前，都会自行验证这个签名。
如需手动验证，需要 OpenSSL 3.0 及以上。签名的内容是文本 `vonode-binary-v1`、一个换行符，加上程序
SHA-256 的十六进制值。

```sh
cat > vonode-update-key.pem <<'EOF'
-----BEGIN PUBLIC KEY-----
MCowBQYDK2VwAyEAnIDCyy4CJVQSC7a+p+OeQsF/NsPguVkmB+OL//k98OY=
-----END PUBLIC KEY-----
EOF

# 解压后的安装包里，程序是 vonode，签名是 vonode.sig。
# 单文件下载时，改用 vonode_<version>_linux_amd64_commercial 和对应的 .sig。
BIN=vonode_${VERSION}_linux_amd64_commercial/vonode
printf 'vonode-binary-v1\n%s' "$(sha256sum "$BIN" | cut -d' ' -f1)" > vonode.msg
base64 -d "$BIN.sig" > vonode.sig.bin
openssl pkeyutl -verify -pubin -inkey vonode-update-key.pem -rawin -in vonode.msg -sigfile vonode.sig.bin
```

最后一条命令必须输出 `Signature Verified Successfully`。

## 用 systemd 安装包安装（推荐）

本版本推荐的安装方式。安装包把节点安装为两个 systemd 服务，不需要 Docker。

### 1. 准备主机

安装 Linux 发行版，连上网络（推荐用有线网口连接路由器），用可以执行 `sudo` 的用户登录。更新系统，
并停用会抢占模组串口的 ModemManager：

```sh
sudo apt update && sudo apt full-upgrade -y        # Debian 和 Ubuntu
sudo systemctl disable --now ModemManager
```

插上模组，确认 Linux 已识别：

```sh
lsusb | grep -i -E 'quectel|2c7c|qualcomm|05c6'
ls /dev/ttyUSB* /dev/cdc-wdm*
```

每个模组应显示一行 Quectel 信息、四个 `ttyUSB` 端口和一个 `cdc-wdm` 端口。DJI Cellular 模块需要先一次性
切换 USB 标识，步骤见 [硬件页面](https://vonode.cc/zh-hans/hardware/)。

### 2. 下载、校验并解压

```sh
VERSION=vX.Y.Z        # 换成版本标签
BASE=https://github.com/vonode/vonode-releases/releases/download/$VERSION
curl -fLO $BASE/vonode_${VERSION}_linux_amd64_commercial.tar.gz
curl -fLO $BASE/vonode_${VERSION}_linux_amd64_commercial.tar.gz.sha256
sha256sum -c vonode_${VERSION}_linux_amd64_commercial.tar.gz.sha256
tar -xzf vonode_${VERSION}_linux_amd64_commercial.tar.gz
```

### 3. 运行安装程序

```sh
cd vonode_${VERSION}_linux_amd64_commercial
sudo ./install.sh
```

安装程序会：

- 把程序放到 `/opt/vonode/vonode`，配置放到 `/opt/vonode/config/config.yaml`（已有的配置和数据库会保留）；
- 创建系统用户 `vonode-gateway`（UID 和 GID 均为 10001）；
- 安装并启动两个 systemd 服务：核心服务 `vonode`，以及 App 连接用的加密 SSH 服务 `vonode-gateway`。

### 4. 首次启动和管理员密码

确认两个服务都显示 `active (running)`：

```sh
systemctl status vonode vonode-gateway
```

节点首次启动时会生成一个随机管理员密码，配置里只保存它的哈希值。明文保存在
`/opt/vonode/config/initial-admin-password`，只有 root 可读。平时很少用到，请妥善保管。

### 5. 放行端口并显示配对二维码

在主机防火墙上放行 TCP 2222 端口；如果手机要从外网连接，还需在路由器上做端口转发（或使用 VPN）：

```sh
sudo ufw allow 2222/tcp
```

用手机访问节点时使用的地址显示配对二维码：做了端口转发时填 DDNS 域名或公网 IP，否则填主机的 VPN 或
局域网地址。如果路由器转发的是其他外部端口，请把 `-port` 后面换成那个端口。

```sh
sudo /opt/vonode/vonode pair -c /opt/vonode/config/config.yaml -host node.example.com -port 2222
```

终端会以字符块图形显示二维码。它 5 分钟内有效，只能使用一次；需要新的二维码时再次运行这条命令。
接下来请看 [配对 App](#配对-app)。

### 日常运维

| 操作 | 命令 |
|---|---|
| 查看状态 | `systemctl status vonode vonode-gateway` |
| 查看日志 | `journalctl -u vonode -f` |
| 重启 | `sudo systemctl restart vonode`（最多需要两分钟：节点会先注销 Wi-Fi 通话并关闭隧道） |
| 新的配对码 | `sudo /opt/vonode/vonode pair -c /opt/vonode/config/config.yaml -host <地址> -port 2222` |
| 忘记管理员密码 | `sudo systemctl stop vonode`，然后 `sudo /opt/vonode/vonode reset-password -c /opt/vonode/config/config.yaml`，再 `sudo systemctl start vonode`。所有手机都会退出登录，需要重新配对。 |

## 用 Docker Compose 安装

Docker Compose 方式用 Docker Hub 上的镜像 [`vonode/vonode`](https://hub.docker.com/r/vonode/vonode)
运行两个容器，节点功能与 systemd 方式相同：`vonode`（核心，负责驱动模组）和 `vonode-gateway`（无特权的
SSH 服务，App 通过 TCP 2222 连接它）。配对、方案和 App 的用法与 systemd 安装包完全一样。镜像标签与版本对应
（`v1.3.0` 对应 `1.3.0`，另有 `latest`），适用于 64 位 x86（amd64）主机。每个版本还会发布一个按 digest
固定镜像的 `docker-compose.yml`。

一台主机只用一种安装方式：不要在已安装 systemd 安装包的主机上再运行 Docker 方式。

### 1. 准备主机

Linux、模组和 SIM 卡的要求与上文相同。macOS 或 Windows 上的 Docker Desktop 无法访问 USB 模组，请在 Linux
上使用 Docker Engine。停用 ModemManager，确认 Linux 能识别模组，然后安装带 Compose 插件的 Docker Engine。
Docker 官方的便捷脚本适用于 Debian、Ubuntu 和 Fedora；最后一条命令必须输出 Compose v2 的版本号。

```sh
sudo systemctl disable --now ModemManager
lsusb | grep -i -E 'quectel|2c7c|qualcomm|05c6'
ls /dev/ttyUSB* /dev/cdc-wdm*
curl -fsSL https://get.docker.com | sh
sudo systemctl enable --now docker
docker compose version
```

模组的串口和 QMI 接口由主机内核的 `option` 和 `qmi_wwan` 驱动提供，插入模组时会自动加载。如果 `lsusb`
能看到模组但没有 `/dev/ttyUSB*`，运行 `sudo modprobe -a option qmi_wwan`，然后重新插拔模组。

**设备访问。** `vonode` 容器使用主机网络、以特权模式运行，并能看到主机的整个 `/dev`：它要驱动 USB 串口和
QMI 接口、为 Wi-Fi 通话创建 TUN/XFRM 接口，并用 ALSA 处理通话音频。绑定整个 `/dev` 后，之后插入的模组、
或复位后以其他 `/dev/ttyUSB*` 编号重新出现的模组，无需重启即可识别。`vonode-gateway` 容器以 UID 10001
运行，文件系统只读、不带任何 capability，只对外发布 TCP 2222。

### 2. 下载 Compose 文件

```sh
mkdir -p ~/vonode && cd ~/vonode
VERSION=vX.Y.Z        # 换成版本标签
BASE=https://github.com/vonode/vonode-releases/releases/download/$VERSION
curl -fLO $BASE/docker-compose.yml
curl -fLO $BASE/docker-compose.yml.sha256
sha256sum -c docker-compose.yml.sha256
```

可选设置写在同一目录的 `.env` 文件里，例如用 `TZ=Asia/Hong_Kong` 设置节点时区（默认 UTC），或用
`VONODE_SSH_PORT=2022` 把 SSH 服务发布到主机的其他端口。

### 3. 启动节点

```sh
sudo docker compose up -d
sudo docker compose ps
```

Docker 会拉取固定的镜像并启动两个容器；大约半分钟后两个容器都显示 `healthy`。节点把配置、数据库和日志
保存在 `docker-compose.yml` 旁边的 `config/`、`data/` 和 `logs/` 中。首次启动时会生成随机管理员密码，
明文保存在 `config/initial-admin-password`，只有 root 可读。

### 4. 开放端口并显示配对二维码

在主机防火墙上放行 TCP 2222；如果手机要从外部网络连接，请在路由器上转发该端口（或使用 VPN）。然后用手机
访问节点时使用的地址和端口显示配对二维码：

```sh
sudo ufw allow 2222/tcp
sudo docker compose exec vonode /app/vonode pair -c /app/config/config.yaml -host node.example.com -port 2222
```

二维码 5 分钟内有效，只能使用一次。接下来请看 [配对 App](#配对-app)。

### Docker 方式的日常运维

以下命令在 `docker-compose.yml` 所在目录下运行。

| 操作 | 命令 |
|---|---|
| 查看状态（两个服务都应为 healthy） | `sudo docker compose ps` |
| 查看日志 | `sudo docker compose logs -f vonode` |
| 新的配对码 | `sudo docker compose exec vonode /app/vonode pair -c /app/config/config.yaml -host <地址> -port 2222` |
| 重启（最多两分钟） | `sudo docker compose restart vonode` |
| 停止并保留数据 | `sudo docker compose down` |
| 重新启动 | `sudo docker compose up -d` |
| 忘记管理员密码 | `sudo docker compose stop vonode`、`sudo docker compose run --rm --no-deps vonode reset-password -c /app/config/config.yaml`、`sudo docker compose start vonode` |

> [!WARNING]
> 除非要删除节点，否则不要运行 `docker compose down -v`。`vonode_gateway-private` 卷保存着已配对手机所固定的
> SSH 主机密钥；它丢失后，所有手机都要重新配对。

### 另一种方式：安装脚本

安装包里还有一个 `docker/` 目录，其中的 `setup.sh` 会检查主机、列出找到的模组、写入 `.env` 和设备覆盖文件、
启动两个容器并显示配对二维码。按 [下载与校验](#下载与校验) 下载并解压安装包，然后运行：

```sh
cd vonode_${VERSION}_linux_amd64_commercial/docker
sudo ./setup.sh --host node.example.com --image vonode/vonode@sha256:<digest>
```

发布说明里给出了 `vonode/vonode@sha256:<digest>` 形式的镜像。`--devices` 只映射当前存在的模组设备节点，
而不是整个 `/dev`；`./setup.sh --help` 会说明每个选项。使用这种模式时，添加或移动模组后请重新运行
`sudo ./setup.sh`；`sudo ./setup.sh --uninstall` 会停止节点并保留数据。

## 配对 App

1. 从 App Store 安装 **Vonode**（免费下载）并打开。
2. 在“连接你的节点”页面点按扫描框，把摄像头对准终端上的二维码。
3. App 通过 SSH 连接节点，固定节点的主机密钥，并在首页显示这个节点和它的号码。

App 和节点必须能通过二维码里的地址互相访问。如果 App 提示连接错误，请先检查端口转发或 VPN，再生成新的
配对码。之后要配对另一部手机，请在 App 中使用“换绑到新手机”，它会显示一次性二维码。

## 免费方案或订阅

未订阅时，节点以**免费方案**运行：打开“号码”标签页，选择一个你想查看短信的号码。选定后在这个节点上不能
更换，恢复备份也会保留它。之后“短信”标签页会显示这个号码收到的短信。

其他功能（电话、Wi-Fi 通话、发短信、通知、eSIM 配置文件、代理、自动任务、更多号码）都需要在 App 内购买
订阅：打开“更多”，点“查看方案”。一份订阅解锁一个节点。节点几秒内就会解锁并以完整模式重启；之后用
“添加模组”加入模组，并为每张 SIM 卡开启 Wi-Fi 通话。

硬件、SIM 套餐、运营商费用和托管不包含在内。

## 升级

- **在 App 中：** 用 systemd 安装包安装的节点会在“更多 > 软件更新”中提供带签名的更新。节点在安装前会验证更新的签名。
- **systemd 安装包：** 下载并校验新的安装包，解压后在其顶层目录运行 `sudo ./install.sh`。配置、数据库和
  SSH 主机密钥都会保留。
- **Docker Compose：** 把新版本的 `docker-compose.yml` 和 `docker-compose.yml.sha256` 下载到同一目录并校验，
  然后运行 `sudo docker compose pull` 和 `sudo docker compose up -d`。配置、数据库和 SSH 主机密钥都会保留，
  两个容器始终使用同一个镜像。App 内更新不会替换容器里的程序，请改为更新镜像。使用安装脚本时，运行
  `sudo ./setup.sh --image vonode/vonode@sha256:<digest>` 并传入新的 digest。

升级前请先备份：App 中的“更多 > 备份记录”。备份包含短信、通话记录和节点的订阅绑定，请妥善保管。迁移到
新硬件时，在旧节点上创建并下载备份，安装并配对新节点，再导入并恢复备份。不要让旧节点继续用同一份数据运行。

## 卸载

systemd 安装包（会删除你的数据和备份）：

```sh
sudo systemctl disable --now vonode-gateway vonode
sudo rm /etc/systemd/system/vonode.service /etc/systemd/system/vonode-gateway.service /etc/tmpfiles.d/vonode.conf
sudo systemctl daemon-reload
sudo rm -rf /opt/vonode /var/lib/vonode-gateway
```

Docker Compose（在 `docker-compose.yml` 所在目录）：`sudo docker compose down` 停止节点并保留全部数据。要删除
节点及其数据，运行 `sudo docker compose down -v`，再运行 `sudo rm -rf config data logs`。使用安装脚本时，
`sudo ./setup.sh --uninstall` 保留数据，`--uninstall --purge` 会删除数据。

## 故障排查

| 问题 | 处理方法 |
|---|---|
| ModemManager 正在运行 | `sudo systemctl disable --now ModemManager`，然后 `sudo systemctl restart vonode`（Docker 方式改为运行 `sudo docker compose restart vonode`）。 |
| 检测不到模组 | 如果 `lsusb` 里没有 Quectel（`2c7c`）设备，请换一根线或换一个接口，使用带供电的集线器，并用 `sudo dmesg \| tail -n 30` 查看 USB 供电错误。如果 `lsusb` 能看到模组但没有 `/dev/ttyUSB*`，运行 `sudo modprobe -a option qmi_wwan`，然后重新插拔模组。 |
| App 连不上节点 | 在你的网络之外的设备上运行 `nc -vz node.example.com 2222`。没有响应说明路由器转发或防火墙设置有误，或者宽带运营商使用了运营商级 NAT；这时请使用 VPN，并用主机的 VPN 地址配对。 |
| 配对码已过期或已被使用 | 配对码 5 分钟内有效，只能使用一次。再次运行 `pair` 命令即可。 |
| Wi-Fi 通话无法注册 | 套餐需要开通 Wi-Fi 通话，模组必须用这张 SIM 卡至少注册过一次蜂窝网络，主机需要能通过 IPv6 或 IPv4 访问运营商。App 的诊断页面会显示当前所处阶段。 |

更多内容见 [Wiki](https://github.com/vonode/vonode-releases/wiki)（英文）和
[分步指南](https://vonode.cc/zh-hans/install/guide/)。

## GPL 与 LGPL 组件的源代码

节点软件是专有软件，但其中包含 strongSwan（GPL-2.0-or-later）以及静态链接的 glibc 和 GMP（LGPL）。每个版本：

- 这些组件完整的对应源代码（含构建脚本）以发布资产 `vonode_<version>_linux_amd64_commercial.sources.tar` 提供；
- 安装包中的 `SOURCES.md` 列出这些组件并包含书面要约：源代码在该版本发布后至少提供三年；如需实体介质
  副本，可发邮件至 support@vonode.cc 索取，收费不超过介质和邮寄成本；
- SPDX SBOM 和 `THIRD_PARTY_NOTICES.txt` 列出全部第三方组件及其许可证。

## 支持与法律信息

- 官网：<https://vonode.cc/zh-hans/>
- 安装指南：<https://vonode.cc/zh-hans/install/> · [分步指南](https://vonode.cc/zh-hans/install/guide/)
- 支持的硬件：<https://vonode.cc/zh-hans/hardware/>
- 支持：[support@vonode.cc](mailto:support@vonode.cc) · <https://vonode.cc/zh-hans/support/>
- [隐私政策](https://vonode.cc/zh-hans/privacy/) · [使用条款](https://vonode.cc/zh-hans/terms/) ·
  [第三方声明](https://vonode.cc/zh-hans/third-party-notices/)

Vonode 不是紧急通信服务。它依赖你的硬件、运营商、网络和 Apple 服务；请勿依靠它进行紧急呼救或其他攸关
生命安全的通信。请只在你有权使用的 SIM 卡、号码和网络上使用，并遵守运营商的条款。

© 2026 VONODE LLC。Vonode 是 VONODE LLC 的产品。
