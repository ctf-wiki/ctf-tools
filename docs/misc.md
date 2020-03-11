# MISC

## 图片隐写

* [Stegsolve](http://down.40huo.cn/misc/Stegsolve.jar)

* [Stegdetect amd64 deb](http://down.40huo.cn/misc/stegdetect_0.6-6_amd64.deb)

    Stegdetect 的主要选项如下：

    q – 仅显示可能包含隐藏内容的图像

    n – 启用检查 JPEG 文件头功能，以降低误报率。如果启用，所有带有批注区域的文件将被视为没有被嵌入信息。如果 JPEG 文件的 JFIF 标识符中的版本号不是 1.1，则禁用 OutGuess 检测。

    s – 修改检测算法的敏感度，该值的默认值为 1。检测结果的匹配度与检测算法的敏感度成正比，算法敏感度的值越大，检测出的可疑文件包含敏感信息的可能性越大。

    d – 打印带行号的调试信息。

    t – 设置要检测哪些隐写工具（默认检测 jopi），可设置的选项如下：

    j – 检测图像中的信息是否是用 jsteg 嵌入的。

    o – 检测图像中的信息是否是用 outguess 嵌入的。

    p – 检测图像中的信息是否是用 jphide 嵌入的。

    i – 检测图像中的信息是否是用 invisible secrets 嵌入的。

* [Steghide 0.5.1 win32](http://down.40huo.cn/misc/steghide-0.5.1-win32.zip)

* [Outguess amd64 deb](http://down.40huo.cn/misc/outguess_0.2-7_amd64.deb)

* [PNGCheck 2.3.0 win32](http://down.40huo.cn/misc/pngcheck-2.3.0-win32.zip)

* [JPHS win32](http://down.40huo.cn/misc/jphs_05.zip)

* [OurSecret](http://down.40huo.cn/misc/oursecret.zip)

## 压缩包

* [Ziperello](http://down.40huo.cn/misc/Ziperello.zip)

    zip 压缩包密码爆破。

* [Advanced Rar Password Recovery](http://down.40huo.cn/misc/AdvancedRARPassword.zip)

* [Advanced Zip Password Recovery](http://down.40huo.cn/misc/AZPR_4.0.zip)

## 无线密码

* [Elcomsoft Wireless Security Auditor](http://down.40huo.cn/misc/Elcomsoft.Wireless.Security.Auditor.Pro.v5.9.359-BRD_tt7z.com.rar)

## 编辑器

* [010 Editor Windows x64](http://down.40huo.cn/misc/010_Editor_v6.0.2_CracKed_For_Windows_x64.zip)

## NTFS 文件流

* [Alternate Stream View](http://down.40huo.cn/misc/alternatestreamview.zip)

## 音频隐写

* [Audacity](http://down.40huo.cn/misc/audacity-win-2.1.2.zip)
* [在线拨号音识别](http://dialabc.com/sound/detect/)

## 取证

* [Elcomsoft Forensic Disk Decryptor](http://down.40huo.cn/misc/efdd_setup_en.msi)
    * [破解工具](http://down.40huo.cn/misc/Elcomsoft.Forensic.Disk.Decryptor.CracKed.By.Hmily.LCG.rar)

## 条形码、二维码

* [条形码、二维码在线识别](https://online-barcode-reader.inliteresearch.com/default.aspx)

## GIF

* [GIF 在线分解](http://ezgif.com/split)

## pyc

- [Stegosaurus](https://github.com/AngelKitty/stegosaurus)

   Stegosaurus 是一款隐写工具，它允许我们在 Python 字节码文件( pyc 或 pyo )中嵌入任意 Payload 。由于编码密度较低，因此我们嵌入 Payload 的过程既不会改变源代码的运行行为，也不会改变源文件的文件大小。 Payload 代码会被分散嵌入到字节码之中，所以类似 strings 这样的代码工具无法查找到实际的 Payload 。 Python 的 dis 模块会返回源文件的字节码，然后我们就可以使用 Stegosaurus 来嵌入 Payload 了。

  > **Tips: Stegosaurus 仅支持 Python3.6 及其以下版本**

  Stegosaurus 的基本用法如下：

  ```shell
  $ python3 -m stegosaurus -h
  usage: stegosaurus.py [-h] [-p PAYLOAD] [-r] [-s] [-v] [-x] carrier
  
  positional arguments:
    carrier               Carrier py, pyc or pyo file
  
  optional arguments:
    -h, --help            show this help message and exit
    -p PAYLOAD, --payload PAYLOAD
                          Embed payload in carrier file
    -r, --report          Report max available payload size carrier supports
    -s, --side-by-side    Do not overwrite carrier file, install side by side
                          instead.
    -v, --verbose         Increase verbosity once per use
    -x, --extract         Extract payload from carrier file
  ```