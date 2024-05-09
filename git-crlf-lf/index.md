# 解决win10下Git自动转换换行符

<!--more-->
windows系统的换行方式`CRLF: "\r\n"`

Linux系统的换行方式`LF: "\n"`

在你使用git的时候，git会自动将代码当中与你当前系统不同的换行方式转化成你当前系统的换行方式，从而造成冲突。  


## 解决方法

```bash
git config --global core.autocrlf false
```


