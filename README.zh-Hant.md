<p align="right"><a href="README.md">English</a> · <a href="README.zh-Hans.md">简体中文</a> · <b>繁體中文</b></p>

<p align="center">
  <img src="assets/logo.png" width="112" height="112" alt="Vonode 標誌">
</p>

<h1 align="center">Vonode 節點</h1>

<p align="center">
  <b>SIM 卡留在家裡，號碼隨身帶著走。</b><br>
  Vonode 節點軟體的發行下載、安裝指南和 Wiki。
</p>

<p align="center">
  <a href="https://github.com/vonode/vonode-releases/releases/latest"><img alt="下載" src="https://img.shields.io/badge/download-latest%20release-2a9468?style=flat-square"></a>
  <a href="https://vonode.cc/zh-hant/install/"><img alt="安裝指南" src="https://img.shields.io/badge/docs-install%20guide-2475c2?style=flat-square"></a>
  <img alt="平台" src="https://img.shields.io/badge/node-Linux%20amd64-141b26?style=flat-square">
  <img alt="App" src="https://img.shields.io/badge/app-iOS%20%26%20iPadOS%2017%2B-141b26?style=flat-square">
</p>

Vonode 將裝有行動網路模組的 Linux 電腦變成你自己的私人節點。用 iPhone 和 iPad 上的 Vonode App
收發簡訊、撥打和接聽電話、切換 eSIM 設定檔、開啟 Wi-Fi 通話，全部透過你自己運作的硬體完成。

本儲存庫**只存放發行檔案和文件**，不包含原始碼。節點軟體是 VONODE LLC 的專有軟體。

## 目錄

