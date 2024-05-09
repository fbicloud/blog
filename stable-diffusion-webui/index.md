# Stable Diffusion Webui部署

<!--more-->

## 需要的工具

- [x] Git
- [x] Miniconda3

## 克隆stable-diffusion-webui

```powershell
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git --depth=1
```



## Miniconda3配置

生成`.condarc`文件

```powershell
conda config --set show_channel_urls yes
```

编辑`.condarc`加入以下配置

```yaml
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

运行 `conda clean -i` 清除索引缓存，保证用的是镜像站提供的索引。

#### 创建虚拟环境

```
conda create -n stable-diffusion-webui python=3.10.6
```

#### 激活虚拟环境

```
conda activate stable-diffusion-webui
```

#### 更换pip源为国内

```powershell
python -m pip install -i https://pypi.tuna.tsinghua.edu.cn/simple --upgrade pip
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

#### 运行stable-diffusion-webui

```powershell
.\webui-user.bat
```

#### 启动参数

```
# webui-user.bat
# 修改局域网允许访问 安装插件
COMMANDLINE_ARGS=--listen --xformers --enable-insecure-extension-access
```


