# Lab2

我们会在Lab2中实现一个真实的操作系统内核的调试，我们采用的是`[xv6-public](https://github.com/mit-pdos/xv6-public)`的代码

# 预备知识（工具篇）

需要编译这个操作系统内核，我们需要了解一些qemu和一些关于link的知识。

## qemu（我们采用的版本是qemu-system-i386）

QEMU是一款开源的硬件虚拟化软件，它允许用户在单个物理机器上创建和运行多个虚拟机（VM）。它可以模拟各种硬件平台，为运行不同操作系统和应用程序提供一个隔离的VM环境。QEMU被广泛用于开发和测试，同时也用于生产环境中部署虚拟服务器。

PS：就是一个提供硬件虚拟化的软件和VMware之类的相同，个人没研究过VMware的全部功能。不知道能不能支持numa

下面请自行阅读Makefile，对于不知道的参数使用`man`指令来进行阅读。

下面的代码提供了一个操作系统的多种启动方式

```jsx
qemu: fs.img xv6.img
	$(QEMU) -serial mon:stdio $(QEMUOPTS)

qemu-memfs: xv6memfs.img
	$(QEMU) -drive file=xv6memfs.img,index=0,media=disk,format=raw -smp $(CPUS) -m 256

qemu-nox: fs.img xv6.img
	$(QEMU) -nographic $(QEMUOPTS)

.gdbinit: .gdbinit.tmpl
	sed "s/localhost:1234/localhost:$(GDBPORT)/" < $^ > $@

qemu-gdb: fs.img xv6.img .gdbinit
	@echo "*** Now run 'gdb'." 1>&2
	$(QEMU) -serial mon:stdio $(QEMUOPTS) -S $(QEMUGDB)

qemu-nox-gdb: fs.img xv6.img .gdbinit
	@echo "*** Now run 'gdb'." 1>&2
	$(QEMU) -nographic $(QEMUOPTS) -S $(QEMUGDB)
```

## 使用.gdbinit（建议掌握）（这一段是ChatGPT生成的）

PS：如果不使用.gdbinit，调试的枯燥程度很难想象的

`gdb` 提供了一种机制来自动加载预定义的命令，这通常通过一个名为 `.gdbinit` 的文件完成。当 `gdb` 启动时，它会在当前目录下查找 `.gdbinit` 文件，并执行其中的命令。这对于设置调试会话的环境非常有用，比如定义自定义命令、设置断点或配置显示选项等。

### 用法示例

下面是一个 `.gdbinit` 文件的简单示例：

```
# .gdbinit
# 设置断点
break main

# 设置调试格式
set print pretty

# 定义自定义命令
define hello
  echo Hello, GDB!\\n
end

```

在上述示例中：

- `break main`: 在 `main` 函数上设置一个断点。
- `set print pretty`: 设置打印输出格式为“pretty”，以便更易于阅读。
- `define ... end`: 定义一个自定义命令，这里定义了一个名为 `hello` 的命令，它将输出 "Hello, GDB!"。

### 注意

- 出于安全原因，`gdb` 默认不会自动加载当前目录中的 `.gdbinit` 文件。要允许加载，请在 `/home/<your_name>/.config/gdb/gdbinit` 中添加`add-auto-load-safe-path /your/safe/path`，其中 `/your/safe/path` 是你的项目目录或其他安全路径。

### 更多命令和用法

- `run`: 带参数启动你的程序。
- `info registers`: 显示寄存器的当前状态。
- `x/...`: 用于内存检查。
- `bt`: 显示调用堆栈。

### 使用 `.gdbinit` 的优点

1. **自动设置**：可以自动设置断点、监视点等。
2. **环境配置**：可以预先定义一些格式和显示的配置。
3. **自定义命令**：可以创建自定义命令以简化复杂的调试流程。

通过合理利用 `.gdbinit`，你可以提高调试的效率和准确性，特别是在复杂项目中。你可以将常用的命令和设置放在 `.gdbinit` 文件中，这样在每次启动 `gdb` 时都会自动加载这些预定义的设置和命令，省去了手动设置的麻烦。

# 预备知识（知识篇）

## 编译过程（结合gcc指令的介绍）（在这里我们着重讨论的是ld的过程）

1. **预处理（Preprocessing）**:
    - 在GCC中，预处理步骤可以通过 `E` 标志单独执行。
    - 命令示例：`gcc -E source.c`。
    - 这将只执行预处理步骤，处理如`#include`、`#define`等指令，并输出扩展的源代码。
2. **编译（Compilation）**:
    - 编译步骤将预处理后的代码转换为汇编代码。
    - 在GCC中，这可以通过 `S` 标志来执行。
    - 命令示例：`gcc -S source.c`。
    - 这将生成一个汇编文件（通常是`.s`扩展名）。
3. **汇编（Assembly）**:
    - GCC将汇编代码转换为机器代码（目标文件）。
    - 通常这一步是自动进行的，但也可以单独执行。
    - 命令示例：`gcc -c source.s`，这将生成一个目标文件（`.o`扩展名）。
