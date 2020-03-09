## 安装 qemu

### 使用包管理
一般情况下，如无特殊需要（如为了运行某个 CTF 比赛中的异架构程序或者 kernel）直接使用对应的包管理直接安装即可
```bash
    Arch: pacman -S qemu

    Debian/Ubuntu: apt-get install qemu

    Fedora: dnf install @virtualization

    Gentoo: emerge --ask app-emulation/qemu

    RHEL/CentOS: yum install qemu-kvm

    SUSE: zypper install qemu
```

> 这里只说明在 linux 下的安装过程，其他系统的安装过程请参考 [官方网站](https://www.qemu.org/download/)

### 从源码编译
通过包管理安装的 qemu 版本一般较老，如果需要新版的 qemu，可以从源码编译，这里以编译最新版的 qemu 为例。

```bash
wget https://download.qemu.org/qemu-3.1.0-rc3.tar.xz
tar xvJf qemu-3.1.0-rc3.tar.xz
cd qemu-3.1.0-rc3
```

通过 `./configure --help` 的查看编译时的选项， `--target-list` 选项为可选的模拟器，默认全选

> `--target-list` 中的 `xxx-soft` 和 `xxx-linux-user` 分别指系统模拟器和应用程序模拟器, 生成的二进制文件名字为 `qemu-system-xxx` 和 `qemu-xxx`

这里直接使用默认选项进行编译
```
./configure
make -j8
```

继续安装
```bash
sudo make install
```

成功安装
```bash
~ qemu-arm --version
qemu-arm version 3.0.93
Copyright (c) 2003-2018 Fabrice Bellard and the QEMU Project developers
```

## 使用 qemu
以 CISCN 2017 的 [babydriver](https://github.com/ctf-wiki/ctf-challenges/tree/master/pwn/kernel/CISCN2017-babydriver) 举例，查看启动脚本

```bash
CISCN2017_babydriver [master●] bat boot.sh 
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: boot.sh
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ #!/bin/bash
   2   │ 
   3   │ qemu-system-x86_64 -initrd rootfs.cpio -kernel bzImage -append 'console=ttyS0 root=/dev/ram oops=panic panic=1' -enable-kvm -monitor /dev/null -m 64M -
       │ -nographic  -smp cores=1,threads=1 -cpu kvm64,+smep
```
可以看出这道题目是用 `qemu-system-x86_64` 启动了以 `rootfs.cpio` 为文件系统的 kernel `bzImage`，启动时的参数为 `console=ttyS0 ... panic=1`，为这个进程分配 64M 内存。

> 更多参数的含义请通过 `-h` 或者 [qemu-doc](https://qemu.weilnetz.de/doc/qemu-doc.html) 查看。


> 如果使用包管理安装 qemu，直接安装 `qemu-system-x86_64` 即可
 ```bash
 sudo apt install qemu-system_x86-64
 ```

因为使用了 kvm，所以启动时要用 root 权限启动
```bash
CISCN2017_babydriver [master●] sudo ./boot.sh
......
......
/ $ id
uid=1000(ctf) gid=1000(ctf) groups=1000(ctf)
/ $ ls
bin      etc      init     linuxrc  root     sys      usr
dev      home     lib      proc     sbin     tmp
```

> 这道题目的更多分析可以看 [link](https://ctf-wiki.github.io/ctf-wiki/pwn/linux/kernel/kernel_uaf/#ciscn2017-babydriver)


同样，再看下 Codegate 2018 的[Melong](https://github.com/ctf-wiki/ctf-challenges/tree/master/pwn/arm/Codegate2018_Melong)
```bash
Codegate2018_Melong [master] check melong
+ file melong
melong: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.3, for GNU/Linux 3.2.0, BuildID[sha1]=2c55e75a072020303e7c802d32a5b82432f329e9, not stripped
+ checksec melong
[*] '/home/m4x/Projects/pwn_repo/Codegate2018_Melong/melong'
    Arch:     arm-32-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x10000)
```
可以看出是 32 位 的 arm 程序，需要安装 `qemu-arm`

> 如果使用包管理安装，则
 ```bash
 $ sudo apt-get install qemu-user
 $ sudo apt-get install qemu-use-binfmt qemu-user-binfmt:i386
 ```
 这样就安装了 `qemu-arm`

但同时因为程序是动态链接的，还需要同时安装对应的 libc，可以使用 `apt search "libc6-" | grep "ARCH"` 搜索，如
```bash
Codegate2018_Melong [master] apt search "libc6-"| grep arm
p   libc6-arm64-cross                                                       - GNU C Library: Shared libraries (for cross-compiling)                             
v   libc6-arm64-dcv1                                                        -                                                                                   
v   libc6-armel-armel-cross                                                 -                                                                                   
p   libc6-armel-armhf-cross                                                 - Dummy package to get libc6:armel installed                                        
p   libc6-armel-cross                                                       - GNU C Library: Shared libraries (for cross-compiling)                             
v   libc6-armel-dcv1                                                        -                                                                                   
p   libc6-armhf-armel-cross                                                 - Dummy package to get libc6:armhf installed                                        
v   libc6-armhf-armhf-cross                                                 -                                                                                   
p   libc6-armhf-cross                                                       - GNU C Library: Shared libraries (for cross-compiling)                             
v   libc6-armhf-dcv1                                                        -                                                                                   
p   libc6-dbg-arm64-cross                                                   - GNU C Library: detached debugging symbols (for cross-compiling)                   
v   libc6-dbg-arm64-dcv1                                                    -                                                                                   
p   libc6-dbg-armel-cross                                                   - GNU C Library: detached debugging symbols (for cross-compiling)                   
v   libc6-dbg-armel-dcv1                                                    -                                                                                   
p   libc6-dbg-armhf-cross                                                   - GNU C Library: detached debugging symbols (for cross-compiling)                   
v   libc6-dbg-armhf-dcv1                                                    -                                                                                   
p   libc6-dev-arm64-cross                                                   - GNU C Library: Development Libraries and Header Files (for cross-compiling)       
v   libc6-dev-arm64-cross:i386                                              -                                                                                   
v   libc6-dev-arm64-dcv1                                                    -                                                                                   
v   libc6-dev-armel-armel-cross                                             -                                                                                   
p   libc6-dev-armel-armhf-cross                                             - Dummy package to get libc6-dev:armel installed                                    
p   libc6-dev-armel-cross                                                   - GNU C Library: Development Libraries and Header Files (for cross-compiling)       
v   libc6-dev-armel-cross:i386                                              -                                                                                   
v   libc6-dev-armel-dcv1                                                    -                                                                                   
p   libc6-dev-armhf-armel-cross                                             - Dummy package to get libc6-dev:armhf installed                                    
v   libc6-dev-armhf-armhf-cross                                             -                                                                                   
p   libc6-dev-armhf-cross                                                   - GNU C Library: Development Libraries and Header Files (for cross-compiling)       
v   libc6-dev-armhf-cross:i386                                              -                                                                                   
v   libc6-dev-armhf-dcv1                                                    -    
```
只需要安装 `libc6-ARCH-cross` 的包即可。

装好后使用 `-L` 指定共享库路径即可运行文件。
```bash
$ qemu-arm -L /usr/arm-linux-gnueabi ./melong
```

> 这道题目的更多分析可以看 [link](http://m4x.fun/post/how-2-pwn-an-arm-binary/#codegate2018-melong)

如果是静态的程序，不需要 libc，则可以不用 `-L` 选项，如 Jarvis-OJ 的 [typo](https://github.com/ctf-wiki/ctf-challenges/tree/master/pwn/arm/jarvisOJ_typo)

```bash
jarvisOJ_typo [master] check ./typo
+ file ./typo
./typo: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), statically linked, for GNU/Linux 2.6.32, BuildID[sha1]=211877f58b5a0e8774b8a3a72c83890f8cd38e63, stripped
+ checksec ./typo
[*] '/home/m4x/Projects/pwn_repo/jarvisOJ_typo/typo'
    Arch:     arm-32-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x8000)
jarvisOJ_typo [master] qemu-arm ./typo
Let's Do Some Typing Exercise~
Press Enter to get start;
Input ~ if you want to quit
^C
```

## 如何 debug

分两种情况

1. 调试 qemu 这个进程
2. 调试 qemu 内运行的程序

### 调试 qemu
对于第一种情况，直接使用 gdb attach 到 qemu 的进程号即可，为了调试时方便可以在编译时加上 `--enable-debug` 选项以保留符号等信息。
```
  --enable-debug           enable common debug build options
```
在之后的 qemu 逃逸中会着重介绍这个过程。

### 调试 qemu 中的进程
qemu 提供了 gdb 的接口，通过 `-g` 指定端口来调用
```
-g port              QEMU_GDB          wait gdb connection to 'port'
```

同时为了调试异架构的程序，需要安装 `gdb-multiarch`
```bash
sudo apt install gdb-multiarch
```
例如 Melong 中，使用
```bash
$ qemu-arm -g 1234 -L /usr/arm-linux-gnueabi ./melong
```
启动程序，在另一个 shell 中使用 `gdb-multiarch` 启动程序并连接到指定的端口即可调试
```bash
Codegate2018_Melong [master] gdb-multiarch ./melong -q
pwndbg: loaded 175 commands. Type pwndbg [filter] for a list.
pwndbg: created $rebase, $ida gdb functions (can be used with print/break)
Reading symbols from ./melong...(no debugging symbols found)...done.
pwndbg> target remote localhost:1234
```

> 使用 gdb-multriarch 可以调试大多数的程序。
>
> 但也有部分程序不能使用 gdb-multiarch，这时可以编译对应架构的 Toolchain，如 `arm-none-eabi-gdb`
>
> 或者使用系统模式的 qemu 创建一个对应架构的虚拟机，文末放了一片链接，以后也会介绍这种方法。

特别的是系统模式的 qemu 还提供了另外几个参数
```
-gdb dev        wait for gdb connection on 'dev'
-s              shorthand for -gdb tcp::1234
-S              freeze CPU at startup (use 'c' to start execution)
```

`-gdb` 作用类似 `-g`，使用 `-gdb tcp::1234` 即可在 gdb 中通过 1234 端口调试。

`-s` 是 `-gdb tcp::1234` 的缩写

`-S` 让虚拟机停在启动的地方方便调试，类似于 pwntools 的 [gdb.debug()](http://docs.pwntools.com/en/stable/gdb.html?highlight=gdb.debug#pwnlib.gdb.debug)


## References

https://www.ringzerolabs.com/2018/03/the-wonderful-world-of-mips.html
