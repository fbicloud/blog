# Debian11安装笔记

<!--more-->

## 使用国内apt源

```bash
cat > /etc/apt/sources.list << EOF
deb https://mirrors.ustc.edu.cn/debian/ bullseye main contrib non-free
deb-src https://mirrors.ustc.edu.cn/debian/ bullseye main contrib non-free

deb https://mirrors.ustc.edu.cn/debian/ bullseye-updates main contrib non-free
deb-src https://mirrors.ustc.edu.cn/debian/ bullseye-updates main contrib non-free

deb https://mirrors.ustc.edu.cn/debian/ bullseye-backports main contrib non-free
deb-src https://mirrors.ustc.edu.cn/debian/ bullseye-backports main contrib non-free

deb https://mirrors.ustc.edu.cn/debian-security/ bullseye-security main contrib non-free
deb-src https://mirrors.ustc.edu.cn/debian-security/ bullseye-security main contrib non-free
EOF
apt update -y && apt upgrade -y
```

​    


## 配置sudo

```bash
apt install sudo -y
cat > /etc/sudoers.d/username << EOF
username ALL=(ALL) NOPASSWD:ALL
EOF
```

​    

## 配置网络

```bash
sudo vim /etc/network/interfaces
# 重启
sudo systemctl restart networking
```

​    


## 设置时区

```bash
# 查看时区，有 CST 正确
date

# 设置
sudo timedatectl set-timezone Asia/Shanghai

# 或者使用向导选择
tzselect
```

​    


## shell自定义

```bash
命令自动补全忽略大小写
echo 'set completion-ignore-case on' >> ~/.inputrc

修改 vmrc（vim 配置文件）
为当前用户创建 ~/.vimrc，内容参看上述 “配置 vi”

为将 .vimrc 添加到默认用户配置文件 cp ~/.vimrc /etc/skel/.vimrc

ll 常规版
一般 Linux 中默认定义了 ll 别名，但参数比较少，需要使用更加强大的 ll 别名。
Debian 默认并没有定义 ll 别名。
写入环境变量（当前用户优先执行）：
bash：
echo 'alias ll="ls -lahF --color=auto --time-style=long-iso"' >> ~/.bashrc

高级版 ls：以数字显示权限
这里我们把命令叫做 lll
命令：
ls -lahF --color=auto --time-style=long-iso | awk '{k=0;s=0;for(i=0;i<=8;i++){k+=((substr($1,i+2,1)~/[rwxst]/)*2^(8-i))}j=4;for(i=4;i<=10;i+=3){s+=((substr($1,i,1)~/[stST]/)*j);j/=2}if(k){printf("%0o%0o ",s,k)}print}'

创建文件
在使用 cat EOF 中出现 $ 变量通常会直接被执行，显示执行的结果。若想保持 $ 变量不变需要使用 \ 符进行注释。

# 如果非 root 用户，切换到 root
sudo -i
cat > /usr/local/bin/lll <<EOF
#!/bin/bash
ls -lahF --color=auto --time-style=long-iso | awk '{k=0;s=0;for(i=0;i<=8;i++){k+=((substr(\$1,i+2,1)~/[rwxst]/)*2^(8-i))}j=4;for(i=4;i<=10;i+=3){s+=((substr(\$1,i,1)~/[stST]/)*j);j/=2}if(k){printf("%0o%0o ",s,k)}print}'
EOF

# 赋予执行权限：
chmod +x /usr/local/bin/lll

# 如果非 root 用户，执行完毕退出
exit

写入环境变量（可选配置，默认不需要）：
bash
echo 'alias lll="/usr/local/bin/lll"' >> ~/.bashrc
```

​    


## 防火墙

```bash
sudo apt install -y nftables
sudo systemctl enable nftables.service

#清空当前规则集：
nft flush ruleset
#查询当前规则集：
nft list ruleset
#添加一个表：
nft add table inet filter
 
#添加input、forward和output三个基本链。input和forward的默认策略是drop。output的默认策略是accept。
nft add chain inet filter input { type filter hook input priority 0 \; policy drop \; }
nft add chain inet filter forward { type filter hook forward priority 0 \; policy drop \; }
nft add chain inet filter output { type filter hook output priority 0 \; policy accept \; }
 
#添加两个与TCP和UDP关联的常规链：
nft add chain inet filter TCP
nft add chain inet filter UDP
#related和established的流量会accept：
nft add rule inet filter input ct state related,established accept
#loopback接口的流量会accept：
nft add rule inet filter input iif lo accept
#无效的流量会drop：
nft add rule inet filter input ct state invalid drop
#新的echo请求（ping）会accept：
nft add rule inet filter input ip protocol icmp icmp type echo-request ct state new accept
#新的UDP流量跳转到UDP链：
nft add rule inet filter input ip protocol udp ct state new jump UDP
#新的TCP流量跳转到TCP链：
nft add rule inet filter input ip protocol tcp tcp flags \& \(fin\|syn\|rst\|ack\) == syn ct state new jump TCP
 
#未由其他规则处理的所有通信会reject：
nft add rule inet filter input ip protocol udp reject
nft add rule inet filter input ip protocol tcp reject with tcp reset
nft add rule inet filter input counter reject with icmp type prot-unreachable
 
#web服务器的连接端口80:
nft add rule inet filter TCP tcp dport 80 accept
#打开web服务器HTTPS连接端口443：
nft add rule inet filter TCP tcp dport 443 accept
#允许SSH连接端口22:
nft add rule inet filter TCP tcp dport 22 accept
 
 
 
#NAT
#删除规则表 dnat1
#nft delete table ip dnat1 
 
#增加规则表 dnat1
nft add table ip dnat1 
 
#在表dnat1中增加一条链 prerouting [SNAT]
nft add chain dnat1 prerouting { type nat hook prerouting priority 0 \;}
 
#在表dnat1中增加一条链 postrouting [DNAT]
nft add chain dnat1 postrouting { type nat hook postrouting priority 100 \; }
 
#
nft add rule dnat1 prerouting ip daddr 192.168.1.1 tcp dport 80 counter dnat 172.16.1.2:80
nft add fule dnat1 postrouting ip daddr 172.16.1.2 tcp dport 80 counter snat to 172.16.1.1 
#
nft add rule dnat1 prerouting ip saddr 172.16.1.2 tcp sport 80 counter dnat 192.168.1.2
nft add rule dnat1 postrouting ip saddr 172.16.1.2 tcp sport 80 counter snat to 192.168.1.1
```


