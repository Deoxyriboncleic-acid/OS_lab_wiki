# vim/gcc/Makefile

# vim的使用

## 为什么选择使用vim进行编辑代码

### 效率高

Vim 的设计哲学强调使用键盘快捷键来提高编辑效率。它提供了大量的命令组合来实现快速的文本操作。

**例子：**

```c
#include <stdio.h>

int main() {
    int a, b;
    char op;

    printf("Enter an operation (+ or -): ");
    scanf(" %c", &op);
    printf("Enter two numbers: ");
    scanf("%d %d", &a, &b);

    if (op == '+') {
        printf("%d + %d = %d\n", a, b, a + b);
    } else if (op == '-') {
        printf("%d - %d = %d\n", a, b, a - b);
    } else {
        printf("Invalid operation!\n");
    }

    return 0;
}
```

将其a，b快速替换为num1，num2

```c
:%s/\ba\b/num1/g
:%s/\bb\b/num2/g
```

这些命令会在全文中将 **`a`** 和 **`b`** 替换为 **`num1`** 和 **`num2`**，**`\b`** 表示单词边界，以确保只替换整个单词。

当然可能会觉得IDE也能执行这些操作，但是vim其实还支持指令重用。（这里就不进行引申）

### 轻量级且具有广泛的**可用性**

它启动迅速，对系统资源的消耗很小。并且vim几乎存在所有的UNIX系统和类UNIX系统上。

### 有强大的社区支持

vim可以支持非常多的插件，它可以让vim具有和IDE一样的使用体验（甚至更爽，毕竟键位都是自己设置的）

## vim的使用

vim的接口其实是一种编程语言。

