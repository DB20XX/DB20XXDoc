# 安装依赖

## 检查网络是否可用

打开一个终端，输入以下指令：

```bash
ping -c 4 mirror.sjtu.edu.cn
```
如果网络通畅，你应该看到如下的输出：

```
PING mirror.sjtu.edu.cn (111.186.58.212) 56(84) bytes of data.
64 bytes from 111.186.58.212 (111.186.58.212): icmp_seq=1 ttl=50 time=3.31 ms
64 bytes from 111.186.58.212 (111.186.58.212): icmp_seq=2 ttl=50 time=2.42 ms
64 bytes from 111.186.58.212 (111.186.58.212): icmp_seq=3 ttl=50 time=2.36 ms
64 bytes from 111.186.58.212 (111.186.58.212): icmp_seq=4 ttl=50 time=1.78 ms

--- mirror.sjtu.edu.cn ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 1.781/2.468/3.306/0.544 ms
```

## APT换源

apt是Debian系Linux中的一个命令行工具，用于管理软件包。它可以自动从互联网上下载并安装软件包，解决软件包之间的依赖关系，并升级已安装软件包。使用apt可以避免手动下载和安装软件包的繁琐过程，使软件包的管理更加简单、快捷和高效。apt的优点是操作简单、功能强大、自动依赖解决，是Linux系统中不可或缺的软件包管理工具之一。apt默认去海外的软件源下载软件包，由于GFW的存在，这个下载速度慢得令人发指，所以需要更换apt的软件源。
请自行依照[SJTU的指引](https://mirror.sjtu.edu.cn/docs/ubuntu)完成换源。

## 安装实验所需的工具和依赖


