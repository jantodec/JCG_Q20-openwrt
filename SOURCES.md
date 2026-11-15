# 来源与构建说明

本文件记录仓库内 4 个二进制文件的可核验来源信息。它是事实清单，不是法律意见。

## 总体结论

当前仓库没有保存完整的 OpenWrt/Padavan 构建链，也没有把镜像与某个固定源码提交、feed 版本和补丁集对应起来。因此这些文件可以按“预编译镜像”分发，但不能被描述为“可由本仓库复现构建”的源码项目。

## `pb-boot.img`

- 文件类型：U-Boot legacy uImage，`NAND Flash I`，MIPS。
- 构建时间：2021-06-13 09:26:52 CST。
- 镜像内标识：`PandoraBox-Boot Version 2.1`。
- 镜像内品牌/版权字符串：PandoraBox Team、D-Team、Lenovo。
- 默认环境字符串：`ethaddr=00:0C:43:30:52:11`、`telnet_en=1`、`uart_en=1`。
- 本仓库未包含对应源码、构建脚本或明确许可证。
- 固定 MAC 是引导器环境中的默认值，不能直接认定为设备真实 MAC。
- 公开前应向 pb-boot/PandoraBox 的权利方确认再分发许可，并验证 Telnet 和 UART 的实际行为。

## `底包-factory.bin`

- 文件类型：U-Boot legacy uImage，MIPS OpenWrt Linux-5.10.138。
- 构建时间：2022-09-10 12:54:21 CST。
- 内嵌 SquashFS 起始偏移：4,460,544。
- 本仓库未提供对应源码、`.config`、feed 锁定版本或构建日志。
- 该文件看起来是 OpenWrt 系底包，但本次未能完成完整 SquashFS rootfs 解包，因此不能把“未发现凭据”当作完整结论。
- 公开前应补齐来源和构建配方，并重新解包检查 `/etc`、启动脚本和默认网络配置。

## `原系统升级刷入老毛子.bin`

- 文件类型：U-Boot legacy uImage，`JCG-Q20`，Linux 3.4.113。
- 构建时间：2022-02-14 22:49:53 CST；内核字符串显示 2022-02-15 构建。
- 内核构建字符串包含 `root@hiboy-VirtualBox`。
- 镜像内包含 Padavan/hiboy 风格脚本、证书和可选扩展。
- 可参考的公开 Padavan 实现：[tsl0922/padavan](https://github.com/tsl0922/padavan)。该仓库说明支持 JCG-Q20，但目前没有证据证明本二进制就是由该仓库的某个提交构建的。
- 脚本会从 `opt.cn2qq.com`、GitHub 等地址下载可选组件；这些组件的版本和许可证不在本仓库控制范围内。
- 本仓库未建立该镜像与具体源码、补丁和构建日期的对应关系。

## `openwrt-02.01.2024-ramips-mt7621-jcg_q20-squashfs-sysupgrade.bin`

- 文件类型：tar 包，包含 `kernel` 和 SquashFS `root`。
- OpenWrt 版本：`23.05-SNAPSHOT`。
- revision：`02.01.2024`。
- target：`ramips/mt7621`。
- board：`jcg_q20`。
- kernel：5.15.147。
- `DISTRIB_DESCRIPTION`：`OpenWrt 02.01.2024 by Kiddin'`。
- `DISTRIB_TAINTS`：`busybox override`。
- 软件源指向 `https://dl.openwrt.ai/23.05/...` 和 `kiddin9` feed。
- 镜像包含 Passwall、Xray、Shadowsocks、ChinaDNS-NG、UPnP 等额外组件。
- OpenWrt 上游源码：[openwrt/openwrt](https://github.com/openwrt/openwrt)。
- 本仓库未保存该镜像的 `.config`、feed 版本、补丁、构建命令或构建日志。

## 公开前应补齐的构建信息

每个二进制至少应记录：

- 对应源码仓库和提交 SHA；
- `.config` 或 `diffconfig`；
- feeds 及锁定版本；
- 设备 DTS、补丁和额外脚本；
- 构建命令、工具链版本和构建日期；
- 产物 SHA-256；
- 第三方组件清单和许可证；
- 真实设备验证记录。
