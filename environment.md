# 环境搭建

## Kali Linux

* [官网](https://www.kali.org)
* [安装镜像下载（中科大）](http://mirrors.ustc.edu.cn/kali-images/)
* [虚拟机镜像下载（官网）](https://www.offensive-security.com/kali-linux-vmware-virtualbox-image-download/)

## Java

* [JRE 8u111 Windows x86](http://down.40huo.cn/environment/java/jre-8u111-windows-i586.exe)
* [JRE 8u111 Windows x64](http://down.40huo.cn/environment/java/jre-8u111-windows-x64.exe)
* [JRE 8u111 Mac OS X](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html)
* [JDK 8u101 Windows x64](http://down.40huo.cn/environment/java/jdk-8u101-windows-x64.exe)

## Python

* [Python 2.7.12 Windows x86](http://down.40huo.cn/environment/python/python-2.7.12.msi)

* [Python 3.5.2 Windows x86](http://down.40huo.cn/environment/python/python-3.5.2.exe)

* pip 豆瓣源设置

  在 `~/.pip/` 目录下新建 `pip.ini`（Windows）或 `pip.conf`（Linux）文件，内容如下：

  ```
  [global]
  index-url = http://pypi.douban.com/simple
  trusted-host = pypi.douban.com
  [list]
  format=columns
  ```


## Offline Docs

当你处于没有外网的环境时，自己留在本地的资料就显得尤为重要。

* [乌云漏洞知识库镜像](https://github.com/hanc00l/wooyun_public)

  * [百度网盘  提取码 5ik7](http://pan.baidu.com/s/1kVtY2rX)

* [Zeal](https://zealdocs.org)

  > Zeal is an offline documentation browser for software developers.

  这东西有个缺点。。。下载的时候很慢，挂了代理好像稍微好点。。。

  * [Zeal portable 0.3.1 Windows x86](http://down.40huo.cn/environment/offline/zeal-portable-0.3.1-windows-x86.7z)

  * [自己打的 docset 压缩包 7z](http://down.40huo.cn/environment/offline/docsets.7z)

    包括 Bash、C、C++、CSS、Django、Docker、ElasticSearch、Flask、Go、HTML、JavaScript、Java SE 8、Laravel、MySQL、Nginx、PHP、Python 2、Python 3、Vim、WordPress。