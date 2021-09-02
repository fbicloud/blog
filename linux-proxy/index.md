# Linux设置代理

<!--more-->
## linux终端设置代理

```bash
export http_proxy=http://192.168.123.3:7890
export https_proxy=http://192.168.123.3:7890
```
取消
```bash
export http_proxy=
export https_proxy=
```

  


## git设置代理

```bash
git config --global http.proxy http://192.168.123.3:7890
git config --global https.proxy http://192.168.123.3:7890
```

取消
```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```
