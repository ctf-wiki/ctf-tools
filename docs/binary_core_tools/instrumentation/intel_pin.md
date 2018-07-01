# intel pin

## 什么是 pin

pin 是 intel 开发的一款二进制程序的插桩分析工具，支持 x86/x64 & windows/linux/mac，提供了丰富的 API 供使用者用 C/C++ 编写 pintool 分析程序

### 什么是插桩(instrument)

通俗的来说，插桩就是在已有的源代码/二进制程序中插入某些代码以便于自己分析，比如在调试时使用 printf 打印变量值就属于在源代码级别的插桩。而intel pin就是在二进制程序级别（没有源代码）插桩的一款工具

## pin 和 pintool

### pin 的安装，pintool 的编译

pin 的安装很简单，这里以 64 位的 Linux 为例来说明，从 [官网](https://software.intel.com/en-us/articles/pin-a-binary-instrumentation-tool-downloads) 上下载 pin 组件后，解压即可，在解压后的文件夹内有编译好的二进制程序 pin

```bash
pin-3.6-gcc-linux ls
doc  extlicense  extras  ia32  intel64  LICENSE  pin  README  redist.txt  source
pin-3.6-gcc-linux file pin 
pin: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), statically linked, BuildID[sha1]=7beaa83f9142955a6e933bf29d4a8aa1269298bc, stripped
pin-3.6-gcc-linux ./pin 
E: Missing application name
Pin: pin-3.6-97554-31f0a167d
Copyright (c) 2003-2017, Intel Corporation. All rights reserved.

Usage: pin [OPTION] [-t <tool> [<toolargs>]] -- <command line>
Use -help for a description of options
```

在 **source/tools/ManualExamples** 中有一些现成的 pintool 可以使用，基本涵盖了各个模块的用法，这里以 inscount0 这个 pintool 为例介绍 pin 的使用方法

```bash
pin-3.6-gcc-linux cd source/tools/ManualExamples
ManualExamples file inscount0.cpp
inscount0.cpp: C source, ASCII text
ManualExamples make obj-intel64/inscount0.so TARGET=intel64
g++ -Wall -Werror -Wno-unknown-pragmas -D__PIN__=1 -DPIN_CRT=1 -fno-stack-protector -fno-exceptions -funwind-tables -fasynchronous-unwind-tables -fno-rtti -DTARGET_IA32E -DHOST_IA32E -fPIC -DTARGET_LINUX -fabi-version=2  -I../../../source/include/pin -I../../../source/include/pin/gen -isystem /home/m4x/pin-3.6-gcc-linux/extras/stlport/include -isystem /home/m4x/pin-3.6-gcc-linux/extras/libstdc++/include -isystem /home/m4x/pin-3.6-gcc-linux/extras/crt/include -isystem /home/m4x/pin-3.6-gcc-linux/extras/crt/include/arch-x86_64 -isystem /home/m4x/pin-3.6-gcc-linux/extras/crt/include/kernel/uapi -isystem /home/m4x/pin-3.6-gcc-linux/extras/crt/include/kernel/uapi/asm-x86 -I../../../extras/components/include -I../../../extras/xed-intel64/include/xed -I../../../source/tools/InstLib -O3 -fomit-frame-pointer -fno-strict-aliasing   -c -o obj-intel64/inscount0.o inscount0.cpp
g++ -shared -Wl,--hash-style=sysv ../../../intel64/runtime/pincrt/crtbeginS.o -Wl,-Bsymbolic -Wl,--version-script=../../../source/include/pin/pintool.ver -fabi-version=2    -o obj-intel64/inscount0.so obj-intel64/inscount0.o  -L../../../intel64/runtime/pincrt -L../../../intel64/lib -L../../../intel64/lib-ext -L../../../extras/xed-intel64/lib -lpin -lxed ../../../intel64/runtime/pincrt/crtendS.o -lpin3dwarf  -ldl-dynamic -nostdlib -lstlport-dynamic -lm-dynamic -lc-dynamic -lunwind-dynamic
ManualExamples file obj-intel64/inscount0.so 
obj-intel64/inscount0.so: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, BuildID[sha1]=3baa29dd54235acaab02edc94bf9ac377dd7b0e5, not stripped
```

此时在 obj-intel64 下编译生成了 inscount0.so，这个 so 即为一种 pintool，功能为记录程序执行的指令的条数；

>   判断 pintool 的功能可以阅读 pintool 源代码或者使用下条指令
>
>   $ pin -t your\_pintool -h -- your_application

类似的，要编译 32 位的 pintool 可以使用

```bash
make obj-ia32/inscount0.so
```

编译 ManualExamples 中的所有 pintool 可以使用

```bash
make all TAEGET=intel64
make all TAEGET=ia32
```

### pin 的使用

pin 的基本命令格式如下

```bash
pin -t your_pintool -- your_binary <arg> 
```

以刚刚编译的 inscount0 这个 pintool 为例

```bash
ManualExamples ../../../pin -t ./obj-intel64/inscount0.so -- /bin/ls -a
.		       inscount2.cpp	  obj-intel64
..		       inscount.out	  pinatrace.cpp
buffer_linux.cpp       inscount_tls.cpp   pin.log
buffer_windows.cpp     invocation.cpp	  proccount.cpp
countreps.cpp	       isampling.cpp	  replacesigprobed.cpp
detach.cpp	       itrace.cpp	  safecopy.cpp
divide_by_zero_unix.c  little_malloc.c	  stack-debugger.cpp
divide_by_zero_win.c   logtrace.cpp	  stack-debugger-tutorial.sln
emudiv.cpp	       makefile		  stack-debugger-tutorial.vcxproj
fibonacci.cpp	       makefile.rules	  stack-debugger-tutorial.vcxproj.filters
follow_child_app1.cpp  malloc.cpp	  statica.cpp
follow_child_app2.cpp  malloc_mt.cpp	  staticcount.cpp
follow_child_tool.cpp  malloctrace.cpp	  strace.cpp
fork_app.cpp	       myInscount0.cpp	  test
fork_jit_tool.cpp      myInscount1.cpp	  test.c
imageload.cpp	       myMalloctrace.cpp  test-packed
inscount0.cpp	       nonstatica.cpp	  tracer.cpp
inscount1.cpp	       obj-ia32		  w_malloctrace.cpp
ManualExamples cat inscount.out 
Count 813449
```

inscount 默认结果保存在 inscount.out 这个文件中，在上例中，即此时 **ls -a** 这条命令共执行了 813449 条指令

### pintool 的分析

同样以 inscount0 为例分析，查看 inscount0.cpp 的内容

```C++
#include <iostream>
#include <fstream>
#include "pin.H"

ofstream OutFile;

// The running count of instructions is kept here
// make it static to help the compiler optimize docount
static UINT64 icount = 0;

// This function is called before every instruction is executed
VOID docount() { icount++; }
    
// Pin calls this function every time a new instruction is encountered
VOID Instruction(INS ins, VOID *v)
{
    // Insert a call to docount before every instruction, no arguments are passed
    INS_InsertCall(ins, IPOINT_BEFORE, (AFUNPTR)docount, IARG_END);
}

KNOB<string> KnobOutputFile(KNOB_MODE_WRITEONCE, "pintool",
    "o", "inscount.out", "specify output file name");

// This function is called when the application exits
VOID Fini(INT32 code, VOID *v)
{
    // Write to a file since cout and cerr maybe closed by the application
    OutFile.setf(ios::showbase);
    OutFile << "Count " << icount << endl;
    OutFile.close();
}

/* ===================================================================== */
/* Print Help Message                                                    */
/* ===================================================================== */

INT32 Usage()
{
    cerr << "This tool counts the number of dynamic instructions executed" << endl;
    cerr << endl << KNOB_BASE::StringKnobSummary() << endl;
    return -1;
}

/* ===================================================================== */
/* Main                                                                  */
/* ===================================================================== */
/*   argc, argv are the entire command line: pin -t <toolname> -- ...    */
/* ===================================================================== */

int main(int argc, char * argv[])
{
    // Initialize pin
    if (PIN_Init(argc, argv)) return Usage();

    OutFile.open(KnobOutputFile.Value().c_str());

    // Register Instruction to be called to instrument instructions
    INS_AddInstrumentFunction(Instruction, 0);

    // Register Fini to be called when the application exits
    PIN_AddFiniFunction(Fini, 0);
    
    // Start the program, never returns
    PIN_StartProgram();
    
    return 0;
}
```

从 main 开始，首先调用了 PIN\_init（53 行）进行初始化，然后使用 INS_AddInstrumenFunction 注册了一个插桩函数(58行)，根据 intel pin 的 [用户手册](https://software.intel.com/sites/landingpage/pintool/docs/97619/Pin/html/)

```C
PIN_CALLBACK LEVEL_PINCLIENT::INS_AddInstrumentFunction	(	INS_INSTRUMENT_CALLBACK 	fun,
VOID * 	val 
)		
Add a function used to instrument at instruction granularity

Parameters:
fun	Instrumentation function for instructions
val	passed as the second argument to the instrumentation function
Returns:
PIN_CALLBACK A handle to a callback that can be used to further modify this callback's properties
Note:
The pin client lock is obtained during the call of this API.
Availability:
Mode: JIT
O/S: Linux, Windows & OS X*
CPU: All
```

在这里该函数的作用是在指令粒度插入 Instruction 函数，即在每条指令执行前，都会进入 Instruction 这个函数中，其第2个参数为一个额外传递给 Instruction 的参数，即对应 `VOID *v` 这个参数，这里没有使用。而 Instruction 接受的第一个参数为 `INS` 结构，用来表示一条指令

而我们再看 Instruction 这个函数

```C
VOID Instruction(INS ins, VOID *v)
{
    // Insert a call to docount before every instruction, no arguments are passed
    INS_InsertCall(ins, IPOINT_BEFORE, (AFUNPTR)docount, IARG_END);
}
```

在 Instruction 函数内部又使用 INS\_InsertCall 注册了一个函数 docount，即在每条指令前实际插入了 docount 这个函数。需要注意的是 INS\_InsertCall 试一个便餐函数，前三个参数分别为指令(ins)，插入的实际(IPOINT\_BEFORE，表示在指令运行之前插入 docount 函数)，函数指针(docount，转化为了 AFUNPTR 类型)，之后的参数为传递给 docount 函数的参数，以 IARG\_END 结尾

而 docount 函数（12 行）的作用就很明显了，每次将一个全局变量加 1，因此此时 /bin/ls -a 运行的模式如下:

```assembly
...
icount++;
sub $0xff, %edx
icount++;
cmp %esi, %edx
icount++;
jle <L1>
icount++;
mov $0x1, %edi
icount++;
add $0x10, %eax
...
```

所以 inscount0 的用途就很明显了，每条指令前都调用 docount 函数将全局变量 icount 自增，最后通过PIN\_AddFiniFunction 函数注册的 Fini 函数(25行)将结果写到一个文件中。

当这些函数都定义完后，就可以使用 PIN\_StartProgram 来启动程序了

这里分析了一个最简单的 pintool 例子，更多 pintool 例子的分析和其他函数的使用可以参考 BrieflyX 的 [博客](http://brieflyx.me/2017/binary-analysis/intel-pin-intro/)



## pin in CTF

因为动态插桩有不重新编译即可收集二进制程序某些信息的特性，因此用 pin 求解**一部分** CTF challenges 会异常的方便，下边给出一些例子和分析

### NDH2K13-crackme-500

首先用常规方法对 crackme 文件进行分析

```bash
NDH2k13-crackme-500 [master] file crackme 
crackme: ELF 64-bit LSB executable, x86-64, invalid version (SYSV), for GNU/Linux 2.6.9, statically linked, corrupted section header size
NDH2k13-crackme-500 [master] nm ./crackme 

nm: out of memory allocating 109524665216 bytes after a total of 0 bytes
NDH2k13-crackme-500 [master] objdump -d ./crackme -M intel
objdump: ./crackme: 不可识别的文件格式
NDH2k13-crackme-500 [master] ./crackme 
Jonathan Salwan loves you <3
----------------------------

Password: test
Bad password
```

发现 section header 受到了损坏，用 IDA 打开时也有很多报错，这时我们尝试使用 intel pin 来求解这道题目，先使用最常见的统计指令条数的方法

```bash
NDH2k13-crackme-500 [master] ~/pin-3.6-gcc-linux/pin -t ./inscount0.so -- ./crackme <<< "a" >> /dev/null; cat inscount.out 
Count 160345
NDH2k13-crackme-500 [master●] ~/pin-3.6-gcc-linux/pin -t ./myInscount0.so -- ./crackme <<< "a" 
Jonathan Salwan loves you <3
----------------------------

Password: Bad password
Count: 163218
NDH2k13-crackme-500 [master●] ~/pin-3.6-gcc-linux/pin -t ./myInscount0.so -- ./crackme <<< "aa"
Jonathan Salwan loves you <3
----------------------------

Password: Bad password
Count: 166014
NDH2k13-crackme-500 [master●] ~/pin-3.6-gcc-linux/pin -t ./myInscount0.so -- ./crackme <<< "aaa"
Jonathan Salwan loves you <3
----------------------------

Password: Bad password
Count: 168810
NDH2k13-crackme-500 [master●] ~/pin-3.6-gcc-linux/pin -t ./myInscount0.so -- ./crackme <<< "aaaa"
Jonathan Salwan loves you <3
----------------------------

Password: Bad password
Count: 171606
NDH2k13-crackme-500 [master●] ~/pin-3.6-gcc-linux/pin -t ./myInscount0.so -- ./crackme <<< "aaaaa"
Jonathan Salwan loves you <3
----------------------------

Password: Bad password
Count: 174402
NDH2k13-crackme-500 [master●] bpython
bpython version 0.17.1 on top of Python 2.7.13+ /usr/bin/python2
>>> 174402 - 171606 == 171606 - 168810 == 168810 - 166014 == 166014 - 163218
True
>>> 
```

此时我们发现了一个很有趣的特性，输入长度每次增加 1 时，指令条数也是以等差的规模递增的

>   myInscount0 是我在 inscount0 的基础上更改的 pintool，实现了从结果保存到文件到结果输出到标准输出的修改

我们写一个简单的脚本查看输入长度递增时，指令条数的变化规律

```python
NDH2k13-crackme-500 [master●] cat guessLen.py 
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from subprocess import Popen, PIPE
from sys import argv
import string

pinPath = "/home/m4x/pin-3.6-gcc-linux/pin"
pinInit = lambda tool, elf: Popen([pinPath, '-t', tool, '--', elf], stdin = PIPE, stdout = PIPE)
pinWrite = lambda cont: pin.stdin.write(cont)
pinRead = lambda : pin.communicate()[0]

if __name__ == "__main__":
    last = 0
    for i in xrange(1, 30):
        pin = pinInit("./myInscount0.so", "./crackme")
        pinWrite("a" * i + '\n')
        now = int(pinRead().split("Count: ")[1])
        
        print "inputLen({:2d}) -> ins({}) -> delta({})".format(i, now, now - last)
        last = now
```

在我电脑上运行结果如下：

```python
NDH2k13-crackme-500 [master●] python guessLen.py 
inputLen( 1) -> ins(160307) -> delta(160307)
inputLen( 2) -> ins(163103) -> delta(2796)
inputLen( 3) -> ins(165899) -> delta(2796)
inputLen( 4) -> ins(168695) -> delta(2796)
inputLen( 5) -> ins(171491) -> delta(2796)
inputLen( 6) -> ins(174287) -> delta(2796)
inputLen( 7) -> ins(177083) -> delta(2796)
inputLen( 8) -> ins(182804) -> delta(5721)
inputLen( 9) -> ins(182676) -> delta(-128)
inputLen(10) -> ins(185472) -> delta(2796)
inputLen(11) -> ins(188268) -> delta(2796)
inputLen(12) -> ins(191064) -> delta(2796)
inputLen(13) -> ins(193860) -> delta(2796)
inputLen(14) -> ins(196656) -> delta(2796)
inputLen(15) -> ins(199452) -> delta(2796)
inputLen(16) -> ins(202248) -> delta(2796)
inputLen(17) -> ins(205044) -> delta(2796)
inputLen(18) -> ins(207840) -> delta(2796)
inputLen(19) -> ins(210636) -> delta(2796)
inputLen(20) -> ins(213432) -> delta(2796)
inputLen(21) -> ins(216228) -> delta(2796)
inputLen(22) -> ins(219024) -> delta(2796)
inputLen(23) -> ins(221820) -> delta(2796)
inputLen(24) -> ins(224616) -> delta(2796)
inputLen(25) -> ins(227412) -> delta(2796)
inputLen(26) -> ins(230208) -> delta(2796)
inputLen(27) -> ins(244188) -> delta(13980)
inputLen(28) -> ins(244188) -> delta(0)
inputLen(29) -> ins(244188) -> delta(0)
```

可以发现，在输入长度 <8 时，指令条数是递增的，但长度为 8 与长度为 7 比较发生了突变，这个时候我们就可以大胆的推测当输入长度为 8 时，程序的运行流程有了较大的变化，正确的 flag 长度即为 8

我们以输入长度是 8 为前提，再查看不同输入下指令条数的变化规律

```bash
NDH2k13-crackme-500 [master●] ~/pin-3.6-gcc-linux/pin -t ./myInscount0.so -- ./crackme <<< ">???????"
Jonathan Salwan loves you <3
----------------------------

Password: Bad password
Count: 185714
NDH2k13-crackme-500 [master●] ~/pin-3.6-gcc-linux/pin -t ./myInscount0.so -- ./crackme <<< "????????"
Jonathan Salwan loves you <3
----------------------------

Password: Bad password
Count: 185714
NDH2k13-crackme-500 [master●] ~/pin-3.6-gcc-linux/pin -t ./myInscount0.so -- ./crackme <<< "@???????"
Jonathan Salwan loves you <3
----------------------------

Password: Bad password
Count: 185714
NDH2k13-crackme-500 [master●] ~/pin-3.6-gcc-linux/pin -t ./myInscount0.so -- ./crackme <<< "A???????"
Jonathan Salwan loves you <3
----------------------------

Password: Bad password
Count: 189752
NDH2k13-crackme-500 [master●] ~/pin-3.6-gcc-linux/pin -t ./myInscount0.so -- ./crackme <<< "B???????"
Jonathan Salwan loves you <3
----------------------------

Password: Bad password
Count: 185714
NDH2k13-crackme-500 [master●] ~/pin-3.6-gcc-linux/pin -t ./myInscount0.so -- ./crackme <<< "C???????"
Jonathan Salwan loves you <3
----------------------------

Password: Bad password
Count: 185714
```

可以发现，输入以 ASCII码 顺序递增时，第一位为 A 时指令条数发生了变化，此时我们在进一步推测正确的 flag 第一位即为 A

再写一个脚本逐位爆破

```python
NDH2k13-crackme-500 [master●] cat guessPWD.py 
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from subprocess import Popen, PIPE
from sys import argv
import string
import pdb

pinPath = "/home/m4x/pin-3.6-gcc-linux/pin"
pinInit = lambda tool, elf: Popen([pinPath, '-t', tool, '--', elf], stdin = PIPE, stdout = PIPE)
pinWrite = lambda cont: pin.stdin.write(cont)
pinRead = lambda : pin.communicate()[0]

if __name__ == "__main__":
    last = 0
    #  dic = map(chr, xrange(0x20, 0x80))
    dic = string.ascii_letters + string.digits + "+_ "
    pwd = '?' * 8
    dicIdx = 0
    pwdIdx = 0

    while True:
        pwd = pwd[: pwdIdx] + dic[dicIdx] + pwd[pwdIdx + 1: ]
        #  pdb.set_trace()
        pin = pinInit("./myInscount1.so", "./crackme")
        pinWrite(pwd + '\n')
        now = int(pinRead().split("Count: ")[1])

        print "input({}) -> now({}) -> delta({})".format(pwd, now, now - last)

        if now - last > 2000 and dicIdx:
            pwdIdx += 1
            dicIdx = -1
            last = 0
            if pwdIdx >= len(pwd):
                print "Found pwd: {}".format(pwd)
                break

        dicIdx += 1
        last = now
```

运行结果如下

```bash
NDH2k13-crackme-500 [master●] time python guessPWD.py 
input(a???????) -> now(182804) -> delta(182804)
input(b???????) -> now(182804) -> delta(0)
input(c???????) -> now(182804) -> delta(0)
input(d???????) -> now(182804) -> delta(0)
input(e???????) -> now(182804) -> delta(0)
input(f???????) -> now(182804) -> delta(0)
input(g???????) -> now(182804) -> delta(0)
input(h???????) -> now(182804) -> delta(0)
input(i???????) -> now(182804) -> delta(0)
input(j???????) -> now(182804) -> delta(0)
input(k???????) -> now(182804) -> delta(0)
input(l???????) -> now(182804) -> delta(0)
input(m???????) -> now(182804) -> delta(0)
input(n???????) -> now(182804) -> delta(0)
input(o???????) -> now(182804) -> delta(0)
input(p???????) -> now(182804) -> delta(0)
input(q???????) -> now(182804) -> delta(0)
......
input(AzI0wBsO) -> now(211070) -> delta(0)
input(AzI0wBsP) -> now(211069) -> delta(-1)
input(AzI0wBsQ) -> now(211069) -> delta(0)
input(AzI0wBsR) -> now(211069) -> delta(0)
input(AzI0wBsS) -> now(211069) -> delta(0)
input(AzI0wBsT) -> now(211069) -> delta(0)
input(AzI0wBsU) -> now(211069) -> delta(0)
input(AzI0wBsV) -> now(211069) -> delta(0)
input(AzI0wBsW) -> now(211069) -> delta(0)
input(AzI0wBsX) -> now(214976) -> delta(3907)
Found pwd: AzI0wBsX
python guessPWD.py  31.04s user 14.72s system 105% cpu 43.341 total
```

验证一下

```bash
NDH2k13-crackme-500 [master●] ./crackme 
Jonathan Salwan loves you <3
----------------------------

Password: AzI0wBsX
Good password
```

这样，我们用不到 5 分钟的时间就猜出了 flag

> inscount1(BB级插桩) 与 inscount0(ins级插桩) 效果相同，但 inscount1 速度更快，实际解题时可以用 inscount1 代替 inscount0

### hxpCTF-2017-main_strip

再以 hxpCTF2017 的一道题目演示改造 pintool 用于解题，我们着重演示改造 pintool 的步骤，因此恢复符号表和分析程序流程的部分可以参考这篇 [writeup](http://eternal.red/2017/dont_panic-writeup/)

我们先尝试用上例的方法解这道题目

```bash
hxpCTF-2017-main_strip [master●●] ~/pin-3.6-gcc-linux/pin -t ./myInscount1.so -- ./main_strip a
Nope.
Count: 517715
hxpCTF-2017-main_strip [master●●] ~/pin-3.6-gcc-linux/pin -t ./myInscount1.so -- ./main_strip a 
Nope.
Count: 545828
hxpCTF-2017-main_strip [master●●] ~/pin-3.6-gcc-linux/pin -t ./myInscount1.so -- ./main_strip a
Nope.
Count: 532656
hxpCTF-2017-main_strip [master●●] ~/pin-3.6-gcc-linux/pin -t ./myInscount1.so -- ./main_strip a
Nope.
Count: 524544
hxpCTF-2017-main_strip [master●●] ~/pin-3.6-gcc-linux/pin -t ./myInscount1.so -- ./main_strip a
Nope.
Count: 582401
```

很不幸，即使我们使用同一个输入，指令数也是有较大变化的，使用现有的 pintool 难以解出这道题目，我们进行更深一步的分析，验证 flag 的关键部分可以表示为如下代码

```C
for (int i=0; i<length(provided_flag); i++)
{
	if (main_mapanic(provided_flag[i]) != constant_binary_blob[i])
	{
		bad_boy();
		exit();
	}
	goodboy();
}
```

可以看出判断相等是逐位进行的，因此可以考虑对 inscount0 的 docount 函数做如下更改

```C
更改前：
VOID docount() { icount++; }
更改后：
VOID docount(void *ip) 
{
  	// .text:000000000047B96E  cmp al, cl; cmp mapanic(provided_flag[i]), constant_binary_blob[i]
	if ((long long int)ip == 0x000000000047B96E)
	 icount++; 
}
```

只有运行到 0x47B96E 一句才会计数，这样我们就可以根据 pintool 的结果来逐位爆破 flag 了

>   然而，因为该题目的指令较多，指令级别的插桩会耗费较长时间，需要1h左右才能得到 flag



##　总结

-   使用 intel pin 可以解一部分 CTF challenges，尤其是虚拟机或者混淆严重的逆向题目，但 pin 的用途绝不局限于求解 CTF challenges
-   使用 pin 可以解一部分 CTF 题目，但也仅仅是一部分题目，多数题目由于插桩代价大，难以提取侧信道信息，pintool 难以编写等原因使用 pin 求解得不偿失，因此使用 pin 求解 CTF challenges 可以总结为下条原则：
    -   能用血赚，凉了不亏

## Reference

-   http://shell-storm.org/blog/A-binary-analysis-count-me-if-you-can/
-   http://brieflyx.me/2017/binary-analysis/intel-pin-intro/
-   http://eternal.red/2017/dont_panic-writeup/
-   https://github.com/M4xW4n9/slides/blob/master/auto/04-PinTutorial.pdf







