# JCG Q20 OpenWrt/Padavan 刷机说明

本仓库提供 JCG Q20 的刷机路径和预编译镜像。仓库不包含完整的 OpenWrt/Padavan 源码、构建配置或补丁，镜像的来源和许可状态需要单独阅读：

- [来源与构建说明](SOURCES.md)
- [第三方许可与再分发说明](THIRD_PARTY_NOTICES.md)
- [兼容范围](COMPATIBILITY.md)
- [恢复与救砖](RECOVERY.md)
- [默认安全配置](SECURITY.md)
- [SHA-256 校验和](SHA256SUMS)

## 适用范围

本说明只针对 **JCG Q20**，目标平台为 `ramips/mt7621`。不要因为其他路由器也使用 MT7621 就套用这些镜像。不同硬件版本、不同原厂固件、不同 Flash 布局都可能不兼容。

刷机有变砖风险。开始前请确认设备型号、备份原厂配置和可用固件，并准备有线网络与稳定供电。

## 文件与校验和

| 文件 | 用途 | 大小 | SHA-256 |
|---|---|---:|---|
| `pb-boot.img` | 引导器/恢复环境 | 172,356 B | `4926bfe642dab4180caeda4290b9fd7255068352e34ea7c7e54b2b3f0811c999` |
| `底包-factory.bin` | pb-boot 后的底包 | 10,485,760 B | `b4bf7eb927b4497aef1d11f294c0eb79093a324021703c3517b03c261a01fde2` |
| `原系统升级刷入老毛子.bin` | 原厂系统直接刷入 Padavan 的备用路径 | 18,194,914 B | `2f8404ff184d7aa0a1809ba5cd0a1fe6af32b3217cca34c72dad4d4c11e363b2` |
| `openwrt-02.01.2024-ramips-mt7621-jcg_q20-squashfs-sysupgrade.bin` | OpenWrt sysupgrade 固件 | 23,071,246 B | `7175903372b7a54411fc7d1abe65a46c2507f46691f73380eab4582b4a8b09c8` |

下载后先校验：

```sh
sha256sum -c SHA256SUMS
```

macOS 也可以使用：

```sh
shasum -a 256 -c SHA256SUMS
```

Windows 可以逐个文件执行：

```text
certutil -hashfile <文件名> SHA256
```

## 刷机流程

以下流程来自原教程。这里的“记录路径”不等于本次审查已经完成真实设备验证。

### 路径 A：原厂系统 -> pb-boot -> OpenWrt

1. 在原厂系统的系统升级页面上传 `pb-boot.img`。
2. 等待设备进入 pb-boot。将电脑接到设备 LAN 口，访问 `http://192.168.1.1`。
3. 在 pb-boot 页面上传 `底包-factory.bin`，等待写入完成，不要断电。
4. 设备重启后，根据网关地址进入极简 OpenWrt。
5. 在 OpenWrt 的升级页面刷入 `openwrt-02.01.2024-ramips-mt7621-jcg_q20-squashfs-sysupgrade.bin`。
6. 首次进入系统后，立即修改默认密码、无线加密和管理服务设置，见 [默认安全配置](SECURITY.md)。

### 路径 B：原厂系统 -> Padavan

在原厂系统升级页面直接上传 `原系统升级刷入老毛子.bin`。这是备用路径，不代表它适合在 pb-boot 或 OpenWrt 中继续刷入。

### 进入 pb-boot 的方法

原教程记录的做法是长按复位键多次，直到设备进入 pb-boot。不同故障状态下指示灯表现可能不同；如果无法进入恢复页面，请先阅读 [恢复与救砖](RECOVERY.md)，不要反复上传随机镜像。

## 默认密码与安全状态

这些是镜像内的默认配置说明，不是推荐配置：

- **OpenWrt 主镜像**：首次启动脚本会把 `root` 密码设置为 `root`。Dropbear 开启密码认证和 root 密码登录，并绑定到 `lan`。
- **Wi-Fi**：初始化脚本将 `SSID_PASSWD` 置空，因此默认无线加密为 `none`。
- **UPnP**：默认启用。
- **uHTTPd**：监听所有 IPv4/IPv6 接口的 80/443；首次启动脚本会关闭 `rfc1918_filter` 和 HTTPS 跳转。
- **Nginx 静态材料**：镜像内有 Nginx EC 私钥、证书和 `root` 的 Basic Auth 哈希。当前镜像未发现 Nginx 可执行文件或启动脚本，因此它不一定是默认启用的服务，但仍不应继续分发这些静态材料。
- **pb-boot**：默认环境包含固定 MAC `00:0C:43:30:52:11`、`telnet_en=1` 和 `uart_en=1`。固定 MAC 可能是引导器默认值，不代表设备真实 MAC；Telnet 是否实际监听需要运行时确认。
- **Padavan 镜像**：本次审查没有确认其默认管理账号和密码。镜像内包含可选的 SSH、Telnet、代理、DDNS 和远程管理脚本，首次启动后应修改管理密码并关闭不需要的服务。

首次启动后至少执行：

1. 修改 `root` 密码。
2. 启用 WPA2/WPA3，不要保留开放 Wi-Fi。
3. 不需要端口映射时关闭 UPnP。
4. 禁止从 WAN 访问 LuCI、SSH、Telnet 和其他管理服务。
5. 检查监听端口和正在运行的服务。

## 来源与许可

- OpenWrt 上游采用 `GPL-2.0-only`。本仓库的 OpenWrt 镜像包含第三方包，具体许可和源码提供义务见 [第三方许可与再分发说明](THIRD_PARTY_NOTICES.md)。
- Padavan 镜像的具体源码和构建配方尚未在本仓库中建立对应关系。公开参考实现见 [tsl0922/padavan](https://github.com/tsl0922/padavan)，但不能仅凭该仓库推断本镜像的完整来源。
- `pb-boot.img` 的字符串包含 PandoraBox、D-Team 和 Lenovo 信息；本仓库没有找到可核验的对应源码和许可证。
- 本仓库原创的说明文字和仓库元数据采用 MIT 许可；预编译二进制不因本仓库的 MIT 文件而改变其上游许可。

## 恢复

如果刷机后无法进入系统，先尝试 pb-boot 恢复页面，不要直接刷入来源不明的镜像。具体步骤、静态 IP、串口注意事项和失败后的处理见 [恢复与救砖](RECOVERY.md)。

## 免责声明

本项目提供的是刷机记录和预编译文件，不保证适用于所有 JCG Q20 硬件版本，也不保证刷机后不丢失配置或数据。刷机前请自行确认设备、镜像和恢复条件。
