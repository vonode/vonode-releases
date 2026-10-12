<p align="right"><b>English</b> · <a href="README.zh-Hans.md">简体中文</a> · <a href="README.zh-Hant.md">繁體中文</a></p>

<p align="center">
  <img src="assets/logo.png" width="112" height="112" alt="Vonode logo">
</p>

<h1 align="center">Vonode node</h1>

<p align="center">
  <b>Keep your SIMs at home. Take your numbers anywhere.</b><br>
  Release downloads, install guides and wiki for the Vonode node software.
</p>

<p align="center">
  <a href="https://vonode.cc/changelog/"><img alt="First release: v1.4.0, coming soon" src="https://img.shields.io/badge/first%20release-v1.4.0%20coming%20soon-2a9468?style=flat-square"></a>
  <a href="https://vonode.cc/install/"><img alt="Install guide" src="https://img.shields.io/badge/docs-install%20guide-2475c2?style=flat-square"></a>
  <img alt="Platform" src="https://img.shields.io/badge/node-Linux%20amd64-141b26?style=flat-square">
  <img alt="App" src="https://img.shields.io/badge/app-iOS%20%26%20iPadOS%2017%2B-141b26?style=flat-square">
</p>

Vonode turns a Linux computer with cellular modules into a private node. The Vonode app for
iPhone and iPad reads and sends SMS, makes and answers calls, switches eSIM profiles and turns
on Wi-Fi calling, all through hardware you run yourself.

This repository holds **release files and documentation only**. It contains no source code.
The node software is proprietary software of VONODE LLC.

> [!NOTE]
> **Coming soon.** The node software is not available for download yet. The first public release,
> **v1.4.0**, will be published here together with the Vonode app 1.4.0 on the App Store; no released
> app works with the node software before that. Version v1.3.0 was briefly published and then
> withdrawn; it is not supported. The instructions below apply from v1.4.0.

## Contents

