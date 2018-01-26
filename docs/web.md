# Web

## 菜刀

- [习科兵器库版菜刀](https://attach.blackbap.org/down/wzaq/caidao.rar)

- [官网新版菜刀](http://down.40huo.cn/web/caidao-20160622-www.maicaidao.com.7z)

    解压密码 `www.maicaidao.com`

- [CKnife](http://pan.baidu.com/s/1nul1mpr)

    密码：f65g

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

    [截至 2016 年 12 月 27 日 master 分支打包](http://down.40huo.cn/web/weakfilescan-master.zip)

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

    [截至 2016 年 12 月 27 日 master 分支打包](http://down.40huo.cn/web/weakfilescan-master.zip)

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

    [截至 2016 年 12 月 27 日 master 分支打包](http://down.40huo.cn/web/GitHack-master.zip)

## 日志分析

* [LogForensics](https://github.com/xti9er/LogForensics)

    日志分析 Perl 脚本，使用方法：

    ```shell
    Perl LogForensics.pl -file logfile -websvr (nginx|httpd) [-ip ip(ip,ip,ip)|-url url(url,url,url)]
    ```

    `file` ：日志文件路径 

    `websvr` ：日志类型

    `ip` ：起始调查 ip 或 ip 列表，以逗号分割

    `url` ：起始调查 cgi 链接或链接列表，以逗号分割

  * [ngxtop](https://github.com/lebinh/ngxtop)

    > real-time metrics for nginx server (and others)

    安装：`pip install ngxtop`

    示例：

    ```shell
    ngxtop top remote_addr    # View top source IPs of clients
    ngxtop -i 'status >= 400' print request status http_referer    # List 4xx or 5xx responses together with HTTP referer
    tail -f /var/log/apache2/access.log | ngxtop -f common    # Parse apache log from remote server with common format
    ```

## 内网

- [Termite](http://rootkiter.com/Termite/)

    跳板机管理工具。[下载](http://pan.baidu.com/s/1pLUB7ar)

    > 1. 以服务模式启动一个 agent 服务。
    >
    >      ```shell
    >      $ ./agent -l p 8888
    >      ```
    >
    > 2. 令管理端连接到 agent 并对 agent 进行管理。
    >
    >      ```shell
    >      $ ./admin -c 127.0.0.1 -p 8888
    >      ```
    >
    > 3. 此时，admin 端会得到一个内置的 shell，输入 help 指令可以得到帮助信息。
    >
    >      ```shell
    >      >> help
    >      ```
    >
    > 4. 通过 show 指令可以得到当前 agent 的拓扑情况。
    >
    >      ```shell
    >      >> show 
    >       0M
    >       +-- 1M
    >       由于当前拓扑中只有一个agent，所以展示结果只有 1M ,
    >        其中1 为节点的ID号，
    >        M为MacOS系统的简写，Linux为L，Windows简写为W。
    >      ```
    >
    > 5. 将新 agent 加入当前拓扑。
    >
    >      ```shell
    >      ./agent -c 127.0.0.1 -p 8888
    >      ```
    >
    > 6. 此时 show 指令将得到如下效果。
    >
    >      ```shell
    >      0M
    >       +-- 1M
    >       |   +-- 2M
    >        这表明，当前拓扑中有两个节点，其中由于2节点需要通过1节点才能访问，所以下挂在1节点下方。
    >      ```
    >
    > 7. 在 2 节点开启 socks 代理，并绑定在本地端口
    >
    >      ```shell
    >      >> goto 2
    >          将当前被管理节点切换为 2 号节点。
    >      >> socks 1080
    >         此时，本地1080 端口会启动个监听服务，而服务提供者为2号节点。
    >      ```
    >
    > 8. 在 1 号节点开启一个 shell 并绑定到本地端口
    >
    >      ```shell
    >      >> goto 1
    >      >> shell 7777
    >           此时，通过nc本地的 7777 端口，就可以得到一个 1 节点提供的 shell.
    >      ```
    >
    > 9. 将远程的文件下载至本地
    >
    >      ```shell
    >      >> goto 1
    >      >> downfile 1.txt 2.txt
    >          将1 节点，目录下的 1.txt 下载至本地，并命名为2.txt
    >      ```
    >
    > 10. 上传文件至远程节点
    >
    >     ```shell
    >     >> goto 2
    >     >> upfile 2.txt 3.txt
    >         将本地的 2.txt 上传至 2号节点的目录，并命名为3.txt
    >     ```
    >
    > 11. 端口转接
    >
    >     ```shell
    >     >> goto 2 
    >     >> lcxtran 3388 10.0.0.1 3389
    >         以2号节点为跳板，将 10.0.0.1 的 3389 端口映射至本地的 3388 端口
    >     ```
    >
    > 12. 更多支持
    >
    >     ```
    >     http://rootkiter.com/toolvideo/toolmp4/1maintalk.mp4
    >     http://rootkiter.com/toolvideo/toolmp4/2socks.mp4
    >     http://rootkiter.com/toolvideo/toolmp4/3lcxtran.mp4
    >     http://rootkiter.com/toolvideo/toolmp4/4shell.mp4
    >     http://rootkiter.com/toolvideo/toolmp4/5file.mp4
    >     ```

