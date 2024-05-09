# 使用zstd算法压缩文件

<!--more-->
***{{< link "https://github.com/facebook/zstd" 项目主页 "去看看" >}}***


## linux下安装

```bash
git clone https://github.com/facebook/zstd.git zstd
cd zstd
make install
```

## 参数
```bash
zstd --help
使用方式 :
      zstd [args] [FILE(s)] [-o file]

参数选项 :
 -#     : 压缩级别(1-19，默认值为3)
 -d     : 解压
 -D file: 使用文件作为字典
 -o file: 结果存储在文件中
 -f     : 在没有提示的情况下覆盖输出并(解压)压缩链接
--rm    : 成功解压缩后删除源文件
 -k     : 保存源文件(默认)
 -h/-H  : 显示帮助/长帮助并退出

高级选项 :
 -V     : 显示版本号并退出
 -v     : 详细模式
 -q     : 静默输出
 -c     : 强制写入标准输出
 -l     : 输出zstd压缩包中的信息
--ultra : 启用超过19级，最多22级(需要更多内存)
 -T#    : 使用几个线程进行压缩(默认值:1个)
 -r     : 递归地操作目录
--format=gzip : 将文件压缩为.gz格式
 -M#    : 为解压设置内存使用限制

字典生成器 :
--train ## : 从一组训练文件中创建一个字典
--train-cover[=k=#,d=#,steps=#] : 使用带有可选参数的cover算法
--train-legacy[=s=#] : 有选择性地使用遗留算法(默认值:9)
 -o file : “file”是字典名(默认:字典)
--maxdict=# : 将字典限制为指定大小(默认值:112640)
--dictID=# : 强制字典ID为指定值(默认:随机)

性能测试参数 :
 -b#    : 基准测试文件，使用#压缩级别(默认为1)
 -e#    : 测试从-bX到#的所有压缩级别(默认值:1)
 -i#    : 最小计算时间(秒)(默认为3s)
 -B#    : 将文件切成大小为#个独立块(默认:无块)
--priority=rt : 将进程优先级设置为实时
```
