# Web

## 注入

* [sqlmap](https://github.com/sqlmapproject/sqlmap)

  > **Installation**
  >
  > You can download the latest tarball by clicking [here](https://github.com/sqlmapproject/sqlmap/tarball/master) or latest zipball by clicking [here](https://github.com/sqlmapproject/sqlmap/zipball/master).
  >
  > Preferably, you can download sqlmap by cloning the [Git](https://github.com/sqlmapproject/sqlmap) repository:
  >
  > ```
  > git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev
  >
  > ```
  >
  > sqlmap works out of the box with [Python](http://www.python.org/download/) version **2.6.x** and **2.7.x** on any platform.
  >
  > **Usage**
  >
  > To get a list of basic options and switches use:
  >
  > ```
  > python sqlmap.py -h
  >
  > ```
  >
  > To get a list of all options and switches use:
  >
  > ```
  > python sqlmap.py -hh
  >
  > ```
  >
  > You can find a sample run [here](https://asciinema.org/a/46601). To get an overview of sqlmap capabilities, list of supported features and description of all options and switches, along with examples, you are advised to consult the [user's manual](https://github.com/sqlmapproject/sqlmap/wiki).

  [截至2016年12月27日 master 分支打包](http://down.40huo.cn/web/weakfilescan-master.zip)

## 抓包

* [Burp Suite 1.6 pro]()

* [WireShark 2.2.3 win32](http://down.40huo.cn/web/Wireshark-win32-2.2.3.exe)

* [PKAV HTTP FUZZER](http://down.40huo.cn/web/Pkav%20HTTP%20Fuzzer%201.5.5.zip)

  带一点简单的验证码识别。

## 目录扫描

* [御剑后台扫描](http://down.40huo.cn/web/%E5%BE%A1%E5%89%91%E5%90%8E%E5%8F%B0%E6%89%AB%E6%8F%8F%E7%8F%8D%E8%97%8F%E7%89%88.zip)

  * [自用御剑字典](http://down.40huo.cn/wordlist/%E5%BE%A1%E5%89%91%E5%AD%97%E5%85%B8.rar)

* [dirfuzz](http://down.40huo.cn/web/dirfuzz-master.zip)

* [weakfilescan](https://github.com/ring04h/weakfilescan)

  dirfuzz 进阶版。

  [截至2016年12月27日 master 分支打包](http://down.40huo.cn/web/weakfilescan-master.zip)

* [猪猪侠字典打包](http://pan.baidu.com/s/1geBDwGz)

* [Github 上某不明字典](http://down.40huo.cn/wordlist/wordlist.txt.gz)

## 源码泄露

* [Seay - SVN 源码泄露利用工具](http://down.40huo.cn/web/Seay-Svn%E6%BA%90%E4%BB%A3%E7%A0%81%E6%B3%84%E9%9C%B2%E6%BC%8F%E6%B4%9E%E5%88%A9%E7%94%A8%E5%B7%A5%E5%85%B72.0.zip)

* [Githack](https://github.com/lijiejie/GitHack)

  > **用法示例**
  >
  > ```
  > GitHack.py http://www.openssl.org/.git/
  > ```

  [截至2016年12月27日 master 分支打包](http://down.40huo.cn/web/GitHack-master.zip)

## 日志分析

* [LogForensics](https://github.com/xti9er/LogForensics)

  日志分析 Perl 脚本，使用方法：

  ```bash
  Perl LogForensics.pl -file logfile -websvr (nginx|httpd) [-ip ip(ip,ip,ip)|-url url(url,url,url)]
  ```

  `file` : 日志文件路径 

  `websvr` : 日志类型

  `ip` : 起始调查 ip 或 ip 列表，以逗号分割

  `url` : 起始调查 cgi 链接或链接列表，以逗号分割

* [ngxtop](https://github.com/lebinh/ngxtop)

  > real-time metrics for nginx server (and others)

  安装：`pip install ngxtop`

  示例：

  ```bash
  ngxtop top remote_addr	# View top source IPs of clients
  ngxtop -i 'status >= 400' print request status http_referer	# List 4xx or 5xx responses together with HTTP referer
  tail -f /var/log/apache2/access.log | ngxtop -f common	# Parse apache log from remote server with common format
  ```

  ​