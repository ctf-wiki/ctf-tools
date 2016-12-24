# 环境搭建

## Kali Linux

* [官网](https://www.kali.org)
* [安装镜像下载（中科大）](http://mirrors.ustc.edu.cn/kali-images/)
* [虚拟机镜像下载（官网）](https://www.offensive-security.com/kali-linux-vmware-virtualbox-image-download/)

## Java

* [JRE 8u111 Windows x86](http://oioe4uzzu.bkt.clouddn.com/environment/java/jre-8u111-windows-i586.exe)
* [JRE 8u111 Windows x64](http://oioe4uzzu.bkt.clouddn.com/environment/java/jre-8u111-windows-x64.exe)
* [JRE 8u111 Mac OS X](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html)
* [JDK 8u101 Windows x64](http://oioe4uzzu.bkt.clouddn.com/environment/java/jdk-8u101-windows-x64.exe)

## Python

* [Python 2.7.12 Windows x86](http://oioe4uzzu.bkt.clouddn.com/environment/python/python-2.7.12.msi)

* [Python 3.5.2 Windows x86](http://oioe4uzzu.bkt.clouddn.com/environment/python/python-3.5.2.exe)

* pip 豆瓣源设置

  在 `~/.pip/` 目录下新建 `pip.ini`（Windows）或 `pip.conf`（Linux）文件，内容如下：

  ```
  [global]
  index-url = http://pypi.douban.com/simple
  trusted-host = pypi.douban.com
  [list]
  format=columns
  ```

  ​