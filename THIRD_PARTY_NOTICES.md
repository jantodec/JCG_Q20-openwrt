# 第三方许可与再分发说明

本文件说明本仓库原创内容与预编译二进制之间的许可边界。它不是法律意见；公开分发前请根据实际来源和适用法律复核。

## 许可范围

本仓库原创的说明文字、目录和仓库元数据采用 MIT 许可，见 `LICENSE`。

`pb-boot.img`、`底包-factory.bin`、`原系统升级刷入老毛子.bin` 和 `openwrt-02.01.2024-ramips-mt7621-jcg_q20-squashfs-sysupgrade.bin` 是第三方或预编译二进制，不从本仓库的 MIT 许可中获得重新授权。它们各自仍受上游版权和许可证约束。

## OpenWrt

OpenWrt 上游的 `COPYING` 文件写明：

```text
SPDX-License-Identifier: GPL-2.0-only
```

参考：[openwrt/openwrt COPYING](https://github.com/openwrt/openwrt/blob/main/COPYING)。

OpenWrt 镜像还包含大量第三方包，各包的许可证可能不同。仅链接 OpenWrt 上游仓库，通常不能替代对应源码、构建配置和许可证声明。

当前仓库没有保存 OpenWrt 镜像的完整对应源码和构建链。公开分发前应：

1. 确认每个镜像对应的源码和构建提交；
2. 提供 GPL 要求的对应源码，或采用许可证允许的源码提供方式；
3. 增加第三方包和许可证清单；
4. 不要把 MIT 许可描述为覆盖整个固件。

## Padavan

`原系统升级刷入老毛子.bin` 的镜像字符串指向 Padavan/hiboy 风格构建，但本仓库没有建立它与某个源码提交的对应关系。

公开参考：[tsl0922/padavan](https://github.com/tsl0922/padavan)。该仓库说明支持 JCG-Q20；本次检查没有在该仓库根目录找到 `LICENSE` 或 `COPYING` 文件，因此不能据此为这个二进制指定一个确定的许可证。

镜像内的可选脚本还会下载第三方组件，相关组件可能有各自的许可证。公开前应向构建者和脚本来源确认授权，并在 `SOURCES.md` 中记录版本。

## PandoraBox / pb-boot

`pb-boot.img` 的字符串包含：

- `PandoraBox-Boot Version 2.1`
- `PandoraBox Team`
- `D-Team Technology Co.,Ltd.`
- `Lenovo`

本仓库没有找到与这个二进制对应的可核验源码和许可证。不要把它重新标记为 MIT、GPL 或其他许可证，除非你能证明权利方已经这样授权。

## 底包

`底包-factory.bin` 是 OpenWrt Linux 5.10.138 镜像，但本仓库没有确认其具体源码和构建配方。它可能同时包含 OpenWrt 和第三方包，不能仅凭“底包”名称判断许可。

## 公开前的许可检查

- [ ] 确认每个二进制的权利方和上游许可证
- [ ] 保存对应源码、构建配置和补丁
- [ ] 为 OpenWrt 组件提供有效源码获取方式
- [ ] 为 Padavan/PandoraBox 组件取得明确授权或合规来源
- [ ] 增加第三方组件清单
- [ ] 在 README 中明确 MIT 只覆盖原创文档
- [ ] 不要用“开源”描述来源和构建链尚未补齐的二进制
