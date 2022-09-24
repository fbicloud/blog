# Esxi安装笔记

<!--more-->

## 用到的工具

***{{< link "https://github.com/VFrontDe/ESXi-Customizer-PS" ESXi-Customizer-PS"去看看" >}}***

***{{< link "https://customerconnect.vmware.com/cn/downloads/info/slug/datacenter_cloud_infrastructure/vmware_vsphere/7_0" ESXi安装镜像"去看看" >}}***

***{{< link "https://flings.vmware.com/community-networking-driver-for-esxi#summary" 社区网卡驱动"去看看" >}}***

***{{< link "https://kb.vmware.com/s/article/2091560?lang=zh_cn" 修改网卡排列顺序"去看看" >}}***
    

## 封装驱动

```powershell
 .\ESXi-Customizer-PS.ps1 -izip .\VMware-ESXi-7.0U3f-20036589-depot.zip -pkgDir .\drive\
```


