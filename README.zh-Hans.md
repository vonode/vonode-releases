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
  <a href="https://github.com/vonode/vonode-releases/releases/latest"><img alt="最新版本" src="https://img.shields.io/github/v/release/vonode/vonode-releases?style=flat-square&label=latest%20release&color=2a9468"></a>
  <a href="https://vonode.cc/zh-hans/install/"><img alt="安装指南" src="https://img.shields.io/badge/docs-install%20guide-2475c2?style=flat-square"></a>
  <img alt="平台" src="https://img.shields.io/badge/node-Linux%20amd64%20%7C%20arm64-141b26?style=flat-square">
  <img alt="App" src="https://img.shields.io/badge/app-iOS%20%26%20iPadOS%2017%2B-141b26?style=flat-square">
  <a href="https://t.me/vonode_group"><img alt="Telegram" src="https://img.shields.io/badge/telegram-%40vonode__group-26a5e4?style=flat-square&logo=telegram&logoColor=white"></a>
</p>

Vonode 把装了蜂窝模组的 Linux 电脑变成你自己的私人节点。用 iPhone 和 iPad 上的 Vonode App
收发短信、拨打和接听电话、切换 eSIM 配置文件、开启 Wi-Fi 通话，全部通过你自己运行的硬件完成。

本仓库**只存放发布文件和文档**，不包含源代码。节点软件是 VONODE LLC 的专有软件。

v1.3.0 曾短暂发布后撤回，不再提供支持，请安装 v1.4.0 或更高版本。

## 目录

