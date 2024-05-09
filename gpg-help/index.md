# GnuPG学习笔记

<!--more-->

## 前言

GNU Privacy Guard（GnuPG或GPG）是一个密码学软件，用于加密、签名通信内容及管理非对称密码学的密钥。GnuPG是自由软件，遵循IETF订定的OpenPGP技术标准设计，并与PGP保持兼容。

 ***{{< link "https://www.gnupg.org/" GnuPG主页 "去看看" >}}***

## 安装

```bash
sudo apt install gpg -y
```

​    

## 生成密钥对

```bash
ubuntu@ubuntu:~$ gpg --full-gen-key
gpg (GnuPG) 2.2.19; Copyright (C) 2019 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

请选择您要使用的密钥类型：
   (1) RSA 和 RSA （默认）
   (2) DSA 和 Elgamal
   (3) DSA（仅用于签名）
   (4) RSA（仅用于签名）
  (14) Existing key from card
您的选择是？ 4
# 这里选择4
RSA 密钥的长度应在 1024 位与 4096 位之间。
您想要使用的密钥长度？(3072) 4096
请求的密钥长度是 4096 位
# 密钥长度拉满
请设定这个密钥的有效期限。
         0 = 密钥永不过期
      <n>  = 密钥在 n 天后过期
      <n>w = 密钥在 n 周后过期
      <n>m = 密钥在 n 月后过期
      <n>y = 密钥在 n 年后过期
密钥的有效期限是？(0) 0
# 这里坑定选择永不过期
密钥永远不会过期
这些内容正确吗？ (y/N) y
# 别问 问就是没问题
GnuPG 需要构建用户标识以辨认您的密钥。

真实姓名： test1
电子邮件地址： a@admin.com
注释： test1
您选定了此用户标识：
    “test1 (test1) <a@admin.com>”

更改姓名（N）、注释（C）、电子邮件地址（E）或确定（O）/退出（Q）？ o
我们需要生成大量的随机字节。在质数生成期间做些其他操作（敲打键盘
、移动鼠标、读写硬盘之类的）将会是一个不错的主意；这会让随机数
发生器有更好的机会获得足够的熵。
gpg: 密钥 7C4A96E3BBC9744C 被标记为绝对信任
gpg: 吊销证书已被存储为‘/home/ubuntu/.gnupg/openpgp-revocs.d/454039FDD30010C960EB66287C4A96E3BBC9744C.rev’
公钥和私钥已经生成并被签名。

请注意这个密钥不能用于加密。您可能想要使用“--edit-key”命令来
生成一个用于此用途的子密钥。
pub   rsa4096 2021-08-29 [SC]
      454039FDD30010C960EB66287C4A96E3BBC9744C
uid                      test1 (test1) <a@admin.com>
```

{{< admonition >}}

通过上面的操作，生成了一个名为`test1`，使用`RSA`加密 且永不过期的账户

`454039FDD30010C960EB66287C4A96E3BBC9744C`为密钥id

{{< /admonition >}}

​    

## 创建子密钥

***我认为主密钥应仅用于管理子密钥，子密钥用来加密解密签名操作***

首先看一下我们刚刚创建的密钥

```bash
ubuntu@ubuntu:~$ gpg -K
# k为小写时列出公钥 k为大写时列出私钥
/home/ubuntu/.gnupg/pubring.kbx
-----------------------------
sec   rsa4096 2021-08-29 [SC]
      454039FDD30010C960EB66287C4A96E3BBC9744C
uid           [ 绝对 ] test1 (test1) <a@admin.com>

```

{{< admonition >}}

| Option |           Description            |
| :----: | :------------------------------: |
|  uid   |      拥有者信息，姓名和邮箱      |
|  pub   |         public key 公钥          |
|  sub   |       public subkey 子公钥       |
|  sec   |         secret key 私钥          |
|  ssb   |       secret subkey 子私钥       |
|   S    |      sign 表示可以用来签名       |
|   C    | Certify 表示可以用于认证其他密钥 |
|   E    |     Encrypt 表示可以用来加密     |
|   A    | Authentication 表示可以用来认证  |

{{< /admonition >}}

​    

### 创建两个子密钥 一个用来加密 一个用来签名

