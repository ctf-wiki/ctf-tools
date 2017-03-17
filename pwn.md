# PWN

## 反汇编

- [IDA Pro 6.8 Green](http://down.52pojie.cn/Tools/Disassemblers/IDA_Pro_v6.8_and_Hex-Rays_Decompiler_(ARM,x64,x86)_Green.rar)

## 调试

* [peda](https://github.com/longld/peda)

  > **Installation**
  >
  > ```
  > git clone https://github.com/longld/peda.git ~/peda
  > echo "source ~/peda/peda.py" >> ~/.gdbinit
  > echo "DONE! debug your program with gdb and enjoy"
  > ```

  [截至2016年12月27日 master 分支打包](http://down.40huo.cn/pwn/peda-master.zip)

## Patch

* [Fentanyl](https://github.com/isislab/Fentanyl)

  IDA Python 脚本，用于快速 patch。

  > **Usage**
  >
  > **Loading Fentanyl.py**
  >
  > 1. `Alt+F7` or `File > Script File` to load scripts
  > 2. Browse to `main.py` and open it
  > 3. That's it!
  >
  > ​
  >
  > **Key Bindings**
  >
  > *Some of these keybindings can be accessed by right-clicking on the screen in graph view.*
  >
  > - `Alt-N` Convert instructions to nops
  > - `Alt-X` Nop all xrefs to this function
  > - `Alt-J` Invert conditional jump
  > - `Alt-P` Patch instruction
  > - `Alt-Z` Undo modification (Won't always work. Should still be careful editing.)
  > - `Alt-Y` Redo modification (Won't always work. Should still be careful editing.)
  > - `Alt-S` Save file
  > - `Alt-C` Find Code Caves
  > - `Ctrl-Alt-F` Make jump unconditional
  > - `Ctrl-Alt-N` Neuter the binary (remove calls to fork, setuid, setgid, getpwnam, setgroups, and chdir)

  [截至2016年12月27日 master 分支打包](http://down.40huo.cn/pwn/Fentanyl-master.zip)