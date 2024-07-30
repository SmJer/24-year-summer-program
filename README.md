# 24 年暑期实训 （重要！**尝试部署前先查看 README 进行基础配置**）

> 此仓库为 24 年暑期实训小组内交流使用。

## 实训项目要求

### 前置条件

虚拟机镜像使用最新 Debian 12.6 镜像，镜像源使用清华源：

```shell
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware

# 以下安全更新软件源包含了官方源与镜像站配置，如有需要可自行修改注释切换
deb https://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware
# deb-src https://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware
```

VMware Workstation 版本使用当前最新 17.5.2 版本。

### 基础配置

新安装的虚拟机需要进行一系列设置以供后续使用。

1. 更改镜像源：

   修改 `/etc/apt/sources.list` 文件，将上述清华源配置替换进去即可。注意需注释源配置或删除原配置。

2. 安装基础软件包：

   ```shell
   apt update && apt install vim htop tree sudo dpkg wget curl locales git gnupg -y
   ```

3. 允许虚拟机使用 root 用户进行 SSH 登录：

   修改 `/etc/ssh/sshd_config` 文件中 `PermitRootLogin` 值为 `yes` 后重启 SSH 服务即可。

4. 修改系统字符集：

   系统默认字符为 `LC_ALL=C`，不支持中文且在一些设备上的支持性不佳，需要将其修该为兼容性更好的字符集如 `en_US.UTF-8`：

   ```shell
   apt install locales
   dpkg-reconfigure locales
   ```

   进行上述步骤后会进入可视化界面，向下翻页后选中 `en_US.UTF-8` 选项并回车，跳转页面后再次选中 `en_US.UTF-8` 即可进行下述步骤。

   修改 `~/.profile` 将文末 `LC_ALL=C` 等移除。将其替换为以下内容：

   ```shell
   export LANG=en_US.UTF-8
   export LANGUAGE=en_US.UTF-8
   export LC_ALL=en_US.UTF-8
   ```

   修改 `/etc/default/locale` 文件，将其内容替换为：

   ```shell
   LANG=en_US.UTF-8
   LANGUAGE=en_US.UTF-8
   LC_ALL=en_US.UTF-8
   ```

   配置上述完成后重启系统即可，使用 `locale` 查看系统当前字符集。

### 可选配置

1. 静态 IP 配置

2. 设置本机 DNS 服务器

### 项目目录

1. [配置 DHCP 服务](./01-配置%20DHCP%20服务.md)

2. [配置 DNS 服务](./02-配置%20DNS%20服务.md)

3. [配置 FTP 服务](./03-配置%20FTP%20服务.md)

4. [配置 Email 服务](./04-部署%20Email%20服务.md)

5. [部署 SSL 证书](./05-部署%20SSL%20证书.md)

6. [反向代理与负载平衡](./06-反向代理与负载平衡.md)

7. [部署 Ansible 自动化运维](./07-部署%20Ansible%20自动化运维.md)

8. [Zabbix 监控系统搭建](./08-Zabbix%20监控系统搭建.md)

9. [DevOps 开发运维结合](./09-DevOps%20开发运维结合.md)
