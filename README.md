Pcap_DNSProxy for OpenWrt
===

简介
---

 本项目是 [Pcap_DNSProxy][1] 运行在 OpenWrt 上的软件包  
 当前版本: 0.4.4.1  

特性
---

 参见原项目说明  

 可执行文件 `KeyPairGenerator` 和 `Pcap_DNSProxy`  

编译
---

 - 从 OpenWrt 的 [SDK][S] 编译

   ```bash
   # 以 ar71xx 平台为例，此处文件名为示例，以实际为准
   # 有对应平台的 SDK 即可编译软件包，不仅限于 ar71xx
   tar xjf OpenWrt-SDK-ar71xx-for-linux-x86_64-gcc-4.8-linaro_uClibc-0.9.33.2.tar.bz2
   cd OpenWrt-SDK-ar71xx-*
   # 获取 Makefile
   git clone https://github.com/wongsyrone/openwrt-Pcap_DNSProxy.git package/pcap-dnsproxy
   # 选择要编译的包 Network -> pcap-dnsproxy
   # 可选择是否编译 LibSodium 支持以及修改默认监听端口，注意不可使用 53 作为端口
   make menuconfig
   # 开始编译
   make package/pcap-dnsproxy/compile V=99
   ```

 - 从 OpenWrt 的代码树编译

 也可将本项目文件夹命名为 `pcap-dnsproxy` 直接放置于 OpenWrt 代码树的 `package` 文件夹下，之后按照编译的正常步骤进行，最后可在 bin 目录中找到编译好的软件包。

配置
---

 - Pcap_DNSProxy 主配置文件目录: `/etc/pcap-dnsproxy` 配置方法参见原[项目文档][2]  

 - Pcap_DNSProxy OpenWrt配置文件: `/etc/config/pcap-dnsproxy`  目前仅用于控制使能  

----------


  [1]: https://github.com/chengr28/Pcap_DNSProxy
  [2]: https://github.com/chengr28/Pcap_DNSProxy/tree/master/Documents
  [S]: http://wiki.openwrt.org/doc/howto/obtain.firmware.sdk
