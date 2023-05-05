# 安装依赖并获取源代码

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

为了方便后续操作，请**务必**将所有的`deb-src`镜像取消注释。

## 安装实验所需的工具和依赖

如果上一步操作你没有将所有`deb-src`镜像取消注释，再次强调，请务必取消。
打开终端，输入以下指令。
```bash
sudo apt build-dep -y mysql-server-8.0
sudo apt install -y git
```

## 配置git
以下内容节选并改编自Github官方文档[Duplicating a repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/duplicating-a-repository)。

1. 打开[这个页面](https://github.com/new)，在你自己的Github账户下创建一个新仓库。并且务必将可见性设为**Private**，确保你的代码只有你自己能看到。
2. 打开终端，输入以下指令。
```bash
git clone --bare https://github.com/FLAYhhh/DB20XX.git db20xx-public
git checkout bplustree_index
```
3. 然后将公共的DB20XX代码仓库[mirror](https://git-scm.com/docs/git-push#Documentation/git-push.txt---mirror)到你的私有仓库。假设你的Github用户名是`student`，私有仓库名是`DB20XX-private`。在刚刚的终端里继续输入以下指令：
```bash
cd db20xx-public
# 如果你使用https进行push / pull
git push https://github.com/student/DB20XX-private.git main
# 如果你使用ssh进行push / pull
git push git@github.com:student/DB20XX-private.git main
```
现在公共代码已经被复制到你的私有仓库里了，可以删掉公共代码了。
```bash
cd ..
rm -rf db20xx-public
```
4. 将你的私有仓库克隆下来
```bash
# 如果你使用https进行push / pull
git clone https://github.com/student/DB20XX-private.git
# 如果你使用ssh进行push / pull
git clone git@github.com:student/DB20XX-private.git
```

5. 将公共的DB20XX代码仓库设为私有仓库的上游，以便跟随公共仓库的更新。
在终端中进入你的代码仓库。
输入以下的指令
```bash
git remote add public https://github.com/FLAYhhh/DB20XX.git
```
你可以通过下面的指令查看当前仓库有几个远程源
```bash
git remote -v
# 以下是输出
origin https://github.com/student/DB20XX-private.git (fetch)
origin https://github.com/student/DB20XX-private.git (push)
public https://github.com/FLAYhhh/DB20XX.git (fetch)
public https://github.com/FLAYhhh/DB20XX.git (push)
```
然后，配置你的用户名和邮箱，这步请务必要做，验收时会查看git log是否符合要求。用户名设为"名字拼音 姓拼音"，例如：张三，用户名配置为"San Zhang"。邮箱则配置为你的学校公邮。
```bash
git config --local user.name "San Zhang" # 用户名
git config --local user.email "xxxxxxx@stu.ecnu.edu.cn" #邮箱
```

6. 如果公共代码仓库更新了，你可以通过下面的指令获取更新
```bash
git pull public main
```
