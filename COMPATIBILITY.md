# 兼容范围

## 目标设备

- 设备：JCG Q20
- 目标平台：`ramips/mt7621`
- 本说明依据：仓库原教程、镜像元数据和本次静态检查

本次文档整理没有新增真实设备刷机验证。原教程记录的成功路径不等同于对所有硬件批次和原厂固件版本的保证。

## 镜像对应关系

| 镜像 | 目标/元数据 | 预期用途 |
|---|---|---|
| `pb-boot.img` | `NAND Flash I`，PandoraBox-Boot 2.1 | 进入 pb-boot 恢复环境 |
| `底包-factory.bin` | MIPS OpenWrt Linux 5.10.138 | pb-boot 后刷入的底包 |
| `原系统升级刷入老毛子.bin` | `JCG-Q20`，Linux 3.4.113 | 原厂系统直接刷入 Padavan 的备用路径 |
| `openwrt-...-sysupgrade.bin` | board `jcg_q20`，target `ramips/mt7621`，kernel 5.15.147 | 兼容底包/OpenWrt 中的 sysupgrade |

## 支持范围

### 记录支持的路径

- JCG Q20 原厂系统 -> `pb-boot.img`
- pb-boot -> `底包-factory.bin`
- 兼容的极简 OpenWrt -> `openwrt-...-sysupgrade.bin`
- JCG Q20 原厂系统 -> `原系统升级刷入老毛子.bin`，作为备用路径

### 不保证

- 其他 JCG 型号；
- 其他 MT7621 路由器；
- 不同硬件版本或不同 Flash 布局的 JCG Q20；
- 不同原厂固件版本；
- 在 pb-boot 中直接上传 sysupgrade 镜像；
- 在 OpenWrt 中上传 Padavan 镜像；
- 在 Padavan 中上传 OpenWrt sysupgrade 镜像。

如果设备型号、板卡标识、Flash 容量或原厂升级页面与教程不一致，应停止操作，先确认硬件和恢复方式。

## 版本边界

主 OpenWrt 镜像的元数据是 `23.05-SNAPSHOT`、revision `02.01.2024`。这不是 OpenWrt 官方稳定版发布标签，也不能自动等同于某个上游提交。需要复现或长期维护时，请以 `SOURCES.md` 中列出的源码、feed 和构建配置为准。