- [功能](#功能)
- [硬體和系統需求](#硬體和系統需求)
- [下載與驗證](#下載與驗證)
- [用 systemd 安裝套件安裝（建議）](#用-systemd-安裝套件安裝建議)
- [用 Docker Compose 安裝（即將推出）](#用-docker-compose-安裝即將推出)
- [配對 App](#配對-app)
- [免費方案或訂閱](#免費方案或訂閱)
- [升級](#升級)
- [解除安裝](#解除安裝)
- [疑難排解](#疑難排解)
- [GPL 與 LGPL 元件的原始碼](#gpl-與-lgpl-元件的原始碼)
- [支援與法律資訊](#支援與法律資訊)

## 功能

| | |
|---|---|
| **簡訊** | 收發節點上所有號碼的簡訊。垃圾簡訊自動分類，回覆時預設使用收到這則對話的號碼。 |
| **電話** | 透過你的節點撥打和接聽電話。來電透過 CallKit 在 iPhone 上響鈴，App 在背景執行時也一樣。 |
| **Wi-Fi 通話** | 每張 SIM 卡都可以在行動網路和 Wi-Fi 通話（VoWiFi）之間切換。節點透過網際網路連線電信業者，需要你的資費方案支援。 |
| **SIM 與 eSIM** | 在可插拔 eUICC 卡上下載、切換、重新命名和刪除 eSIM 設定檔。實體 SIM 卡插上即可使用。 |
| **檢查網路** | 用淺白的文字檢查 SIM 卡、電信業者、行動資料、簡訊和通話服務；需要時，技術細節點一下就能看到。 |
| **通知** | 透過 Apple 推播接收簡訊和來電提醒。可選的簡訊預覽由節點加密，只在你的手機上解密。 |
| **還有更多** | 電信業者代碼（USSD）、自動任務、代理、備份與還原、換綁到新手機，都在同一個 App 裡。 |

你和你的號碼之間沒有 Vonode 雲端服務。App 透過加密 SSH 直接連線你的節點，並固定節點的主機金鑰；
VONODE LLC 營運的中繼伺服器只負責傳送 Apple 通知。簡訊、通話紀錄和設定都儲存在你運作的節點上。

節點沒有網頁介面：安裝好、和 App 配對，之後的一切都在 App 裡管理。

## 硬體和系統需求

購買硬體前，請先在 **[硬體頁面](https://vonode.cc/zh-hant/hardware/)** 查看完整清單。

| | |
|---|---|
| **主機** | 具備 systemd 的 64 位元 x86（amd64）Linux：Debian 12、Ubuntu 22.04 以上版本，或 Fedora。需要 root 權限和完整的 `iproute2` 套件。Intel N100 迷你電腦、二手精簡型電腦或 NUC 都適合；節點本身只需 2 GB 記憶體和 4 GB 磁碟空間。已驗證平台：Ubuntu 22.04，amd64。 |
| **行動網路模組** | 具備 USB 序列埠（AT）和 QMI 介面、採用高通平台的 Quectel 模組。已實測：DJI Cellular 模組（內建 Quectel EG25-G，需依硬體頁面的說明一次性切換 USB 識別碼）。EG25-G、EC25、EC20、EC21/EG21、EM05/EM06/EG06 和 RM500Q 已在程式碼中支援，但尚未實測。每個節點最多連接五個模組。 |
| **天線和供電** | 主天線和分集天線都要接上。連接多個模組時，請使用有外接電源的 USB 集線器。 |
| **SIM 卡與網路** | 已關閉 PIN 碼的 SIM 卡；如需 eSIM，請使用可插拔 eUICC 卡。電信業者已在資費方案中開通 Wi-Fi 通話。手機能連到 TCP 2222 連接埠：在路由器上設定連接埠轉送，或使用 WireGuard、Tailscale 等 VPN。 |
| **App** | iPhone 和 iPad 上的 Vonode，需要 iOS 或 iPadOS 17 以上版本。App Store 免費下載。 |

本版本不支援：Raspberry Pi 等 ARM（arm64）主機（預計在 1.3.1 提供）、OpenWrt 路由器、macOS 或 Windows
上的 Docker Desktop、沒有 USB 直通的虛擬機器，以及只提供網路卡功能的 USB 網卡（HiLink、僅 RNDIS 或僅
ECM 韌體）。行動網路通話目前還沒有在任何模組上得到確認；通話可以透過 Wi-Fi 通話進行。

Vonode App 在中國大陸以外的 App Store 國家和地區提供，台灣、香港和澳門的 App Store 均可下載；中國大陸的
App Store 目前不提供。

## 下載與驗證

每個版本都發布在 **[最新版本](https://github.com/vonode/vonode-releases/releases/latest)** 頁面。
請把下面的 `<version>` 換成版本標籤，例如該頁面上顯示的標籤。

| 檔案 | 說明 |
|---|---|
| `vonode_<version>_linux_amd64_commercial.tar.gz` | 安裝套件：節點程式、`install.sh`、systemd 單元、`docker/` 部署檔案、指南和授權條款 |
| `vonode_<version>_linux_amd64_commercial.tar.gz.sha256` | 安裝套件的 SHA-256 檢查碼 |
| `vonode_<version>_linux_amd64_commercial` 和 `.sig` | 單一檔案形式的節點程式及其發行簽章，供 App 內軟體更新使用 |
| `latest-commercial.json` | 已簽章的更新清單，節點據此在 App 中提供更新 |
| `vonode_<version>_linux_amd64_commercial.sources.tar` | GPL 和 LGPL 元件的對應原始碼 |
| `vonode_<version>_linux_amd64_commercial.spdx.json` | 軟體物料清單（SPDX SBOM） |
| `vonode_<version>_linux_amd64_commercial.THIRD_PARTY_NOTICES.txt` | 第三方元件及其授權條款 |

### 核對 SHA-256

```sh
VERSION=<version>
BASE=https://github.com/vonode/vonode-releases/releases/download/$VERSION
curl -fLO $BASE/vonode_${VERSION}_linux_amd64_commercial.tar.gz
curl -fLO $BASE/vonode_${VERSION}_linux_amd64_commercial.tar.gz.sha256
sha256sum -c vonode_${VERSION}_linux_amd64_commercial.tar.gz.sha256
```

`sha256sum -c` 必須輸出 `OK`。驗證不通過的安裝套件請不要安裝。

### 驗證發行簽章（選用）

節點程式以 Vonode 更新金鑰（Ed25519）簽章。節點在啟動時、以及安裝 App 內更新之前，都會自行驗證這個簽章。
如需手動驗證，需要 OpenSSL 3.0 以上版本。簽章的內容是文字 `vonode-binary-v1`、一個換行字元，加上程式
SHA-256 的十六進位值。

```sh
cat > vonode-update-key.pem <<'EOF'
-----BEGIN PUBLIC KEY-----
MCowBQYDK2VwAyEAnIDCyy4CJVQSC7a+p+OeQsF/NsPguVkmB+OL//k98OY=
-----END PUBLIC KEY-----
EOF

# 解壓縮後的安裝套件裡，程式是 vonode，簽章是 vonode.sig。
# 單一檔案下載時，改用 vonode_<version>_linux_amd64_commercial 和對應的 .sig。
BIN=vonode_${VERSION}_linux_amd64_commercial/vonode
printf 'vonode-binary-v1\n%s' "$(sha256sum "$BIN" | cut -d' ' -f1)" > vonode.msg
base64 -d "$BIN.sig" > vonode.sig.bin
openssl pkeyutl -verify -pubin -inkey vonode-update-key.pem -rawin -in vonode.msg -sigfile vonode.sig.bin
```

最後一道指令必須輸出 `Signature Verified Successfully`。

## 用 systemd 安裝套件安裝（建議）

本版本建議的安裝方式。安裝套件把節點安裝為兩個 systemd 服務，不需要 Docker。

### 1. 準備主機

安裝 Linux 發行版，連上網路（建議用有線網路連接路由器），以可以執行 `sudo` 的使用者登入。更新系統，
並停用會搶占模組序列埠的 ModemManager：

```sh
sudo apt update && sudo apt full-upgrade -y        # Debian 和 Ubuntu
sudo systemctl disable --now ModemManager
```

插上模組，確認 Linux 已辨識：

```sh
lsusb | grep -i -E 'quectel|2c7c|qualcomm|05c6'
ls /dev/ttyUSB* /dev/cdc-wdm*
```

每個模組應顯示一行 Quectel 資訊、四個 `ttyUSB` 連接埠和一個 `cdc-wdm` 連接埠。DJI Cellular 模組需要先
一次性切換 USB 識別碼，步驟見 [硬體頁面](https://vonode.cc/zh-hant/hardware/)。

### 2. 下載、驗證並解壓縮

```sh
VERSION=<version>
BASE=https://github.com/vonode/vonode-releases/releases/download/$VERSION
curl -fLO $BASE/vonode_${VERSION}_linux_amd64_commercial.tar.gz
curl -fLO $BASE/vonode_${VERSION}_linux_amd64_commercial.tar.gz.sha256
sha256sum -c vonode_${VERSION}_linux_amd64_commercial.tar.gz.sha256
tar -xzf vonode_${VERSION}_linux_amd64_commercial.tar.gz
```

### 3. 執行安裝程式

```sh
cd vonode_${VERSION}_linux_amd64_commercial
sudo ./install.sh
```

安裝程式會：

- 把程式放到 `/opt/vonode/vonode`，設定檔放到 `/opt/vonode/config/config.yaml`（既有的設定和資料庫會保留）；
- 建立系統使用者 `vonode-gateway`（UID 和 GID 皆為 10001）；
- 安裝並啟動兩個 systemd 服務：核心服務 `vonode`，以及 App 連線用的加密 SSH 服務 `vonode-gateway`。

### 4. 首次啟動和管理員密碼

確認兩個服務都顯示 `active (running)`：

```sh
systemctl status vonode vonode-gateway
```

節點首次啟動時會產生一組隨機管理員密碼，設定中只保存它的雜湊值。明文存放在
`/opt/vonode/config/initial-admin-password`，只有 root 可以讀取。平時很少用到，請妥善保管。

### 5. 開放連接埠並顯示配對 QR Code

在主機防火牆上允許 TCP 2222 連接埠；如果手機要從外部網路連線，還需在路由器上設定連接埠轉送（或使用 VPN）：

```sh
sudo ufw allow 2222/tcp
```

用手機連到節點時使用的位址顯示配對 QR Code：有設定連接埠轉送時填 DDNS 網域名稱或公用 IP，否則填主機的
VPN 或區域網路位址。如果轉送的是其他連接埠，請加上 `-port <連接埠>`。

```sh
sudo /opt/vonode/vonode pair -c /opt/vonode/config/config.yaml -host node.example.com
```

終端機會以字元區塊圖形顯示 QR Code。它 5 分鐘內有效，只能使用一次；需要新的 QR Code 時再執行一次這道指令。
接下來請看 [配對 App](#配對-app)。

### 日常維運

| 操作 | 指令 |
|---|---|
| 查看狀態 | `systemctl status vonode vonode-gateway` |
| 查看記錄 | `journalctl -u vonode -f` |
| 重新啟動 | `sudo systemctl restart vonode`（最多需要兩分鐘：節點會先取消註冊 Wi-Fi 通話並關閉通道） |
| 新的配對碼 | `sudo /opt/vonode/vonode pair -c /opt/vonode/config/config.yaml -host <位址>` |
| 忘記管理員密碼 | `sudo systemctl stop vonode`，接著 `sudo /opt/vonode/vonode reset-password -c /opt/vonode/config/config.yaml`，再 `sudo systemctl start vonode`。所有手機都會登出，需要重新配對。 |

## 用 Docker Compose 安裝（即將推出）

> [!IMPORTANT]
> **商用 Docker 映像檔尚未發布。** 發布之前，請使用 [systemd 安裝套件](#用-systemd-安裝套件安裝建議)，
> 這是本版本支援的安裝方式。下面的步驟說明 Docker 方式日後的用法。兩種方式的配對和方案用法完全相同。

Docker 方式以同一個映像檔 `vonode/vonode` 執行兩個容器，節點功能與 systemd 方式相同：`vonode`（核心，
使用主機網路，負責與模組通訊）和 `vonode-gateway`（無特權的 SSH 服務，App 透過 TCP 2222 連線）。
部署檔案位於安裝套件的 `docker/` 目錄。

### 1. 準備主機

Linux、模組和 SIM 卡的需求與上文相同。停用 ModemManager，並安裝含 Compose 外掛程式的 Docker Engine；
Docker 官方的便利指令碼適用於 Debian、Ubuntu 和 Fedora。最後一道指令必須輸出 Compose v2 的版本號碼。

```sh
sudo systemctl disable --now ModemManager
curl -fsSL https://get.docker.com | sh
sudo systemctl enable --now docker
docker compose version
```

### 2. 下載並驗證安裝套件

依 [下載與驗證](#下載與驗證) 下載、驗證並解壓縮安裝套件，然後進入它的 `docker/` 目錄：

```sh
cd vonode_${VERSION}_linux_amd64_commercial/docker
```

### 3. 執行安裝指令碼

```sh
sudo ./setup.sh --host node.example.com --image vonode/vonode@sha256:<digest>
```

- `--host` 是手機連到節點時使用的網域名稱或 IP 位址。不填時指令碼會建議使用主機的區域網路位址，在同一個
  Wi-Fi 下首次配對時這樣就可以。
- `--image` 填發行說明中經過核驗的 `vonode/vonode@sha256:<digest>`（僅在該版本提供映像檔時）。如果安裝
  套件已附帶經過核驗的映像檔，可以省略。可變動的映像檔標籤會被拒絕，除非加上 `--allow-tag`。

指令碼會檢查主機（Linux、amd64、root、Docker、Compose、ModemManager），列出找到的模組及其裝置節點，在
指令碼旁建立 `config/`、`data/` 和 `logs/`，寫入 `.env` 和 `docker-compose.override.yml`，下載映像檔，
啟動兩個容器並等待兩者都回報健康（首次啟動約半分鐘）。接著顯示初始管理員密碼的位置
（`config/initial-admin-password`，只有 root 可以讀取）和配對 QR Code。請像 systemd 方式一樣在主機防火牆
上允許 TCP 2222。用 `--port <n>` 改用其他 SSH 連接埠；路由器轉送的外部連接埠不同時，用 `--public-port <n>`。

再次執行 `sudo ./setup.sh` 會保留設定和資料。`./setup.sh --help` 會說明每個選項。

### 裝置對應

預設情況下，`setup.sh` 會把整個 `/dev` 繫結到核心容器中，所以之後插上的模組、或重設後以不同
`/dev/ttyUSB*` 編號重新列舉的模組，不必重新執行指令碼就會出現。核心容器以特權模式執行，因為它需要
TUN/XFRM、USB 和 ALSA。

如果只想把目前存在的模組裝置節點交給核心容器，請用 `sudo ./setup.sh --devices`；這種模式下，插拔、移動或
更換模組後要重新執行 `setup.sh`。`--no-devices` 不對應任何裝置。所選模式會記錄在 `.env` 中。

### Docker 方式的日常維運

以下指令在安裝套件的 `docker/` 目錄下執行。

| 操作 | 指令 |
|---|---|
| 查看狀態（兩個服務都應為 healthy） | `sudo docker compose ps` |
| 查看記錄 | `sudo docker compose logs -f vonode` |
| 新的配對碼 | `sudo docker compose exec vonode /app/vonode pair -c /app/config/config.yaml -host <位址> -port 2222` |
| 新增或移動了模組 | `sudo ./setup.sh` |
| 重新啟動（最多兩分鐘） | `sudo docker compose restart vonode` |
| 停止並保留資料 | `sudo ./setup.sh --uninstall` |
| 全部刪除 | `sudo ./setup.sh --uninstall --purge` |
| 忘記管理員密碼 | `sudo docker compose stop vonode`、`sudo docker compose run --rm --no-deps vonode reset-password -c /app/config/config.yaml`、`sudo docker compose start vonode` |

> [!WARNING]
> 請勿執行 `docker compose down -v`。`vonode_gateway-private` 磁碟區保存著已配對手機所固定的 SSH 主機金鑰。
> `setup.sh --uninstall` 會保留它，只有 `--purge` 才會刪除。

## 配對 App

1. 從 App Store 安裝 **Vonode**（免費下載）並開啟。
2. 在「連接你的節點」頁面點一下掃描框，把相機對準終端機上的 QR Code。
3. App 透過 SSH 連線到節點，固定節點的主機金鑰，並在首頁顯示這個節點和它的號碼。

App 和節點必須能透過 QR Code 裡的位址互相連線。如果 App 顯示連線錯誤，請先檢查連接埠轉送或 VPN，再產生
新的配對碼。之後要配對另一支手機，請在 App 中使用「換綁到新手機」，它會顯示一次性 QR Code。

## 免費方案或訂閱

未訂閱時，節點以**免費方案**執行：開啟「號碼」標籤頁，選擇一個你想查看簡訊的號碼。選定後在這個節點上不能
更換，還原備份也會保留它。之後「簡訊」標籤頁會顯示這個號碼收到的簡訊。

其他功能（電話、Wi-Fi 通話、傳簡訊、通知、eSIM 設定檔、代理、自動任務、更多號碼）都需要在 App 內購買
訂閱：開啟「更多」，點「查看方案」。一份訂閱解鎖一個節點。節點幾秒內就會解鎖並以完整模式重新啟動；之後用
「新增模組」加入模組，並為每張 SIM 卡開啟 Wi-Fi 通話。

硬體、SIM 資費方案、電信費用和主機代管不包含在內。

## 升級

- **在 App 中：** 節點會在「更多 > 軟體更新」中提供已簽章的更新。節點在安裝前會驗證更新的簽章。
- **systemd 安裝套件：** 下載並驗證新的安裝套件，解壓縮後在最上層目錄執行 `sudo ./install.sh`。設定、
  資料庫和 SSH 主機金鑰都會保留。
- **Docker：** 把新的 `vonode/vonode@sha256:<digest>` 寫入 `.env`（`VONODE_IMAGE=...`）或用 `--image`
  傳入，然後再次執行 `sudo ./setup.sh`。兩個容器一律使用同一個映像檔。

升級前請先備份：App 中的「更多 > 備份紀錄」。備份包含簡訊、通話紀錄和節點的訂閱綁定，請妥善保管。移轉到
新硬體時，在舊節點上建立並下載備份，安裝並配對新節點，再匯入並還原備份。不要讓舊節點繼續用同一份資料執行。

## 解除安裝

systemd 安裝套件（會刪除你的資料和備份）：

```sh
sudo systemctl disable --now vonode-gateway vonode
sudo rm /etc/systemd/system/vonode.service /etc/systemd/system/vonode-gateway.service /etc/tmpfiles.d/vonode.conf
sudo systemctl daemon-reload
sudo rm -rf /opt/vonode /var/lib/vonode-gateway
```

Docker：`sudo ./setup.sh --uninstall` 會停止節點並保留全部資料；加上 `--purge` 會刪除容器、磁碟區以及
`config/`、`data/`、`logs/`。

## 疑難排解

| 問題 | 處理方式 |
|---|---|
| ModemManager 正在執行 | `sudo systemctl disable --now ModemManager`，然後 `sudo systemctl restart vonode`（Docker 方式改為重新執行 `sudo ./setup.sh`）。 |
| 偵測不到模組 | 如果 `lsusb` 裡沒有 Quectel（`2c7c`）裝置，請換一條線或換一個連接埠，使用有外接電源的集線器，並用 `sudo dmesg \| tail -n 30` 查看 USB 供電錯誤。如果 `lsusb` 看得到模組但沒有 `/dev/ttyUSB*`，執行 `sudo modprobe -a option qmi_wwan`，然後重新插拔模組。 |
| App 連不上節點 | 在你的網路之外的裝置上執行 `nc -vz node.example.com 2222`。沒有回應表示路由器轉送或防火牆設定有誤，或者網路服務供應商使用了電信級 NAT；這時請使用 VPN，並用主機的 VPN 位址配對。 |
| 配對碼已過期或已使用 | 配對碼 5 分鐘內有效，只能使用一次。再執行一次 `pair` 指令即可。 |
| Wi-Fi 通話無法註冊 | 資費方案需要開通 Wi-Fi 通話，模組必須用這張 SIM 卡至少註冊過一次行動網路，主機需要能透過 IPv6 或 IPv4 連到電信業者。App 的診斷頁面會顯示目前所處階段。 |

更多內容請見 [Wiki](https://github.com/vonode/vonode-releases/wiki)（英文）和
[逐步指南](https://vonode.cc/zh-hant/install/guide/)。

## GPL 與 LGPL 元件的原始碼

節點軟體是專有軟體，但其中包含 strongSwan（GPL-2.0-or-later）以及靜態連結的 glibc 和 GMP（LGPL）。每個版本：

- 這些元件完整的對應原始碼（含建置指令碼）以發行資產 `vonode_<version>_linux_amd64_commercial.sources.tar` 提供；
- 安裝套件中的 `SOURCES.md` 列出這些元件並包含書面要約：原始碼在該版本發布後至少提供三年；如需實體媒體
  副本，可寄信至 support@vonode.cc 索取，收費不超過媒體和郵寄成本；
- SPDX SBOM 和 `THIRD_PARTY_NOTICES.txt` 列出全部第三方元件及其授權條款。

## 支援與法律資訊

- 官網：<https://vonode.cc/zh-hant/>
- 安裝指南：<https://vonode.cc/zh-hant/install/> · [逐步指南](https://vonode.cc/zh-hant/install/guide/)
- 支援的硬體：<https://vonode.cc/zh-hant/hardware/>
- 支援：[support@vonode.cc](mailto:support@vonode.cc) · <https://vonode.cc/zh-hant/support/>
- [隱私權政策](https://vonode.cc/zh-hant/privacy/) · [使用條款](https://vonode.cc/zh-hant/terms/) ·
  [第三方聲明](https://vonode.cc/zh-hant/third-party-notices/)

Vonode 不是緊急通訊服務。它仰賴你的硬體、電信業者、網路和 Apple 服務；請勿依賴它進行緊急求救或其他攸關
生命安全的通訊。請只在你有權使用的 SIM 卡、號碼和網路上使用，並遵守電信業者的條款。

© 2026 VONODE LLC。Vonode 是 VONODE LLC 的產品。
