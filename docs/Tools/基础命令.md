# 课时一

## apt

`apt` 是 Linux 系统上用于管理软件包的命令行工具，常用于 Debian 及其衍生发行版（如 Ubuntu）。

![image-20231126003413316](https://gitee.com/paulinghong/typora-chart-bed/raw/master/img/202311271310323.png)

这个错误提示表明在你的系统上没有找到 Java 运行时环境（Java Runtime）。为了解决这个问题，你需要安装 Java 运行时环境。以下是一些常见操作系统的解决方法：

### 对于 Ubuntu 或其他基于 Debian 的系统：

1. 打开终端。

2. 运行以下命令来安装 OpenJDK（Java 的开源实现之一）：

   ```bash
   sudo apt update
   sudo apt install default-jre
   ```

   如果你需要 Java 开发工具包（JDK），可以使用以下命令：

   ```bash
   sudo apt install default-jdk
   ```

### 对于 Fedora 或其他基于 Red Hat 的系统：

1. 打开终端。

2. 运行以下命令来安装 OpenJDK：

   ```bash
   sudo dnf install java-latest-openjdk
   ```

   如果你需要 JDK，可以运行：

   ```bash
   sudo dnf install java-latest-openjdk-devel
   ```



---



以下是一个简单的 `apt` 使用例子，假设你想安装一个软件包：

1. **更新软件包列表：** 在使用 `apt` 安装软件包之前，最好先更新本地的软件包列表，以确保获取最新的信息。可以使用以下命令：

   ```bash
   sudo apt update
   ```

2. **安装软件包：** 一旦更新了软件包列表，你可以使用 `apt install` 命令来安装软件包。例如，安装 `curl` 工具：

   ```bash
   sudo apt install curl
   ```

   在这个例子中，`apt` 将下载并安装 `curl` 软件包及其依赖项。

3. **卸载软件包：** 如果你想卸载已经安装的软件包，可以使用 `apt remove` 命令。例如，卸载 `curl`：

   ```bash
   sudo apt remove curl
   ```

   这将卸载 `curl` 软件包，但保留其配置文件。

4. **清理不再需要的依赖项：** 有时，卸载软件包可能会留下一些不再需要的依赖项。你可以使用 `apt autoremove` 命令来清理这些不再需要的依赖项：

   ```bash
   sudo apt autoremove
   ```

   这将删除不再需要的软件包及其依赖项。

请注意，在使用 `apt` 命令时，需要以管理员身份运行，因此会使用 `sudo`。以上只是一些基本的 `apt` 使用例子，具体的命令和选项可能会根据你的系统和需求有所不同。



## wget

`wget` 是一个用于从网络上下载文件的命令行工具。以下是一个简单的使用 `wget` 的例子：

```bash
wget [URL]
```

其中 `[URL]` 是你要下载文件的网址。

例如，要从一个网址下载一个文件，可以执行如下命令：

```bash
wget http://www.minjieren.com/wordpress-3.1-zh_CN.zip
```

这将下载名为 `file.zip` 的文件，并保存在当前工作目录中。

一些常用的 `wget` 选项包括：

- **指定保存文件名：** 你可以使用 `-O` 选项来指定保存文件的名称。例如：

  ```bash
  wget -O mydownloadedfile.zip http://www.minjieren.com/wordpress-3.1-zh_CN.zip
  ```

  这会将文件保存为 `mydownloadedfile.zip`。

- **后台下载：** 你可以使用 `-b` 选项将下载放在后台运行。例如：

  ```bash
  wget -b http://www.minjieren.com/wordpress-3.1-zh_CN.zip
  ```

- **限速下载：** 通过 `-Q` 选项，你可以限制下载的速度。例如：

  ```bash
  wget --limit-rate=200k http://www.minjieren.com/wordpress-3.1-zh_CN.zip
  ```

这只是 `wget` 的一些基本用法，该工具有很多其他选项和功能，可以根据你的需要进行调整。要查看所有选项和详细信息，你可以查看 `wget` 的帮助文档：

```bash
man wget
```

这将显示 `wget` 的手册页，其中包含所有命令和选项的详细说明。

## shell命令

**列出目录内容：**

```bash
ls
```

列出当前目录下的文件和子目录。

![img](https://gitee.com/paulinghong/typora-chart-bed/raw/master/img/202311260058649.png)

1. 列出所有文件（包括隐藏文件）：ls -a
2. 列出文件的详细信息：ls -l 或 ll
3. 列出根目录（/）下的所有目录：ls /
4. 列出当前工作目录下所有名称是“s”开头的文件：ls -ltr s*
5. 列出/bin目录下的所有目录及文件的详细信息：ls -lR /bin
6. 列出当前工作目录下所有文件及目录以文件的大小进行排序：ls -AS



**pwd - 显示当前工作目录：**

```bash
pwd
```

显示当前所在的工作目录的路径。

**cd - 切换目录：**

```bash
cd /path/to/directory
```

进入指定路径的目录。

**cp - 复制文件或目录：**

```bash
cp source_file destination
```

复制文件或目录。

**mv - 移动或重命名文件或目录：**

```bash
mv old_name new_name
```

移动或重命名文件或目录。

**rm - 删除文件或目录：**

```bash
rm file_name
```

删除文件。请小心使用，删除的文件无法恢复。

**mkdir - 创建目录：**

```bash
mkdir new_directory
```

创建新目录。

**rmdir - 删除目录：**

```bash
rmdir empty_directory
```

删除空目录。

**cat - 查看文件内容：**

```bash
cat file_name
```

将文件内容输出到终端。

**echo - 输出文本：**

```bash
echo "Hello, World!"
```

输出文本到终端。

**grep - 在文件中搜索文本：**

```bash
grep "pattern" file_name
```

在文件中搜索包含指定模式的文本。

**chmod - 修改文件权限：**

```bash
chmod 755 file_name
```

修改文件的权限。这个例子将文件设置为所有者可读写执行，组用户和其他用户可读和执行。

在 Linux 中，权限管理规则主要是通过文件和目录的访问权限来控制的。Linux 使用一种基于用户、群组和其他用户的权限模型。每个文件或目录都有一个所有者（owner）、一个群组（group），以及其他用户的权限设置。

以下是关于 Linux 权限管理规则的一些重要概念：

1. **文件类型和权限：** 每个文件或目录的权限信息包含文件类型和访问权限。文件类型可以是常规文件（`-`）、目录（`d`）、符号链接（`l`）等。访问权限分为读（`r`）、写（`w`）和执行（`x`）权限。
2. **权限分组：** 权限信息按照用户类别分为三组：所有者、群组和其他用户。所有者是文件或目录的创建者，群组是文件或目录所属的用户组，其他用户是系统上的其他用户。
3. **权限符号表示：** 权限信息以符号形式表示，例如 `rw-r--r--`。其中，第一个组 `rw-` 表示所有者的权限，第二个组 `r--` 表示群组的权限，最后一个 `r--` 表示其他用户的权限。
4. **数字表示权限：** 权限也可以用数字表示，分别是读（4）、写（2）和执行（1）。通过将这些数字相加，你可以得到一个三位数的权限表示，例如 `644` 表示 `rw-r--r--`。
5. **修改权限：** 你可以使用 `chmod` 命令来修改文件或目录的权限。例如，`chmod 755 filename` 表示将文件的权限设置为 `rwxr-xr-x`。
6. **所有者和群组：** 文件或目录的所有者可以使用 `chown` 命令更改所有者，`chgrp` 命令更改群组。
7. **sudo 和 su：** 通过 `sudo` 命令，普通用户可以以超级用户的身份执行某些命令。`su` 命令允许用户切换到其他用户的身份。
8. **特殊权限：** 除了常规的读、写和执行权限外，还有一些特殊权限，如 Set User ID（SUID）、Set Group ID（SGID）和粘着位（Sticky Bit）。这些特殊权限对于一些特殊的文件或目录非常重要。

这些规则和概念形成了 Linux 中权限管理的基础，通过它们，系统管理员和文件所有者可以控制文件和目录的访问权限，以保护系统和用户的数据。

**ps - 显示当前运行的进程：**

```bash
ps aux
```

显示系统上所有用户的当前进程。

**kill - 终止进程：**

```bash
kill process_id
```

终止指定进程。

**df - 显示磁盘空间使用情况：**

```bash
df -h
```

显示磁盘空间使用情况，以人类可读的格式。

学习链接：https://www.cnblogs.com/xiao-wang-tong-xue/p/17306525.html#_lab2_1_0

### **$\star$作业1$\star$**

将该学习链接中所有的Linux常见命令全部敲一遍

要求：

1. 先创建一个以自己学号命名的文件夹，所有命令的截图都必须该文件夹目录下

   ![image-20231126010522111](https://gitee.com/paulinghong/typora-chart-bed/raw/master/img/202311260105857.png)

2. 将所有命令的所有参数都尝试一遍，如：第一个ls命令，将该表中的所有参数选项都尝试一遍，并截图

   ![](https://gitee.com/paulinghong/typora-chart-bed/raw/master/img/202311260106641.png)

3. 提交的作业文件只能markdown文档，提交word，pdf等格式的一律零分处理



## shell脚本

可以将shell脚本看做一门独立的语言(类似C语言和C++)

```bash
#!/bin/bash

echo "Hello World"
```

确保你的脚本文件有执行权限，可以使用以下命令：

```bash
chmod +x your_script.sh
```

然后执行脚本：

```bash
./your_script.sh
```

学习链接：https://www.runoob.com/linux/linux-shell.html

### **$\star$作业2$\star$**

使用shell脚本实现冒泡函数，需要排序的数据由时间戳生成。如：1700932976

<u>1</u> 70093297 <u>6</u>

<u>17</u>009329<u>76</u>

<u>170</u>0932<u>976</u>

<u>1700</u>93<u>2976</u>

<u>17009</u> <u>32976</

则需要排序的数据为1,6,17,76,170,976,1700,2976,17009,32976



要求：

1. 输出两项内容，一项为当前时间戳，一项为排序后的数据。请注意，我们会根据时间戳的数值来评分！在课上生成的时间戳为满分，之后生成的以天数递减。

   ![image-20231126013641437](https://gitee.com/paulinghong/typora-chart-bed/raw/master/img/202311260139208.png)

2. 截图必须在自己学号文件夹的目录下

