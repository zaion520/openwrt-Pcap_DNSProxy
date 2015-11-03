Pcap_DNSProxy for OpenWrt
===

[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/wongsyrone/openwrt-Pcap_DNSProxy?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[![Download Pcap_DNSProxy for OpenWrt Dist](https://img.shields.io/sourceforge/dt/pcap-dnsproxy-for-openwrt-dist.svg)](https://sourceforge.net/projects/pcap-dnsproxy-for-openwrt-dist/files/latest/download)
简介
---

 本项目是 [Pcap_DNSProxy][1] 运行在 OpenWrt 上的软件包  
 当前版本: 0.4.4.4-1  
 [预编译 IPK 下载][D]  

特性
---

 主要参见原项目说明  

 - 主要可执行文件 `Pcap_DNSProxy`，可选是否附带 `KeyPairGenerator`。  

 - 可选 LibSodium 和 LibPcap 依赖，其中 LibPcap 强烈建议勾选，LibSodium 根据原项目说明自行决定是否编译。  

 - 监听端口预置为 1053 ，可自行修改，注意不可使用 53 作为端口，会与 dnsmasq 相冲突导致 LAN 口不能分配 IP 等。  

 - 可以使用 `/etc/init.d/pcap-dnsproxy flush` 来清除本程序和 OpenWrt 系统的 DNS 缓存，使用 `/etc/init.d/pcap-dnsproxy libver` 查询链接的库版本，其余的 `/etc/init.d/pcap-dnsproxy {start|stop|enable|disable}` 与其他 OpenWrt 软件包的用法无异。  

编译
---

 - 从 OpenWrt 的 [SDK][S] 编译

   ```bash
   # 以 ar71xx 平台为例，此处文件名为示例，仅供参考，请以实际文件名为准
   # 有对应平台的 SDK 即可编译软件包，不仅限于 ar71xx
   tar xjf OpenWrt-SDK-ar71xx-for-linux-x86_64-gcc-4.8-linaro_uClibc-0.9.33.2.tar.bz2
   # 进入 SDK 根目录
   cd OpenWrt-SDK-ar71xx-*
   # 首先验证 SDK 是否需要 ccache
   cat .config | grep CONFIG_CCACHE
   # 如果返回结果为 "y"，则需要使用系统软件包管理器，如 yum、apt-get，安装 ccache
   # 接下来更新 feeds，因为编译需要 libsodium 和 libpcap
   ./scripts/feeds update -a
   ./scripts/feeds install -a
   # 获取 Makefile
   git clone https://github.com/wongsyrone/openwrt-Pcap_DNSProxy.git package/pcap-dnsproxy
   # 选择要编译的包 Network -> pcap-dnsproxy 并进行个人定制，或者保持默认
   # 这时根据提供的选项确认依赖已经被选中
   make menuconfig
   # 开始编译
   make package/pcap-dnsproxy/compile V=99
   # 编译结束之后从 bin 文件夹复制依赖库以及本程序的 ipk 文件到设备中，使用 opkg 进行安装
   ```

 - 从 OpenWrt 的代码树编译

 也可将本项目文件夹命名为 `pcap-dnsproxy` 直接放置于 OpenWrt 代码树的 `package` 文件夹下，之后按照编译的正常步骤进行，最后可在 bin 目录中找到编译好的软件包。下面简述编译步骤

   ```bash
   # 获取OpenWrt代码树，根据需求选择稳定版（如Chaos Calmer 15.05）或开发版Trunk
   # 如果是 Trunk 使用
   git clone git://git.openwrt.org/openwrt.git
   # 如果是 Chaos Calmer 15.05 稳定版使用
   git clone git://git.openwrt.org/15.05/openwrt.git
   # 进入代码树根目录
   cd openwrt
   # 接下来更新 feeds，因为编译需要 libsodium 和 libpcap
   ./scripts/feeds update -a
   ./scripts/feeds install -a
   # 获取 Makefile
   git clone https://github.com/wongsyrone/openwrt-Pcap_DNSProxy.git package/pcap-dnsproxy
   # 首先选择目标平台以及设备型号
   # 接下来选择要编译的包 Network -> pcap-dnsproxy 并进行个人定制，或者保持默认
   make menuconfig
   # 如果只想编译 pcap-dnsproxy 使用
   make package/pcap-dnsproxy/{prepare,compile} V=99
   # 如果想编译集成好 pcap-dnsproxy 的固件使用
   make V=99
   ```

 如果下载的 SDK 不能正常编译 pcap-dnsproxy，需要手动编译 SDK，首先在配置界面设置好目标平台和设备型号，接下来选择 Build the OpenWrt SDK，其他的设置都保持默认即可，最后运行 `make V=99`

注意
---

 1. 监听端口绝对不能是 53，会与 OpenWrt 的 dnsmasq 相冲突。  
 2. 由于目前官方 SDK 默认使用 GCC 4.8，故预编译软件包存在有些 Hosts 那边的正则表达式不能使用的问题，请周知。  
 3. 使用 SDK 编译之前验证 SDK 是否需要 ccache。  
 4. 如果 SDK 的文件名注明 GCC 版本为 4.8，由于该版本的 GCC 对 STL 的正则表达式支持不完整，会导致有些 Hosts 那边的正则表达式用不了，如果确实需要使用正则表达式，请使用 GCC 4.9 或以上版本编译。  
 5. 如果下载的 SDK 不能编译本项目，首先尝试手动编译 SDK，一般都可以解决问题了；否则尝试从 OpenWrt 的代码树编译。  

配置
---

 - Pcap_DNSProxy 主配置文件目录: `/etc/pcap-dnsproxy` 配置方法参见原[项目文档][2]  

 - Pcap_DNSProxy OpenWrt配置文件: `/etc/config/pcap-dnsproxy`  目前仅用于控制使能  

反馈
---

 - 使用上面的 Gitter 图标进入聊天室留言，我会定期查看

 - 使用 Issue 功能进行反馈，注意贴日志要贴全，如果日志行数太多，使用外链

----------


  [1]: https://github.com/chengr28/Pcap_DNSProxy
  [2]: https://github.com/chengr28/Pcap_DNSProxy/tree/master/Documents
  [D]: https://sourceforge.net/projects/pcap-dnsproxy-for-openwrt-dist/files/
  [S]: http://wiki.openwrt.org/doc/howto/obtain.firmware.sdk
