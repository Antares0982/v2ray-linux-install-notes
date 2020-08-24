# v2ray-linux-install-notes

参考https://www.imcaviare.com/2018-12-18-1/

适用于国内linux服务器，下载github来源的v2ray速度极慢或下载停止的问题。

用root权限下载安装脚本：
```
curl -O https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh
curl -O https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-dat-release.sh
```
运行脚本
```
bash install-release.sh
```

需要等待10分钟。

安装完成后，还需要安装polipo，将网络请求转到代理上访问。这里参考：[polipo/privoxy 实现 Linux 系统全局/自动代理](https://juejin.im/post/6844903813393055751)。

修改文件`/etc/polipo/config`
```
nano /etc/polipo/config
```
并添加以下内容（第一个端口填v2ray端口）：
```
# SS 的代理地址
socksParentProxy = "127.0.0.1:1080"
# 类型
socksProxyType = socks5
# 转换为 HTTP 之后的端口
proxyPort = 8123
# 下面的就不清楚了
chunkHighMark = 50331648
objectHighMark = 16384
serverMaxSlots = 64
serverSlots = 16
serverSlots1 = 32
proxyAddress = "0.0.0.0"
```
之后修改`/etc/profile`
```
nano /etc/profile
```
在最后添加两行
```
export http_proxy="http://127.0.0.1:8123"
export https_proxy="http://127.0.0.1:8123"
```
重载配置：
```
source /etc/profile
```
测试
```
curl www.google.com
```
在使用完成后应该删除`/etc/polipo/config`以及`/etc/profile`中多余的内容。然后再次重载。并且
```
systemctl disable polipo
systemctl stop polipo
```



