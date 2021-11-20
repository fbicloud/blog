# win git使用gpg


<!--more-->

```powershell
#设置key id
git config --global user.signingkey "your key id"
#启用commit gpg加密
git config --global commit.gpgsign true
#修改默认gpg路径
git config --global gpg.program "c:/Program Files (x86)/GNU/GnuPG/gpg2.exe" 
```


