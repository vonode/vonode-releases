<p align="right"><a href="README.md">English</a> · <a href="README.zh-Hans.md">简体中文</a> · <b>繁體中文</b></p>

<p align="center">
  <img src="assets/logo.png" width="112" height="112" alt="Vonode 標誌">
</p>

<h1 align="center">Vonode 節點</h1>

<p align="center">
  <b>SIM 卡留在家裡，號碼隨身帶著走。</b><br>
  Vonode 節點軟體的發行下載、安裝指南和 Wiki。
</p>

## 一鍵安裝

在已安裝 Docker 的 64 位元 Linux（amd64 或 arm64）上，一道指令即可用 Docker Compose 裝好 Vonode 節點；
升級時再執行同一道指令即可。

```bash
curl -fsSL https://vonode.cc/install/compose.sh | sudo sh
```

選項、只驗證不安裝（dry run）以及指令碼如何驗證發行檔案：見 [Docker Compose 一鍵安裝](#docker-compose-一鍵安裝)。
其他安裝方式：[systemd 安裝套件（建議）](#用-systemd-安裝套件安裝建議) ·
[分步用 Docker Compose 安裝](#用-docker-compose-安裝) · [vonode.cc 上的安裝指南](https://vonode.cc/zh-hant/install/)

<p align="center">
  <a href="https://github.com/vonode/vonode-releases/releases/latest"><img alt="最新版本" src="https://img.shields.io/github/v/release/vonode/vonode-releases?style=flat-square&label=latest%20release&color=2a9468"></a>
  <a href="https://vonode.cc/zh-hant/install/"><img alt="安裝指南" src="https://img.shields.io/badge/docs-install%20guide-2475c2?style=flat-square"></a>
  <img alt="平台" src="https://img.shields.io/badge/node-Linux%20amd64%20%7C%20arm64-141b26?style=flat-square">
  <img alt="App" src="https://img.shields.io/badge/app-iOS%20%26%20iPadOS%2017%2B-141b26?style=flat-square">
  <a href="https://t.me/vonode_group"><img alt="Telegram" src="https://img.shields.io/badge/telegram-%40vonode__group-26a5e4?style=flat-square&logo=telegram&logoColor=white"></a>
</p>

Vonode 將裝有行動網路模組的 Linux 電腦變成你自己的私人節點。用 iPhone 和 iPad 上的 Vonode App
收發簡訊、撥打和接聽電話、切換 eSIM 設定檔、開啟 Wi-Fi 通話，全部透過你自己運作的硬體完成。

本儲存庫**只存放發行檔案和文件**，不包含原始碼。節點軟體是 VONODE LLC 的專有軟體。

v1.3.0 曾短暫發布後撤回，不再提供支援，請安裝 v1.4.0 或更新版本。

## 目錄

- [一鍵安裝](#一鍵安裝)
- [功能](#功能)
- [硬體和系統需求](#硬體和系統需求)
- [支援的電信業者（Wi-Fi 通話）](#支援的電信業者wi-fi-通話)
- [下載與驗證](#下載與驗證)
- [用 systemd 安裝套件安裝（建議）](#用-systemd-安裝套件安裝建議)
- [用 Docker Compose 安裝](#用-docker-compose-安裝) · [一鍵安裝](#docker-compose-一鍵安裝)
- [NAS（群暉、威聯通、Unraid、TrueNAS）](#nas群暉威聯通unraidtruenas)
- [配對 App](#配對-app)
- [免費方案、訂閱或終身版](#免費方案訂閱或終身版)
- [升級](#升級)
- [解除安裝](#解除安裝)
- [疑難排解](#疑難排解)
- [GPL 與 LGPL 元件的原始碼](#gpl-與-lgpl-元件的原始碼)
- [支援與法律資訊](#支援與法律資訊)

## 功能

| 功能 | 說明 |
|---|---|
| **簡訊** | 收發節點上所有號碼的簡訊。垃圾簡訊自動分類，回覆時預設使用收到這則對話的號碼。 |
| **電話** | 透過你的節點以 Wi-Fi 通話撥打和接聽電話。來電透過 CallKit 在 iPhone 上響鈴，App 在背景執行時也一樣。 |
| **Wi-Fi 通話** | 依 SIM 卡開啟 Wi-Fi 通話（VoWiFi），電話透過 Wi-Fi 通話進行。節點透過網際網路連線電信業者，需要你的資費方案支援。 |
| **SIM 與 eSIM** | 在可插拔 eUICC 卡上切換 eSIM 設定檔；下載、重新命名和刪除在 App 中可用，但尚未在硬體上驗證。實體 SIM 卡插上即可使用。 |
| **檢查網路** | 用淺白的文字檢查 SIM 卡、電信業者、行動資料、簡訊和通話服務；需要時，技術細節點一下就能看到。 |
| **通知** | 透過 Apple 推播接收簡訊和來電提醒。可選的簡訊預覽由節點加密，只在你的手機上解密。 |
| **還有更多** | 電信業者代碼（USSD）、自動任務、代理、備份與還原、換綁到新手機，都在同一個 App 裡。 |

你和你的號碼之間沒有 Vonode 雲端服務。App 透過加密 SSH 直接連線你的節點，並固定節點的主機金鑰；
簡訊、通話紀錄和設定都儲存在你運作的節點上。VONODE LLC 營運的中繼負責傳送 Apple 通知和確認訂閱，不儲存明文簡訊內容，只能看到通知的類型和時間這類中繼資料；詳見[隱私權政策](https://vonode.cc/zh-hant/privacy/)。

節點沒有網頁介面：安裝好、和 App 配對，之後的一切都在 App 裡管理。

## 硬體和系統需求

購買硬體前，請先在 **[硬體頁面](https://vonode.cc/zh-hant/hardware/)** 查看完整清單。

| 項目 | 需求 |
|---|---|
| **主機** | 64 位元 x86（amd64）或 64 位元 ARM（arm64）Linux：Debian 12、Ubuntu 22.04 以上版本，或 Fedora；安裝套件方式需要 systemd，Docker 方式需要 Docker Engine 和 Compose v2。需要 root 權限；systemd 安裝套件還需要完整的 `iproute2` 套件（Docker 映像檔已內建）。Intel N100 迷你電腦、二手精簡型電腦或 NUC 都適合。自 v1.4.2 起，arm64 與 amd64 一同以正式版本發行：Raspberry Pi 4 或 5 安裝 64 位元系統（例如 64 位元 Raspberry Pi OS）即可使用，32 位元系統不行。節點本身只需 2 GB 記憶體和 4 GB 磁碟空間。已驗證平台：Ubuntu 22.04，amd64。 |
| **行動網路模組** | 具備 USB 序列埠（AT）和 QMI 介面、採用高通平台的 Quectel 模組。已實測：DJI Cellular 模組（內建 Quectel EG25-G，需依硬體頁面的說明一次性切換 USB 識別碼）。EG25-G、EC25、EC20、EC21/EG21、EM05/EM06/EG06 和 RM500Q 已在程式碼中支援，但尚未實測。每個節點最多連接五個模組。 |
| **天線和供電** | 主天線和分集天線都要接上。連接多個模組時，請使用有外接電源的 USB 集線器。 |
| **SIM 卡與網路** | 已關閉 PIN 碼的 SIM 卡；如需 eSIM，請使用可插拔 eUICC 卡。電信業者已在資費方案中開通 Wi-Fi 通話。手機能連到 TCP 2222 連接埠：在路由器上設定連接埠轉送，或使用 WireGuard、Tailscale 等 VPN。 |
| **App** | iPhone 和 iPad 上的 Vonode 1.4.0 或更新版本，需要 iOS 或 iPadOS 17 以上版本。App Store 免費下載。 |

暫不支援：32 位元 ARM（armv7）主機和 32 位元作業系統、OpenWrt 路由器、macOS 或 Windows
上的 Docker Desktop、沒有 USB 直通的虛擬機器，以及只提供網路卡功能的 USB 網卡（HiLink、僅 RNDIS 或僅
ECM 韌體）。行動網路通話目前還沒有在任何模組上得到確認；通話可以透過 Wi-Fi 通話進行。

Vonode App 在中國大陸以外的 App Store 國家和地區提供，台灣、香港和澳門的 App Store 均可下載；中國大陸的
App Store 目前不提供。

<!-- CARRIERS:START 由節點發布工具產生，請勿手動修改。 digest=sha256:14e60b354fb148f55683dacc8a4df29dd20d1a79ed676dafb703ebdaa395d775 -->

## 支援的電信業者（Wi-Fi 通話）

Vonode 用節點裡的 SIM 卡進行 Wi-Fi 通話（VoWiFi）和收發簡訊。本表由節點內的電信業者設定自動產生：428 筆，涵蓋 109 個國家和地區的 423 個網路代碼。網站上可以搜尋：https://vonode.cc/zh-hant/docs/carriers/

- 可能需要電信業者在你的方案上開通 Wi-Fi 通話。
- 部分電信業者會核對線路的 Wi-Fi 通話授權（TS.43，例如 T-Mobile US 和 AT&T），部分電信業者只允許經過認證的裝置。Vonode 從不修改 IMEI。
- 中國大陸（MCC 460/461）和印度（MCC 404/405/406）的 SIM 卡：不提供 Wi-Fi 通話（92 個網路代碼）；透過行動網路的通話和簡訊照常可用。
- 未標「已實測」的項目我們沒有實測——結果取決於電信業者。
- 未列出的電信業者也會使用標準 3GPP 設定，可能可以使用；結果取決於電信業者。
- 試過某家電信業者？請告訴我們：support@vonode.cc 或 Telegram 群組 https://t.me/vonode_group。

| 狀態 | 項目 | 說明 |
| --- | ---: | --- |
| 已實測 | 5 | 我們用這家電信業者的真實 SIM 卡，在自己的硬體和目前節點軟體上完成了 Wi-Fi 通話註冊。 |
| 使用者回報可用 | 7 | 使用者告訴我們可以使用，我們還沒有在自己的硬體上驗證。 |
| 已設定 | 51 | 節點內有為這家電信業者網路制定的設定。 |
| 廠商資料設定 | 190 | 根據手機廠商公開的電信業者設定產生的設定，節點 v1.4.2 起提供。 |
| 自動探索（盡力而為） | 175 | 節點從標準 3GPP 設定和廠商資料提示出發，自動調整。 |

### 已實測和使用者回報可用的電信業者

| 國家或地區 | 電信業者 | MCC-MNC | 狀態 | 專用設定 | 說明 |
| --- | --- | --- | --- | --- | --- |
| 英國 | giffgaff (O2) | `234-10` | 已實測 | 已內建 | 用 giffgaff SIM 卡在 O2 網路上實測。 |
| 英國 | Vodafone UK | `234-15` | 已實測 | 已內建 | 用 Vodafone UK SIM 卡實測。 |
| 英國 | CMLink UK (EE) | `234-33` | 已實測 | 已內建 | 用 CMLink UK SIM 卡在 EE 網路上實測。 |
| 英國 | CTExcel (EE) | `234-33` | 已實測 | 已內建 | 用 CTExcel SIM 卡在 EE 網路上實測。 |
| 美國 | Red Pocket (AT&T) | `310-280` | 已實測 | 已內建 | 用 Red Pocket SIM 卡在 AT&T 網路上實測。 |
| 菲律賓 | DITO | `515-66` | 使用者回報可用 | 節點 v1.4.2 起 | 使用者回報可用；我們未實測。 |
| 台灣 | Far EasTone 遠傳電信 | `466-01` | 使用者回報可用 | 節點 v1.4.2 起 | 使用者回報可用；我們未實測。 |
| 台灣 | Chunghwa Telecom 中華電信 | `466-92` | 使用者回報可用 | 節點 v1.4.2 起 | 使用者回報可用；我們未實測。 |
| 台灣 | Taiwan Mobile 台灣大哥大 | `466-97` | 使用者回報可用 | 節點 v1.4.2 起 | 使用者回報可用；我們未實測。 |
| 泰國 | AIS | `520-01` | 使用者回報可用 | 節點 v1.4.2 起 | 使用者回報可用；我們未實測。 |
| 泰國 | AIS | `520-03` | 使用者回報可用 | 節點 v1.4.2 起 | 使用者回報可用；我們未實測。 |
| 英國 | Lebara UK | `204-04` | 使用者回報可用 | 節點 v1.4.2 起 | 使用者回報可用；我們未實測。 Lebara UK 的 SIM 卡使用網路代碼 204-04（Vodafone 荷蘭號段）。 |

### 依國家和地區列出全部電信業者

含說明的完整表格：[CARRIERS.zh-Hant.md](CARRIERS.zh-Hant.md) · 可搜尋的網站頁面：https://vonode.cc/zh-hant/docs/carriers/

<details><summary>阿爾巴尼亞 (1)</summary>

- Vodafone `276-02` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>阿根廷 (7)</summary>

- Movistar `722-007` · 自動探索（盡力而為）
- Telecom Personal S.A. `722-034` · 廠商資料設定 · 節點 v1.4.2 起
- Nuestro `722-036` · 自動探索（盡力而為）
- Claro AR `722-310` · 廠商資料設定 · 節點 v1.4.2 起
- Compañía de Telefonos del Interior Norte S.A. `722-320` · 自動探索（盡力而為）
- Compañía de Telefonos del Interior S.A. `722-330` · 自動探索（盡力而為）
- Telecom Personal S.A. `722-341` · 自動探索（盡力而為）

</details>
<details><summary>亞美尼亞 (1)</summary>

- K Telecom CJSC `283-05` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>澳洲 (7)</summary>

- Telstra `505-01` · 廠商資料設定 · 節點 v1.4.2 起
- Optus `505-02` · 廠商資料設定 · 節點 v1.4.2 起
- Vodafone `505-03` · 廠商資料設定 · 節點 v1.4.2 起
- AAPT Ltd. `505-14` · 自動探索（盡力而為）
- Telstra `505-71` · 廠商資料設定 · 節點 v1.4.2 起
- Telstra `505-72` · 廠商資料設定 · 節點 v1.4.2 起
- Optus `505-90` · 自動探索（盡力而為）

</details>
<details><summary>奧地利 (9)</summary>

- A1 `232-01` · 廠商資料設定 · 節點 v1.4.2 起
- A1 `232-02` · 自動探索（盡力而為）
- Magenta Telekom `232-03` · 廠商資料設定 · 節點 v1.4.2 起
- Drei `232-05` · 自動探索（盡力而為）
- Magenta Telekom `232-07` · 廠商資料設定 · 節點 v1.4.2 起
- Drei `232-10` · 廠商資料設定 · 節點 v1.4.2 起
- A1 `232-11` · 自動探索（盡力而為）
- A1 `232-12` · 自動探索（盡力而為）
- Spusu AT `232-17` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>亞塞拜然 (2)</summary>

- Azercell `400-01` · 自動探索（盡力而為）
- Bakcell `400-02` · 自動探索（盡力而為）

</details>
<details><summary>巴哈馬 (1)</summary>

- BTC `364-039` · 自動探索（盡力而為）

</details>
<details><summary>巴林 (2)</summary>

- Batelco `426-01` · 廠商資料設定 · 節點 v1.4.2 起
- stc BH `426-04` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>孟加拉 (1)</summary>

- Grameenphone `470-01` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>巴貝多 (1)</summary>

- Cable & Wireless (Barbados) Ltd. `342-600` · 自動探索（盡力而為）

</details>
<details><summary>白俄羅斯 (2)</summary>

- A1 BY `257-01` · 廠商資料設定 · 節點 v1.4.2 起
- MTS `257-02` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>比利時 (4)</summary>

- Proximus `206-01` · 廠商資料設定 · 節點 v1.4.2 起
- Telenet `206-05` · 廠商資料設定 · 節點 v1.4.2 起
- Orange `206-10` · 廠商資料設定 · 節點 v1.4.2 起
- BASE `206-20` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>不丹 (1)</summary>

- Bhutan Telecom Ltd `402-11` · 自動探索（盡力而為）

</details>
<details><summary>巴西 (8)</summary>

- Claro BR `724-05` · 廠商資料設定 · 節點 v1.4.2 起
- Vivo `724-06` · 廠商資料設定 · 節點 v1.4.2 起
- Vivo `724-10` · 廠商資料設定 · 節點 v1.4.2 起
- Vivo `724-11` · 廠商資料設定 · 節點 v1.4.2 起
- Americel `724-12` · 自動探索（盡力而為）
- Vivo `724-23` · 廠商資料設定 · 節點 v1.4.2 起
- Claro BR `724-38` · 自動探索（盡力而為）
- Brisanet `724-77` · 自動探索（盡力而為）

</details>
<details><summary>汶萊 (2)</summary>

- B-Mobile `528-02` · 自動探索（盡力而為）
- DST Com `528-11` · 自動探索（盡力而為）

</details>
<details><summary>保加利亞 (2)</summary>

- A1 `284-01` · 自動探索（盡力而為）
- VIVACOM `284-03` · 自動探索（盡力而為）

</details>
<details><summary>柬埔寨 (3)</summary>

- Smart `456-02` · 自動探索（盡力而為）
- Smart `456-05` · 廠商資料設定 · 節點 v1.4.2 起
- Smart `456-06` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>加拿大 (12)</summary>

- TELUS `302-220` · 廠商資料設定 · 節點 v1.4.2 起
- TELUS `302-221` · 廠商資料設定 · 節點 v1.4.2 起
- EastLink `302-270` · 自動探索（盡力而為）
- Fido `302-370` · 廠商資料設定 · 節點 v1.4.2 起
- Freedom Mobile `302-490` · 自動探索（盡力而為）
- Videotron `302-500` · 廠商資料設定 · 節點 v1.4.2 起
- Bell Mobility `302-610` · 廠商資料設定 · 節點 v1.4.2 起
- Bell Mobility `302-630` · 自動探索（盡力而為）
- Bell Mobility `302-640` · 自動探索（盡力而為）
- Bell `302-690` · 自動探索（盡力而為）
- Rogers `302-720` · 自動探索（盡力而為）
- Sask Tel Mobility `302-780` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>智利 (6)</summary>

- Entel `730-01` · 廠商資料設定 · 節點 v1.4.2 起
- Movistar `730-02` · 廠商資料設定 · 節點 v1.4.2 起
- Claro CL `730-03` · 廠商資料設定 · 節點 v1.4.2 起
- Movistar `730-07` · 自動探索（盡力而為）
- WOM `730-09` · 廠商資料設定 · 節點 v1.4.2 起
- Entel `730-10` · 自動探索（盡力而為）

</details>
<details><summary>哥倫比亞 (3)</summary>

- Claro CO `732-101` · 廠商資料設定 · 節點 v1.4.2 起
- Movistar `732-123` · 自動探索（盡力而為）
- WOM `732-360` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>哥斯大黎加 (1)</summary>

- Claro CR `712-03` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>克羅埃西亞 (3)</summary>

- Hrvatski Telekom `219-01` · 廠商資料設定 · 節點 v1.4.2 起
- A1/Tomato `219-10` · 廠商資料設定 · 節點 v1.4.2 起
- Hrvatski Telekom `219-20` · 自動探索（盡力而為）

</details>
<details><summary>賽普勒斯 (3)</summary>

- CYTA `280-01` · 自動探索（盡力而為）
- epic `280-10` · 廠商資料設定 · 節點 v1.4.2 起
- PrimeTel `280-20` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>捷克 (4)</summary>

- T-Mobile `230-01` · 廠商資料設定 · 節點 v1.4.2 起
- O2 `230-02` · 廠商資料設定 · 節點 v1.4.2 起
- Vodafone `230-03` · 廠商資料設定 · 節點 v1.4.2 起
- Vodafone `230-99` · 自動探索（盡力而為）

</details>
<details><summary>丹麥 (5)</summary>

- TDC Mobil `238-01` · 廠商資料設定 · 節點 v1.4.2 起
- Telenor `238-02` · 廠商資料設定 · 節點 v1.4.2 起
- 3 `238-06` · 廠商資料設定 · 節點 v1.4.2 起
- Telia `238-20` · 廠商資料設定 · 節點 v1.4.2 起
- Telia `238-30` · 自動探索（盡力而為）

</details>
<details><summary>多明尼加 (2)</summary>

- Altice Dominicana `370-01` · 廠商資料設定 · 節點 v1.4.2 起
- Claro RD `370-02` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>厄瓜多 (2)</summary>

- Claro EC `740-01` · 廠商資料設定 · 節點 v1.4.2 起
- Telecsa S.A. `740-02` · 自動探索（盡力而為）

</details>
<details><summary>埃及 (2)</summary>

- Vodafone `602-02` · 自動探索（盡力而為）
- Etisalat `602-03` · 自動探索（盡力而為）

</details>
<details><summary>薩爾瓦多 (1)</summary>

- Claro SV `706-01` · 自動探索（盡力而為）

</details>
<details><summary>愛沙尼亞 (3)</summary>

- Telia `248-01` · 廠商資料設定 · 節點 v1.4.2 起
- Elisa `248-02` · 自動探索（盡力而為）
- Tele2 `248-03` · 自動探索（盡力而為）

</details>
<details><summary>芬蘭 (4)</summary>

- Elisa `244-05` · 廠商資料設定 · 節點 v1.4.2 起
- DNA `244-12` · 自動探索（盡力而為）
- Elisa `244-21` · 自動探索（盡力而為）
- Telia `244-91` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>法國 (12)</summary>

- Orange `208-01` · 廠商資料設定 · 節點 v1.4.2 起
- Orange `208-02` · 廠商資料設定 · 節點 v1.4.2 起
- SFR `208-09` · 自動探索（盡力而為）
- SFR `208-10` · 廠商資料設定 · 節點 v1.4.2 起
- SFR `208-11` · 自動探索（盡力而為）
- SFR `208-13` · 自動探索（盡力而為）
- Free Mobile `208-15` · 自動探索（盡力而為）
- Bouygues Telecom `208-20` · 廠商資料設定 · 節點 v1.4.2 起
- Bouygues Telecom `208-21` · 自動探索（盡力而為）
- EURO INFORMATION TELECOM `208-26` · 廠商資料設定 · 節點 v1.4.2 起
- Coriolis `208-27` · 廠商資料設定 · 節點 v1.4.2 起
- Bouygues Telecom `208-88` · 自動探索（盡力而為）

</details>
<details><summary>法屬安地列斯 (1)</summary>

- Orange Caraïbe Mobiles `340-01` · 自動探索（盡力而為）

</details>
<details><summary>法屬玻里尼西亞 (1)</summary>

- Viti `547-05` · 自動探索（盡力而為）

</details>
<details><summary>德國 (9)</summary>

- Telekom `262-01` · 廠商資料設定 · 節點 v1.4.2 起
- Vodafone `262-02` · 廠商資料設定 · 節點 v1.4.2 起
- O2 `262-03` · 已設定
- Telekom `262-06` · 廠商資料設定 · 節點 v1.4.2 起
- O2 `262-07` · 已設定
- O2 `262-11` · 已設定
- 1&1 `262-23` · 廠商資料設定 · 節點 v1.4.2 起
- Truphone `262-42` · 自動探索（盡力而為）
- Airbus DE `262-98` · 自動探索（盡力而為）

</details>
<details><summary>迦納 (1)</summary>

- MTN `620-01` · 自動探索（盡力而為）

</details>
<details><summary>希臘 (4)</summary>

- COSMOTE `202-01` · 廠商資料設定 · 節點 v1.4.2 起
- Vodafone `202-05` · 廠商資料設定 · 節點 v1.4.2 起
- Info Quest S.A. `202-09` · 自動探索（盡力而為）
- Telestet `202-10` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>瓜地馬拉 (2)</summary>

- Claro GT `704-01` · 廠商資料設定 · 節點 v1.4.2 起
- Movistar `704-03` · 自動探索（盡力而為）

</details>
<details><summary>宏都拉斯 (1)</summary>

- Claro HN `708-001` · 自動探索（盡力而為）

</details>
<details><summary>香港 (13)</summary>

- csl / 1O1O `454-00` · 已設定
- 3 Hong Kong `454-03` · 已設定
- Hutchison 2G `454-04` · 自動探索（盡力而為）
- SmarTone HK `454-06` · 廠商資料設定 · 節點 v1.4.2 起
- MVNO/Trident `454-08` · 自動探索（盡力而為）
- China Mobile Hong Kong 中國移動香港 `454-12` · 廠商資料設定 · 節點 v1.4.2 起
- SmarTone HK `454-15` · 自動探索（盡力而為）
- PCCW `454-16` · 自動探索（盡力而為）
- SmarTone HK `454-17` · 自動探索（盡力而為）
- csl / 1O1O `454-19` · 已設定
- Public Mobile Networks/Reserved `454-20` · 自動探索（盡力而為）
- Public Mobile Networks/Reserved `454-30` · 自動探索（盡力而為）
- Public Mobile Networks/Reserved `454-31` · 自動探索（盡力而為）

</details>
<details><summary>匈牙利 (3)</summary>

- Yettel `216-01` · 廠商資料設定 · 節點 v1.4.2 起
- Magyar Telekom `216-30` · 廠商資料設定 · 節點 v1.4.2 起
- Vodafone `216-70` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>冰島 (2)</summary>

- Tal hf `274-02` · 廠商資料設定 · 節點 v1.4.2 起
- Nova `274-11` · 自動探索（盡力而為）

</details>
<details><summary>印尼 (3)</summary>

- Smartfren `510-09` · 廠商資料設定 · 節點 v1.4.2 起
- Telkomsel `510-10` · 自動探索（盡力而為）
- Smartfren `510-28` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>國際 (1)</summary>

- AT&T Mobility (Wireless Maritime Services) `901-18` · 自動探索（盡力而為）

</details>
<details><summary>伊拉克 (1)</summary>

- Asiacell `418-05` · 自動探索（盡力而為）

</details>
<details><summary>愛爾蘭 (4)</summary>

- Vodafone `272-01` · 自動探索（盡力而為）
- 3 `272-02` · 自動探索（盡力而為）
- Meteor Mobile Communications Ltd. `272-03` · 廠商資料設定 · 節點 v1.4.2 起
- Sky IE `272-25` · 自動探索（盡力而為）

</details>
<details><summary>以色列 (5)</summary>

- Partner Communications Co. Ltd. `425-01` · 廠商資料設定 · 節點 v1.4.2 起
- Cellcom Israel Ltd. `425-02` · 廠商資料設定 · 節點 v1.4.2 起
- Pelephone Communications Ltd. `425-03` · 廠商資料設定 · 節點 v1.4.2 起
- Hot Mobile `425-07` · 廠商資料設定 · 節點 v1.4.2 起
- Partner Communications Co. Ltd. `425-10` · 自動探索（盡力而為）

</details>
<details><summary>義大利 (6)</summary>

- TIM `222-01` · 廠商資料設定 · 節點 v1.4.2 起
- Kena `222-07` · 自動探索（盡力而為）
- Vodafone `222-10` · 自動探索（盡力而為）
- Spusu IT `222-56` · 自動探索（盡力而為）
- WINDTRE `222-88` · 自動探索（盡力而為）
- WINDTRE `222-99` · 自動探索（盡力而為）

</details>
<details><summary>牙買加 (1)</summary>

- Cable & Wireless `338-180` · 自動探索（盡力而為）

</details>
<details><summary>日本 (5)</summary>

- Rakuten Mobile(MNO) `440-11` · 自動探索（盡力而為）
- au (KDDI) `440-50` · 自動探索（盡力而為）
- au (KDDI) `440-51` · 廠商資料設定 · 節點 v1.4.2 起
- au `440-52` · 自動探索（盡力而為）
- au (KDDI) `440-54` · 自動探索（盡力而為）

</details>
<details><summary>約旦 (2)</summary>

- Fastlink `416-01` · 自動探索（盡力而為）
- Umniah `416-03` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>哈薩克 (1)</summary>

- Beeline `401-01` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>肯亞 (2)</summary>

- Safaricom `639-02` · 廠商資料設定 · 節點 v1.4.2 起
- Airtel Kenya `639-03` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>科威特 (3)</summary>

- Zain `419-02` · 自動探索（盡力而為）
- Ooredoo `419-03` · 廠商資料設定 · 節點 v1.4.2 起
- STC `419-04` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>寮國 (1)</summary>

- Unitel `457-03` · 自動探索（盡力而為）

</details>
<details><summary>拉脫維亞 (1)</summary>

- Tele2/ZZ `247-02` · 自動探索（盡力而為）

</details>
<details><summary>列支敦斯登 (2)</summary>

- Telecom FL AG `295-01` · 自動探索（盡力而為）
- Mobilkom Liechtenstein `295-05` · 自動探索（盡力而為）

</details>
<details><summary>立陶宛 (2)</summary>

- Telia `246-01` · 自動探索（盡力而為）
- Tele2 `246-03` · 自動探索（盡力而為）

</details>
<details><summary>盧森堡 (3)</summary>

- P&T Luxembourg `270-01` · 廠商資料設定 · 節點 v1.4.2 起
- Tango `270-77` · 廠商資料設定 · 節點 v1.4.2 起
- Orange `270-99` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>澳門 (1)</summary>

- Hutchison Telecom `455-03` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>馬來西亞 (12)</summary>

- Digi `502-10` · 自動探索（盡力而為）
- unifi mobile `502-11` · 自動探索（盡力而為）
- Maxis/Hotlink `502-12` · 廠商資料設定 · 節點 v1.4.2 起
- Celcom `502-13` · 自動探索（盡力而為）
- Digi `502-143` · 自動探索（盡力而為）
- Digi `502-146` · 自動探索（盡力而為）
- Yes 5G `502-152` · 廠商資料設定 · 節點 v1.4.2 起
- unifi mobile `502-153` · 廠商資料設定 · 節點 v1.4.2 起
- Digi `502-16` · 廠商資料設定 · 節點 v1.4.2 起
- TimeCel `502-17` · 自動探索（盡力而為）
- U Mobile `502-18` · 自動探索（盡力而為）
- Celcom `502-19` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>馬爾地夫 (1)</summary>

- Ooredoo `472-02` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>墨西哥 (4)</summary>

- Telcel `334-002` · 自動探索（盡力而為）
- Telcel `334-020` · 廠商資料設定 · 節點 v1.4.2 起
- AT&T MX `334-050` · 廠商資料設定 · 節點 v1.4.2 起
- AT&T MX `334-090` · 自動探索（盡力而為）

</details>
<details><summary>摩納哥 (1)</summary>

- MONACO TELECOM `212-10` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>蒙特內哥羅 (1)</summary>

- Crnogorski Telekom `297-02` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>摩洛哥 (2)</summary>

- Méditélécom `604-00` · 廠商資料設定 · 節點 v1.4.2 起
- Maroc `604-01` · 自動探索（盡力而為）

</details>
<details><summary>莫三比克 (1)</summary>

- Vodacom `643-04` · 自動探索（盡力而為）

</details>
<details><summary>尼泊爾 (1)</summary>

- Nepal Telecommunications `429-01` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>荷蘭 (5)</summary>

- Vodafone NL `204-04` · 已設定
- KPN Mobiel `204-08` · 廠商資料設定 · 節點 v1.4.2 起
- Odido `204-16` · 廠商資料設定 · 節點 v1.4.2 起
- Truphone `204-33` · 自動探索（盡力而為）
- KPN Lab `204-69` · 自動探索（盡力而為）

</details>
<details><summary>紐西蘭 (3)</summary>

- Vodafone `530-01` · 已設定
- Spark `530-05` · 已設定
- 2degrees `530-24` · 已設定

</details>
<details><summary>尼加拉瓜 (1)</summary>

- Claro NI `710-21` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>北馬其頓 (2)</summary>

- Makedonski Telekom `294-01` · 自動探索（盡力而為）
- Nov Operator `294-03` · 自動探索（盡力而為）

</details>
<details><summary>挪威 (4)</summary>

- Telenor `242-01` · 自動探索（盡力而為）
- Telia `242-02` · 廠商資料設定 · 節點 v1.4.2 起
- OneCall `242-05` · 廠商資料設定 · 節點 v1.4.2 起
- Ice Norway `242-14` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>阿曼 (1)</summary>

- Omantel `422-02` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>巴基斯坦 (3)</summary>

- Jazz `410-01` · 廠商資料設定 · 節點 v1.4.2 起
- Telenor `410-06` · 自動探索（盡力而為）
- Jazz `410-07` · 自動探索（盡力而為）

</details>
<details><summary>巴拿馬 (4)</summary>

- Cable & Wireless Panama S.A. `714-01` · 廠商資料設定 · 節點 v1.4.2 起
- Movistar `714-02` · 自動探索（盡力而為）
- Movistar `714-020` · 廠商資料設定 · 節點 v1.4.2 起
- Claro PA `714-03` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>巴拉圭 (1)</summary>

- Claro PY `744-02` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>秘魯 (3)</summary>

- Movistar `716-06` · 廠商資料設定 · 節點 v1.4.2 起
- Claro PE `716-10` · 廠商資料設定 · 節點 v1.4.2 起
- Entel `716-17` · 自動探索（盡力而為）

</details>
<details><summary>菲律賓 (3)</summary>

- Globe Telecom `515-02` · 廠商資料設定 · 節點 v1.4.2 起
- Smart Communications `515-03` · 廠商資料設定 · 節點 v1.4.2 起
- DITO `515-66` · 使用者回報可用 · 節點 v1.4.2 起

</details>
<details><summary>波蘭 (5)</summary>

- Plus `260-01` · 自動探索（盡力而為）
- T-Mobile `260-02` · 廠商資料設定 · 節點 v1.4.2 起
- Orange `260-03` · 廠商資料設定 · 節點 v1.4.2 起
- IDEA (UMTS)/PTK Centertel sp. Z.o.o. `260-05` · 自動探索（盡力而為）
- PLAY `260-06` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>葡萄牙 (6)</summary>

- NOS `268-03` · 廠商資料設定 · 節點 v1.4.2 起
- MEO `268-06` · 自動探索（盡力而為）
- NOS `268-93` · 自動探索（盡力而為）
- NOS `268-97` · 自動探索（盡力而為）
- NOS `268-98` · 自動探索（盡力而為）
- NOS `268-99` · 自動探索（盡力而為）

</details>
<details><summary>波多黎各 (1)</summary>

- Claro PR `330-110` · 自動探索（盡力而為）

</details>
<details><summary>卡達 (2)</summary>

- Ooredoo `427-01` · 自動探索（盡力而為）
- Vodafone `427-02` · 自動探索（盡力而為）

</details>
<details><summary>羅馬尼亞 (3)</summary>

- Vodafone `226-01` · 廠商資料設定 · 節點 v1.4.2 起
- Digi.Mobil `226-05` · 自動探索（盡力而為）
- Orange `226-10` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>俄羅斯 (6)</summary>

- MTS (МТС) `250-01` · 廠商資料設定 · 節點 v1.4.2 起
- MegaFon `250-02` · 廠商資料設定 · 節點 v1.4.2 起
- Orensot `250-11` · 自動探索（盡力而為）
- Tele2 `250-20` · 自動探索（盡力而為）
- Sberbank-Telecom `250-50` · 廠商資料設定 · 節點 v1.4.2 起
- Beeline (Билайн) `250-99` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>留尼旺 (2)</summary>

- Orange La Réunion `647-00` · 廠商資料設定 · 節點 v1.4.2 起
- Zeop `647-04` · 自動探索（盡力而為）

</details>
<details><summary>沙烏地阿拉伯 (4)</summary>

- STC `420-01` · 廠商資料設定 · 節點 v1.4.2 起
- Mobily `420-03` · 廠商資料設定 · 節點 v1.4.2 起
- Zain Saudi Arabia `420-04` · 廠商資料設定 · 節點 v1.4.2 起
- Virgin `420-05` · 自動探索（盡力而為）

</details>
<details><summary>塞內加爾 (1)</summary>

- Sentel GSM `608-02` · 自動探索（盡力而為）

</details>
<details><summary>塞爾維亞 (1)</summary>

- A1 SRB `220-05` · 自動探索（盡力而為）

</details>
<details><summary>新加坡 (5)</summary>

- Singtel `525-01` · 廠商資料設定 · 節點 v1.4.2 起
- Singtel `525-02` · 自動探索（盡力而為）
- M1 `525-03` · 廠商資料設定 · 節點 v1.4.2 起
- StarHub `525-05` · 廠商資料設定 · 節點 v1.4.2 起
- SIMBA `525-10` · 自動探索（盡力而為）

</details>
<details><summary>斯洛伐克 (4)</summary>

- Orange `231-01` · 廠商資料設定 · 節點 v1.4.2 起
- Slovak Telekom `231-02` · 自動探索（盡力而為）
- 4ka SK `231-03` · 廠商資料設定 · 節點 v1.4.2 起
- O2 `231-06` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>斯洛維尼亞 (1)</summary>

- Telekom Slovenije `293-41` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>南非 (4)</summary>

- Vodacom `655-01` · 廠商資料設定 · 節點 v1.4.2 起
- Telkom `655-02` · 廠商資料設定 · 節點 v1.4.2 起
- Cell C (Pty) Ltd. `655-07` · 廠商資料設定 · 節點 v1.4.2 起
- MTN `655-10` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>西班牙 (7)</summary>

- Vodafone `214-01` · 廠商資料設定 · 節點 v1.4.2 起
- Orange `214-03` · 廠商資料設定 · 節點 v1.4.2 起
- Movistar `214-05` · 自動探索（盡力而為）
- Euskaltel `214-06` · 自動探索（盡力而為）
- Movistar `214-07` · 廠商資料設定 · 節點 v1.4.2 起
- Digi Spain `214-22` · 廠商資料設定 · 節點 v1.4.2 起
- Truphone `214-27` · 自動探索（盡力而為）

</details>
<details><summary>斯里蘭卡 (2)</summary>

- Sri Lanka Telecom Mobitel `413-01` · 自動探索（盡力而為）
- Dialog Sri Lanka `413-02` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>蘇丹 (1)</summary>

- Sudani `634-07` · 自動探索（盡力而為）

</details>
<details><summary>瑞典 (4)</summary>

- Telia Sverige AB `240-01` · 廠商資料設定 · 節點 v1.4.2 起
- 3 (Hi3G Access AB) `240-02` · 廠商資料設定 · 節點 v1.4.2 起
- Tele2/Comviq Sverige/Com Hem `240-07` · 廠商資料設定 · 節點 v1.4.2 起
- Telenor Sverige AB `240-08` · 自動探索（盡力而為）

</details>
<details><summary>瑞士 (5)</summary>

- Swisscom `228-01` · 廠商資料設定 · 節點 v1.4.2 起
- Sunrise `228-02` · 已設定
- Salt `228-03` · 廠商資料設定 · 節點 v1.4.2 起
- Sunrise `228-12` · 已設定
- UPC `228-53` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>台灣 (7)</summary>

- Far EasTone 遠傳電信 `466-01` · 使用者回報可用 · 節點 v1.4.2 起
- Far EasTone 遠傳電信 `466-02` · 自動探索（盡力而為）
- Far EasTone 遠傳電信 `466-03` · 自動探索（盡力而為）
- Far EasTone 遠傳電信 (former Asia Pacific Telecom) `466-05` · 廠商資料設定 · 節點 v1.4.2 起
- Chunghwa Telecom 中華電信 `466-92` · 使用者回報可用 · 節點 v1.4.2 起
- Taiwan Mobile 台灣大哥大 `466-97` · 使用者回報可用 · 節點 v1.4.2 起
- TransAsia Telecoms `466-99` · 自動探索（盡力而為）

</details>
<details><summary>泰國 (6)</summary>

- CAT CDMA `520-00` · 廠商資料設定 · 節點 v1.4.2 起
- AIS `520-01` · 使用者回報可用 · 節點 v1.4.2 起
- AIS `520-03` · 使用者回報可用 · 節點 v1.4.2 起
- TrueMove H 4G LTE `520-04` · 廠商資料設定 · 節點 v1.4.2 起
- dtac `520-05` · 自動探索（盡力而為）
- dtac `520-18` · 自動探索（盡力而為）

</details>
<details><summary>土耳其 (4)</summary>

- Turkcell `286-01` · 廠商資料設定 · 節點 v1.4.2 起
- Vodafone `286-02` · 廠商資料設定 · 節點 v1.4.2 起
- Türk Telekom `286-03` · 廠商資料設定 · 節點 v1.4.2 起
- Türk Telekom `286-04` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>烏干達 (1)</summary>

- Airtel Uganda `641-01` · 自動探索（盡力而為）

</details>
<details><summary>烏克蘭 (1)</summary>

- Astelit `255-06` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>阿聯 (2)</summary>

- e& UAE `424-02` · 廠商資料設定 · 節點 v1.4.2 起
- du `424-03` · 自動探索（盡力而為）

</details>
<details><summary>英國 (22)</summary>

- Lebara UK `204-04` · 使用者回報可用 · 節點 v1.4.2 起
- Mapesbury Communications Ltd. `234-01` · 自動探索（盡力而為）
- FMS Solutions Ltd `234-04` · 自動探索（盡力而為）
- BT One Phone `234-08` · 自動探索（盡力而為）
- giffgaff (O2) `234-10` · 已實測
- O2 `234-10` · 已設定
- Vodafone UK `234-15` · 已實測
- Three Mobile `234-20` · 已設定
- Truphone `234-25` · 自動探索（盡力而為）
- EE `234-30` · 已設定
- EE `234-31` · 已設定
- EE `234-32` · 已設定
- CMLink UK (EE) `234-33` · 已實測
- CTExcel (EE) `234-33` · 已實測
- EE `234-33` · 已設定
- Virgin `234-38` · 自動探索（盡力而為）
- Spusu GB `234-40` · 廠商資料設定 · 節點 v1.4.2 起
- Cable and Wireless Guernsey `234-55` · 自動探索（盡力而為）
- Sky `234-57` · 廠商資料設定 · 節點 v1.4.2 起
- EE `234-86` · 已設定
- Three Mobile `234-94` · 已設定
- Three Mobile `235-94` · 已設定

</details>
<details><summary>美國 (63)</summary>

- T-Mobile - US `310-026` · 已設定
- AT&T `310-030` · 已設定
- AT&T `310-070` · 已設定
- AT&T Private `310-090` · 自動探索（盡力而為）
- Pacific Telecom Inc `310-110` · 自動探索（盡力而為）
- Sprint `310-120` · 廠商資料設定 · 節點 v1.4.2 起
- Cricket Wireless `310-150` · 廠商資料設定 · 節點 v1.4.2 起
- T-Mobile - US `310-160` · 已設定
- AT&T `310-170` · 已設定
- T-Mobile - US `310-200` · 已設定
- T-Mobile - US `310-210` · 已設定
- T-Mobile - US `310-220` · 已設定
- T-Mobile - US `310-230` · 已設定
- T-Mobile - US `310-240` · 已設定
- T-Mobile - US `310-250` · 已設定
- T-Mobile - US `310-260` · 已設定
- T-Mobile - US `310-270` · 已設定
- Red Pocket (AT&T) `310-280` · 已實測
- AT&T `310-280` · 已設定
- T-Mobile - US `310-300` · 已設定
- T-Mobile - US `310-310` · 已設定
- AT&T `310-380` · 已設定
- AT&T `310-410` · 已設定
- North East Cellular Inc. `310-450` · 自動探索（盡力而為）
- T-Mobile - US `310-490` · 已設定
- T-Mobile - US `310-530` · 已設定
- AT&T `310-560` · 已設定
- Verizon Wireless `310-590` · 自動探索（盡力而為）
- T-Mobile - US `310-640` · 已設定
- Jasper Wireless Inc. `310-650` · 自動探索（盡力而為）
- T-Mobile - US `310-660` · 已設定
- AT&T `310-680` · 已設定
- Sea Mobile `310-730` · 自動探索（盡力而為）
- T-Mobile - US `310-800` · 已設定
- Verizon Wireless `310-890` · 自動探索（盡力而為）
- AT&T `310-950` · 已設定
- AT&T `311-180` · 已設定
- U.S. Cellular `311-220` · 自動探索（盡力而為）
- C Spire `311-230` · 自動探索（盡力而為）
- Verizon Wireless `311-270` · 自動探索（盡力而為）
- GCI Communications Corp. `311-370` · 自動探索（盡力而為）
- Verizon Wireless `311-480` · 廠商資料設定 · 節點 v1.4.2 起
- Verizon Wireless `311-481` · 自動探索（盡力而為）
- T-Mobile - US `311-490` · 已設定
- U.S. Cellular `311-580` · 廠商資料設定 · 節點 v1.4.2 起
- U.S. Cellular `311-589` · 自動探索（盡力而為）
- United Wireless `311-650` · 自動探索（盡力而為）
- T-Mobile - Private 5G `311-660` · 廠商資料設定 · 節點 v1.4.2 起
- Cellcom `311-840` · 廠商資料設定 · 節點 v1.4.2 起
- T-Mobile - US `311-882` · 已設定
- T-Mobile - US `312-250` · 已設定
- strata `312-290` · 自動探索（盡力而為）
- NexTech Ota `312-420` · 自動探索（盡力而為）
- Sprint `312-530` · 廠商資料設定 · 節點 v1.4.2 起
- FirstNet (Lab) `312-670` · 廠商資料設定 · 節點 v1.4.2 起
- FirstNet `313-100` · 廠商資料設定 · 節點 v1.4.2 起
- FirstNet `313-110` · 自動探索（盡力而為）
- FirstNet `313-120` · 自動探索（盡力而為）
- FirstNet `313-130` · 自動探索（盡力而為）
- FirstNet `313-140` · 自動探索（盡力而為）
- Dish Wireless (Boost) `313-340` · 廠商資料設定 · 節點 v1.4.2 起
- Optimum `313-390` · 廠商資料設定 · 節點 v1.4.2 起
- Liberty Mobile `313-790` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>烏拉圭 (1)</summary>

- Claro UY `748-10` · 廠商資料設定 · 節點 v1.4.2 起

</details>
<details><summary>越南 (4)</summary>

- Mobifone `452-01` · 廠商資料設定 · 節點 v1.4.2 起
- Vinaphone `452-02` · 廠商資料設定 · 節點 v1.4.2 起
- Viettel Telecom `452-04` · 廠商資料設定 · 節點 v1.4.2 起
- Vietnamobile `452-05` · 自動探索（盡力而為）

</details>
<details><summary>尚比亞 (2)</summary>

- Airtel Zambia `645-01` · 自動探索（盡力而為）
- Telecel Zambia Ltd. `645-02` · 自動探索（盡力而為）

</details>

<!-- CARRIERS:END -->

## 下載與驗證

自 v1.4.0 起，每個版本都發布在 **[Releases](https://github.com/vonode/vonode-releases/releases)** 頁面。
請把下面的 `VERSION` 設為該頁面上顯示的版本標籤（檔名使用同一標籤）。

| 檔案 | 說明 |
|---|---|
| `vonode_<version>_linux_<arch>_commercial.tar.gz` | 安裝套件：節點程式、`install.sh`、systemd 單元、`docker/` 部署檔案、指南和授權條款，以及 `SHA256SUMS` 和涵蓋整個安裝套件的發行簽章 `SHA256SUMS.sig` |
| `vonode_<version>_linux_<arch>_commercial.tar.gz.sha256` | 安裝套件的 SHA-256 檢查碼 |
| `vonode_<version>_linux_<arch>_commercial` 和 `.sig` | 單一檔案形式的節點程式及其發行簽章，供 App 內軟體更新使用 |
| `vonode_<version>_linux_<arch>_commercial.THIRD_PARTY_NOTICES.txt` | 第三方元件及其授權條款 |
| `vonode_<version>_linux_<arch>_commercial.spdx.json` | 軟體物料清單（SPDX SBOM） |
| `vonode_<version>_linux_<arch>_commercial.sources.tar` | GPL 和 LGPL 元件的對應原始碼 |
| `latest-commercial.json` 和 `latest-commercial.json.sig` | 已簽章的發行清單（所有架構共用一份），節點和 Docker Compose 一鍵安裝指令碼據此取得版本，以及它的發行簽章 |
| `docker-compose.yml`、`docker-compose.yml.sha256` 和 `docker-compose.yml.sig` | 本版本的 Docker Compose 檔案（以 digest 固定映像檔）、檢查碼，以及（自 v1.4.2 起）以發行金鑰所做的簽章 |
| `compose-install.sh` 和 `compose-install.sh.sha256` | [Docker Compose 一鍵安裝](#docker-compose-一鍵安裝)指令碼及其檢查碼 |
| `vonode-<version>-nas.tar.gz` 及 `.sha256` | 供 [NAS](#nas群暉威聯通unraidtruenas) 容器應用程式使用的現成檔案（自 v1.4.2 起）及其檢查碼 |
| `vonode-<version>-docker-alpine-sources.tar` 及 `.sha256` | Docker 映像檔內 GPL、LGPL 和 MPL 套件的對應原始碼 |

`<arch>` 在 64 位元 x86 上是 `amd64`（`uname -m` 輸出 `x86_64`），自 v1.4.2 起在 64 位元 ARM 上是 `arm64`
（`uname -m` 輸出 `aarch64`）。下方指令以 `amd64` 為例；在 arm64 主機上請改成 `arm64`。

### 核對 SHA-256

```sh
VERSION=vX.Y.Z        # 換成版本標籤
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
# 單一檔案下載時，改用 vonode_<version>_linux_<arch>_commercial 和對應的 .sig。
BIN=vonode_${VERSION}_linux_amd64_commercial/vonode
printf 'vonode-binary-v1\n%s' "$(sha256sum "$BIN" | cut -d' ' -f1)" > vonode.msg
base64 -d "$BIN.sig" > vonode.sig.bin
openssl pkeyutl -verify -pubin -inkey vonode-update-key.pem -rawin -in vonode.msg -sigfile vonode.sig.bin
```

最後一道指令必須輸出 `Signature Verified Successfully`。每個安裝套件裡還有 `SHA256SUMS` 和
`SHA256SUMS.sig`，後者是以發行金鑰對套件內所有檔案檢查碼所做的簽章。

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
VERSION=vX.Y.Z        # 換成版本標籤
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
VPN 或區域網路位址。如果路由器轉送的是其他外部連接埠，請把 `-port` 後面換成那個連接埠。

```sh
sudo /opt/vonode/vonode pair -c /opt/vonode/config/config.yaml -host node.example.com -port 2222
```

終端機會以字元區塊圖形顯示 QR Code。它 5 分鐘內有效，只能使用一次；需要新的 QR Code 時再執行一次這道指令。
接下來請看 [配對 App](#配對-app)。

### 日常維運

| 操作 | 指令 |
|---|---|
| 查看狀態 | `systemctl status vonode vonode-gateway` |
| 查看記錄 | `journalctl -u vonode -f` |
| 重新啟動 | `sudo systemctl restart vonode`（最多需要兩分鐘：節點會先取消註冊 Wi-Fi 通話並關閉通道） |
| 新的配對碼 | `sudo /opt/vonode/vonode pair -c /opt/vonode/config/config.yaml -host <位址> -port 2222` |
| 忘記管理員密碼 | `sudo systemctl stop vonode`，接著 `sudo /opt/vonode/vonode reset-password -c /opt/vonode/config/config.yaml`，再 `sudo systemctl start vonode`。所有手機都會登出，需要重新配對。 |

## 用 Docker Compose 安裝

Docker Compose 方式以 Docker Hub 上的映像檔 `vonode/vonode`（自 v1.4.0 起發布）
執行兩個容器，節點功能與 systemd 方式相同：`vonode`（核心，負責驅動模組）和 `vonode-gateway`（無特權的
SSH 服務，App 透過 TCP 2222 連線到它）。配對、方案和 App 的用法與 systemd 安裝套件完全相同。映像檔標籤與
版本對應（`v1.4.0` 對應 `1.4.0`，另有 `latest`）。自 v1.4.2 起映像檔為多平台映像檔，適用於 64 位元 x86（amd64）
和 64 位元 ARM（arm64）主機，Docker 會自動選擇合適的版本。每個版本也會發布一個以 digest 固定映像檔的
`docker-compose.yml`，自 v1.4.2 起以 Vonode 發行金鑰簽章。

一台主機只用一種安裝方式：不要在已安裝 systemd 安裝套件的主機上再執行 Docker 方式。

### Docker Compose 一鍵安裝

在裝好 Docker Engine 和 Compose 外掛的 Linux 主機上（見下方第 1 步），一道指令即可下載並驗證對應版本的
`docker-compose.yml`，安裝到 `/opt/vonode-compose`，啟動兩個容器，等它們都 healthy 後顯示配對 QR Code：

```sh
curl -fsSL https://vonode.cc/install/compose.sh | sudo sh
```

- 只下載並驗證、不變更任何東西：
  `curl -fsSL https://vonode.cc/install/compose.sh | sh -s -- --dry-run`
- 選項：`--host <手機連到節點的位址>`、`--port <連接埠>`、`--dir <目錄>`、`--version vX.Y.Z`、`--no-start`、
  `--no-pair`。再次執行同一道指令即可更新。
- 這個指令碼也是發行資產 `compose-install.sh`（逐位元組相同，SHA-256 為
  `ca1740be6b659dc44335775b712a9409f16a5592a5e00b8ba2bd8303540a4232`，也寫在 `compose-install.sh.sha256` 中）。也可以先下載、閱讀，再用
  `sudo sh compose-install.sh` 執行，不經管線。
- 驗證方式：指令碼從已簽章的發行清單 `latest-commercial.json` 中找到目前版本 v1.4.2，在使用
  `docker-compose.yml` 之前以 Vonode 發行金鑰驗證它的簽章 `docker-compose.yml.sig`。映像檔始終以 digest 固定。
- 同一道指令適用於 amd64 和 arm64 主機；在 arm64 主機上，它現在會安裝正式版 v1.4.2（不再是先前實驗性的 arm64
  預先發行版本）。
- 它不會停用 ModemManager，也不會變更其他服務；ModemManager 正在執行時，它會印出停用它的指令。不需要帳號，
  不傳送任何資料。

下方第 1 到第 4 步是同樣操作的手動版本。

### 1. 準備主機

Linux、模組和 SIM 卡的需求與上文相同。macOS 或 Windows 上的 Docker Desktop 無法存取 USB 模組，請在 Linux
上使用 Docker Engine。停用 ModemManager，確認 Linux 能辨識模組，然後安裝含 Compose 外掛程式的 Docker
Engine。Docker 官方的便利指令碼適用於 Debian、Ubuntu 和 Fedora；最後一道指令必須輸出 Compose v2 的版本號碼。

```sh
sudo systemctl disable --now ModemManager
lsusb | grep -i -E 'quectel|2c7c|qualcomm|05c6'
ls /dev/ttyUSB* /dev/cdc-wdm*
curl -fsSL https://get.docker.com | sh
sudo systemctl enable --now docker
docker compose version
```

模組的序列埠和 QMI 介面由主機核心的 `option` 和 `qmi_wwan` 驅動程式提供，插入模組時會自動載入。如果
`lsusb` 能看到模組但沒有 `/dev/ttyUSB*`，請執行 `sudo modprobe -a option qmi_wwan`，然後重新插拔模組。

**裝置存取。** `vonode` 容器使用主機網路、以特權模式執行，並能看到主機的整個 `/dev`：它要驅動 USB 序列埠和
QMI 介面、為 Wi-Fi 通話建立 TUN/XFRM 介面，並用 ALSA 處理通話音訊。綁定整個 `/dev` 後，之後插入的模組、
或重設後以其他 `/dev/ttyUSB*` 編號重新出現的模組，不必重新啟動即可辨識。`vonode-gateway` 容器以 UID 10001
執行，檔案系統唯讀、不帶任何 capability，只對外發布 TCP 2222。

### 2. 下載 Compose 檔案

```sh
mkdir -p ~/vonode && cd ~/vonode
VERSION=vX.Y.Z        # 換成版本標籤
BASE=https://github.com/vonode/vonode-releases/releases/download/$VERSION
curl -fLO $BASE/docker-compose.yml
curl -fLO $BASE/docker-compose.yml.sha256
sha256sum -c docker-compose.yml.sha256
```

選用設定寫在同一目錄的 `.env` 檔案中，例如以 `TZ=Asia/Taipei` 設定節點時區（預設為 UTC），或以
`VONODE_SSH_PORT=2022` 把 SSH 服務發布到主機的其他連接埠。

### 3. 啟動節點

```sh
sudo docker compose up -d
sudo docker compose ps
```

Docker 會下載固定的映像檔並啟動兩個容器；大約半分鐘後兩個容器都會顯示 `healthy`。節點把設定、資料庫和記錄
保存在 `docker-compose.yml` 旁邊的 `config/`、`data/` 和 `logs/` 中。首次啟動時會產生隨機管理員密碼，
明文保存在 `config/initial-admin-password`，只有 root 可讀取。

### 4. 開放連接埠並顯示配對 QR Code

在主機防火牆上允許 TCP 2222；如果手機要從外部網路連線，請在路由器上轉送該連接埠（或使用 VPN）。然後以手機
連到節點時使用的位址和連接埠顯示配對 QR Code：

```sh
sudo ufw allow 2222/tcp
sudo docker compose exec vonode /app/vonode pair -c /app/config/config.yaml -host node.example.com -port 2222
```

QR Code 5 分鐘內有效，只能使用一次。接下來請看 [配對 App](#配對-app)。

### Docker 方式的日常維運

以下指令在 `docker-compose.yml` 所在目錄下執行。

| 操作 | 指令 |
|---|---|
| 查看狀態（兩個服務都應為 healthy） | `sudo docker compose ps` |
| 查看記錄 | `sudo docker compose logs -f vonode` |
| 新的配對碼 | `sudo docker compose exec vonode /app/vonode pair -c /app/config/config.yaml -host <位址> -port 2222` |
| 重新啟動（最多兩分鐘） | `sudo docker compose restart vonode` |
| 停止並保留資料 | `sudo docker compose down` |
| 再次啟動 | `sudo docker compose up -d` |
| 忘記管理員密碼 | `sudo docker compose stop vonode`、`sudo docker compose run --rm --no-deps vonode reset-password -c /app/config/config.yaml`、`sudo docker compose start vonode` |

> [!WARNING]
> 除非要刪除節點，否則請勿執行 `docker compose down -v`。`vonode_gateway-private` 磁碟區保存著已配對手機所
> 固定的 SSH 主機金鑰；它遺失後，所有手機都必須重新配對。

### 另一種方式：安裝指令碼

安裝套件中還有一個 `docker/` 目錄，其中的 `setup.sh` 會檢查主機、列出找到的模組、寫入 `.env` 和裝置覆寫
檔案、啟動兩個容器並顯示配對 QR Code。依 [下載與驗證](#下載與驗證) 下載並解壓縮安裝套件，然後執行：

```sh
cd vonode_${VERSION}_linux_amd64_commercial/docker
sudo ./setup.sh --host node.example.com --image vonode/vonode@sha256:<digest>
```

發行說明中提供 `vonode/vonode@sha256:<digest>` 形式的映像檔。`--devices` 只對應目前存在的模組裝置節點，
而不是整個 `/dev`；`./setup.sh --help` 會說明每個選項。使用這種模式時，新增或移動模組後請重新執行
`sudo ./setup.sh`；`sudo ./setup.sh --uninstall` 會停止節點並保留資料。

## NAS（群暉、威聯通、Unraid、TrueNAS）

> [!WARNING]
> **尚未在實際 NAS 硬體上測試。** 這些檔案依照各廠商公開的文件撰寫，只做過離線檢查；還沒有人在群暉（Synology）、
> 威聯通（QNAP）、Unraid 或 TrueNAS 裝置上實際執行過。歡迎將使用結果寄到 support@vonode.cc。可以的話，建議優先
> 使用一台小型 Linux 主機搭配 systemd 安裝套件或 [Docker Compose 一鍵安裝](#docker-compose-一鍵安裝)。

自 v1.4.2 起，每個版本都提供一個 NAS 套件，以 NAS 內建的容器應用程式執行同樣的兩個容器，不需要 `setup.sh`。從
[v1.4.2 版本頁面](https://github.com/vonode/vonode-releases/releases/tag/v1.4.2)下載 `vonode-1.4.2-nas.tar.gz`
及其 `.sha256`，以 `sha256sum -c vonode-1.4.2-nas.tar.gz.sha256` 驗證後解壓縮。每個目錄裡都有一份逐步說明 `README.md`。

| 目錄 | NAS 軟體 | 安裝方式 |
|---|---|---|
| `synology/` | DSM 7.2 以上版本，Container Manager | Container Manager › 專案 › 新增，貼上 `docker-compose.yml` |
| `qnap/` | QTS / QuTS hero 5，Container Station 3 | Container Station › 應用程式 › 建立，貼上 `docker-compose.yml` |
| `unraid/` | Unraid 6.12 以上版本，已啟用 Docker | 將 `vonode.xml` 和 `vonode-gateway.xml` 複製到使用者範本目錄，再新增這兩個容器 |
| `truenas/` | TrueNAS SCALE 24.10（Electric Eel）以上版本 | Apps › Discover Apps › ⋮ › Install via YAML，貼上 `docker-compose.yml` |

每個檔案都固定到該版本映像檔的 digest，與發行說明和 `docker-compose.yml` 中的相同。映像檔為多平台（amd64 和
arm64），Docker 會自動選擇；不支援 32 位元 ARM 的 NAS 機型。

- **主機網路和特權核心容器：** `vonode` 容器需要 `network_mode: host` 和 `privileged: true`（用於 Wi-Fi 通話通道和
  USB 模組）；`vonode-gateway` 始終無特權，只發布 TCP 2222。
- **裝置：** 核心容器取得 NAS 的整個 `/dev`，其中包括 `/dev/bus/usb` 和 `/dev/net/tun`。如果 NAS 不允許掛載
  `/dev`，請依 NAS 套件中 README 的範例逐一列出裝置。
- **ModemManager 不能佔用模組：** 原廠 DSM、QTS 和 Unraid 沒有 ModemManager；在 TrueNAS SCALE 上，如果它正在執行，
  請執行 `sudo systemctl disable --now ModemManager`。
- **核心驅動程式：** 容器無法載入驅動程式。模組需要 NAS 核心內有 `option`、`qmi_wwan` 和 `cdc_wdm`。原廠群暉 DSM
  的核心通常沒有這些驅動程式（DSM 7 已不再支援 USB 3G/4G 網卡），因此在原廠 DSM 上模組方式很可能無法使用；威聯通
  請依機型檢查。USB 讀卡機搭配你的 SIM 卡只需要 `/dev/bus/usb`，在這類 NAS 上較可行（僅 Wi-Fi 通話；讀卡機線路
  需要訂閱或 Vonode 終身版，免費方案無法使用）。
- 刪除或重建應用程式時，請勿刪除具名磁碟區：`vonode_gateway-private` 保存著已配對手機所固定的 SSH 主機金鑰。

透過 SSH 配對：`sudo docker exec -it vonode /app/vonode pair -c /app/config/config.yaml -host <NAS 位址> -port 2222`。
升級時請使用新版本的 NAS 套件。

## 配對 App

1. 從 App Store 安裝 **Vonode**（免費下載）並開啟。
2. 在「連接你的節點」頁面點一下掃描框，把相機對準終端機上的 QR Code。
3. App 透過 SSH 連線到節點，固定節點的主機金鑰，並在首頁顯示這個節點和它的號碼。

App 和節點必須能透過 QR Code 裡的位址互相連線。如果 App 顯示連線錯誤，請先檢查連接埠轉送或 VPN，再產生
新的配對碼。之後要配對另一支手機，請在 App 中使用「換綁到新手機」，它會顯示一次性 QR Code。

## 免費方案、訂閱或終身版

未訂閱也未購買終身版時，節點以**免費方案**執行：開啟「號碼」標籤頁，選擇一個你想查看簡訊的號碼。選定後在這個節點上不能
更換，還原備份也會保留它。之後「簡訊」標籤頁會顯示這個號碼收到的簡訊。

其他功能（電話、Wi-Fi 通話、傳簡訊、通知、eSIM 設定檔、代理、自動任務、更多號碼）需要這個節點的 Vonode 訂閱或 Vonode
終身版（在 App 內購買）：開啟「更多 › 訂閱」。每份訂閱或終身版解鎖一個節點；你可以在 App 中把它移轉到另一個節點，30 天內最多 3 次，在新硬體上
重裝節點軟體或還原備份也算一次移轉。節點幾秒內就會解鎖並以完整模式重新啟動；之後用
「新增模組」加入模組，並為每張 SIM 卡開啟 Wi-Fi 通話。

硬體、SIM 資費方案、電信費用和主機代管不包含在內。

## 升級

- **在 App 中：** 以 systemd 安裝套件安裝的節點會在「更多 › 軟體更新」中提供已簽章的更新。節點在安裝前會驗證更新的簽章。
- **systemd 安裝套件：** 下載並驗證新的安裝套件，解壓縮後在最上層目錄執行 `sudo ./install.sh`。設定、
  資料庫和 SSH 主機金鑰都會保留。
- **Docker Compose：** 用[一鍵安裝](#docker-compose-一鍵安裝)裝的節點，再執行一次那道指令即可。手動安裝時，把新版本的 `docker-compose.yml` 和 `docker-compose.yml.sha256` 下載到同一目錄並驗證，
  然後執行 `sudo docker compose pull` 和 `sudo docker compose up -d`。設定、資料庫和 SSH 主機金鑰都會保留，
  兩個容器一律使用同一個映像檔。App 內更新不會替換容器裡的程式，請改為更新映像檔。使用安裝指令碼時，執行
  `sudo ./setup.sh --image vonode/vonode@sha256:<digest>` 並傳入新的 digest。

升級前請先備份：App 中的「更多 › 備份與還原」。備份包含簡訊、通話紀錄和節點的訂閱或終身版綁定，請妥善保管。移轉到
新硬體時，在舊節點上建立並下載備份，安裝並配對新節點，再匯入並還原備份；這計入 30 天內 3 次移轉。不要讓舊節點繼續用同一份資料執行。

## 解除安裝

systemd 安裝套件（會刪除你的資料和備份）：

```sh
sudo systemctl disable --now vonode-gateway vonode
sudo rm /etc/systemd/system/vonode.service /etc/systemd/system/vonode-gateway.service /etc/tmpfiles.d/vonode.conf
sudo systemctl daemon-reload
sudo rm -rf /opt/vonode /var/lib/vonode-gateway
```

Docker Compose（在 `docker-compose.yml` 所在目錄）：`sudo docker compose down` 會停止節點並保留全部資料。要刪除
節點及其資料，請執行 `sudo docker compose down -v`，再執行 `sudo rm -rf config data logs`。使用安裝指令碼時，
`sudo ./setup.sh --uninstall` 會保留資料，`--uninstall --purge` 會刪除資料。

## 疑難排解

| 問題 | 處理方式 |
|---|---|
| ModemManager 正在執行 | `sudo systemctl disable --now ModemManager`，然後 `sudo systemctl restart vonode`（Docker 方式改為執行 `sudo docker compose restart vonode`）。 |
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

Docker 映像檔也包含採用 GPL、LGPL 和 MPL 授權條款的 Alpine Linux 套件；它們的對應原始碼以發行資產
`vonode-<version>-docker-alpine-sources.tar` 提供，條件相同（至少三年；實體副本可向 support@vonode.cc 索取）。

## 支援與法律資訊

- 官網：<https://vonode.cc/zh-hant/>
- 安裝指南：<https://vonode.cc/zh-hant/install/> · [逐步指南](https://vonode.cc/zh-hant/install/guide/)
- 支援的硬體：<https://vonode.cc/zh-hant/hardware/>
- 支援：[support@vonode.cc](mailto:support@vonode.cc) · <https://vonode.cc/zh-hant/support/>
- Telegram：[交流群](https://t.me/vonode_group) · [更新公告頻道](https://t.me/vonode_app)
- [隱私權政策](https://vonode.cc/zh-hant/privacy/) · [使用條款](https://vonode.cc/zh-hant/terms/) ·
  [第三方聲明](https://vonode.cc/zh-hant/third-party-notices/)

Vonode 不是緊急通訊服務。它仰賴你的硬體、電信業者、網路和 Apple 服務；請勿依賴它進行緊急求救或其他攸關
生命安全的通訊。請只在你有權使用的 SIM 卡、號碼和網路上使用，並遵守電信業者的條款。

DJI 是 SZ DJI Technology Co., Ltd. 的商標；Quectel 是 Quectel Wireless Solutions Co., Ltd. 的商標；Apple、iPhone、iPad
和 App Store 是 Apple Inc. 的商標；Docker 是 Docker, Inc. 的商標。Vonode 與它們沒有關聯，也未獲其認可。

© 2026 VONODE LLC。Vonode 是 VONODE LLC 的產品。