[计算机中缺失的一课](https://missing-semester-cn.github.io/2020/editors/)（个人认为这个教程已经可以入门了，但是下面还是提供一个快速入门）

### vim的模式

- **正常模式**：在文件中四处移动光标进行修改
- **插入模式**：插入文本
- **替换模式**：替换文本
- **可视化模式**（一般，行，块）：选中文本块
- **命令模式**：用于执行命令

## **命令行**

在正常模式下键入 `:` 进入命令行模式。 在键入 `:` 后，你的光标会立即跳到屏幕下方的命令行。 这个模式有很多功能，包括打开，保存，关闭文件，以及 退出vim

****命令行****

- `:q` 退出（关闭窗口）
- `:w` 保存（写）
- `:wq` 保存然后退出
- `:e {文件名}` 打开要编辑的文件
- `:ls` 显示打开的缓存
- `:help {标题}` 打开帮助文档
    - `:help :w` 打开 `:w` 命令的帮助文档
    - `:help w` 打开 `w` 移动的帮助文档

## **移动**

多数时候你会在正常模式下，使用移动命令在缓存中导航。在 Vim 里面移动也被称为 “名词”， 因为它们指向文字块。

- 基本移动: `hjkl` （左， 下， 上， 右）
- 词： `w` （下一个词）， `b` （词初）， `e` （词尾）
- 行： `0` （行初）， `^` （第一个非空格字符）， `$` （行尾）
- 屏幕： `H` （屏幕首行）， `M` （屏幕中间）， `L` （屏幕底部）
- 翻页： `Ctrl-u` （上翻）， `Ctrl-d` （下翻）
- 文件： `gg` （文件头）， `G` （文件尾）
- 行数： `:{行数}<CR>` 或者 `{行数}G` ({行数}为行数)
- 杂项： `%` （找到配对，比如括号或者 /* */ 之类的注释对）
- 查找： `f{字符}`， `t{字符}`， `F{字符}`， `T{字符}`
    - 查找/到 向前/向后 在本行的{字符}
    - `,` / `;` 用于导航匹配
- 搜索: `/{正则表达式}`, `n` / `N` 用于导航匹配

# GCC的使用和makefile

在下面我们将会编译两个文件

```c
gcc -o sum sum.c -g
```

- -o 重命名
- -O0 进行O0级别的优化
- -O1进行O1级别的优化
- -O2
- -O3 同理
- -g 生成调试信息，方便使用gdb进行调试
- -Wall 警告全部”（Warn All），意在让编译器报告所有类型的潜在代码问题，如未使用的变量、未初始化的变量、执行不到的代码等

### 大家将下面两个文件按照O0，O1，O2优化编译运行

这个时候我们要对这个文件进行多次的重复编译，我们在这里可以使用Makefile进行编译，如果你是一个中型或者大型程序的开发人员，你应该已经了解这个内容了

**Makefile的使用（[跟我一起写Makefile](https://github.com/seisman/how-to-write-makefile)）（我是通过这个手册学会的）**

下面这个手册是我们写的简单手册

**规则（Rules）**：Makefile 中的每条规则通常有三个部分：目标、依赖和命令。

- **目标（Target）**：通常是要生成的文件，例如可执行文件或对象文件。
- **依赖（Dependencies）**：目标文件依赖的文件，通常是源代码或其他目标文件。
- **命令（Commands）**：生成目标文件的实际命令，如编译命令。

每次修改优化等级就只需要修改CFLAGS的值

```c
# 定义编译器
CC = gcc

# 定义编译选项
CFLAGS = -Wall -g -O0

# 定义目标
all: sum

# 定义如何生成最终目标
sum: sum.o
	$(CC) $(CFLAGS) -o sum sum.o

# 定义对象文件如何从源文件生成
sum.o: sum.c
	$(CC) $(CFLAGS) -c sum.c

# 清理编译生成的文件
clean:
	rm -f sum sum.o
```

第一个文件时sum.c

```c
#include "thread.h"

#define N 10000000

long sum = 0;

void Tsum() {
  for (int i = 0; i < N; i++) {
    sum++;
  }
}

int main() {
  create(Tsum);
  create(Tsum);
  join();
  printf("sum = %ld\n", sum);
}
```

PS：解释一下sum.c的内容，共享变量x被两个进行争夺。然后分别进行N次加法，最后使用join（）进行合并操作。

其中的thread.h我们已经写好了，封装好了pthread的功能

```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <stdatomic.h>
#include <assert.h>
#include <unistd.h>
#include <pthread.h>

#define NTHREAD 64
enum { T_FREE = 0, T_LIVE, T_DEAD, };
struct thread {
  int id, status;
  pthread_t thread;
  void (*entry)(int);
};

struct thread tpool[NTHREAD], *tptr = tpool;

void *wrapper(void *arg) {
  struct thread *thread = (struct thread *)arg;
  thread->entry(thread->id);
  return NULL;
}

void create(void *fn) {
  assert(tptr - tpool < NTHREAD);
  *tptr = (struct thread) {
    .id = tptr - tpool + 1,
    .status = T_LIVE,
    .entry = fn,
  };
  pthread_create(&(tptr->thread), NULL, wrapper, tptr);
  ++tptr;
}

void join() {
  for (int i = 0; i < NTHREAD; i++) {
    struct thread *t = &tpool[i];
    if (t->status == T_LIVE) {
      pthread_join(t->thread, NULL);
      t->status = T_DEAD;
    }
  }
}

__attribute__((destructor)) void cleanup() {
  join();
}
```

### GDB调试

[CSAPP提供的GDB的简单手册](https://csapp.cs.cmu.edu/3e/docs/gdbnotes-x86-64.pdf)

**常用命令：**

**程序控制:**

- run - 运行调试的程序
- break - 设置断点
- continue - 继续运行,直到下一个断点
- next - 单步执行下一行代码
- step - 单步调试,会进入函数内部
- finish - 执行完当前函数后停止

**查看信息:**

- print - 打印变量的值
- bt - 显示当前的函数调用堆栈
- frame - 切换到指定的栈帧
- info b - 显示当前设置的所有断点
- info threads - 显示当前的线程

**查看数据:**

- x - 检查内存地址的数据
- display - 持续输出表达式的值

**多线程调试:**

- thread - 切换当前调试线程
- info threads - 显示所有线程信息

**其它常用:**

- quit - 退出GDB
- set - 更改GDB设置
- help - 查看命令帮助、文档

通过对代码执行的观察，可以发现一个有趣的现象

**使用O0优化的代码的值时介于N到2N之间**

**使用O1优化的代码的值为N**

**而O2优化的代码的值为2N**

对于这个预料之外的结果我们对其使用gdb进行反汇编

O0经过分析发现就是最普通的代码，编译器没有进行任何层面的优化。

并且汇编指令的执行没有**原子性**，对一个共享变量进行访问，肯定会产生错误的结果

![Untitled](vim%20gcc%20Makefile%205544bd1284b542d6bc803b704795210e/Untitled.png)

O1的代码就比较巧妙了，编译器使用了寄存器来加速变量的访问，其中产生问题的代码来自**<+34>**这一行

编译器创建了一个**使用寄存器临时变量，来替换位于位于内存上的全局变量**

最后的时候将这个变量的值cover全局变量的值

两个进程分别在自己的临时变量计算sum值最后都为N，然后替换两个全局变量，所以值恒定为N

![Untitled](vim%20gcc%20Makefile%205544bd1284b542d6bc803b704795210e/Untitled%201.png)

O2的方法就更加简单了，直接就选择了**循环合并**

并且这个方法使得这个汇编具有了**原子性**，导致了最后的代码的正确性

（这是一个优化理论中很重要的方法）（有些过于复杂的代码不一定进行这样的全局合并，但是增加代码的步距也能起到很好的效果）（很多时候是减少了cache miss）

PS： 0x989680 = 10,000,000

![Untitled](vim%20gcc%20Makefile%205544bd1284b542d6bc803b704795210e/Untitled%202.png)

# asm volatile的使用

**`asm volatile`** 是在 C 或 C++ 程序中嵌入汇编代码的一种方式。这个指令用于告诉编译器，嵌入的汇编代码可能会产生某些不明显的副作用，因此编译器在优化代码时**不应该忽略、修改或重新排序这些汇编指令**。

```c
asm volatile("assembly code here" 
             : "output operands"   // 输出操作数
             : "input operands"    // 输入操作数
             : "clobbered registers" // 受影响的寄存器列表
            );
```

```c
#include <stdio.h>

int main() {
    int a = 5;    // 定义第一个整数
    int b = 3;    // 定义第二个整数
    int sum;

    // 使用内嵌汇编进行加法
    asm volatile (
        "add %1, %2\n\t"     // 汇编指令，将 %1 和 %2 相加
        "mov %2, %0\n\t"     // 将结果移动到 %0
        : "=r" (sum)         // 输出操作数：'=' 表示输出，'r' 表示任意寄存器
        : "r" (a), "r" (b)   // 输入操作数：两个 'r' 表示这两个输入都在任意寄存器中
    );

    printf("The sum of %d and %d is %d\n", a, b, sum); // 打印结果

    return 0;
}
```

# 作业

1. 在命令行输入vimtutor，然后完成lab，并进行截图
2. 用C语言写一个1+2+3+……+10的程序，并且使用Makefile在O0情况下编译，并且使用gdb进行调试，在循环出增添变量，查看sum值得变换，并进行截图（需要截取到n = 3）

# 拓展补充

## 关于SUM的那个问题的额外解释

现代的操作系统的结构远远超出了课本的知识范围,所以有些时候为了保证严谨性我需要提供给大家一些课外的知识。

### 在**O0优化**的情况下，一定**sum的值就大于等于N**吗？

答案是否定的，让我们来做一个小小的测试（如果无法观测，就提高i的循环次数）

```c
#!/bin/bash

for i in {1..100000}; do
	result=$(./sum)  # 执行程序并捕获输出
	number=$(echo $result | awk '{print $3}')  # 解析输出中的数值

	if (( number < 10000000 )); then
		echo $result >> a.log  # 如果小于10000000，则写入文件
	fi
done
```

脚本没有运行结束，我们就已经发现a.log中有输出了，这时候就可以停止脚本了

```c
sum = 9603079
sum = 9863297
sum = 9964411
```

这个问题的具体答案我也不知道，个人直觉是在内存一致性和缓存一致性上，或者是宽松内存模型，反正最后是在内存的角度来考虑，但是当让可以从调度层面考虑。

希望有大佬来解决我的疑惑

下面的第一篇文章是关于内存一致性和缓存一致性的

第二篇为X86的宽松内存模型 

[A Primer on Memory Consistency and Cache Coherence.pdf](vim%20gcc%20Makefile%205544bd1284b542d6bc803b704795210e/A_Primer_on_Memory_Consistency_and_Cache_Coherence.pdf)

[https://research.swtch.com/hwmm](https://research.swtch.com/hwmm)

## 内存屏障和****volatile****

内存屏障与volatile是高并发编程中比较常用的两个技术，**无锁队列**的时候就会用到这两项技术。

[https://blog.csdn.net/obvious__/article/details/118653001](https://blog.csdn.net/obvious__/article/details/118653001)