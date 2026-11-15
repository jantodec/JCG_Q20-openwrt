# 默认安全配置说明

本文件说明镜像中已经发现的默认密码、开放服务和静态材料，以及首次启动后的处理建议。这里只做说明，不修改现有二进制。

## OpenWrt 主镜像

### root 密码

`/etc/uci-defaults/99-default-settings` 包含：

```sh
(echo "root"; sleep 1; echo "root") | /bin/busybox passwd root
```

因此首次启动脚本会把 `root` 密码设置为 `root`。镜像里的 `/etc/shadow` 初始内容为空，但首次启动后不能按空密码理解。

### Dropbear

`/etc/config/dropbear` 设置：

```text
PasswordAuth on
RootPasswordAuth on
Port 22
```

启动脚本还会把 Dropbear 绑定到 `lan`。这降低了 WAN 直接暴露的概率，但如果 LAN 中存在不可信设备，默认 root/root 仍然有风险。

### Wi-Fi

初始化脚本中 `SSID_PASSWD=""`，随后在密码为空时设置：

```text
encryption='none'
```

因此默认无线网络可能是开放网络。首次启动后应立即设置 WPA2/WPA3 密码。

### UPnP

初始化脚本多次设置：

```text
upnpd.config.enabled='1'
```

不需要自动端口映射时应关闭 UPnP。

### uHTTPd

`/etc/config/uhttpd` 默认监听：

- `0.0.0.0:80`
- `[::]:80`
- `0.0.0.0:443`
- `[::]:443`

首次启动脚本还会设置：

```text
uhttpd.main.rfc1918_filter=0
uhttpd.main.redirect_https=0
```

防火墙默认规则通常会拒绝 WAN 输入，但不要把这一点当作唯一保护。不要从公网暴露 LuCI 或其他管理端口。

### Nginx 静态材料

镜像内存在：

- `/etc/nginx/conf.d/_lan.key`：EC 私钥；
- `/etc/nginx/conf.d/_lan.crt`：自签证书；
- `/etc/nginx/htpasswd.conf`：一个旧的 `root` Basic Auth 哈希；
- `/etc/config/nginx`：引用了上述证书和私钥。

当前 rootfs 中没有发现 Nginx 可执行文件、init 脚本或启动链接，所以这些材料不一定是默认启用的服务。但只要镜像被公开，静态私钥和认证哈希就不应继续复用。启用 Nginx 前应删除旧材料并在设备上重新生成唯一密钥。

## pb-boot

`pb-boot.img` 的默认环境包含：

- `ethaddr=00:0C:43:30:52:11`
- `telnet_en=1`
- `uart_en=1`

固定 MAC 可能是引导器的默认环境，不代表设备真实 MAC。`telnet_en=1` 也不等于 Telnet 一定在某个接口上监听；需要在真实设备上确认接口、认证和访问范围。不要把 pb-boot 页面暴露给不可信网络。

## Padavan 镜像

本次审查没有确认 Padavan 镜像的默认管理账号和密码。镜像内包含可选的 SSH、Telnet、代理、DDNS、远程管理和运行时下载脚本，默认服务范围会随配置变化。

首次启动后建议：

1. 修改管理密码；
2. 关闭不需要的 Telnet、SSH 和 WAN 管理；
3. 检查可选扩展是否会自动下载或启动；
4. 不要把管理页面暴露到公网。

## 首次启动后的最低操作

- [ ] 修改 root/管理密码
- [ ] 启用 WPA2/WPA3，关闭开放 Wi-Fi
- [ ] 不需要时关闭 UPnP
- [ ] 禁止 WAN 访问 LuCI、SSH、Telnet
- [ ] 检查监听端口和运行服务
- [ ] 删除或轮换镜像内的静态证书、私钥和认证哈希
- [ ] 保留一份可恢复的原厂固件和配置
