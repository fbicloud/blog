# 国内Linux下配置esp-idf开发环境


<!--more-->

## 依赖安装

```bash
sudo apt update -y
sudo apt-get install git wget make libncurses-dev flex bison gperf python python-is-python2 python3-pip cmake -y
```

{{< admonition tip "提示" open >}}

觉得慢的可以把源换成中科大或者阿里云的

{{< /admonition >}}

​    

## 设置pip源为阿里云

```bash
mkdir ~/.pip
cat > ~/.pip/pip.conf << EOF
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/
[install]
trusted-host=mirrors.aliyun.com
disable-pip-version-check = true
timeout = 60
EOF
```



​    

## 使用Github国内镜像

```bash
# 使用镜像
git config --global url."https://hub.fastgit.org/".insteadof https://github.com/
# 取消镜像
git config --global --unset url.https://hub.fastgit.org/.insteadof
```

​    

## 获取`esp-idf`

```bash
mkdir ~/esp
cd ~/esp
git clone --recursive https://github.com/espressif/esp-idf.git
cd ~/esp/esp-idf
# 设置python3为默认
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2 100
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 150
# 国内使用使用乐鑫的下载服务器
export IDF_GITHUB_ASSETS=dl.espressif.com/github_assets
./install.sh
~/.espressif/python_env/idf4.4_py3.8_env/bin/python -m pip install --upgrade pip
```

​    

## 设置环境变量

```bash
# 临时环境变量
. ~/esp/esp-idf/export.sh
# 持久化环境变量
echo ". ~/esp/esp-idf/export.sh" >> /etc/profile.d/esp-idf.sh
```

​    
  


