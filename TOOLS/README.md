# 在Ubuntu（虚拟机）中配置6.828实验环境

> [官方配置教程](https://pdos.csail.mit.edu/6.828/2019/tools.html)

## 配置

因为我使用的是Ubuntu系统，所以直接跳到官方Other Linux distributions教程里，如果是其他系统的话可以去官方文档里查看配置方法，以下是配置教程：

1. 克隆 RISC-V GNU编译工具链：

   ```
   //进入/usr/local目录下，以下所有的操作都是在该目录下进行。
   $ cd /usr/local
   $ git clone --recursive https://github.com/riscv/riscv-gnu-toolchain
   ```

   这会持续很长一段时间，而且有些部分网速很慢，需要自己想办法。

2. 确保你安装了编译工具链的工具包：

   ```
   //执行以下代码，这部分安装很快
   $ sudo apt-get install autoconf automake autotools-dev curl libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev
   ```

3. 配置和构建工具链：

   ```
   //一步一步执行
   $ cd riscv-gnu-toolchain
   $ ./configure --prefix=/usr/local
   $ sudo make
   $ cd ..
   ```

4. 下载QEMU 4.1源代码：

   ```
   $ wget https://download.qemu.org/qemu-4.1.0.tar.xz
   $ tar xf qemu-4.1.0.tar.xz
   ```

5. 构建QWMU：

   ```
   $ cd qemu-4.1.0
   $ ./configure --disable-kvm --disable-werror --prefix=/usr/local --target-list="riscv64-softmmu"
   $ make
   $ sudo make install
   $ cd ..
   ```

## 测试

1. 你需要执行以下代码，查看riscv64和QEMU是否正确安装：

   ```
   $ riscv64-unknown-elf-gcc --version
   riscv64-unknown-elf-gcc (GCC) 9.2.0
   $ qemu-system-riscv64 --version
   QEMU emulator version 4.1.0
   ```

   如果你的输出和上面相同，表示你配置成功。

2. 你可以编译并运行xv6系统

   这一测试步骤在LAB 1中进行。
