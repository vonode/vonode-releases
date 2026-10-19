<p align="right"><b>English</b> · <a href="README.zh-Hans.md">简体中文</a> · <a href="README.zh-Hant.md">繁體中文</a></p>

<p align="center">
  <img src="assets/logo.png" width="112" height="112" alt="Vonode logo">
</p>

<h1 align="center">Vonode node</h1>

<p align="center">
  <b>Keep your SIMs at home. Take your numbers anywhere.</b><br>
  Release downloads, install guides and wiki for the Vonode node software.
</p>

## One-click install

On 64-bit Linux (amd64 or arm64) with Docker installed, one command installs the Vonode node with Docker Compose.
Run the same command again to upgrade.

```bash
curl -fsSL https://vonode.cc/install/compose.sh | sudo sh
```

Options, a dry run and how the script verifies the release: [Docker Compose one-click](#docker-compose-one-click).
Other ways to install: [systemd package (recommended)](#install-with-the-systemd-package-recommended) ·
[Docker Compose step by step](#install-with-docker-compose) · [install guide on vonode.cc](https://vonode.cc/install/)

<p align="center">
  <a href="https://github.com/vonode/vonode-releases/releases/latest"><img alt="Latest release" src="https://img.shields.io/github/v/release/vonode/vonode-releases?style=flat-square&label=latest%20release&color=2a9468"></a>
  <a href="https://vonode.cc/install/"><img alt="Install guide" src="https://img.shields.io/badge/docs-install%20guide-2475c2?style=flat-square"></a>
  <img alt="Platform" src="https://img.shields.io/badge/node-Linux%20amd64%20%7C%20arm64-141b26?style=flat-square">
  <img alt="App" src="https://img.shields.io/badge/app-iOS%20%26%20iPadOS%2017%2B-141b26?style=flat-square">
  <a href="https://t.me/vonode_group"><img alt="Telegram" src="https://img.shields.io/badge/telegram-%40vonode__group-26a5e4?style=flat-square&logo=telegram&logoColor=white"></a>
</p>

Vonode turns a Linux computer with cellular modules into a private node. The Vonode app for
iPhone and iPad reads and sends SMS, makes and answers calls, switches eSIM profiles and turns
on Wi-Fi calling, all through hardware you run yourself.

This repository holds **release files and documentation only**. It contains no source code.
The node software is proprietary software of VONODE LLC.

Version v1.3.0 was briefly published and then withdrawn; it is not supported. Install v1.4.0 or later.

## Contents

- [One-click install](#one-click-install)
- [Features](#features)
- [Requirements](#requirements)
- [Supported carriers (Wi-Fi Calling)](#supported-carriers-wi-fi-calling)
- [Download and verify](#download-and-verify)
- [Install with the systemd package (recommended)](#install-with-the-systemd-package-recommended)
- [Install with Docker Compose](#install-with-docker-compose) · [one-click](#docker-compose-one-click)
- [NAS (Synology, QNAP, Unraid, TrueNAS)](#nas-synology-qnap-unraid-truenas)
- [Pair the app](#pair-the-app)
- [Free plan, subscription or Lifetime](#free-plan-subscription-or-lifetime)
- [Upgrade](#upgrade)
- [Uninstall](#uninstall)
- [Troubleshooting](#troubleshooting)
- [Source code of GPL and LGPL components](#source-code-of-gpl-and-lgpl-components)
- [Support and legal](#support-and-legal)

## Features

| Feature | What it does |
|---|---|
| **Messages** | Read and send SMS for every number on the node. Junk is sorted automatically, and a reply goes out from the number that received the thread. |
| **Calls** | Make and answer calls through your node over Wi-Fi calling. Incoming calls ring on your iPhone through CallKit, even when the app is in the background. |
| **Wi-Fi Calling** | Turn Wi-Fi calling (VoWiFi) on per SIM; calls run over Wi-Fi calling. The node reaches your carrier over the internet; your plan must support it. |
| **SIM and eSIM** | Switch eSIM profiles on a removable eUICC card; downloading, renaming and deleting are in the app but not yet verified on hardware. Physical SIMs work as they are. |
| **Check network** | A plain-language check of SIM, carrier, mobile data, SMS and calling service, with technical details one tap away. |
| **Notifications** | SMS and call alerts through Apple push. Optional message previews are encrypted by your node and decrypted only on your phone. |
| **And the rest** | Carrier codes (USSD), automations, proxies, backups and moving to a new phone, all in the same app. |

There is no Vonode cloud between you and your numbers. The app talks to your node directly over
encrypted SSH and pins the node's host key. Messages, call history and settings stay on the node you operate.
A small relay run by VONODE LLC delivers Apple notifications and confirms subscriptions; it does not
store plaintext message content, and it sees only notification metadata such as the type and time of a notification. Details are in the [Privacy Policy](https://vonode.cc/privacy/).

The node has no web interface: install it once, pair it with the app, and manage everything from
the app.

## Requirements

Check the full list on the **[hardware page](https://vonode.cc/hardware/)** before you buy anything.

| Item | Requirement |
|---|---|
| **Host computer** | 64-bit x86 (amd64) or 64-bit ARM (arm64) Linux: Debian 12, Ubuntu 22.04 or later, or Fedora, with systemd for the install package or Docker Engine with Compose v2 for Docker. Root access; the systemd package also needs the full `iproute2` package (the Docker image brings its own). An Intel N100 mini PC, a used thin client or a NUC works well. From v1.4.2, arm64 ships alongside amd64 as a regular release: a Raspberry Pi 4 or 5 works with a 64-bit OS (for example Raspberry Pi OS 64-bit); a 32-bit OS does not. 2 GB RAM and 4 GB disk are enough for the node. Verified platform: Ubuntu 22.04, amd64. |
| **Cellular module** | A Qualcomm-based Quectel module with USB serial (AT) and QMI ports. Tested: the DJI Cellular module (Quectel EG25-G inside, after a one-time USB identity switch described on the hardware page). EG25-G, EC25, EC20, EC21/EG21, EM05/EM06/EG06 and RM500Q are supported by the code but not tested yet. Up to five modules per node. |
| **Antennas and power** | Attach both antennas (main and diversity). Use a powered USB hub when you connect more than one module. |
| **SIM and network** | A SIM with its PIN removed, or a removable eUICC card for eSIM. Wi-Fi calling enabled on the plan by your carrier. TCP port 2222 reachable from the phone, through a port forward or a VPN such as WireGuard or Tailscale. |
| **App** | Vonode for iPhone and iPad, version 1.4.0 or later, iOS and iPadOS 17 or later. Free download on the App Store. The Vonode app is available on the App Store in all countries and regions except mainland China. |

Not supported yet: 32-bit ARM (armv7) hosts and 32-bit operating systems,
OpenWrt routers, Docker Desktop on macOS or Windows, virtual machines without USB pass-through,
and USB dongles that only present a network card (HiLink, RNDIS-only or ECM-only firmware).
Calls over the cellular network have not been confirmed on any module yet; calls work over
Wi-Fi calling.

<!-- CARRIERS:START Generated by the node release tooling; do not edit by hand. digest=sha256:14e60b354fb148f55683dacc8a4df29dd20d1a79ed676dafb703ebdaa395d775 -->

## Supported carriers (Wi-Fi Calling)

Vonode places Wi-Fi Calling (VoWiFi) calls and texts with the SIM in your node. This list is generated from the node's carrier settings: 428 entries for 423 network codes in 109 countries and regions. Search it on the website: https://vonode.cc/docs/carriers/

- Wi-Fi Calling may need to be enabled on your plan by your carrier.
- Some carriers check a Wi-Fi Calling entitlement for the line (TS.43; T-Mobile US and AT&T, for example), and some carriers only allow certified devices. Vonode never changes the IMEI.
- Mainland China (MCC 460/461) and India (MCC 404/405/406) SIMs: Wi-Fi Calling is not offered (92 network codes); calls and SMS over the mobile network still work.
- Rows that are not marked “Tested by us” have not been tested by us — results depend on the carrier.
- Carriers that are not listed still get the standard 3GPP settings and may work; results depend on the carrier.
- Tried a carrier? Tell us at support@vonode.cc or in the Telegram group https://t.me/vonode_group.

| Status | Entries | Notes |
| --- | ---: | --- |
| Tested by us | 5 | A real SIM of this carrier registered for Wi-Fi Calling on our own hardware with the current node software. |
| Reported working | 7 | Users told us it works. We have not verified it on our own hardware yet. |
| Configured | 51 | The node has settings made for this carrier's network. |
| Configured from vendor data | 190 | Settings built from phone makers' published carrier configuration, from node v1.4.2. |
| Automatic (best effort) | 175 | The node starts from standard 3GPP settings plus hints from vendor data and adapts automatically. |

### Tested and reported carriers

| Country or region | Carrier | MCC-MNC | Status | Settings | Notes |
| --- | --- | --- | --- | --- | --- |
| United Kingdom | giffgaff (O2) | `234-10` | Tested by us | Built in | Tested with a giffgaff SIM on the O2 network. |
| United Kingdom | Vodafone UK | `234-15` | Tested by us | Built in | Tested with a Vodafone UK SIM. |
| United Kingdom | CMLink UK (EE) | `234-33` | Tested by us | Built in | Tested with a CMLink UK SIM on the EE network. |
| United Kingdom | CTExcel (EE) | `234-33` | Tested by us | Built in | Tested with a CTExcel SIM on the EE network. |
| United States | Red Pocket (AT&T) | `310-280` | Tested by us | Built in | Tested with a Red Pocket SIM on the AT&T network. |
| Philippines | DITO | `515-66` | Reported working | From node v1.4.2 | Reported working by users; not tested by us. |
| Taiwan | Far EasTone 遠傳電信 | `466-01` | Reported working | From node v1.4.2 | Reported working by users; not tested by us. |
| Taiwan | Chunghwa Telecom 中華電信 | `466-92` | Reported working | From node v1.4.2 | Reported working by users; not tested by us. |
| Taiwan | Taiwan Mobile 台灣大哥大 | `466-97` | Reported working | From node v1.4.2 | Reported working by users; not tested by us. |
| Thailand | AIS | `520-01` | Reported working | From node v1.4.2 | Reported working by users; not tested by us. |
| Thailand | AIS | `520-03` | Reported working | From node v1.4.2 | Reported working by users; not tested by us. |
| United Kingdom | Lebara UK | `204-04` | Reported working | From node v1.4.2 | Reported working by users; not tested by us. Lebara UK SIMs use the network code 204-04 (Vodafone Netherlands range). |

### All carriers by country

Full table with notes: [CARRIERS.md](CARRIERS.md) · Website with search: https://vonode.cc/docs/carriers/

<details><summary>Albania (1)</summary>

- Vodafone `276-02` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Argentina (7)</summary>

- Movistar `722-007` · Automatic (best effort)
- Telecom Personal S.A. `722-034` · Configured from vendor data · From node v1.4.2
- Nuestro `722-036` · Automatic (best effort)
- Claro AR `722-310` · Configured from vendor data · From node v1.4.2
- Compañía de Telefonos del Interior Norte S.A. `722-320` · Automatic (best effort)
- Compañía de Telefonos del Interior S.A. `722-330` · Automatic (best effort)
- Telecom Personal S.A. `722-341` · Automatic (best effort)

</details>
<details><summary>Armenia (1)</summary>

- K Telecom CJSC `283-05` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Australia (7)</summary>

- Telstra `505-01` · Configured from vendor data · From node v1.4.2
- Optus `505-02` · Configured from vendor data · From node v1.4.2
- Vodafone `505-03` · Configured from vendor data · From node v1.4.2
- AAPT Ltd. `505-14` · Automatic (best effort)
- Telstra `505-71` · Configured from vendor data · From node v1.4.2
- Telstra `505-72` · Configured from vendor data · From node v1.4.2
- Optus `505-90` · Automatic (best effort)

</details>
<details><summary>Austria (9)</summary>

- A1 `232-01` · Configured from vendor data · From node v1.4.2
- A1 `232-02` · Automatic (best effort)
- Magenta Telekom `232-03` · Configured from vendor data · From node v1.4.2
- Drei `232-05` · Automatic (best effort)
- Magenta Telekom `232-07` · Configured from vendor data · From node v1.4.2
- Drei `232-10` · Configured from vendor data · From node v1.4.2
- A1 `232-11` · Automatic (best effort)
- A1 `232-12` · Automatic (best effort)
- Spusu AT `232-17` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Azerbaijan (2)</summary>

- Azercell `400-01` · Automatic (best effort)
- Bakcell `400-02` · Automatic (best effort)

</details>
<details><summary>Bahamas (1)</summary>

- BTC `364-039` · Automatic (best effort)

</details>
<details><summary>Bahrain (2)</summary>

- Batelco `426-01` · Configured from vendor data · From node v1.4.2
- stc BH `426-04` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Bangladesh (1)</summary>

- Grameenphone `470-01` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Barbados (1)</summary>

- Cable & Wireless (Barbados) Ltd. `342-600` · Automatic (best effort)

</details>
<details><summary>Belarus (2)</summary>

- A1 BY `257-01` · Configured from vendor data · From node v1.4.2
- MTS `257-02` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Belgium (4)</summary>

- Proximus `206-01` · Configured from vendor data · From node v1.4.2
- Telenet `206-05` · Configured from vendor data · From node v1.4.2
- Orange `206-10` · Configured from vendor data · From node v1.4.2
- BASE `206-20` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Bhutan (1)</summary>

- Bhutan Telecom Ltd `402-11` · Automatic (best effort)

</details>
<details><summary>Brazil (8)</summary>

- Claro BR `724-05` · Configured from vendor data · From node v1.4.2
- Vivo `724-06` · Configured from vendor data · From node v1.4.2
- Vivo `724-10` · Configured from vendor data · From node v1.4.2
- Vivo `724-11` · Configured from vendor data · From node v1.4.2
- Americel `724-12` · Automatic (best effort)
- Vivo `724-23` · Configured from vendor data · From node v1.4.2
- Claro BR `724-38` · Automatic (best effort)
- Brisanet `724-77` · Automatic (best effort)

</details>
<details><summary>Brunei (2)</summary>

- B-Mobile `528-02` · Automatic (best effort)
- DST Com `528-11` · Automatic (best effort)

</details>
<details><summary>Bulgaria (2)</summary>

- A1 `284-01` · Automatic (best effort)
- VIVACOM `284-03` · Automatic (best effort)

</details>
<details><summary>Cambodia (3)</summary>

- Smart `456-02` · Automatic (best effort)
- Smart `456-05` · Configured from vendor data · From node v1.4.2
- Smart `456-06` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Canada (12)</summary>

- TELUS `302-220` · Configured from vendor data · From node v1.4.2
- TELUS `302-221` · Configured from vendor data · From node v1.4.2
- EastLink `302-270` · Automatic (best effort)
- Fido `302-370` · Configured from vendor data · From node v1.4.2
- Freedom Mobile `302-490` · Automatic (best effort)
- Videotron `302-500` · Configured from vendor data · From node v1.4.2
- Bell Mobility `302-610` · Configured from vendor data · From node v1.4.2
- Bell Mobility `302-630` · Automatic (best effort)
- Bell Mobility `302-640` · Automatic (best effort)
- Bell `302-690` · Automatic (best effort)
- Rogers `302-720` · Automatic (best effort)
- Sask Tel Mobility `302-780` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Chile (6)</summary>

- Entel `730-01` · Configured from vendor data · From node v1.4.2
- Movistar `730-02` · Configured from vendor data · From node v1.4.2
- Claro CL `730-03` · Configured from vendor data · From node v1.4.2
- Movistar `730-07` · Automatic (best effort)
- WOM `730-09` · Configured from vendor data · From node v1.4.2
- Entel `730-10` · Automatic (best effort)

</details>
<details><summary>Colombia (3)</summary>

- Claro CO `732-101` · Configured from vendor data · From node v1.4.2
- Movistar `732-123` · Automatic (best effort)
- WOM `732-360` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Costa Rica (1)</summary>

- Claro CR `712-03` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Croatia (3)</summary>

- Hrvatski Telekom `219-01` · Configured from vendor data · From node v1.4.2
- A1/Tomato `219-10` · Configured from vendor data · From node v1.4.2
- Hrvatski Telekom `219-20` · Automatic (best effort)

</details>
<details><summary>Cyprus (3)</summary>

- CYTA `280-01` · Automatic (best effort)
- epic `280-10` · Configured from vendor data · From node v1.4.2
- PrimeTel `280-20` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Czechia (4)</summary>

- T-Mobile `230-01` · Configured from vendor data · From node v1.4.2
- O2 `230-02` · Configured from vendor data · From node v1.4.2
- Vodafone `230-03` · Configured from vendor data · From node v1.4.2
- Vodafone `230-99` · Automatic (best effort)

</details>
<details><summary>Denmark (5)</summary>

- TDC Mobil `238-01` · Configured from vendor data · From node v1.4.2
- Telenor `238-02` · Configured from vendor data · From node v1.4.2
- 3 `238-06` · Configured from vendor data · From node v1.4.2
- Telia `238-20` · Configured from vendor data · From node v1.4.2
- Telia `238-30` · Automatic (best effort)

</details>
<details><summary>Dominican Republic (2)</summary>

- Altice Dominicana `370-01` · Configured from vendor data · From node v1.4.2
- Claro RD `370-02` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Ecuador (2)</summary>

- Claro EC `740-01` · Configured from vendor data · From node v1.4.2
- Telecsa S.A. `740-02` · Automatic (best effort)

</details>
<details><summary>Egypt (2)</summary>

- Vodafone `602-02` · Automatic (best effort)
- Etisalat `602-03` · Automatic (best effort)

</details>
<details><summary>El Salvador (1)</summary>

- Claro SV `706-01` · Automatic (best effort)

</details>
<details><summary>Estonia (3)</summary>

- Telia `248-01` · Configured from vendor data · From node v1.4.2
- Elisa `248-02` · Automatic (best effort)
- Tele2 `248-03` · Automatic (best effort)

</details>
<details><summary>Finland (4)</summary>

- Elisa `244-05` · Configured from vendor data · From node v1.4.2
- DNA `244-12` · Automatic (best effort)
- Elisa `244-21` · Automatic (best effort)
- Telia `244-91` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>France (12)</summary>

- Orange `208-01` · Configured from vendor data · From node v1.4.2
- Orange `208-02` · Configured from vendor data · From node v1.4.2
- SFR `208-09` · Automatic (best effort)
- SFR `208-10` · Configured from vendor data · From node v1.4.2
- SFR `208-11` · Automatic (best effort)
- SFR `208-13` · Automatic (best effort)
- Free Mobile `208-15` · Automatic (best effort)
- Bouygues Telecom `208-20` · Configured from vendor data · From node v1.4.2
- Bouygues Telecom `208-21` · Automatic (best effort)
- EURO INFORMATION TELECOM `208-26` · Configured from vendor data · From node v1.4.2
- Coriolis `208-27` · Configured from vendor data · From node v1.4.2
- Bouygues Telecom `208-88` · Automatic (best effort)

</details>
<details><summary>French Antilles (1)</summary>

- Orange Caraïbe Mobiles `340-01` · Automatic (best effort)

</details>
<details><summary>French Polynesia (1)</summary>

- Viti `547-05` · Automatic (best effort)

</details>
<details><summary>Germany (9)</summary>

- Telekom `262-01` · Configured from vendor data · From node v1.4.2
- Vodafone `262-02` · Configured from vendor data · From node v1.4.2
- O2 `262-03` · Configured
- Telekom `262-06` · Configured from vendor data · From node v1.4.2
- O2 `262-07` · Configured
- O2 `262-11` · Configured
- 1&1 `262-23` · Configured from vendor data · From node v1.4.2
- Truphone `262-42` · Automatic (best effort)
- Airbus DE `262-98` · Automatic (best effort)

</details>
<details><summary>Ghana (1)</summary>

- MTN `620-01` · Automatic (best effort)

</details>
<details><summary>Greece (4)</summary>

- COSMOTE `202-01` · Configured from vendor data · From node v1.4.2
- Vodafone `202-05` · Configured from vendor data · From node v1.4.2
- Info Quest S.A. `202-09` · Automatic (best effort)
- Telestet `202-10` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Guatemala (2)</summary>

- Claro GT `704-01` · Configured from vendor data · From node v1.4.2
- Movistar `704-03` · Automatic (best effort)

</details>
<details><summary>Honduras (1)</summary>

- Claro HN `708-001` · Automatic (best effort)

</details>
<details><summary>Hong Kong (13)</summary>

- csl / 1O1O `454-00` · Configured
- 3 Hong Kong `454-03` · Configured
- Hutchison 2G `454-04` · Automatic (best effort)
- SmarTone HK `454-06` · Configured from vendor data · From node v1.4.2
- MVNO/Trident `454-08` · Automatic (best effort)
- China Mobile Hong Kong 中國移動香港 `454-12` · Configured from vendor data · From node v1.4.2
- SmarTone HK `454-15` · Automatic (best effort)
- PCCW `454-16` · Automatic (best effort)
- SmarTone HK `454-17` · Automatic (best effort)
- csl / 1O1O `454-19` · Configured
- Public Mobile Networks/Reserved `454-20` · Automatic (best effort)
- Public Mobile Networks/Reserved `454-30` · Automatic (best effort)
- Public Mobile Networks/Reserved `454-31` · Automatic (best effort)

</details>
<details><summary>Hungary (3)</summary>

- Yettel `216-01` · Configured from vendor data · From node v1.4.2
- Magyar Telekom `216-30` · Configured from vendor data · From node v1.4.2
- Vodafone `216-70` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Iceland (2)</summary>

- Tal hf `274-02` · Configured from vendor data · From node v1.4.2
- Nova `274-11` · Automatic (best effort)

</details>
<details><summary>Indonesia (3)</summary>

- Smartfren `510-09` · Configured from vendor data · From node v1.4.2
- Telkomsel `510-10` · Automatic (best effort)
- Smartfren `510-28` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>International (1)</summary>

- AT&T Mobility (Wireless Maritime Services) `901-18` · Automatic (best effort)

</details>
<details><summary>Iraq (1)</summary>

- Asiacell `418-05` · Automatic (best effort)

</details>
<details><summary>Ireland (4)</summary>

- Vodafone `272-01` · Automatic (best effort)
- 3 `272-02` · Automatic (best effort)
- Meteor Mobile Communications Ltd. `272-03` · Configured from vendor data · From node v1.4.2
- Sky IE `272-25` · Automatic (best effort)

</details>
<details><summary>Israel (5)</summary>

- Partner Communications Co. Ltd. `425-01` · Configured from vendor data · From node v1.4.2
- Cellcom Israel Ltd. `425-02` · Configured from vendor data · From node v1.4.2
- Pelephone Communications Ltd. `425-03` · Configured from vendor data · From node v1.4.2
- Hot Mobile `425-07` · Configured from vendor data · From node v1.4.2
- Partner Communications Co. Ltd. `425-10` · Automatic (best effort)

</details>
<details><summary>Italy (6)</summary>

- TIM `222-01` · Configured from vendor data · From node v1.4.2
- Kena `222-07` · Automatic (best effort)
- Vodafone `222-10` · Automatic (best effort)
- Spusu IT `222-56` · Automatic (best effort)
- WINDTRE `222-88` · Automatic (best effort)
- WINDTRE `222-99` · Automatic (best effort)

</details>
<details><summary>Jamaica (1)</summary>

- Cable & Wireless `338-180` · Automatic (best effort)

</details>
<details><summary>Japan (5)</summary>

- Rakuten Mobile(MNO) `440-11` · Automatic (best effort)
- au (KDDI) `440-50` · Automatic (best effort)
- au (KDDI) `440-51` · Configured from vendor data · From node v1.4.2
- au `440-52` · Automatic (best effort)
- au (KDDI) `440-54` · Automatic (best effort)

</details>
<details><summary>Jordan (2)</summary>

- Fastlink `416-01` · Automatic (best effort)
- Umniah `416-03` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Kazakhstan (1)</summary>

- Beeline `401-01` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Kenya (2)</summary>

- Safaricom `639-02` · Configured from vendor data · From node v1.4.2
- Airtel Kenya `639-03` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Kuwait (3)</summary>

- Zain `419-02` · Automatic (best effort)
- Ooredoo `419-03` · Configured from vendor data · From node v1.4.2
- STC `419-04` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Laos (1)</summary>

- Unitel `457-03` · Automatic (best effort)

</details>
<details><summary>Latvia (1)</summary>

- Tele2/ZZ `247-02` · Automatic (best effort)

</details>
<details><summary>Liechtenstein (2)</summary>

- Telecom FL AG `295-01` · Automatic (best effort)
- Mobilkom Liechtenstein `295-05` · Automatic (best effort)

</details>
<details><summary>Lithuania (2)</summary>

- Telia `246-01` · Automatic (best effort)
- Tele2 `246-03` · Automatic (best effort)

</details>
<details><summary>Luxembourg (3)</summary>

- P&T Luxembourg `270-01` · Configured from vendor data · From node v1.4.2
- Tango `270-77` · Configured from vendor data · From node v1.4.2
- Orange `270-99` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Macao (1)</summary>

- Hutchison Telecom `455-03` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Malaysia (12)</summary>

- Digi `502-10` · Automatic (best effort)
- unifi mobile `502-11` · Automatic (best effort)
- Maxis/Hotlink `502-12` · Configured from vendor data · From node v1.4.2
- Celcom `502-13` · Automatic (best effort)
- Digi `502-143` · Automatic (best effort)
- Digi `502-146` · Automatic (best effort)
- Yes 5G `502-152` · Configured from vendor data · From node v1.4.2
- unifi mobile `502-153` · Configured from vendor data · From node v1.4.2
- Digi `502-16` · Configured from vendor data · From node v1.4.2
- TimeCel `502-17` · Automatic (best effort)
- U Mobile `502-18` · Automatic (best effort)
- Celcom `502-19` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Maldives (1)</summary>

- Ooredoo `472-02` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Mexico (4)</summary>

- Telcel `334-002` · Automatic (best effort)
- Telcel `334-020` · Configured from vendor data · From node v1.4.2
- AT&T MX `334-050` · Configured from vendor data · From node v1.4.2
- AT&T MX `334-090` · Automatic (best effort)

</details>
<details><summary>Monaco (1)</summary>

- MONACO TELECOM `212-10` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Montenegro (1)</summary>

- Crnogorski Telekom `297-02` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Morocco (2)</summary>

- Méditélécom `604-00` · Configured from vendor data · From node v1.4.2
- Maroc `604-01` · Automatic (best effort)

</details>
<details><summary>Mozambique (1)</summary>

- Vodacom `643-04` · Automatic (best effort)

</details>
<details><summary>Nepal (1)</summary>

- Nepal Telecommunications `429-01` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Netherlands (5)</summary>

- Vodafone NL `204-04` · Configured
- KPN Mobiel `204-08` · Configured from vendor data · From node v1.4.2
- Odido `204-16` · Configured from vendor data · From node v1.4.2
- Truphone `204-33` · Automatic (best effort)
- KPN Lab `204-69` · Automatic (best effort)

</details>
<details><summary>New Zealand (3)</summary>

- Vodafone `530-01` · Configured
- Spark `530-05` · Configured
- 2degrees `530-24` · Configured

</details>
<details><summary>Nicaragua (1)</summary>

- Claro NI `710-21` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>North Macedonia (2)</summary>

- Makedonski Telekom `294-01` · Automatic (best effort)
- Nov Operator `294-03` · Automatic (best effort)

</details>
<details><summary>Norway (4)</summary>

- Telenor `242-01` · Automatic (best effort)
- Telia `242-02` · Configured from vendor data · From node v1.4.2
- OneCall `242-05` · Configured from vendor data · From node v1.4.2
- Ice Norway `242-14` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Oman (1)</summary>

- Omantel `422-02` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Pakistan (3)</summary>

- Jazz `410-01` · Configured from vendor data · From node v1.4.2
- Telenor `410-06` · Automatic (best effort)
- Jazz `410-07` · Automatic (best effort)

</details>
<details><summary>Panama (4)</summary>

- Cable & Wireless Panama S.A. `714-01` · Configured from vendor data · From node v1.4.2
- Movistar `714-02` · Automatic (best effort)
- Movistar `714-020` · Configured from vendor data · From node v1.4.2
- Claro PA `714-03` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Paraguay (1)</summary>

- Claro PY `744-02` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Peru (3)</summary>

- Movistar `716-06` · Configured from vendor data · From node v1.4.2
- Claro PE `716-10` · Configured from vendor data · From node v1.4.2
- Entel `716-17` · Automatic (best effort)

</details>
<details><summary>Philippines (3)</summary>

- Globe Telecom `515-02` · Configured from vendor data · From node v1.4.2
- Smart Communications `515-03` · Configured from vendor data · From node v1.4.2
- DITO `515-66` · Reported working · From node v1.4.2

</details>
<details><summary>Poland (5)</summary>

- Plus `260-01` · Automatic (best effort)
- T-Mobile `260-02` · Configured from vendor data · From node v1.4.2
- Orange `260-03` · Configured from vendor data · From node v1.4.2
- IDEA (UMTS)/PTK Centertel sp. Z.o.o. `260-05` · Automatic (best effort)
- PLAY `260-06` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Portugal (6)</summary>

- NOS `268-03` · Configured from vendor data · From node v1.4.2
- MEO `268-06` · Automatic (best effort)
- NOS `268-93` · Automatic (best effort)
- NOS `268-97` · Automatic (best effort)
- NOS `268-98` · Automatic (best effort)
- NOS `268-99` · Automatic (best effort)

</details>
<details><summary>Puerto Rico (1)</summary>

- Claro PR `330-110` · Automatic (best effort)

</details>
<details><summary>Qatar (2)</summary>

- Ooredoo `427-01` · Automatic (best effort)
- Vodafone `427-02` · Automatic (best effort)

</details>
<details><summary>Romania (3)</summary>

- Vodafone `226-01` · Configured from vendor data · From node v1.4.2
- Digi.Mobil `226-05` · Automatic (best effort)
- Orange `226-10` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Russia (6)</summary>

- MTS (МТС) `250-01` · Configured from vendor data · From node v1.4.2
- MegaFon `250-02` · Configured from vendor data · From node v1.4.2
- Orensot `250-11` · Automatic (best effort)
- Tele2 `250-20` · Automatic (best effort)
- Sberbank-Telecom `250-50` · Configured from vendor data · From node v1.4.2
- Beeline (Билайн) `250-99` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Réunion (2)</summary>

- Orange La Réunion `647-00` · Configured from vendor data · From node v1.4.2
- Zeop `647-04` · Automatic (best effort)

</details>
<details><summary>Saudi Arabia (4)</summary>

- STC `420-01` · Configured from vendor data · From node v1.4.2
- Mobily `420-03` · Configured from vendor data · From node v1.4.2
- Zain Saudi Arabia `420-04` · Configured from vendor data · From node v1.4.2
- Virgin `420-05` · Automatic (best effort)

</details>
<details><summary>Senegal (1)</summary>

- Sentel GSM `608-02` · Automatic (best effort)

</details>
<details><summary>Serbia (1)</summary>

- A1 SRB `220-05` · Automatic (best effort)

</details>
<details><summary>Singapore (5)</summary>

- Singtel `525-01` · Configured from vendor data · From node v1.4.2
- Singtel `525-02` · Automatic (best effort)
- M1 `525-03` · Configured from vendor data · From node v1.4.2
- StarHub `525-05` · Configured from vendor data · From node v1.4.2
- SIMBA `525-10` · Automatic (best effort)

</details>
<details><summary>Slovakia (4)</summary>

- Orange `231-01` · Configured from vendor data · From node v1.4.2
- Slovak Telekom `231-02` · Automatic (best effort)
- 4ka SK `231-03` · Configured from vendor data · From node v1.4.2
- O2 `231-06` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Slovenia (1)</summary>

- Telekom Slovenije `293-41` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>South Africa (4)</summary>

- Vodacom `655-01` · Configured from vendor data · From node v1.4.2
- Telkom `655-02` · Configured from vendor data · From node v1.4.2
- Cell C (Pty) Ltd. `655-07` · Configured from vendor data · From node v1.4.2
- MTN `655-10` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Spain (7)</summary>

- Vodafone `214-01` · Configured from vendor data · From node v1.4.2
- Orange `214-03` · Configured from vendor data · From node v1.4.2
- Movistar `214-05` · Automatic (best effort)
- Euskaltel `214-06` · Automatic (best effort)
- Movistar `214-07` · Configured from vendor data · From node v1.4.2
- Digi Spain `214-22` · Configured from vendor data · From node v1.4.2
- Truphone `214-27` · Automatic (best effort)

</details>
<details><summary>Sri Lanka (2)</summary>

- Sri Lanka Telecom Mobitel `413-01` · Automatic (best effort)
- Dialog Sri Lanka `413-02` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Sudan (1)</summary>

- Sudani `634-07` · Automatic (best effort)

</details>
<details><summary>Sweden (4)</summary>

- Telia Sverige AB `240-01` · Configured from vendor data · From node v1.4.2
- 3 (Hi3G Access AB) `240-02` · Configured from vendor data · From node v1.4.2
- Tele2/Comviq Sverige/Com Hem `240-07` · Configured from vendor data · From node v1.4.2
- Telenor Sverige AB `240-08` · Automatic (best effort)

</details>
<details><summary>Switzerland (5)</summary>

- Swisscom `228-01` · Configured from vendor data · From node v1.4.2
- Sunrise `228-02` · Configured
- Salt `228-03` · Configured from vendor data · From node v1.4.2
- Sunrise `228-12` · Configured
- UPC `228-53` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Taiwan (7)</summary>

- Far EasTone 遠傳電信 `466-01` · Reported working · From node v1.4.2
- Far EasTone 遠傳電信 `466-02` · Automatic (best effort)
- Far EasTone 遠傳電信 `466-03` · Automatic (best effort)
- Far EasTone 遠傳電信 (former Asia Pacific Telecom) `466-05` · Configured from vendor data · From node v1.4.2
- Chunghwa Telecom 中華電信 `466-92` · Reported working · From node v1.4.2
- Taiwan Mobile 台灣大哥大 `466-97` · Reported working · From node v1.4.2
- TransAsia Telecoms `466-99` · Automatic (best effort)

</details>
<details><summary>Thailand (6)</summary>

- CAT CDMA `520-00` · Configured from vendor data · From node v1.4.2
- AIS `520-01` · Reported working · From node v1.4.2
- AIS `520-03` · Reported working · From node v1.4.2
- TrueMove H 4G LTE `520-04` · Configured from vendor data · From node v1.4.2
- dtac `520-05` · Automatic (best effort)
- dtac `520-18` · Automatic (best effort)

</details>
<details><summary>Türkiye (4)</summary>

- Turkcell `286-01` · Configured from vendor data · From node v1.4.2
- Vodafone `286-02` · Configured from vendor data · From node v1.4.2
- Türk Telekom `286-03` · Configured from vendor data · From node v1.4.2
- Türk Telekom `286-04` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Uganda (1)</summary>

- Airtel Uganda `641-01` · Automatic (best effort)

</details>
<details><summary>Ukraine (1)</summary>

- Astelit `255-06` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>United Arab Emirates (2)</summary>

- e& UAE `424-02` · Configured from vendor data · From node v1.4.2
- du `424-03` · Automatic (best effort)

</details>
<details><summary>United Kingdom (22)</summary>

- Lebara UK `204-04` · Reported working · From node v1.4.2
- Mapesbury Communications Ltd. `234-01` · Automatic (best effort)
- FMS Solutions Ltd `234-04` · Automatic (best effort)
- BT One Phone `234-08` · Automatic (best effort)
- giffgaff (O2) `234-10` · Tested by us
- O2 `234-10` · Configured
- Vodafone UK `234-15` · Tested by us
- Three Mobile `234-20` · Configured
- Truphone `234-25` · Automatic (best effort)
- EE `234-30` · Configured
- EE `234-31` · Configured
- EE `234-32` · Configured
- CMLink UK (EE) `234-33` · Tested by us
- CTExcel (EE) `234-33` · Tested by us
- EE `234-33` · Configured
- Virgin `234-38` · Automatic (best effort)
- Spusu GB `234-40` · Configured from vendor data · From node v1.4.2
- Cable and Wireless Guernsey `234-55` · Automatic (best effort)
- Sky `234-57` · Configured from vendor data · From node v1.4.2
- EE `234-86` · Configured
- Three Mobile `234-94` · Configured
- Three Mobile `235-94` · Configured

</details>
<details><summary>United States (63)</summary>

- T-Mobile - US `310-026` · Configured
- AT&T `310-030` · Configured
- AT&T `310-070` · Configured
- AT&T Private `310-090` · Automatic (best effort)
- Pacific Telecom Inc `310-110` · Automatic (best effort)
- Sprint `310-120` · Configured from vendor data · From node v1.4.2
- Cricket Wireless `310-150` · Configured from vendor data · From node v1.4.2
- T-Mobile - US `310-160` · Configured
- AT&T `310-170` · Configured
- T-Mobile - US `310-200` · Configured
- T-Mobile - US `310-210` · Configured
- T-Mobile - US `310-220` · Configured
- T-Mobile - US `310-230` · Configured
- T-Mobile - US `310-240` · Configured
- T-Mobile - US `310-250` · Configured
- T-Mobile - US `310-260` · Configured
- T-Mobile - US `310-270` · Configured
- Red Pocket (AT&T) `310-280` · Tested by us
- AT&T `310-280` · Configured
- T-Mobile - US `310-300` · Configured
- T-Mobile - US `310-310` · Configured
- AT&T `310-380` · Configured
- AT&T `310-410` · Configured
- North East Cellular Inc. `310-450` · Automatic (best effort)
- T-Mobile - US `310-490` · Configured
- T-Mobile - US `310-530` · Configured
- AT&T `310-560` · Configured
- Verizon Wireless `310-590` · Automatic (best effort)
- T-Mobile - US `310-640` · Configured
- Jasper Wireless Inc. `310-650` · Automatic (best effort)
- T-Mobile - US `310-660` · Configured
- AT&T `310-680` · Configured
- Sea Mobile `310-730` · Automatic (best effort)
- T-Mobile - US `310-800` · Configured
- Verizon Wireless `310-890` · Automatic (best effort)
- AT&T `310-950` · Configured
- AT&T `311-180` · Configured
- U.S. Cellular `311-220` · Automatic (best effort)
- C Spire `311-230` · Automatic (best effort)
- Verizon Wireless `311-270` · Automatic (best effort)
- GCI Communications Corp. `311-370` · Automatic (best effort)
- Verizon Wireless `311-480` · Configured from vendor data · From node v1.4.2
- Verizon Wireless `311-481` · Automatic (best effort)
- T-Mobile - US `311-490` · Configured
- U.S. Cellular `311-580` · Configured from vendor data · From node v1.4.2
- U.S. Cellular `311-589` · Automatic (best effort)
- United Wireless `311-650` · Automatic (best effort)
- T-Mobile - Private 5G `311-660` · Configured from vendor data · From node v1.4.2
- Cellcom `311-840` · Configured from vendor data · From node v1.4.2
- T-Mobile - US `311-882` · Configured
- T-Mobile - US `312-250` · Configured
- strata `312-290` · Automatic (best effort)
- NexTech Ota `312-420` · Automatic (best effort)
- Sprint `312-530` · Configured from vendor data · From node v1.4.2
- FirstNet (Lab) `312-670` · Configured from vendor data · From node v1.4.2
- FirstNet `313-100` · Configured from vendor data · From node v1.4.2
- FirstNet `313-110` · Automatic (best effort)
- FirstNet `313-120` · Automatic (best effort)
- FirstNet `313-130` · Automatic (best effort)
- FirstNet `313-140` · Automatic (best effort)
- Dish Wireless (Boost) `313-340` · Configured from vendor data · From node v1.4.2
- Optimum `313-390` · Configured from vendor data · From node v1.4.2
- Liberty Mobile `313-790` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Uruguay (1)</summary>

- Claro UY `748-10` · Configured from vendor data · From node v1.4.2

</details>
<details><summary>Vietnam (4)</summary>

- Mobifone `452-01` · Configured from vendor data · From node v1.4.2
- Vinaphone `452-02` · Configured from vendor data · From node v1.4.2
- Viettel Telecom `452-04` · Configured from vendor data · From node v1.4.2
- Vietnamobile `452-05` · Automatic (best effort)

</details>
<details><summary>Zambia (2)</summary>

- Airtel Zambia `645-01` · Automatic (best effort)
- Telecel Zambia Ltd. `645-02` · Automatic (best effort)

</details>

<!-- CARRIERS:END -->

## Download and verify

From v1.4.0, every release is published on the **[Releases](https://github.com/vonode/vonode-releases/releases)**
page. Set `VERSION` below to the release tag shown on that page (file names use the same tag).

| File | What it is |
|---|---|
| `vonode_<version>_linux_<arch>_commercial.tar.gz` | The install package: node program, `install.sh`, systemd units, `docker/` setup, guides, licenses, and `SHA256SUMS` with `SHA256SUMS.sig`, the release signature over the whole package |
| `vonode_<version>_linux_<arch>_commercial.tar.gz.sha256` | SHA-256 checksum of the install package |
| `vonode_<version>_linux_<arch>_commercial` and `.sig` | The node program as a single file and its release signature, used by in-app software updates |
| `vonode_<version>_linux_<arch>_commercial.THIRD_PARTY_NOTICES.txt` | Third-party components and their licenses |
| `vonode_<version>_linux_<arch>_commercial.spdx.json` | Software bill of materials (SPDX) |
| `vonode_<version>_linux_<arch>_commercial.sources.tar` | Corresponding source of the GPL and LGPL components |
| `latest-commercial.json` and `latest-commercial.json.sig` | The signed release manifest (one for all architectures) that nodes and the Compose one-click script read, and its release signature |
| `docker-compose.yml`, `docker-compose.yml.sha256` and `docker-compose.yml.sig` | The Docker Compose file for this release, with the image pinned by digest, its checksum and (from v1.4.2) its release-key signature |
| `compose-install.sh` and `compose-install.sh.sha256` | The [Docker Compose one-click](#docker-compose-one-click) script and its checksum |
| `vonode-<version>-nas.tar.gz` and `.sha256` | Ready-made files for [NAS](#nas-synology-qnap-unraid-truenas) container apps (from v1.4.2) and their checksum |
| `vonode-<version>-docker-alpine-sources.tar` and `.sha256` | Corresponding source of the GPL, LGPL and MPL packages inside the Docker image |

`<arch>` is `amd64` for 64-bit x86 (`uname -m` prints `x86_64`) or, from v1.4.2, `arm64` for 64-bit
ARM (`uname -m` prints `aarch64`). The commands below use `amd64`; on an arm64 host write `arm64`
instead.

### Checksum

```sh
VERSION=vX.Y.Z        # replace with the release tag
BASE=https://github.com/vonode/vonode-releases/releases/download/$VERSION
curl -fLO $BASE/vonode_${VERSION}_linux_amd64_commercial.tar.gz
curl -fLO $BASE/vonode_${VERSION}_linux_amd64_commercial.tar.gz.sha256
sha256sum -c vonode_${VERSION}_linux_amd64_commercial.tar.gz.sha256
```

`sha256sum -c` must print `OK`. Do not install a package that fails the check.

### Release signature (optional)

The node program is signed with the Vonode update key (Ed25519). The node checks this signature
itself at startup and before it installs an in-app update. To check it by hand, you need
OpenSSL 3.0 or later. The signature covers the text `vonode-binary-v1`, a newline and the
program's SHA-256 in hex.

```sh
cat > vonode-update-key.pem <<'EOF'
-----BEGIN PUBLIC KEY-----
MCowBQYDK2VwAyEAnIDCyy4CJVQSC7a+p+OeQsF/NsPguVkmB+OL//k98OY=
-----END PUBLIC KEY-----
EOF

# Inside the extracted package the program is "vonode" with "vonode.sig".
# For the single-file download use vonode_<version>_linux_<arch>_commercial and its .sig.
BIN=vonode_${VERSION}_linux_amd64_commercial/vonode
printf 'vonode-binary-v1\n%s' "$(sha256sum "$BIN" | cut -d' ' -f1)" > vonode.msg
base64 -d "$BIN.sig" > vonode.sig.bin
openssl pkeyutl -verify -pubin -inkey vonode-update-key.pem -rawin -in vonode.msg -sigfile vonode.sig.bin
```

The last command must print `Signature Verified Successfully`. Each package also holds `SHA256SUMS`
and `SHA256SUMS.sig`, a release-key signature over the checksums of every file in the package.

## Install with the systemd package (recommended)

Recommended for this release. The package installs the node as two systemd services, without
Docker.

### 1. Prepare the host

Install the Linux distribution, connect it to the network (wired Ethernet is recommended) and log
in as a user who can use `sudo`. Update the system and stop ModemManager, which competes for the
module's serial ports:

```sh
sudo apt update && sudo apt full-upgrade -y        # Debian and Ubuntu
sudo systemctl disable --now ModemManager
```

Plug in the module and check that Linux sees it:

```sh
lsusb | grep -i -E 'quectel|2c7c|qualcomm|05c6'
ls /dev/ttyUSB* /dev/cdc-wdm*
```

You should see one Quectel line, four `ttyUSB` ports and one `cdc-wdm` port per module. A DJI
Cellular module needs its USB identity switched once first; the steps are on the
[hardware page](https://vonode.cc/hardware/).

### 2. Download, verify and extract

```sh
VERSION=vX.Y.Z        # replace with the release tag
BASE=https://github.com/vonode/vonode-releases/releases/download/$VERSION
curl -fLO $BASE/vonode_${VERSION}_linux_amd64_commercial.tar.gz
curl -fLO $BASE/vonode_${VERSION}_linux_amd64_commercial.tar.gz.sha256
sha256sum -c vonode_${VERSION}_linux_amd64_commercial.tar.gz.sha256
tar -xzf vonode_${VERSION}_linux_amd64_commercial.tar.gz
```

### 3. Run the installer

```sh
cd vonode_${VERSION}_linux_amd64_commercial
sudo ./install.sh
```

The installer:

- puts the program in `/opt/vonode/vonode` and the configuration in
  `/opt/vonode/config/config.yaml` (an existing configuration and database are kept);
- creates the system user `vonode-gateway` (UID and GID 10001);
- installs and starts two systemd services: `vonode`, the core, and `vonode-gateway`, the
  encrypted SSH service the app connects to.

### 4. First start and the administrator password

Check that both services are `active (running)`:

```sh
systemctl status vonode vonode-gateway
```

On first start the node creates a random administrator password and keeps only its hash in the
configuration. The plain text is in `/opt/vonode/config/initial-admin-password`, readable by
root only. You rarely need it; keep it somewhere safe.

### 5. Open the port and show the pairing QR code

Allow TCP 2222 through the host firewall and, if the phone will connect from outside your
network, forward it on your router (or use a VPN):

```sh
sudo ufw allow 2222/tcp
```

Show the pairing QR code with the address the phone will use to reach the node: your DDNS name or
public IP when you forward the port, or the host's VPN or LAN address. If the router forwards a
different external port, give that port after `-port`.

```sh
sudo /opt/vonode/vonode pair -c /opt/vonode/config/config.yaml -host node.example.com -port 2222
```

The terminal shows the QR code as a block graphic. It is valid for five minutes and works once;
run the command again for a new one. Continue with [Pair the app](#pair-the-app).

### Everyday operation

| Task | Command |
|---|---|
| Status | `systemctl status vonode vonode-gateway` |
| Logs | `journalctl -u vonode -f` |
| Restart | `sudo systemctl restart vonode` (up to two minutes: the node de-registers Wi-Fi calling and closes its tunnels first) |
| New pairing code | `sudo /opt/vonode/vonode pair -c /opt/vonode/config/config.yaml -host <address> -port 2222` |
| Lost administrator password | `sudo systemctl stop vonode`, then `sudo /opt/vonode/vonode reset-password -c /opt/vonode/config/config.yaml`, then `sudo systemctl start vonode`. All phones are signed out and pair again. |

## Install with Docker Compose

Docker Compose runs the same node as two containers from the image
`vonode/vonode` on Docker Hub (published from v1.4.0): `vonode`, the core, which
talks to the modules, and `vonode-gateway`, the unprivileged SSH service the app connects to on
TCP 2222. Pairing, plans and the app work exactly as with the systemd package. Image tags follow
the release (`1.4.0` for `v1.4.0`, plus `latest`). From v1.4.2 the image is multi-platform for 64-bit
x86 (amd64) and 64-bit ARM (arm64) hosts, and Docker picks the right one. Each release also publishes a
`docker-compose.yml` that pins its image by digest, signed with the Vonode release key from v1.4.2.

Use one install method per host: do not run the Docker setup next to the systemd package.

### Docker Compose one-click

On a Linux host with Docker Engine and the Compose plugin ([step 1](#1-prepare-the-host-1) below), one
command downloads the release's `docker-compose.yml`, verifies it, installs it in `/opt/vonode-compose`,
starts both containers, waits until they are healthy and shows the pairing QR code:

```sh
curl -fsSL https://vonode.cc/install/compose.sh | sudo sh
```

- Download and verify only, without changing anything:
  `curl -fsSL https://vonode.cc/install/compose.sh | sh -s -- --dry-run`
- Options: `--host <address the phone uses>`, `--port <port>`, `--dir <folder>`, `--version vX.Y.Z`,
  `--no-start`, `--no-pair`. Run the command again to update.
- The script is also the release asset `compose-install.sh` (the same bytes, SHA-256
  `ca1740be6b659dc44335775b712a9409f16a5592a5e00b8ba2bd8303540a4232`, also in `compose-install.sh.sha256`). You can download it, read it and run
  `sudo sh compose-install.sh` instead of piping it.
- Verification: the script finds the current release, v1.4.2, in the signed release manifest
  `latest-commercial.json` and checks `docker-compose.yml` against its signature `docker-compose.yml.sig`
  with the Vonode release key before it uses it. The image stays pinned by digest.
- The same command works on amd64 and arm64 hosts; on arm64 it now installs the regular v1.4.2 release
  (no longer the experimental arm64 pre-release).
- It does not disable ModemManager or change other services; when ModemManager is running it prints the
  command to disable it. It needs no account and sends no data.

Steps 1 to 4 below do the same by hand.

### 1. Prepare the host

The Linux, module and SIM requirements are the same as above. Docker Desktop on macOS or Windows
cannot reach USB modules; use Docker Engine on Linux. Stop ModemManager, check that Linux sees the
module, then install Docker Engine with the Compose plugin. Docker's convenience script works on
Debian, Ubuntu and Fedora; the last command must print a Compose v2 version.

```sh
sudo systemctl disable --now ModemManager
lsusb | grep -i -E 'quectel|2c7c|qualcomm|05c6'
ls /dev/ttyUSB* /dev/cdc-wdm*
curl -fsSL https://get.docker.com | sh
sudo systemctl enable --now docker
docker compose version
```

The module's serial and QMI ports come from the host kernel's `option` and `qmi_wwan` drivers,
which load automatically when a module is plugged in. If `lsusb` lists the module but
`/dev/ttyUSB*` is missing, run `sudo modprobe -a option qmi_wwan` and plug the module in again.

**Device access.** The `vonode` container uses the host network, runs privileged and sees the
host's whole `/dev`, because it drives the USB serial and QMI ports, creates TUN/XFRM interfaces
for Wi-Fi calling and uses ALSA for call audio. Binding all of `/dev` means a module plugged in
later, or one that comes back with other `/dev/ttyUSB*` numbers after a reset, appears without a
restart. The `vonode-gateway` container runs as UID 10001 with a read-only file system and no
capabilities, and only publishes TCP 2222.

### 2. Download the Compose file

```sh
mkdir -p ~/vonode && cd ~/vonode
VERSION=vX.Y.Z        # replace with the release tag
BASE=https://github.com/vonode/vonode-releases/releases/download/$VERSION
curl -fLO $BASE/docker-compose.yml
curl -fLO $BASE/docker-compose.yml.sha256
sha256sum -c docker-compose.yml.sha256
```

Optional settings go into a `.env` file in the same folder, for example `TZ=Europe/London` for the
node's time zone (the default is UTC) or `VONODE_SSH_PORT=2022` to publish the SSH service on
another host port.

### 3. Start the node

```sh
sudo docker compose up -d
sudo docker compose ps
```

Docker pulls the pinned image and starts both containers; after about half a minute both show
`healthy`. The node keeps its configuration, database and logs in `config/`, `data/` and `logs/`
next to `docker-compose.yml`. On first start it creates a random administrator password; the plain
text is in `config/initial-admin-password`, readable by root only.

### 4. Open the port and show the pairing QR code

Allow TCP 2222 through the host firewall and, if the phone will connect from outside your network,
forward it on your router (or use a VPN). Then show the pairing QR code with the address the phone
will use, and the port it connects to:

```sh
sudo ufw allow 2222/tcp
sudo docker compose exec vonode /app/vonode pair -c /app/config/config.yaml -host node.example.com -port 2222
```

The code is valid for five minutes and works once. Continue with [Pair the app](#pair-the-app).

### Everyday operation with Docker

Run these in the folder that holds `docker-compose.yml`.

| Task | Command |
|---|---|
| Status (both services healthy) | `sudo docker compose ps` |
| Logs | `sudo docker compose logs -f vonode` |
| New pairing code | `sudo docker compose exec vonode /app/vonode pair -c /app/config/config.yaml -host <address> -port 2222` |
| Restart (up to two minutes) | `sudo docker compose restart vonode` |
| Stop, keep data | `sudo docker compose down` |
| Start again | `sudo docker compose up -d` |
| Lost administrator password | `sudo docker compose stop vonode`, `sudo docker compose run --rm --no-deps vonode reset-password -c /app/config/config.yaml`, `sudo docker compose start vonode` |

> [!WARNING]
> Do not run `docker compose down -v` unless you want to delete the node. The
> `vonode_gateway-private` volume holds the SSH host key that paired phones pin; without it every
> phone must pair again.

### Alternative: the setup script

The install package also contains a `docker/` folder with `setup.sh`, which checks the host, lists
the modules it finds, writes `.env` and a device override, starts both containers and prints the
pairing QR code. Download and extract the package as in [Download and verify](#download-and-verify),
then:

```sh
cd vonode_${VERSION}_linux_amd64_commercial/docker
sudo ./setup.sh --host node.example.com --image vonode/vonode@sha256:<digest>
```

The release notes give the image as `vonode/vonode@sha256:<digest>`. `--devices` maps only the
module device nodes present now instead of the whole `/dev`; `./setup.sh --help` explains every
option. Re-run `sudo ./setup.sh` after adding or moving a module in that mode;
`sudo ./setup.sh --uninstall` stops the node and keeps its data.

## NAS (Synology, QNAP, Unraid, TrueNAS)

> [!WARNING]
> **Untested on real NAS hardware.** These files follow the vendors' public documentation and have
> only been checked offline; nobody has run them on a Synology, QNAP, Unraid or TrueNAS box yet.
> Reports are welcome at support@vonode.cc. If you can, prefer a small Linux host with the systemd
> package or the [one-click Compose command](#docker-compose-one-click).

From v1.4.2 each release has a NAS package that runs the same two containers with the NAS's own
container app, without `setup.sh`. Download `vonode-1.4.2-nas.tar.gz` and its `.sha256` from the
[v1.4.2 release](https://github.com/vonode/vonode-releases/releases/tag/v1.4.2), check it with
`sha256sum -c vonode-1.4.2-nas.tar.gz.sha256` and extract it. Each folder has a `README.md` with the steps.

| Folder | NAS software | How you install it |
|---|---|---|
| `synology/` | DSM 7.2 or later, Container Manager | Container Manager › Project › Create, paste `docker-compose.yml` |
| `qnap/` | QTS / QuTS hero 5, Container Station 3 | Container Station › Applications › Create, paste `docker-compose.yml` |
| `unraid/` | Unraid 6.12 or later, Docker enabled | Copy `vonode.xml` and `vonode-gateway.xml` to the user template folder, add both containers |
| `truenas/` | TrueNAS SCALE 24.10 (Electric Eel) or later | Apps › Discover Apps › ⋮ › Install via YAML, paste `docker-compose.yml` |

Every file is pinned to the release's image digest, the same as in the release notes and
`docker-compose.yml`. The image is multi-platform (amd64 and arm64) and Docker picks the right one;
32-bit ARM NAS models are not supported.

- **Host network and privileged core.** The `vonode` container needs `network_mode: host` and
  `privileged: true` (Wi-Fi calling tunnels, USB modems); `vonode-gateway` stays unprivileged and
  publishes only TCP 2222.
- **Devices.** The core gets the NAS's `/dev`, which includes `/dev/bus/usb` and `/dev/net/tun`. If the
  NAS refuses a `/dev` bind, list the devices one by one as the package README shows.
- **ModemManager must not grab the modems.** Stock DSM, QTS and Unraid do not ship it; on TrueNAS SCALE
  run `sudo systemctl disable --now ModemManager` if it is active.
- **Kernel drivers.** Containers cannot load drivers. Modems need `option`, `qmi_wwan` and `cdc_wdm` in
  the NAS kernel. Stock Synology DSM kernels usually lack them (DSM 7 dropped USB 3G/4G dongles), so
  expect the modem path not to work on stock DSM; check QNAP per model. A USB card reader with your SIM
  needs only `/dev/bus/usb` and is the more likely option there (Wi-Fi calling only; card-reader lines
  need a subscription or Vonode Lifetime, not the free plan).
- Never remove the named volumes when you delete or re-create the app: `vonode_gateway-private` holds
  the SSH host key that paired phones pin.

Pair over SSH with `sudo docker exec -it vonode /app/vonode pair -c /app/config/config.yaml -host <NAS address> -port 2222`.
To upgrade, use the NAS package of the new release.

## Pair the app

1. Install **Vonode** from the App Store (free download) and open it.
2. On the **Connect your node** screen, tap the scanner and point the camera at the QR code in
   the terminal.
3. The app connects to the node over SSH, pins the node's host key and shows the node on its home
   screen with its numbers.

The app and the node must reach each other at the address in the code. If the app shows a
connection error, check the port forward or VPN first, then create a fresh code. To pair a second
phone later, use **Move to a new phone** in the app; it shows a one-time QR code.

## Free plan, subscription or Lifetime

Without a subscription or Lifetime the node runs the **free plan**: open the **Numbers** tab and choose the
one number whose SMS you want to see. The choice cannot be changed later; a backup restore
keeps it. The **Messages** tab then shows the SMS that number receives.

Every other feature (calls, Wi-Fi calling, sending SMS, notifications, eSIM profiles, proxies,
automations, more numbers) needs a Vonode subscription or Vonode Lifetime for this node, bought in the
app: open **More › Subscription**. Each subscription or Lifetime purchase unlocks one node. You can move it to another node in the app up to 3 times in 30
days; reinstalling the node software on new hardware or restoring a backup there also counts as a move. The node unlocks within seconds and restarts
into full mode; then add your modules with **Add Modem** and switch Wi-Fi calling on per SIM.

Hardware, SIM plans, carrier charges and hosting are not included.

## Upgrade

- **In the app:** nodes installed with the systemd package offer signed updates under **More › Software update**. The node checks the
  update's signature before it installs it.
- **systemd package:** download and verify the new package, extract it and run `sudo ./install.sh`
  from its top folder. Configuration, database and the SSH host key are kept.
- **Docker Compose:** download the new release's `docker-compose.yml` and `docker-compose.yml.sha256`
  into the same folder and check them, then run `sudo docker compose pull` and `sudo docker compose up -d`.
  Configuration, database and the SSH host key are kept, and both containers always run the same image.
  In-app updates do not replace the program inside a container; update the image instead. With the
  setup script, run `sudo ./setup.sh --image vonode/vonode@sha256:<digest>` with the new digest.
  Installed with the [one-click command](#docker-compose-one-click)? Run it again.

Back up first: **More › Backup & restore** in the app. A backup carries messages, call history and the
node's subscription or Lifetime binding; keep it private. To move to new hardware, create and download a
backup on the old node, install and pair the new node, then import and restore the backup; this
counts as one of the 3 moves in 30 days. Do not keep running the old node with the same data.

## Uninstall

systemd package (this deletes your data and backups):

```sh
sudo systemctl disable --now vonode-gateway vonode
sudo rm /etc/systemd/system/vonode.service /etc/systemd/system/vonode-gateway.service /etc/tmpfiles.d/vonode.conf
sudo systemctl daemon-reload
sudo rm -rf /opt/vonode /var/lib/vonode-gateway
```

Docker Compose, in the folder with `docker-compose.yml`: `sudo docker compose down` stops the node
and keeps everything. To delete the node and its data, run `sudo docker compose down -v`, then
`sudo rm -rf config data logs`. With the setup script, `sudo ./setup.sh --uninstall` keeps the data
and `--uninstall --purge` deletes it.

## Troubleshooting

| Problem | What to do |
|---|---|
| ModemManager is running | `sudo systemctl disable --now ModemManager`, then `sudo systemctl restart vonode` (with Docker, `sudo docker compose restart vonode`). |
| No module detected | If `lsusb` shows nothing from Quectel (`2c7c`), try another cable and port, use a powered hub and check `sudo dmesg \| tail -n 30` for USB power errors. If `lsusb` lists the module but `/dev/ttyUSB*` is missing, run `sudo modprobe -a option qmi_wwan`, then unplug and replug. |
| The app cannot connect | From a device outside your network, run `nc -vz node.example.com 2222`. No answer means the router forward or firewall is wrong, or your provider uses carrier-grade NAT; then use a VPN and pair with the host's VPN address. |
| Pairing code expired or used | Codes last five minutes and work once. Run the `pair` command again. |
| Wi-Fi calling will not register | The plan needs Wi-Fi calling enabled, the module must have registered on the cellular network at least once with that SIM, and the host needs working IPv6 or IPv4 to the carrier. The app's diagnostics page shows the current phase. |

More in the [wiki](https://github.com/vonode/vonode-releases/wiki) and the
[step-by-step guide](https://vonode.cc/install/guide/).

## Source code of GPL and LGPL components

The node software is proprietary, but it includes strongSwan (GPL-2.0-or-later) and statically
linked glibc and GMP (LGPL). For every release:

- the complete corresponding source of these components, with build scripts, is published as the
  release asset `vonode_<version>_linux_amd64_commercial.sources.tar`;
- `SOURCES.md` in the install package lists the components and contains the written offer: the
  source is available for at least three years after the version is published, and a copy on
  physical media can be requested from support@vonode.cc for no more than the cost of the media
  and shipping;
- the SPDX SBOM and `THIRD_PARTY_NOTICES.txt` list all third-party components and their licenses.

The Docker image also contains Alpine Linux packages under GPL, LGPL and MPL licenses; their corresponding
source is published as the release asset `vonode-<version>-docker-alpine-sources.tar`, on the same terms
(at least three years; physical copy from support@vonode.cc).

## Support and legal

- Website: <https://vonode.cc>
- Install guide: <https://vonode.cc/install/> · [step-by-step guide](https://vonode.cc/install/guide/)
- Supported hardware: <https://vonode.cc/hardware/>
- Support: [support@vonode.cc](mailto:support@vonode.cc) · <https://vonode.cc/support/>
- Telegram: [community group](https://t.me/vonode_group) · [announcements](https://t.me/vonode_app)
- [Privacy Policy](https://vonode.cc/privacy/) · [Terms](https://vonode.cc/terms/) ·
  [Third-party notices](https://vonode.cc/third-party-notices/)

Vonode is not an emergency service. It depends on your hardware, carrier, network and Apple
services; do not rely on it for emergency or life-safety communication. Use it only with SIMs,
numbers and networks you are authorized to use, and follow your carrier's terms.

DJI is a trademark of SZ DJI Technology Co., Ltd.; Quectel is a trademark of Quectel Wireless Solutions Co., Ltd.;
Apple, iPhone, iPad and App Store are trademarks of Apple Inc.; Docker is a trademark of Docker, Inc. Vonode is not
affiliated with or endorsed by them.

© 2026 VONODE LLC. Vonode is a product of VONODE LLC.
