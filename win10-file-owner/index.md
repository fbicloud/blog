# win10批量修改文件所有者

<!--more-->

修改当前目录下所有文件所有者为当前用户

```powershell
takeown /f * /r /d y
```


