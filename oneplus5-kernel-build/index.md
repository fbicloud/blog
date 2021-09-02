# 一加五内核编译

<!--more-->
{{< admonition warning >}}
*本次构建基于ubuntu 18.04*  
*编译器Arm gcc 10.3*  
*此源码暂未支持Clang*  
{{< /admonition >}}



## 依赖安装

```bash
sudo apt update -y
sudo apt install -y gcc g++ python make texinfo texlive bc bison build-essential ccache curl flex g++-multilib gcc-multilib git gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev liblz4-tool libncurses5-dev libsdl1.2-dev libssl-dev libwxgtk3.0-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev unzip language-pack-zh-hans
```

## 编译器下载

```bash
cd ~/
export GCC="gcc-arm-10.3-2021.07-x86_64-aarch64-none-linux-gnu"
wget "https://developer.arm.com/-/media/Files/downloads/gnu-a/10.3-2021.07/binrel/${GCC}.tar.xz"
xz -d ${GCC}.tar.xz
tar xvf ${GCC}.tar
rm -rf ${GCC}.tar
```

  


## 设置工具链环境变量

```bash
export ARCH="arm64"
export PATH="~/${GCC}/bin:$PATH"
export CROSS_COMPILE="aarch64-none-linux-gnu-"
```

  


## 克隆内核源码

```bash
git clone -b blu_spark-10 https://github.com/engstk/op5.git
```

  


## 编译

```bash
export args="-j16 \
    O=out"
    make ${args} blu_spark_defconfig
    make ${args}
```

{{< admonition >}}
blu_spark_defconfig在源码目录arch/arm64/configs/下，根据自己的源码来。
{{< /admonition >}}