4. **链接（Linking）**:
    - 最后，GCC的链接器（通常是`ld`）将一个或多个目标文件与所需的库文件链接在一起，生成可执行文件。
    - 命令示例：`gcc source.o -o program`。
    - 这里，`source.o`是目标文件，`program`是最终生成的可执行文件。

综合命令:

- 通常，上述步骤可以通过单个GCC命令一次性完成：`gcc source.c -o program`。
- 这个命令将执行全部的编译步骤：预处理、编译、汇编和链接，最终生成名为`program`的可执行文件。

GCC还有许多其他选项和功能，例如优化选项（如`-O2`）、调试信息生成（如`-g`）等，可以根据具体需求进行选择和使用。

### 链接器脚本

[GNU手册](https://ftp.gnu.org/old-gnu/Manuals/ld-2.9.1/html_chapter/ld_3.html)

链接器脚本定义了程序的内存布局，指定了不同段（如文本段、数据段、bss段等）在可执行文件中的位置和地址。这对于操作系统这样需要精确控制内存布局的程序来说**非常重要**。

下面的代码也正好能说明2G user + 2G kernel的分配

我们会在后面的例子中分析xv6的链接器脚本（**kernel.ld**）

```c
/* Simple linker script for the JOS kernel.
   See the GNU ld 'info' manual ("info ld") to learn the syntax. */

OUTPUT_FORMAT("elf32-i386", "elf32-i386", "elf32-i386")
OUTPUT_ARCH(i386)
ENTRY(_start)

SECTIONS
{
	/* Link the kernel at this address: "." means the current address */
        /* Must be equal to KERNLINK */
	. = 0x80100000;

	.text : AT(0x100000) {
		*(.text .stub .text.* .gnu.linkonce.t.*)
	}

	PROVIDE(etext = .);	/* Define the 'etext' symbol to this value */

	.rodata : {
		*(.rodata .rodata.* .gnu.linkonce.r.*)
	}

	/* Include debugging information in kernel memory */
	.stab : {
		PROVIDE(__STAB_BEGIN__ = .);
		*(.stab);
		PROVIDE(__STAB_END__ = .);
	}

	.stabstr : {
		PROVIDE(__STABSTR_BEGIN__ = .);
		*(.stabstr);
		PROVIDE(__STABSTR_END__ = .);
	}

	/* Adjust the address for the data segment to the next page */
	. = ALIGN(0x1000);

	/* Conventionally, Unix linkers provide pseudo-symbols
	 * etext, edata, and end, at the end of the text, data, and bss.
	 * For the kernel mapping, we need the address at the beginning
	 * of the data section, but that's not one of the conventional
	 * symbols, because the convention started before there was a
	 * read-only rodata section between text and data. */
	PROVIDE(data = .);

	/* The data segment */
	.data : {
		*(.data)
	}

	PROVIDE(edata = .);

	.bss : {
		*(.bss)
	}

	PROVIDE(end = .);

	/DISCARD/ : {
		*(.eh_frame .note.GNU-stack)
	}
}
```

# 启动调试

## 启动的分类

- **冷启动**，是指计算机在关机状态下按 POWER 键启动，又叫硬件启动，比如开机，这种启动方式在启动之前计算机处于断电状态，像内存这种需要加电维持的存储部件里面的内容都丢失了，加电开机那一刻里面的值都是随机的，操作系统会对其进行初始化。
- **热启动**是在加电的情况下启动，又叫软件启动，比如重启，这种启动方式在启动之前和启动之后电没断过，内存等存储部件里面的值不会改变，但毕竟是启动过程，操作系统会对其进行初始化。
- PS：不知道大家有没有遇到电脑卡死的情况，这种情况下一般不是长按开机键，进行reset。在世纪初左右的电脑，reset键和开机键是分开的。

# **BIOS->MBR->Bootloader->OS->~~Multiprocessor~~**

在这引入启动的过程，然后我们正式开始实验，我们这里不考虑多核启动的问题

### 编译运行

建议先把CPU数设置为1，然后进行调试，简化启动流程。当然可以在Makefile中进行修改（**建议修改`Makefile`减少后续的操作**）

```c
make 
make qemu
make CPU=1 qemu-gdb 
```

### 如何启动调试

在一个窗口中输入`make qemu-gdb`，另外一个相同目录下的窗口中启动`gdb`即可。

### 手册的重要性

PS：RTFM，STFW

手册规定了计算机所有可能的行为，计算机永远不会错，所以当我们想要学习这种人定的机器时，我们应该使用手册。

如果大家在后面的实验中遇到了问题，应该积极的RTFM和STFW

**Intel® 64 and IA-32 Architectures Software Developer’s Manual**

[intel 32位和64位开发手册.pdf](Lab2%207b46a09ca2ce4fc1a0dd8eaceaf5b97c/intel_32%25E4%25BD%258D%25E5%2592%258C64%25E4%25BD%258D%25E5%25BC%2580%25E5%258F%2591%25E6%2589%258B%25E5%2586%258C.pdf)

### 理论过程

![Untitled](Lab2%207b46a09ca2ce4fc1a0dd8eaceaf5b97c/Untitled.png)

**BIOS的作用：**

- **将硬盘(通常引导设备就是硬盘)最开始那个扇区 MBR 加载到0x7c00**
- 自检，然后对一些硬件设备做简单的初始化
- 构建中断向量表加载中断服务程序

**MBR的作用：**

**MBR 的代码在分区表中寻找可以引导存在操作系统的分区，也就是寻找标记为 0x80 的活动分区，然后加载该活动分区的引导块，再执行其中的操作系统引导程序 Bootloader**。

****Bootloader的作用：****

**主要作用就是将操作系统加载到内存里面**。**操作系统也是一个程序，需要加载到内存里面才能运行**

还做了一些其他事情，比如进入保护模式，开启分页机制，建立内存的映射等等

**OS的作用：**

操作系统内核加载到内存之后，就做一些初始化工作建立好工作环境，比如各个硬件的初始化，重新设置 GDT，IDT 等等初始的操作。初始化启动其他处理器(如果有多个处理器的话)。这里不细说，也不好叙述。

****Multiprocessor：（拓展知识）****

**先启动一个 CPU，用它作为基础启动其他的处理器**。**先启动的这个 CPU 称作 BSP(BootStrap Processor)，其他处理器叫做 AP(Application Processor)**。

## BIOS

在启动的瞬间就会完成CS和IP的初始化：$C S=0 x f 000, I P=0 x f f f 0 \text { }$

操作系统在开机时处于**实地址模式**

刚启动的时候计算机处于实模式，实模式地址下总线只用了20位，只有$2^{20}=1 M$的寻址空间，换句话说只用了内存的低1M，这时候分页机制还没建立，**CPU就是运行在实际的物理地址**上。（实模式）

**如何访问实地址模式下的内存：**

在实模式下，物理地址（即实际RAM中的地址）是通过将**`CS`**左移4位（或者说乘以16）并与**`IP`**相加来计算的。下面是计算公式：

$\text { Physical Address }=(\mathrm{CS} \times 16)+\mathrm{IP}$

根据$C S=0 x f 000, I P=0 x f f f 0 \text {}$，可以得到

$\text { address }=0 x f 000<<4+0 x f f f 0=0 x f f f f 0$

![Untitled](Lab2%207b46a09ca2ce4fc1a0dd8eaceaf5b97c/Untitled%201.png)

**LAB1：**

**使用gdb来查看$0xffff0$的语句是什么？并且说明BIOS在内存中的位置。**

### MBR过程

**介绍：**MBR（Master Boot Record）是位于存储设备（如硬盘或SSD）最开始部分的一个特殊区域，具有重要的启动和分区表信息。MBR的大小通常为512字节。

bootasm.S 和 bootmain.c 两文件联合在一起**编译成二进制文件 bootblock 放在磁盘最开始的那个扇区**，然后被 BIOS 加载到0x7c00处，从0x7c00处开始执行。

使用file指令查看文件的属性

```c
file bootblock
bootblock: DOS/MBR boot sector
```

查看Makefile

```c
bootblock: bootasm.S bootmain.c
	$(CC) $(CFLAGS) -fno-pic -O -nostdinc -I. -c bootmain.c
	$(CC) $(CFLAGS) -fno-pic -nostdinc -I. -c bootas1m.S
	$(LD) $(LDFLAGS) -N -e start **-Ttext 0x7C00** -o bootblock.o bootasm.o bootmain.o
	$(OBJDUMP) -S bootblock.o > bootblock.asm
	$(OBJCOPY) -S -O binary -j .text bootblock.o bootblock
	./sign.pl bootblock
```

PS：这里我就不多做解释了，如果感兴趣，我会给出一个相似的实验，**写一个能直接再硬件上运行的程序（拓展补充）**

**LAB2：**

**使用gdb进行内存追踪，来查看具体是BIOS的哪一段代码加载了MBR。**

**LAB3：**

**阅读bootasm.S，给出A20线如何开启**

**给出操作系统如何切换到保护模式**

# 拓展补充

[BIOS VS UEFI](https://www.zhihu.com/question/21672895)

**写一个能直接再硬件上运行的程序（拓展补充）**

# 实验任务

**要求：**

**需要有自己信息的截图，gdb需要输出自定义字符串（带学号和姓名），需要高亮关键调试信息并进行解释**

```c
(gdb) printf "学号-姓名"
```

**关于2-3需要给出详细解释，具体到函数层面。（解释每个代码段的功能）**

**2-1 LAB：**

**使用gdb来查看地址$0xffff0$的语句是什么？并且说明BIOS在内存中的位置。** 

**2-2 LAB：**

**使用gdb进行内存追踪，来查看具体是BIOS的哪一段代码加载了MBR。**

**2-3 LAB：**

**阅读bootasm.S，给出A20线如何开启**

**给出操作系统如何切换到保护模式的，结合代码说明，最好能结合手册说明。**