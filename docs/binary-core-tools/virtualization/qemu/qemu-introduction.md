## 什么是 qemu

[qemu](https://www.qemu.org/) 是一款由 [Fabrice Bellard](https://bellard.org/) 等人编写的可以执行硬件虚拟化的开源托管虚拟机，具有运行速度快（配合 kvm），跨平台等优点。

qemu 通过动态的二进制转化模拟 CPU，并且提供一组设备模型，使其能够运行多种未修改的客户机OS。

在 CTF 比赛中，qemu 多用于启动异架构的程序（mips, arm 等）、kernel 及 bootloader 等二进制程序，有时也会作为要 pwn 掉的程序出现。

### 运行模式

qemu 有多种运行模式，常用的有 `User-mode emulation` 和 `System emulation` 两种。

#### User-mode emulation
用户模式，在这个模式下，qemu 可以运行单个其他指令集的 linux 或者 macOS/darwin 程序，**允许了为一种架构编译的程序在另外一种架构上面运行**。

#### System emulation
系统模式，在这个模式下，qemu 将模拟一个完整的计算机系统，包括外围设备。

> 之后将分别为两种情况举例

## Reference

https://wiki.qemu.org/Main_Page

https://qemu.weilnetz.de/doc/qemu-doc.html

https://wiki.qemu.org/Documentation

https://en.wikipedia.org/wiki/QEMU

