# 微软远程桌面开启显卡加速

<!--more-->
## 显卡加速

> win+r打开运行 输入`gpedit.msc`  
> 依次找到**计算机配置->管理模板->Windows组件->远程桌面服务->远程桌面会话主机->远程会话环境**  
> 在右边选择**将硬件图形适配器应用于所有远程桌面服务会话**  
> 右键编辑，选择已启用确定保存。  
> 重启远程主机。  
> 现在可以在远程桌面里运行需要GPU支持的应用了

## 提升传输帧率

> RDP默认的帧率是30，可以设置为60帧传输  
> 实际效果取决于客户端设置、网络环境等等因素  
> 打开远程主机上的注册表编辑器 win+r输入`regedit`  
> 找到`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations`   
> 在空白处右键->新建->DWORD（32位）值，命名为`DWMFRAMEINTERVAL`  
> 双击刚添加的这一项，**基数**选择为**十进制**，**数值数据**填写`15`  
> 确定保存，重启生效 

## Nvidia优化

> 运行nvidiaopenglrdp.exe