```bash
ubuntu@ubuntu:~$ gpg --edit-key test1
gpg (GnuPG) 2.2.19; Copyright (C) 2019 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

私钥可用。

sec  rsa4096/7C4A96E3BBC9744C
     创建于：2021-08-29  有效至：永不       可用于：SC  
     信任度：绝对        有效性：绝对
[ 绝对 ] (1). test1 (test1) <a@admin.com>

gpg> addkey
# addkey是创建子密钥的意思
请选择您要使用的密钥类型：
   (3) DSA（仅用于签名）
   (4) RSA（仅用于签名）
   (5) ElGamal（仅用于加密）
   (6) RSA（仅用于加密）
  (14) Existing key from card
您的选择是？ 4
# 选择4 仅用于签名
RSA 密钥的长度应在 1024 位与 4096 位之间。
您想要使用的密钥长度？(3072) 4096
请求的密钥长度是 4096 位
请设定这个密钥的有效期限。
         0 = 密钥永不过期
      <n>  = 密钥在 n 天后过期
      <n>w = 密钥在 n 周后过期
      <n>m = 密钥在 n 月后过期
      <n>y = 密钥在 n 年后过期
密钥的有效期限是？(0) 0
密钥永远不会过期
这些内容正确吗？ (y/N) y
真的要创建吗？(y/N) y
我们需要生成大量的随机字节。在质数生成期间做些其他操作（敲打键盘
、移动鼠标、读写硬盘之类的）将会是一个不错的主意；这会让随机数
发生器有更好的机会获得足够的熵。

sec  rsa4096/7C4A96E3BBC9744C
     创建于：2021-08-29  有效至：永不       可用于：SC  
     信任度：绝对        有效性：绝对
ssb  rsa4096/17B455B1D1CF2DC4
     创建于：2021-08-29  有效至：永不       可用于：S   
[ 绝对 ] (1). test1 (test1) <a@admin.com>

gpg> addkey
请选择您要使用的密钥类型：
   (3) DSA（仅用于签名）
   (4) RSA（仅用于签名）
   (5) ElGamal（仅用于加密）
   (6) RSA（仅用于加密）
  (14) Existing key from card
您的选择是？ 6
# 选择6 仅用于加密
RSA 密钥的长度应在 1024 位与 4096 位之间。
您想要使用的密钥长度？(3072) 4096
请求的密钥长度是 4096 位
请设定这个密钥的有效期限。
         0 = 密钥永不过期
      <n>  = 密钥在 n 天后过期
      <n>w = 密钥在 n 周后过期
      <n>m = 密钥在 n 月后过期
      <n>y = 密钥在 n 年后过期
密钥的有效期限是？(0) 0
密钥永远不会过期
这些内容正确吗？ (y/N) y
真的要创建吗？(y/N) y
我们需要生成大量的随机字节。在质数生成期间做些其他操作（敲打键盘
、移动鼠标、读写硬盘之类的）将会是一个不错的主意；这会让随机数
发生器有更好的机会获得足够的熵。

sec  rsa4096/7C4A96E3BBC9744C
     创建于：2021-08-29  有效至：永不       可用于：SC  
     信任度：绝对        有效性：绝对
ssb  rsa4096/17B455B1D1CF2DC4
     创建于：2021-08-29  有效至：永不       可用于：S   
ssb  rsa4096/77C63F3A7A718DE9
     创建于：2021-08-29  有效至：永不       可用于：E   
[ 绝对 ] (1). test1 (test1) <a@admin.com>

gpg> save
# 一定要记得保存

```

​    

## 导出
### 导出公钥

```bash
gpg -o pub.key -a --export test1
```

​    

### 导出私钥

```bash
gpg -o sec.key -a --export-secret-keys test1
```

​    

### 导出全部子私钥

```bash
gpg -o ssb.key -a --export-secret-subkeys test1
```

​    

### 导出单个子私钥

```bash
gpg -o ssb.key -a --export-secret-subkey 私钥id! # 感叹号是必要的
```

​    

### 导出吊销证书

```bash
gpg --output revoke.asc --gen-revoke mykey
```

{{< admonition >}}
参数`mykey`必须是密钥说明符，可以是主密钥对的密钥 ID，也可以是标识密钥对的用户 ID 的任何部分。生成的证书将保留在文件 `revoke.asc` 中。如果省略`--output`选项，结果将放在标准输出上。由于证书很短，您可能希望打印证书的硬拷贝以存放在安全的地方，例如您的保险箱。证书不应存储在其他人可以访问的地方，因为任何人都可以发布吊销证书并使相应的公钥无用。
{{< /admonition >}}

​    

## 导入
### 导入公钥

```bash
gpg --import pub.key
```

​    

### 导入私钥

```bash
gpg --import sec.key
```

{{< admonition >}}

`gpg -K`时密钥后面的`#`表示主密钥不在本地

{{< /admonition >}}

​    
## 删除
### 删除公钥

```bash
gpg --delete-key test1
```

​    

### 删除私钥

```bash
gpg --delete-secret-key test1
```

​    

### 删除所有

```bash
gpg --delete-keys test1
```


