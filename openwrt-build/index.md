# Openwrt编译笔记

<!--more-->
## 依赖安装
```bash
sudo apt update -y && sudo apt upgrade -y
sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib \
git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev \
libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libreadline-dev libssl-dev libtool lrzsz \
mkisofs msmtp nano ninja-build p7zip p7zip-full patch pkgconf python2.7 python3 python3-pip libpython3-dev qemu-utils \
rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl unzip vim wget xmlto xxd zlib1g-dev
```

​    


## 自定义

### 修改第一次启动初始化配置

```bash
touch package/base-files/files/etc/uci-defaults/99_custom
vim package/base-files/files/etc/uci-defaults/99_custom
```
### 保留配置文件升级编译
```bash
./scripts/diffconfig.sh > x86.config
git pull
./scripts/feeds update -a
./scripts/feeds install -a
rm -rf ./tmp && rm -rf .config
cat x86.config > .config
#make menuconfig
make defconfig
make V=s -j$(nproc)
```
### 旁路由
```bash
kmod-br-netfilter
```