- [Features](#features)
- [Requirements](#requirements)
- [Download and verify](#download-and-verify)
- [Install with the systemd package (recommended)](#install-with-the-systemd-package-recommended)
- [Install with Docker Compose](#install-with-docker-compose)
- [Pair the app](#pair-the-app)
- [Free plan or subscription](#free-plan-or-subscription)
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
| **Host computer** | 64-bit x86 (amd64) Linux: Debian 12, Ubuntu 22.04 or later, or Fedora, with systemd for the install package or Docker Engine with Compose v2 for Docker. Root access; the systemd package also needs the full `iproute2` package (the Docker image brings its own). An Intel N100 mini PC, a used thin client or a NUC works well; 2 GB RAM and 4 GB disk are enough for the node. Verified platform: Ubuntu 22.04, amd64. |
| **Cellular module** | A Qualcomm-based Quectel module with USB serial (AT) and QMI ports. Tested: the DJI Cellular module (Quectel EG25-G inside, after a one-time USB identity switch described on the hardware page). EG25-G, EC25, EC20, EC21/EG21, EM05/EM06/EG06 and RM500Q are supported by the code but not tested yet. Up to five modules per node. |
| **Antennas and power** | Attach both antennas (main and diversity). Use a powered USB hub when you connect more than one module. |
| **SIM and network** | A SIM with its PIN removed, or a removable eUICC card for eSIM. Wi-Fi calling enabled on the plan by your carrier. TCP port 2222 reachable from the phone, through a port forward or a VPN such as WireGuard or Tailscale. |
| **App** | Vonode for iPhone and iPad, version 1.4.0 or later (coming soon), iOS and iPadOS 17 or later. Free download on the App Store. The Vonode app is available on the App Store in all countries and regions except mainland China. |

Not supported yet: ARM (arm64) hosts such as the Raspberry Pi (planned for a later release),
OpenWrt routers, Docker Desktop on macOS or Windows, virtual machines without USB pass-through,
and USB dongles that only present a network card (HiLink, RNDIS-only or ECM-only firmware).
Calls over the cellular network have not been confirmed on any module yet; calls work over
Wi-Fi calling.

## Download and verify

From v1.4.0, every release is published on the **[Releases](https://github.com/vonode/vonode-releases/releases)**
page. Set `VERSION` below to the release tag shown on that page (file names use the same tag).

| File | What it is |
|---|---|
| `vonode_<version>_linux_amd64_commercial.tar.gz` | The install package: node program, `install.sh`, systemd units, `docker/` setup, guides, licenses |
| `vonode_<version>_linux_amd64_commercial.tar.gz.sha256` | SHA-256 checksum of the install package |
| `docker-compose.yml` and `docker-compose.yml.sha256` | The Docker Compose file for this release, with the image pinned by digest, and its checksum |
| `vonode_<version>_linux_amd64_commercial` and `.sig` | The node program as a single file and its release signature, used by in-app software updates |
| `latest-commercial.json` | The signed update manifest that nodes read to offer updates in the app |
| `vonode_<version>_linux_amd64_commercial.sources.tar` | Corresponding source of the GPL and LGPL components |
| `vonode-<version>-docker-alpine-sources.tar` and `.sha256` | Corresponding source of the GPL, LGPL and MPL packages inside the Docker image |
| `vonode_<version>_linux_amd64_commercial.spdx.json` | Software bill of materials (SPDX) |
| `vonode_<version>_linux_amd64_commercial.THIRD_PARTY_NOTICES.txt` | Third-party components and their licenses |

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
# For the single-file download use vonode_<version>_linux_amd64_commercial and its .sig.
BIN=vonode_${VERSION}_linux_amd64_commercial/vonode
printf 'vonode-binary-v1\n%s' "$(sha256sum "$BIN" | cut -d' ' -f1)" > vonode.msg
base64 -d "$BIN.sig" > vonode.sig.bin
openssl pkeyutl -verify -pubin -inkey vonode-update-key.pem -rawin -in vonode.msg -sigfile vonode.sig.bin
```

The last command must print `Signature Verified Successfully`.

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
the release (`1.4.0` for `v1.4.0`, plus `latest`); images are for 64-bit x86 (amd64) hosts. Each
release also publishes a `docker-compose.yml` that pins its image by digest.

Use one install method per host: do not run the Docker setup next to the systemd package.

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

## Pair the app

1. Install **Vonode** from the App Store (free download) and open it.
2. On the **Connect your node** screen, tap the scanner and point the camera at the QR code in
   the terminal.
3. The app connects to the node over SSH, pins the node's host key and shows the node on its home
   screen with its numbers.

The app and the node must reach each other at the address in the code. If the app shows a
connection error, check the port forward or VPN first, then create a fresh code. To pair a second
phone later, use **Move to a new phone** in the app; it shows a one-time QR code.

## Free plan or subscription

Without a subscription the node runs the **free plan**: open the **Numbers** tab and choose the
one number whose SMS you want to see. The choice cannot be changed later; a backup restore
keeps it. The **Messages** tab then shows the SMS that number receives.

Every other feature (calls, Wi-Fi calling, sending SMS, notifications, eSIM profiles, proxies,
automations, more numbers) needs a subscription, bought in the app: open **More › Subscription**.
One subscription unlocks one node. You can move it to another node in the app up to 3 times in 30
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

Back up first: **More › Backup & restore** in the app. A backup carries messages, call history and the
node's subscription binding; keep it private. To move to new hardware, create and download a
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
- [Privacy Policy](https://vonode.cc/privacy/) · [Terms](https://vonode.cc/terms/) ·
  [Third-party notices](https://vonode.cc/third-party-notices/)

Vonode is not an emergency service. It depends on your hardware, carrier, network and Apple
services; do not rely on it for emergency or life-safety communication. Use it only with SIMs,
numbers and networks you are authorized to use, and follow your carrier's terms.

DJI is a trademark of SZ DJI Technology Co., Ltd.; Quectel is a trademark of Quectel Wireless Solutions Co., Ltd.;
Apple, iPhone, iPad and App Store are trademarks of Apple Inc.; Docker is a trademark of Docker, Inc. Vonode is not
affiliated with or endorsed by them.

© 2026 VONODE LLC. Vonode is a product of VONODE LLC.