- [功能](#功能)
- [硬件和系统要求](#硬件和系统要求)
- [支持的运营商（Wi-Fi 通话）](#支持的运营商wi-fi-通话)
- [下载与校验](#下载与校验)
- [用 systemd 安装包安装（推荐）](#用-systemd-安装包安装推荐)
- [用 Docker Compose 安装](#用-docker-compose-安装) · [一键安装](#docker-compose-一键安装)
- [NAS（群晖、威联通、Unraid、TrueNAS）](#nas群晖威联通unraidtruenas)
- [配对 App](#配对-app)
- [免费方案、订阅或终身版](#免费方案订阅或终身版)
- [升级](#升级)
- [卸载](#卸载)
- [故障排查](#故障排查)
- [GPL 与 LGPL 组件的源代码](#gpl-与-lgpl-组件的源代码)
- [支持与法律信息](#支持与法律信息)

## 功能

| 功能 | 说明 |
|---|---|
| **短信** | 收发节点上所有号码的短信。垃圾短信自动归类，回复时默认使用收到这条会话的号码。 |
| **电话** | 通过你的节点用 Wi-Fi 通话拨打和接听电话。来电通过 CallKit 在 iPhone 上响铃，App 在后台时也一样。 |
| **Wi-Fi 通话** | 按 SIM 卡开启 Wi-Fi 通话（VoWiFi），电话经 Wi-Fi 通话进行。节点通过互联网连接运营商，需要你的套餐支持。 |
| **SIM 与 eSIM** | 在可插拔 eUICC 卡上切换 eSIM 配置文件；下载、重命名和删除在 App 中可用，但尚未在硬件上验证。实体 SIM 卡插上即可使用。 |
| **检查网络** | 用通俗的话检查 SIM 卡、运营商、移动数据、短信和通话服务；需要时，技术细节一点即看。 |
| **通知** | 通过 Apple 推送接收短信和来电提醒。可选的短信预览由节点加密，只在你的手机上解密。 |
| **还有更多** | 运营商代码（USSD）、自动任务、代理、备份与恢复、换绑到新手机，都在同一个 App 里。 |

你和你的号码之间没有 Vonode 云服务。App 通过加密 SSH 直接连接你的节点，并固定节点的主机密钥；
短信、通话记录和设置都保存在你运行的节点上。VONODE LLC 运营的中继负责投递 Apple 通知和确认订阅，不存储明文短信内容，只能看到通知的类型和时间这类元数据；详见[隐私政策](https://vonode.cc/zh-hans/privacy/)。

节点没有网页界面：装好、和 App 配对，之后的一切都在 App 里管理。

## 硬件和系统要求

购买硬件前，请先在 **[硬件页面](https://vonode.cc/zh-hans/hardware/)** 查看完整清单。

| 项目 | 要求 |
|---|---|
| **主机** | 64 位 x86（amd64）或 64 位 ARM（arm64）Linux：Debian 12、Ubuntu 22.04 及以上，或 Fedora；安装包方式需要 systemd，Docker 方式需要 Docker Engine 和 Compose v2。需要 root 权限；systemd 安装包还需要完整的 `iproute2` 软件包（Docker 镜像自带）。Intel N100 迷你主机、二手瘦客户机或 NUC 都合适。自 v1.4.2 起，arm64 与 amd64 一同作为正式版本发布：树莓派 4 或 5 装 64 位系统（例如 64 位 Raspberry Pi OS）即可使用，32 位系统不行。节点本身只需 2 GB 内存和 4 GB 磁盘。已验证平台：Ubuntu 22.04，amd64。 |
| **蜂窝模组** | 带 USB 串口（AT）和 QMI 接口、基于高通平台的 Quectel 模组。已实测：DJI Cellular 模块（内置 Quectel EG25-G，需按硬件页面的说明一次性切换 USB 标识）。EG25-G、EC25、EC20、EC21/EG21、EM05/EM06/EG06 和 RM500Q 已在代码中支持，但尚未实测。每个节点最多连接五个模组。 |
| **天线和供电** | 主天线和分集天线都要接上。连接多个模组时，请使用带供电的 USB 集线器。 |
| **SIM 卡与网络** | 已关闭 PIN 码的 SIM 卡；如需 eSIM，请使用可插拔 eUICC 卡。运营商已在套餐中开通 Wi-Fi 通话。手机能访问到 TCP 2222 端口：在路由器上做端口转发，或使用 WireGuard、Tailscale 等 VPN。 |
| **App** | iPhone 和 iPad 上的 Vonode 1.4.0 或更高版本，需要 iOS 或 iPadOS 17 及以上。App Store 免费下载。 |

暂不支持：32 位 ARM（armv7）主机和 32 位操作系统、OpenWrt 路由器、macOS 或 Windows
上的 Docker Desktop、没有 USB 直通的虚拟机，以及只提供网卡功能的 USB 上网卡（HiLink、仅 RNDIS 或仅
ECM 固件）。蜂窝网络通话目前还没有在任何模组上得到确认；通话可以通过 Wi-Fi 通话进行。

Vonode App 在中国大陆以外的 App Store 国家和地区提供；中国大陆的 App Store 目前不提供。

<!-- CARRIERS:START 由节点发布工具生成，请勿手工修改。 digest=sha256:603eeb3403084b9701f3e5fb0f4d687ff67c2b6b25932536f65081fe187b53be -->

## 支持的运营商（Wi-Fi 通话）

Vonode 用节点里的 SIM 卡进行 Wi-Fi 通话（VoWiFi）和收发短信。本表由节点内的运营商设置自动生成：428 条，覆盖 109 个国家和地区的 423 个网络代码。网站上可以搜索：https://vonode.cc/zh-hans/docs/carriers/

- 可能需要运营商在你的套餐上开通 Wi-Fi 通话。
- 部分运营商会核对线路的 Wi-Fi 通话授权（TS.43，例如 T-Mobile US 和 AT&T），部分运营商只允许经过认证的设备。Vonode 从不修改 IMEI。
- 中国大陆（MCC 460/461）和印度（MCC 404/405/406）的 SIM 卡：不提供 Wi-Fi 通话（92 个网络代码）；通过移动网络的通话和短信照常可用。
- 未标“已实测”的条目我们没有实测——结果取决于运营商。
- 未列出的运营商也会使用标准 3GPP 设置，可能可以使用；结果取决于运营商。
- 试过某家运营商？请告诉我们：support@vonode.cc 或 Telegram 群 https://t.me/vonode_group。

| 状态 | 条目 | 说明 |
| --- | ---: | --- |
| 已实测 | 5 | 我们用这家运营商的真实 SIM 卡，在自己的硬件和当前节点软件上完成了 Wi-Fi 通话注册。 |
| 用户反馈可用 | 7 | 用户告诉我们可以使用，我们还没有在自己的硬件上验证。 |
| 已配置 | 51 | 节点内有为这家运营商网络制定的设置。 |
| 厂商数据配置 | 190 | 根据手机厂商公开的运营商配置生成的设置，节点 v1.4.2 起提供。 |
| 自动发现（尽力而为） | 175 | 节点从标准 3GPP 设置和厂商数据提示出发，自动调整。 |

### 已实测和用户反馈可用的运营商

| 国家或地区 | 运营商 | MCC-MNC | 状态 | 专用设置 | 说明 |
| --- | --- | --- | --- | --- | --- |
| 英国 | giffgaff (O2) | `234-10` | 已实测 | 已内置 | 用 giffgaff SIM 卡在 O2 网络上实测。 |
| 英国 | Vodafone UK | `234-15` | 已实测 | 已内置 | 用 Vodafone UK SIM 卡实测。 |
| 英国 | CMLink UK (EE) | `234-33` | 已实测 | 已内置 | 用 CMLink UK SIM 卡在 EE 网络上实测。 |
| 英国 | CTExcel (EE) | `234-33` | 已实测 | 已内置 | 用 CTExcel SIM 卡在 EE 网络上实测。 |
| 美国 | Red Pocket (AT&T) | `310-280` | 已实测 | 已内置 | 用 Red Pocket SIM 卡在 AT&T 网络上实测。 |
| 菲律宾 | DITO | `515-66` | 用户反馈可用 | 节点 v1.4.2 起 | 用户反馈可用；我们未实测。 |
| 台湾 | Far EasTone 遠傳電信 | `466-01` | 用户反馈可用 | 节点 v1.4.2 起 | 用户反馈可用；我们未实测。 |
| 台湾 | Chunghwa Telecom 中華電信 | `466-92` | 用户反馈可用 | 节点 v1.4.2 起 | 用户反馈可用；我们未实测。 |
| 台湾 | Taiwan Mobile 台灣大哥大 | `466-97` | 用户反馈可用 | 节点 v1.4.2 起 | 用户反馈可用；我们未实测。 |
| 泰国 | AIS | `520-01` | 用户反馈可用 | 节点 v1.4.2 起 | 用户反馈可用；我们未实测。 |
| 泰国 | AIS | `520-03` | 用户反馈可用 | 节点 v1.4.2 起 | 用户反馈可用；我们未实测。 |
| 英国 | Lebara UK | `204-04` | 用户反馈可用 | 节点 v1.4.2 起 | 用户反馈可用；我们未实测。 Lebara UK 的 SIM 卡使用网络代码 204-04（Vodafone 荷兰号段）。 |

### 按国家和地区列出全部运营商

带说明的完整表格：[CARRIERS.zh-Hans.md](CARRIERS.zh-Hans.md) · 可搜索的网站页面：https://vonode.cc/zh-hans/docs/carriers/

<details><summary>阿尔巴尼亚 (1)</summary>

- Vodafone `276-02` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>阿根廷 (7)</summary>

- Movistar `722-007` · 自动发现（尽力而为）
- Telecom Personal S.A. `722-034` · 厂商数据配置 · 节点 v1.4.2 起
- Nuestro `722-036` · 自动发现（尽力而为）
- Claro AR `722-310` · 厂商数据配置 · 节点 v1.4.2 起
- Compañía de Telefonos del Interior Norte S.A. `722-320` · 自动发现（尽力而为）
- Compañía de Telefonos del Interior S.A. `722-330` · 自动发现（尽力而为）
- Telecom Personal S.A. `722-341` · 自动发现（尽力而为）

</details>
<details><summary>亚美尼亚 (1)</summary>

- K Telecom CJSC `283-05` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>澳大利亚 (7)</summary>

- Telstra `505-01` · 厂商数据配置 · 节点 v1.4.2 起
- Optus `505-02` · 厂商数据配置 · 节点 v1.4.2 起
- Vodafone `505-03` · 厂商数据配置 · 节点 v1.4.2 起
- AAPT Ltd. `505-14` · 自动发现（尽力而为）
- Telstra `505-71` · 厂商数据配置 · 节点 v1.4.2 起
- Telstra `505-72` · 厂商数据配置 · 节点 v1.4.2 起
- Optus `505-90` · 自动发现（尽力而为）

</details>
<details><summary>奥地利 (9)</summary>

- A1 `232-01` · 厂商数据配置 · 节点 v1.4.2 起
- A1 `232-02` · 自动发现（尽力而为）
- Magenta Telekom `232-03` · 厂商数据配置 · 节点 v1.4.2 起
- Drei `232-05` · 自动发现（尽力而为）
- Magenta Telekom `232-07` · 厂商数据配置 · 节点 v1.4.2 起
- Drei `232-10` · 厂商数据配置 · 节点 v1.4.2 起
- A1 `232-11` · 自动发现（尽力而为）
- A1 `232-12` · 自动发现（尽力而为）
- Spusu AT `232-17` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>阿塞拜疆 (2)</summary>

- Azercell `400-01` · 自动发现（尽力而为）
- Bakcell `400-02` · 自动发现（尽力而为）

</details>
<details><summary>巴哈马 (1)</summary>

- BTC `364-039` · 自动发现（尽力而为）

</details>
<details><summary>巴林 (2)</summary>

- Batelco `426-01` · 厂商数据配置 · 节点 v1.4.2 起
- stc BH `426-04` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>孟加拉国 (1)</summary>

- Grameenphone `470-01` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>巴巴多斯 (1)</summary>

- Cable & Wireless (Barbados) Ltd. `342-600` · 自动发现（尽力而为）

</details>
<details><summary>白俄罗斯 (2)</summary>

- A1 BY `257-01` · 厂商数据配置 · 节点 v1.4.2 起
- MTS `257-02` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>比利时 (4)</summary>

- Proximus `206-01` · 厂商数据配置 · 节点 v1.4.2 起
- Telenet `206-05` · 厂商数据配置 · 节点 v1.4.2 起
- Orange `206-10` · 厂商数据配置 · 节点 v1.4.2 起
- BASE `206-20` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>不丹 (1)</summary>

- Bhutan Telecom Ltd `402-11` · 自动发现（尽力而为）

</details>
<details><summary>巴西 (8)</summary>

- Claro BR `724-05` · 厂商数据配置 · 节点 v1.4.2 起
- Vivo `724-06` · 厂商数据配置 · 节点 v1.4.2 起
- Vivo `724-10` · 厂商数据配置 · 节点 v1.4.2 起
- Vivo `724-11` · 厂商数据配置 · 节点 v1.4.2 起
- Americel `724-12` · 自动发现（尽力而为）
- Vivo `724-23` · 厂商数据配置 · 节点 v1.4.2 起
- Claro BR `724-38` · 自动发现（尽力而为）
- Brisanet `724-77` · 自动发现（尽力而为）

</details>
<details><summary>文莱 (2)</summary>

- B-Mobile `528-02` · 自动发现（尽力而为）
- DST Com `528-11` · 自动发现（尽力而为）

</details>
<details><summary>保加利亚 (2)</summary>

- A1 `284-01` · 自动发现（尽力而为）
- VIVACOM `284-03` · 自动发现（尽力而为）

</details>
<details><summary>柬埔寨 (3)</summary>

- Smart `456-02` · 自动发现（尽力而为）
- Smart `456-05` · 厂商数据配置 · 节点 v1.4.2 起
- Smart `456-06` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>加拿大 (12)</summary>

- TELUS `302-220` · 厂商数据配置 · 节点 v1.4.2 起
- TELUS `302-221` · 厂商数据配置 · 节点 v1.4.2 起
- EastLink `302-270` · 自动发现（尽力而为）
- Fido `302-370` · 厂商数据配置 · 节点 v1.4.2 起
- Freedom Mobile `302-490` · 自动发现（尽力而为）
- Videotron `302-500` · 厂商数据配置 · 节点 v1.4.2 起
- Bell Mobility `302-610` · 厂商数据配置 · 节点 v1.4.2 起
- Bell Mobility `302-630` · 自动发现（尽力而为）
- Bell Mobility `302-640` · 自动发现（尽力而为）
- Bell `302-690` · 自动发现（尽力而为）
- Rogers `302-720` · 自动发现（尽力而为）
- Sask Tel Mobility `302-780` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>智利 (6)</summary>

- Entel `730-01` · 厂商数据配置 · 节点 v1.4.2 起
- Movistar `730-02` · 厂商数据配置 · 节点 v1.4.2 起
- Claro CL `730-03` · 厂商数据配置 · 节点 v1.4.2 起
- Movistar `730-07` · 自动发现（尽力而为）
- WOM `730-09` · 厂商数据配置 · 节点 v1.4.2 起
- Entel `730-10` · 自动发现（尽力而为）

</details>
<details><summary>哥伦比亚 (3)</summary>

- Claro CO `732-101` · 厂商数据配置 · 节点 v1.4.2 起
- Movistar `732-123` · 自动发现（尽力而为）
- WOM `732-360` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>哥斯达黎加 (1)</summary>

- Claro CR `712-03` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>克罗地亚 (3)</summary>

- Hrvatski Telekom `219-01` · 厂商数据配置 · 节点 v1.4.2 起
- A1/Tomato `219-10` · 厂商数据配置 · 节点 v1.4.2 起
- Hrvatski Telekom `219-20` · 自动发现（尽力而为）

</details>
<details><summary>塞浦路斯 (3)</summary>

- CYTA `280-01` · 自动发现（尽力而为）
- epic `280-10` · 厂商数据配置 · 节点 v1.4.2 起
- PrimeTel `280-20` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>捷克 (4)</summary>

- T-Mobile `230-01` · 厂商数据配置 · 节点 v1.4.2 起
- O2 `230-02` · 厂商数据配置 · 节点 v1.4.2 起
- Vodafone `230-03` · 厂商数据配置 · 节点 v1.4.2 起
- Vodafone `230-99` · 自动发现（尽力而为）

</details>
<details><summary>丹麦 (5)</summary>

- TDC Mobil `238-01` · 厂商数据配置 · 节点 v1.4.2 起
- Telenor `238-02` · 厂商数据配置 · 节点 v1.4.2 起
- 3 `238-06` · 厂商数据配置 · 节点 v1.4.2 起
- Telia `238-20` · 厂商数据配置 · 节点 v1.4.2 起
- Telia `238-30` · 自动发现（尽力而为）

</details>
<details><summary>多米尼加共和国 (2)</summary>

- Altice Dominicana `370-01` · 厂商数据配置 · 节点 v1.4.2 起
- Claro RD `370-02` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>厄瓜多尔 (2)</summary>

- Claro EC `740-01` · 厂商数据配置 · 节点 v1.4.2 起
- Telecsa S.A. `740-02` · 自动发现（尽力而为）

</details>
<details><summary>埃及 (2)</summary>

- Vodafone `602-02` · 自动发现（尽力而为）
- Etisalat `602-03` · 自动发现（尽力而为）

</details>
<details><summary>萨尔瓦多 (1)</summary>

- Claro SV `706-01` · 自动发现（尽力而为）

</details>
<details><summary>爱沙尼亚 (3)</summary>

- Telia `248-01` · 厂商数据配置 · 节点 v1.4.2 起
- Elisa `248-02` · 自动发现（尽力而为）
- Tele2 `248-03` · 自动发现（尽力而为）

</details>
<details><summary>芬兰 (4)</summary>

- Elisa `244-05` · 厂商数据配置 · 节点 v1.4.2 起
- DNA `244-12` · 自动发现（尽力而为）
- Elisa `244-21` · 自动发现（尽力而为）
- Telia `244-91` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>法国 (12)</summary>

- Orange `208-01` · 厂商数据配置 · 节点 v1.4.2 起
- Orange `208-02` · 厂商数据配置 · 节点 v1.4.2 起
- SFR `208-09` · 自动发现（尽力而为）
- SFR `208-10` · 厂商数据配置 · 节点 v1.4.2 起
- SFR `208-11` · 自动发现（尽力而为）
- SFR `208-13` · 自动发现（尽力而为）
- Free Mobile `208-15` · 自动发现（尽力而为）
- Bouygues Telecom `208-20` · 厂商数据配置 · 节点 v1.4.2 起
- Bouygues Telecom `208-21` · 自动发现（尽力而为）
- EURO INFORMATION TELECOM `208-26` · 厂商数据配置 · 节点 v1.4.2 起
- Coriolis `208-27` · 厂商数据配置 · 节点 v1.4.2 起
- Bouygues Telecom `208-88` · 自动发现（尽力而为）

</details>
<details><summary>法属安的列斯 (1)</summary>

- Orange Caraïbe Mobiles `340-01` · 自动发现（尽力而为）

</details>
<details><summary>法属波利尼西亚 (1)</summary>

- Viti `547-05` · 自动发现（尽力而为）

</details>
<details><summary>德国 (9)</summary>

- Telekom `262-01` · 厂商数据配置 · 节点 v1.4.2 起
- Vodafone `262-02` · 厂商数据配置 · 节点 v1.4.2 起
- O2 `262-03` · 已配置
- Telekom `262-06` · 厂商数据配置 · 节点 v1.4.2 起
- O2 `262-07` · 已配置
- O2 `262-11` · 已配置
- 1&1 `262-23` · 厂商数据配置 · 节点 v1.4.2 起
- Truphone `262-42` · 自动发现（尽力而为）
- Airbus DE `262-98` · 自动发现（尽力而为）

</details>
<details><summary>加纳 (1)</summary>

- MTN `620-01` · 自动发现（尽力而为）

</details>
<details><summary>希腊 (4)</summary>

- COSMOTE `202-01` · 厂商数据配置 · 节点 v1.4.2 起
- Vodafone `202-05` · 厂商数据配置 · 节点 v1.4.2 起
- Info Quest S.A. `202-09` · 自动发现（尽力而为）
- Telestet `202-10` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>危地马拉 (2)</summary>

- Claro GT `704-01` · 厂商数据配置 · 节点 v1.4.2 起
- Movistar `704-03` · 自动发现（尽力而为）

</details>
<details><summary>洪都拉斯 (1)</summary>

- Claro HN `708-001` · 自动发现（尽力而为）

</details>
<details><summary>香港 (13)</summary>

- csl / 1O1O `454-00` · 已配置
- 3 Hong Kong `454-03` · 已配置
- Hutchison 2G `454-04` · 自动发现（尽力而为）
- SmarTone HK `454-06` · 厂商数据配置 · 节点 v1.4.2 起
- MVNO/Trident `454-08` · 自动发现（尽力而为）
- China Mobile Hong Kong 中國移動香港 `454-12` · 厂商数据配置 · 节点 v1.4.2 起
- SmarTone HK `454-15` · 自动发现（尽力而为）
- PCCW `454-16` · 自动发现（尽力而为）
- SmarTone HK `454-17` · 自动发现（尽力而为）
- csl / 1O1O `454-19` · 已配置
- Public Mobile Networks/Reserved `454-20` · 自动发现（尽力而为）
- Public Mobile Networks/Reserved `454-30` · 自动发现（尽力而为）
- Public Mobile Networks/Reserved `454-31` · 自动发现（尽力而为）

</details>
<details><summary>匈牙利 (3)</summary>

- Yettel `216-01` · 厂商数据配置 · 节点 v1.4.2 起
- Magyar Telekom `216-30` · 厂商数据配置 · 节点 v1.4.2 起
- Vodafone `216-70` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>冰岛 (2)</summary>

- Tal hf `274-02` · 厂商数据配置 · 节点 v1.4.2 起
- Nova `274-11` · 自动发现（尽力而为）

</details>
<details><summary>印度尼西亚 (3)</summary>

- Smartfren `510-09` · 厂商数据配置 · 节点 v1.4.2 起
- Telkomsel `510-10` · 自动发现（尽力而为）
- Smartfren `510-28` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>国际 (1)</summary>

- AT&T Mobility (Wireless Maritime Services) `901-18` · 自动发现（尽力而为）

</details>
<details><summary>伊拉克 (1)</summary>

- Asiacell `418-05` · 自动发现（尽力而为）

</details>
<details><summary>爱尔兰 (4)</summary>

- Vodafone `272-01` · 自动发现（尽力而为）
- 3 `272-02` · 自动发现（尽力而为）
- Meteor Mobile Communications Ltd. `272-03` · 厂商数据配置 · 节点 v1.4.2 起
- Sky IE `272-25` · 自动发现（尽力而为）

</details>
<details><summary>以色列 (5)</summary>

- Partner Communications Co. Ltd. `425-01` · 厂商数据配置 · 节点 v1.4.2 起
- Cellcom Israel Ltd. `425-02` · 厂商数据配置 · 节点 v1.4.2 起
- Pelephone Communications Ltd. `425-03` · 厂商数据配置 · 节点 v1.4.2 起
- Hot Mobile `425-07` · 厂商数据配置 · 节点 v1.4.2 起
- Partner Communications Co. Ltd. `425-10` · 自动发现（尽力而为）

</details>
<details><summary>意大利 (6)</summary>

- TIM `222-01` · 厂商数据配置 · 节点 v1.4.2 起
- Kena `222-07` · 自动发现（尽力而为）
- Vodafone `222-10` · 自动发现（尽力而为）
- Spusu IT `222-56` · 自动发现（尽力而为）
- WINDTRE `222-88` · 自动发现（尽力而为）
- WINDTRE `222-99` · 自动发现（尽力而为）

</details>
<details><summary>牙买加 (1)</summary>

- Cable & Wireless `338-180` · 自动发现（尽力而为）

</details>
<details><summary>日本 (5)</summary>

- Rakuten Mobile(MNO) `440-11` · 自动发现（尽力而为）
- au (KDDI) `440-50` · 自动发现（尽力而为）
- au (KDDI) `440-51` · 厂商数据配置 · 节点 v1.4.2 起
- au `440-52` · 自动发现（尽力而为）
- au (KDDI) `440-54` · 自动发现（尽力而为）

</details>
<details><summary>约旦 (2)</summary>

- Fastlink `416-01` · 自动发现（尽力而为）
- Umniah `416-03` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>哈萨克斯坦 (1)</summary>

- Beeline `401-01` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>肯尼亚 (2)</summary>

- Safaricom `639-02` · 厂商数据配置 · 节点 v1.4.2 起
- Airtel Kenya `639-03` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>科威特 (3)</summary>

- Zain `419-02` · 自动发现（尽力而为）
- Ooredoo `419-03` · 厂商数据配置 · 节点 v1.4.2 起
- STC `419-04` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>老挝 (1)</summary>

- Unitel `457-03` · 自动发现（尽力而为）

</details>
<details><summary>拉脱维亚 (1)</summary>

- Tele2/ZZ `247-02` · 自动发现（尽力而为）

</details>
<details><summary>列支敦士登 (2)</summary>

- Telecom FL AG `295-01` · 自动发现（尽力而为）
- Mobilkom Liechtenstein `295-05` · 自动发现（尽力而为）

</details>
<details><summary>立陶宛 (2)</summary>

- Telia `246-01` · 自动发现（尽力而为）
- Tele2 `246-03` · 自动发现（尽力而为）

</details>
<details><summary>卢森堡 (3)</summary>

- P&T Luxembourg `270-01` · 厂商数据配置 · 节点 v1.4.2 起
- Tango `270-77` · 厂商数据配置 · 节点 v1.4.2 起
- Orange `270-99` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>澳门 (1)</summary>

- Hutchison Telecom `455-03` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>马来西亚 (12)</summary>

- Digi `502-10` · 自动发现（尽力而为）
- unifi mobile `502-11` · 自动发现（尽力而为）
- Maxis/Hotlink `502-12` · 厂商数据配置 · 节点 v1.4.2 起
- Celcom `502-13` · 自动发现（尽力而为）
- Digi `502-143` · 自动发现（尽力而为）
- Digi `502-146` · 自动发现（尽力而为）
- Yes 5G `502-152` · 厂商数据配置 · 节点 v1.4.2 起
- unifi mobile `502-153` · 厂商数据配置 · 节点 v1.4.2 起
- Digi `502-16` · 厂商数据配置 · 节点 v1.4.2 起
- TimeCel `502-17` · 自动发现（尽力而为）
- U Mobile `502-18` · 自动发现（尽力而为）
- Celcom `502-19` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>马尔代夫 (1)</summary>

- Ooredoo `472-02` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>墨西哥 (4)</summary>

- Telcel `334-002` · 自动发现（尽力而为）
- Telcel `334-020` · 厂商数据配置 · 节点 v1.4.2 起
- AT&T MX `334-050` · 厂商数据配置 · 节点 v1.4.2 起
- AT&T MX `334-090` · 自动发现（尽力而为）

</details>
<details><summary>摩纳哥 (1)</summary>

- MONACO TELECOM `212-10` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>黑山 (1)</summary>

- Crnogorski Telekom `297-02` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>摩洛哥 (2)</summary>

- Méditélécom `604-00` · 厂商数据配置 · 节点 v1.4.2 起
- Maroc `604-01` · 自动发现（尽力而为）

</details>
<details><summary>莫桑比克 (1)</summary>

- Vodacom `643-04` · 自动发现（尽力而为）

</details>
<details><summary>尼泊尔 (1)</summary>

- Nepal Telecommunications `429-01` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>荷兰 (5)</summary>

- Vodafone NL `204-04` · 已配置
- KPN Mobiel `204-08` · 厂商数据配置 · 节点 v1.4.2 起
- Odido `204-16` · 厂商数据配置 · 节点 v1.4.2 起
- Truphone `204-33` · 自动发现（尽力而为）
- KPN Lab `204-69` · 自动发现（尽力而为）

</details>
<details><summary>新西兰 (3)</summary>

- Vodafone `530-01` · 已配置
- Spark `530-05` · 已配置
- 2degrees `530-24` · 已配置

</details>
<details><summary>尼加拉瓜 (1)</summary>

- Claro NI `710-21` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>北马其顿 (2)</summary>

- Makedonski Telekom `294-01` · 自动发现（尽力而为）
- Nov Operator `294-03` · 自动发现（尽力而为）

</details>
<details><summary>挪威 (4)</summary>

- Telenor `242-01` · 自动发现（尽力而为）
- Telia `242-02` · 厂商数据配置 · 节点 v1.4.2 起
- OneCall `242-05` · 厂商数据配置 · 节点 v1.4.2 起
- Ice Norway `242-14` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>阿曼 (1)</summary>

- Omantel `422-02` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>巴基斯坦 (3)</summary>

- Jazz `410-01` · 厂商数据配置 · 节点 v1.4.2 起
- Telenor `410-06` · 自动发现（尽力而为）
- Jazz `410-07` · 自动发现（尽力而为）

</details>
<details><summary>巴拿马 (4)</summary>

- Cable & Wireless Panama S.A. `714-01` · 厂商数据配置 · 节点 v1.4.2 起
- Movistar `714-02` · 自动发现（尽力而为）
- Movistar `714-020` · 厂商数据配置 · 节点 v1.4.2 起
- Claro PA `714-03` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>巴拉圭 (1)</summary>

- Claro PY `744-02` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>秘鲁 (3)</summary>

- Movistar `716-06` · 厂商数据配置 · 节点 v1.4.2 起
- Claro PE `716-10` · 厂商数据配置 · 节点 v1.4.2 起
- Entel `716-17` · 自动发现（尽力而为）

</details>
<details><summary>菲律宾 (3)</summary>

- Globe Telecom `515-02` · 厂商数据配置 · 节点 v1.4.2 起
- Smart Communications `515-03` · 厂商数据配置 · 节点 v1.4.2 起
- DITO `515-66` · 用户反馈可用 · 节点 v1.4.2 起

</details>
<details><summary>波兰 (5)</summary>

- Plus `260-01` · 自动发现（尽力而为）
- T-Mobile `260-02` · 厂商数据配置 · 节点 v1.4.2 起
- Orange `260-03` · 厂商数据配置 · 节点 v1.4.2 起
- IDEA (UMTS)/PTK Centertel sp. Z.o.o. `260-05` · 自动发现（尽力而为）
- PLAY `260-06` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>葡萄牙 (6)</summary>

- NOS `268-03` · 厂商数据配置 · 节点 v1.4.2 起
- MEO `268-06` · 自动发现（尽力而为）
- NOS `268-93` · 自动发现（尽力而为）
- NOS `268-97` · 自动发现（尽力而为）
- NOS `268-98` · 自动发现（尽力而为）
- NOS `268-99` · 自动发现（尽力而为）

</details>
<details><summary>波多黎各 (1)</summary>

- Claro PR `330-110` · 自动发现（尽力而为）

</details>
<details><summary>卡塔尔 (2)</summary>

- Ooredoo `427-01` · 自动发现（尽力而为）
- Vodafone `427-02` · 自动发现（尽力而为）

</details>
<details><summary>罗马尼亚 (3)</summary>

- Vodafone `226-01` · 厂商数据配置 · 节点 v1.4.2 起
- Digi.Mobil `226-05` · 自动发现（尽力而为）
- Orange `226-10` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>俄罗斯 (6)</summary>

- MTS (МТС) `250-01` · 厂商数据配置 · 节点 v1.4.2 起
- MegaFon `250-02` · 厂商数据配置 · 节点 v1.4.2 起
- Orensot `250-11` · 自动发现（尽力而为）
- Tele2 `250-20` · 自动发现（尽力而为）
- Sberbank-Telecom `250-50` · 厂商数据配置 · 节点 v1.4.2 起
- Beeline (Билайн) `250-99` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>留尼汪 (2)</summary>

- Orange La Réunion `647-00` · 厂商数据配置 · 节点 v1.4.2 起
- Zeop `647-04` · 自动发现（尽力而为）

</details>
<details><summary>沙特阿拉伯 (4)</summary>

- STC `420-01` · 厂商数据配置 · 节点 v1.4.2 起
- Mobily `420-03` · 厂商数据配置 · 节点 v1.4.2 起
- Zain Saudi Arabia `420-04` · 厂商数据配置 · 节点 v1.4.2 起
- Virgin `420-05` · 自动发现（尽力而为）

</details>
<details><summary>塞内加尔 (1)</summary>

- Sentel GSM `608-02` · 自动发现（尽力而为）

</details>
<details><summary>塞尔维亚 (1)</summary>

- A1 SRB `220-05` · 自动发现（尽力而为）

</details>
<details><summary>新加坡 (5)</summary>

- Singtel `525-01` · 厂商数据配置 · 节点 v1.4.2 起
- Singtel `525-02` · 自动发现（尽力而为）
- M1 `525-03` · 厂商数据配置 · 节点 v1.4.2 起
- StarHub `525-05` · 厂商数据配置 · 节点 v1.4.2 起
- SIMBA `525-10` · 自动发现（尽力而为）

</details>
<details><summary>斯洛伐克 (4)</summary>

- Orange `231-01` · 厂商数据配置 · 节点 v1.4.2 起
- Slovak Telekom `231-02` · 自动发现（尽力而为）
- 4ka SK `231-03` · 厂商数据配置 · 节点 v1.4.2 起
- O2 `231-06` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>斯洛文尼亚 (1)</summary>

- Telekom Slovenije `293-41` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>南非 (4)</summary>

- Vodacom `655-01` · 厂商数据配置 · 节点 v1.4.2 起
- Telkom `655-02` · 厂商数据配置 · 节点 v1.4.2 起
- Cell C (Pty) Ltd. `655-07` · 厂商数据配置 · 节点 v1.4.2 起
- MTN `655-10` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>西班牙 (7)</summary>

- Vodafone `214-01` · 厂商数据配置 · 节点 v1.4.2 起
- Orange `214-03` · 厂商数据配置 · 节点 v1.4.2 起
- Movistar `214-05` · 自动发现（尽力而为）
- Euskaltel `214-06` · 自动发现（尽力而为）
- Movistar `214-07` · 厂商数据配置 · 节点 v1.4.2 起
- Digi Spain `214-22` · 厂商数据配置 · 节点 v1.4.2 起
- Truphone `214-27` · 自动发现（尽力而为）

</details>
<details><summary>斯里兰卡 (2)</summary>

- Sri Lanka Telecom Mobitel `413-01` · 自动发现（尽力而为）
- Dialog Sri Lanka `413-02` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>苏丹 (1)</summary>

- Sudani `634-07` · 自动发现（尽力而为）

</details>
<details><summary>瑞典 (4)</summary>

- Telia Sverige AB `240-01` · 厂商数据配置 · 节点 v1.4.2 起
- 3 (Hi3G Access AB) `240-02` · 厂商数据配置 · 节点 v1.4.2 起
- Tele2/Comviq Sverige/Com Hem `240-07` · 厂商数据配置 · 节点 v1.4.2 起
- Telenor Sverige AB `240-08` · 自动发现（尽力而为）

</details>
<details><summary>瑞士 (5)</summary>

- Swisscom `228-01` · 厂商数据配置 · 节点 v1.4.2 起
- Sunrise `228-02` · 已配置
- Salt `228-03` · 厂商数据配置 · 节点 v1.4.2 起
- Sunrise `228-12` · 已配置
- UPC `228-53` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>台湾 (7)</summary>

- Far EasTone 遠傳電信 `466-01` · 用户反馈可用 · 节点 v1.4.2 起
- Far EasTone 遠傳電信 `466-02` · 自动发现（尽力而为）
- Far EasTone 遠傳電信 `466-03` · 自动发现（尽力而为）
- Far EasTone 遠傳電信 (former Asia Pacific Telecom) `466-05` · 厂商数据配置 · 节点 v1.4.2 起
- Chunghwa Telecom 中華電信 `466-92` · 用户反馈可用 · 节点 v1.4.2 起
- Taiwan Mobile 台灣大哥大 `466-97` · 用户反馈可用 · 节点 v1.4.2 起
- TransAsia Telecoms `466-99` · 自动发现（尽力而为）

</details>
<details><summary>泰国 (6)</summary>

- CAT CDMA `520-00` · 厂商数据配置 · 节点 v1.4.2 起
- AIS `520-01` · 用户反馈可用 · 节点 v1.4.2 起
- AIS `520-03` · 用户反馈可用 · 节点 v1.4.2 起
- TrueMove H 4G LTE `520-04` · 厂商数据配置 · 节点 v1.4.2 起
- dtac `520-05` · 自动发现（尽力而为）
- dtac `520-18` · 自动发现（尽力而为）

</details>
<details><summary>土耳其 (4)</summary>

- Turkcell `286-01` · 厂商数据配置 · 节点 v1.4.2 起
- Vodafone `286-02` · 厂商数据配置 · 节点 v1.4.2 起
- Türk Telekom `286-03` · 厂商数据配置 · 节点 v1.4.2 起
- Türk Telekom `286-04` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>乌干达 (1)</summary>

- Airtel Uganda `641-01` · 自动发现（尽力而为）

</details>
<details><summary>乌克兰 (1)</summary>

- Astelit `255-06` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>阿联酋 (2)</summary>

- e& UAE `424-02` · 厂商数据配置 · 节点 v1.4.2 起
- du `424-03` · 自动发现（尽力而为）

</details>
<details><summary>英国 (22)</summary>

- Lebara UK `204-04` · 用户反馈可用 · 节点 v1.4.2 起
- Mapesbury Communications Ltd. `234-01` · 自动发现（尽力而为）
- FMS Solutions Ltd `234-04` · 自动发现（尽力而为）
- BT One Phone `234-08` · 自动发现（尽力而为）
- giffgaff (O2) `234-10` · 已实测
- O2 `234-10` · 已配置
- Vodafone UK `234-15` · 已实测
- Three Mobile `234-20` · 已配置
- Truphone `234-25` · 自动发现（尽力而为）
- EE `234-30` · 已配置
- EE `234-31` · 已配置
- EE `234-32` · 已配置
- CMLink UK (EE) `234-33` · 已实测
- CTExcel (EE) `234-33` · 已实测
- EE `234-33` · 已配置
- Virgin `234-38` · 自动发现（尽力而为）
- Spusu GB `234-40` · 厂商数据配置 · 节点 v1.4.2 起
- Cable and Wireless Guernsey `234-55` · 自动发现（尽力而为）
- Sky `234-57` · 厂商数据配置 · 节点 v1.4.2 起
- EE `234-86` · 已配置
- Three Mobile `234-94` · 已配置
- Three Mobile `235-94` · 已配置

</details>
<details><summary>美国 (63)</summary>

- T-Mobile - US `310-026` · 已配置
- AT&T `310-030` · 已配置
- AT&T `310-070` · 已配置
- AT&T Private `310-090` · 自动发现（尽力而为）
- Pacific Telecom Inc `310-110` · 自动发现（尽力而为）
- Sprint `310-120` · 厂商数据配置 · 节点 v1.4.2 起
- Cricket Wireless `310-150` · 厂商数据配置 · 节点 v1.4.2 起
- T-Mobile - US `310-160` · 已配置
- AT&T `310-170` · 已配置
- T-Mobile - US `310-200` · 已配置
- T-Mobile - US `310-210` · 已配置
- T-Mobile - US `310-220` · 已配置
- T-Mobile - US `310-230` · 已配置
- T-Mobile - US `310-240` · 已配置
- T-Mobile - US `310-250` · 已配置
- T-Mobile - US `310-260` · 已配置
- T-Mobile - US `310-270` · 已配置
- Red Pocket (AT&T) `310-280` · 已实测
- AT&T `310-280` · 已配置
- T-Mobile - US `310-300` · 已配置
- T-Mobile - US `310-310` · 已配置
- AT&T `310-380` · 已配置
- AT&T `310-410` · 已配置
- North East Cellular Inc. `310-450` · 自动发现（尽力而为）
- T-Mobile - US `310-490` · 已配置
- T-Mobile - US `310-530` · 已配置
- AT&T `310-560` · 已配置
- Verizon Wireless `310-590` · 自动发现（尽力而为）
- T-Mobile - US `310-640` · 已配置
- Jasper Wireless Inc. `310-650` · 自动发现（尽力而为）
- T-Mobile - US `310-660` · 已配置
- AT&T `310-680` · 已配置
- Sea Mobile `310-730` · 自动发现（尽力而为）
- T-Mobile - US `310-800` · 已配置
- Verizon Wireless `310-890` · 自动发现（尽力而为）
- AT&T `310-950` · 已配置
- AT&T `311-180` · 已配置
- U.S. Cellular `311-220` · 自动发现（尽力而为）
- C Spire `311-230` · 自动发现（尽力而为）
- Verizon Wireless `311-270` · 自动发现（尽力而为）
- GCI Communications Corp. `311-370` · 自动发现（尽力而为）
- Verizon Wireless `311-480` · 厂商数据配置 · 节点 v1.4.2 起
- Verizon Wireless `311-481` · 自动发现（尽力而为）
- T-Mobile - US `311-490` · 已配置
- U.S. Cellular `311-580` · 厂商数据配置 · 节点 v1.4.2 起
- U.S. Cellular `311-589` · 自动发现（尽力而为）
- United Wireless `311-650` · 自动发现（尽力而为）
- T-Mobile - Private 5G `311-660` · 厂商数据配置 · 节点 v1.4.2 起
- Cellcom `311-840` · 厂商数据配置 · 节点 v1.4.2 起
- T-Mobile - US `311-882` · 已配置
- T-Mobile - US `312-250` · 已配置
- strata `312-290` · 自动发现（尽力而为）
- NexTech Ota `312-420` · 自动发现（尽力而为）
- Sprint `312-530` · 厂商数据配置 · 节点 v1.4.2 起
- FirstNet (Lab) `312-670` · 厂商数据配置 · 节点 v1.4.2 起
- FirstNet `313-100` · 厂商数据配置 · 节点 v1.4.2 起
- FirstNet `313-110` · 自动发现（尽力而为）
- FirstNet `313-120` · 自动发现（尽力而为）
- FirstNet `313-130` · 自动发现（尽力而为）
- FirstNet `313-140` · 自动发现（尽力而为）
- Dish Wireless (Boost) `313-340` · 厂商数据配置 · 节点 v1.4.2 起
- Optimum `313-390` · 厂商数据配置 · 节点 v1.4.2 起
- Liberty Mobile `313-790` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>乌拉圭 (1)</summary>

- Claro UY `748-10` · 厂商数据配置 · 节点 v1.4.2 起

</details>
<details><summary>越南 (4)</summary>

- Mobifone `452-01` · 厂商数据配置 · 节点 v1.4.2 起
- Vinaphone `452-02` · 厂商数据配置 · 节点 v1.4.2 起
- Viettel Telecom `452-04` · 厂商数据配置 · 节点 v1.4.2 起
- Vietnamobile `452-05` · 自动发现（尽力而为）

</details>
<details><summary>赞比亚 (2)</summary>

- Airtel Zambia `645-01` · 自动发现（尽力而为）
- Telecel Zambia Ltd. `645-02` · 自动发现（尽力而为）

</details>

<!-- CARRIERS:END -->

## 下载与校验

自 v1.4.0 起，每个版本都发布在 **[Releases](https://github.com/vonode/vonode-releases/releases)** 页面。
把下面的 `VERSION` 设为该页面上显示的版本标签（文件名使用同一标签）。

| 文件 | 说明 |
|---|---|
| `vonode_<version>_linux_<arch>_commercial.tar.gz` | 安装包：节点程序、`install.sh`、systemd 单元、`docker/` 部署文件、指南和许可证，以及 `SHA256SUMS` 和覆盖整个安装包的发布签名 `SHA256SUMS.sig` |
| `vonode_<version>_linux_<arch>_commercial.tar.gz.sha256` | 安装包的 SHA-256 校验值 |
| `vonode_<version>_linux_<arch>_commercial` 和 `.sig` | 单文件形式的节点程序及其发布签名，供 App 内软件更新使用 |
| `vonode_<version>_linux_<arch>_commercial.THIRD_PARTY_NOTICES.txt` | 第三方组件及其许可证 |
| `vonode_<version>_linux_<arch>_commercial.spdx.json` | 软件物料清单（SPDX SBOM） |
| `vonode_<version>_linux_<arch>_commercial.sources.tar` | GPL 和 LGPL 组件的对应源代码 |
| `latest-commercial.json` 和 `latest-commercial.json.sig` | 带签名的发布清单（所有架构共用一份），节点和 Docker Compose 一键安装脚本据此获取版本，以及它的发布签名 |
| `docker-compose.yml`、`docker-compose.yml.sha256` 和 `docker-compose.yml.sig` | 本版本的 Docker Compose 文件（按 digest 固定镜像）、校验值，以及（自 v1.4.2 起）用发布密钥做的签名 |
| `compose-install.sh` 和 `compose-install.sh.sha256` | [Docker Compose 一键安装](#docker-compose-一键安装)脚本及其校验值 |
| `vonode-<version>-nas.tar.gz` 及 `.sha256` | 供 [NAS](#nas群晖威联通unraidtruenas) 容器应用使用的现成文件（自 v1.4.2 起）及其校验值 |
| `vonode-<version>-docker-alpine-sources.tar` 及 `.sha256` | Docker 镜像内 GPL、LGPL 和 MPL 软件包的对应源代码 |

`<arch>` 在 64 位 x86 上是 `amd64`（`uname -m` 输出 `x86_64`），自 v1.4.2 起在 64 位 ARM 上是 `arm64`
（`uname -m` 输出 `aarch64`）。下面的命令以 `amd64` 为例；在 arm64 主机上请改成 `arm64`。

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
# 单文件下载时，改用 vonode_<version>_linux_<arch>_commercial 和对应的 .sig。
BIN=vonode_${VERSION}_linux_amd64_commercial/vonode
printf 'vonode-binary-v1\n%s' "$(sha256sum "$BIN" | cut -d' ' -f1)" > vonode.msg
base64 -d "$BIN.sig" > vonode.sig.bin
openssl pkeyutl -verify -pubin -inkey vonode-update-key.pem -rawin -in vonode.msg -sigfile vonode.sig.bin
```

最后一条命令必须输出 `Signature Verified Successfully`。每个安装包里还有 `SHA256SUMS` 和
`SHA256SUMS.sig`，后者是用发布密钥对包内所有文件校验值所做的签名。

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

Docker Compose 方式用 Docker Hub 上的镜像 `vonode/vonode`（自 v1.4.0 起发布）
运行两个容器，节点功能与 systemd 方式相同：`vonode`（核心，负责驱动模组）和 `vonode-gateway`（无特权的
SSH 服务，App 通过 TCP 2222 连接它）。配对、方案和 App 的用法与 systemd 安装包完全一样。镜像标签与版本对应
（`v1.4.0` 对应 `1.4.0`，另有 `latest`）。自 v1.4.2 起镜像为多平台镜像，适用于 64 位 x86（amd64）和
64 位 ARM（arm64）主机，Docker 会自动选择合适的版本。每个版本还会发布一个按 digest 固定镜像的
`docker-compose.yml`，自 v1.4.2 起用 Vonode 发布密钥签名。

一台主机只用一种安装方式：不要在已安装 systemd 安装包的主机上再运行 Docker 方式。

### Docker Compose 一键安装

在装好 Docker Engine 和 Compose 插件的 Linux 主机上（见下面第 1 步），一条命令即可下载并校验对应版本的
`docker-compose.yml`，安装到 `/opt/vonode-compose`，启动两个容器，等它们都 healthy 后显示配对二维码：

```sh
curl -fsSL https://vonode.cc/install/compose.sh | sudo sh
```

- 只下载并校验、不改动任何东西：
  `curl -fsSL https://vonode.cc/install/compose.sh | sh -s -- --dry-run`
- 选项：`--host <手机访问节点的地址>`、`--port <端口>`、`--dir <目录>`、`--version vX.Y.Z`、`--no-start`、
  `--no-pair`。再次运行同一条命令即可更新。
- 这个脚本也是发布资产 `compose-install.sh`（逐字节相同，SHA-256 为
  `ca1740be6b659dc44335775b712a9409f16a5592a5e00b8ba2bd8303540a4232`，也写在 `compose-install.sh.sha256` 中）。也可以先下载、阅读，再用
  `sudo sh compose-install.sh` 运行，不经管道。
- 校验方式：脚本从带签名的发布清单 `latest-commercial.json` 中找到当前版本 v1.4.2，在使用
  `docker-compose.yml` 之前用 Vonode 发布密钥校验它的签名 `docker-compose.yml.sig`。镜像始终按 digest 固定。
- 同一条命令适用于 amd64 和 arm64 主机；在 arm64 主机上，它现在安装正式版 v1.4.2（不再是此前的实验性 arm64
  预发布版本）。
- 它不会停用 ModemManager，也不会改动其他服务；ModemManager 正在运行时，它会打印停用它的命令。不需要账号，
  不发送任何数据。

下面第 1 到第 4 步是同样操作的手动版本。

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

## NAS（群晖、威联通、Unraid、TrueNAS）

> [!WARNING]
> **尚未在真实 NAS 硬件上测试。** 这些文件依照各厂商公开的文档编写，只做过离线检查；还没有人在群晖（Synology）、威联通（QNAP）、
> Unraid 或 TrueNAS 设备上实际运行过。欢迎把使用结果发送到 support@vonode.cc。条件允许时，建议优先用一台小型
> Linux 主机配合 systemd 安装包或 [Docker Compose 一键安装](#docker-compose-一键安装)。

自 v1.4.2 起，每个版本都提供一个 NAS 包，用 NAS 自带的容器应用运行同样的两个容器，不需要 `setup.sh`。从
[v1.4.2 版本页面](https://github.com/vonode/vonode-releases/releases/tag/v1.4.2)下载 `vonode-1.4.2-nas.tar.gz`
及其 `.sha256`，用 `sha256sum -c vonode-1.4.2-nas.tar.gz.sha256` 校验后解压。每个目录里都有一份逐步说明 `README.md`。

| 目录 | NAS 软件 | 安装方式 |
|---|---|---|
| `synology/` | DSM 7.2 及以上，Container Manager | Container Manager › 项目 › 新增，粘贴 `docker-compose.yml` |
| `qnap/` | QTS / QuTS hero 5，Container Station 3 | Container Station › 应用程序 › 创建，粘贴 `docker-compose.yml` |
| `unraid/` | Unraid 6.12 及以上，已启用 Docker | 把 `vonode.xml` 和 `vonode-gateway.xml` 复制到用户模板目录，再添加这两个容器 |
| `truenas/` | TrueNAS SCALE 24.10（Electric Eel）及以上 | Apps › Discover Apps › ⋮ › Install via YAML，粘贴 `docker-compose.yml` |

每个文件都固定到该版本镜像的 digest，与发布说明和 `docker-compose.yml` 中的相同。镜像是多平台的（amd64 和
arm64），Docker 会自动选择；不支持 32 位 ARM 的 NAS 机型。

- **主机网络和特权核心容器：** `vonode` 容器需要 `network_mode: host` 和 `privileged: true`（用于 Wi-Fi 通话隧道和
  USB 模组）；`vonode-gateway` 始终无特权，只发布 TCP 2222。
- **设备：** 核心容器获得 NAS 的整个 `/dev`，其中包括 `/dev/bus/usb` 和 `/dev/net/tun`。如果 NAS 不允许绑定
  `/dev`，按 NAS 包里 README 的示例逐个列出设备。
- **ModemManager 不能占用模组：** 原版 DSM、QTS 和 Unraid 不带 ModemManager；在 TrueNAS SCALE 上，如果它在运行，
  请执行 `sudo systemctl disable --now ModemManager`。
- **内核驱动：** 容器无法加载驱动。模组需要 NAS 内核里有 `option`、`qmi_wwan` 和 `cdc_wdm`。原版群晖 DSM 的内核
  通常没有这些驱动（DSM 7 已不再支持 USB 3G/4G 上网卡），因此在原版 DSM 上模组方式很可能无法使用；威联通请按机型
  检查。USB 读卡器配合你的 SIM 卡只需要 `/dev/bus/usb`，在这类 NAS 上更可行（仅 Wi-Fi 通话；读卡器线路需要订阅或
  Vonode 终身版，免费方案不可用）。
- 删除或重建应用时，不要删除命名卷：`vonode_gateway-private` 保存着已配对手机所固定的 SSH 主机密钥。

通过 SSH 配对：`sudo docker exec -it vonode /app/vonode pair -c /app/config/config.yaml -host <NAS 地址> -port 2222`。
升级时使用新版本的 NAS 包。

## 配对 App

1. 从 App Store 安装 **Vonode**（免费下载）并打开。
2. 在“连接你的节点”页面点按扫描框，把摄像头对准终端上的二维码。
3. App 通过 SSH 连接节点，固定节点的主机密钥，并在首页显示这个节点和它的号码。

App 和节点必须能通过二维码里的地址互相访问。如果 App 提示连接错误，请先检查端口转发或 VPN，再生成新的
配对码。之后要配对另一部手机，请在 App 中使用“换绑到新手机”，它会显示一次性二维码。

## 免费方案、订阅或终身版

未订阅也未购买终身版时，节点以**免费方案**运行：打开“号码”标签页，选择一个你想查看短信的号码。选定后在这个节点上不能
更换，恢复备份也会保留它。之后“短信”标签页会显示这个号码收到的短信。

其他功能（电话、Wi-Fi 通话、发短信、通知、eSIM 配置文件、代理、自动任务、更多号码）需要这个节点的 Vonode 订阅或 Vonode
终身版（在 App 内购买）：打开“更多 › 订阅”。每份订阅或终身版解锁一个节点；你可以在 App 中把它转到另一个节点，30 天内最多 3 次，在新硬件上
重装节点软件或恢复备份也算一次转移。节点几秒内就会解锁并以完整模式重启；之后用
“添加模组”加入模组，并为每张 SIM 卡开启 Wi-Fi 通话。

硬件、SIM 套餐、运营商费用和托管不包含在内。

## 升级

- **在 App 中：** 用 systemd 安装包安装的节点会在“更多 › 软件更新”中提供带签名的更新。节点在安装前会验证更新的签名。
- **systemd 安装包：** 下载并校验新的安装包，解压后在其顶层目录运行 `sudo ./install.sh`。配置、数据库和
  SSH 主机密钥都会保留。
- **Docker Compose：** 用[一键安装](#docker-compose-一键安装)装的节点，再运行一次那条命令即可。手动安装时，把新版本的 `docker-compose.yml` 和 `docker-compose.yml.sha256` 下载到同一目录并校验，
  然后运行 `sudo docker compose pull` 和 `sudo docker compose up -d`。配置、数据库和 SSH 主机密钥都会保留，
  两个容器始终使用同一个镜像。App 内更新不会替换容器里的程序，请改为更新镜像。使用安装脚本时，运行
  `sudo ./setup.sh --image vonode/vonode@sha256:<digest>` 并传入新的 digest。

升级前请先备份：App 中的“更多 › 备份与恢复”。备份包含短信、通话记录和节点的订阅或终身版绑定，请妥善保管。迁移到
新硬件时，在旧节点上创建并下载备份，安装并配对新节点，再导入并恢复备份；这计入 30 天内 3 次转移。不要让旧节点继续用同一份数据运行。

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

Docker 镜像还包含采用 GPL、LGPL 和 MPL 许可证的 Alpine Linux 软件包；它们的对应源代码以发布资产
`vonode-<version>-docker-alpine-sources.tar` 提供，条件相同（至少三年；实体副本可向 support@vonode.cc 索取）。

## 支持与法律信息

- 官网：<https://vonode.cc/zh-hans/>
- 安装指南：<https://vonode.cc/zh-hans/install/> · [分步指南](https://vonode.cc/zh-hans/install/guide/)
- 支持的硬件：<https://vonode.cc/zh-hans/hardware/>
- 支持：[support@vonode.cc](mailto:support@vonode.cc) · <https://vonode.cc/zh-hans/support/>
- Telegram：[交流群](https://t.me/vonode_group) · [更新公告频道](https://t.me/vonode_app)
- [隐私政策](https://vonode.cc/zh-hans/privacy/) · [使用条款](https://vonode.cc/zh-hans/terms/) ·
  [第三方声明](https://vonode.cc/zh-hans/third-party-notices/)

Vonode 不是紧急通信服务。它依赖你的硬件、运营商、网络和 Apple 服务；请勿依靠它进行紧急呼救或其他攸关
生命安全的通信。请只在你有权使用的 SIM 卡、号码和网络上使用，并遵守运营商的条款。

DJI 是 SZ DJI Technology Co., Ltd. 的商标；Quectel 是 Quectel Wireless Solutions Co., Ltd. 的商标；Apple、iPhone、iPad
和 App Store 是 Apple Inc. 的商标；Docker 是 Docker, Inc. 的商标。Vonode 与它们没有关联，也未获其认可。

© 2026 VONODE LLC。Vonode 是 VONODE LLC 的产品。
