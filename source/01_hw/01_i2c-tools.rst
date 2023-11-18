i2c-tools
=========

1 介绍
------

i2c-tools可以通过shell控制i2c直接设备

2 资源链接
----------

===================== =============================================================
i2c-tools包           https://mirrors.edge.kernel.org/pub/software/utils/i2c-tools/
i2c-tools-4.2.tar.gz_ i2c-tools-4.2版本
===================== =============================================================

.. _i2c-tools-4.2.tar.gz: http://120.48.82.24:9100/note_linux_bsp/i2c-tools-4.2.tar.gz

.. warning::

   i2c-tools-4.2以前的版本不支持i2ctransfer工具

3 程序编译
----------

.. code:: c


   # 解压
   tar -vxf i2c-tools-4.2.tar.gz

   # 进入目录, 并创建安装目录
   cd i2c-tools-4.2 && mkdir _install

   # 修改Makefile
   PREFIX ?= xxx/i2c-tools-4.2/_install
   CC = arm-linux-gnueabi-gcc

   # 编译(选择不使用动态库)
   make USE_STATIC_LIB=1

   # 安装
   make install


4 工具使用
----------

4.1 i2cdetect - 检测工具
************************

.. code:: shell

   # 查看有多少主控设备
   buildroot:~# i2cdetect -l
   i2c-0   i2c             twi0                                    I2C adapter
   i2c-1   i2c             twi1                                    I2C adapter
   i2c-2   i2c             twi2                                    I2C adapter
   i2c-3   i2c             twi3                                    I2C adapter

   # 查看i2c1控制器下有哪些设备
   i2cdetect -r -y 0

        0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
   00:          -- -- -- -- -- -- -- -- -- -- -- -- --
   10: -- -- -- -- -- -- -- -- -- -- UU -- -- -- -- --
   20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
   30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
   40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
   50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
   60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
   70: -- -- -- -- -- -- -- --

.. note::
   UU - 表示设备当前被占用, 上图UU的地址为0x1a

4.2 i2ctransfer - 读写工具
**************************

4.2.1 读寄存器
^^^^^^^^^^^^^^

i2ctransfer -f -y <i2c adapter id> w2@<i2c addr> <reg_h> <reg_l> r<val>

.. code:: bash

   # 读取i2c-2, 设备地址0x1A, 地址0x0001, 3个值
   i2ctransfer -f -y 2 w2@0x1A 0x00 0x01 r3

   # 读取i2c-1, 设备地址0x0E, 地址0x0A, 2个值
   i2ctransfer -f -y 1 w1@0x0E 0x0A r2

   # 读取寄存器 i2c-1 设备地址0x50, 寄存器0x01的1个值
   i2cget -y 1 0x50 0x01

4.2.2 写寄存器
^^^^^^^^^^^^^^

i2ctransfer -f -y <i2c adapter id> w3@<i2c addr> <reg_h> <reg_l> <val>

.. code:: shell

   # 控制i2c-2, 设备地址0x1A, 寄存器地址0x0001, 写入0x03
   i2ctransfer-f -y 2 w3@0x1A 0x00 0x01 0x03

   # 控制i2c-1, 设备地址0x0e, 寄存器地址0x0A
   i2ctransfer-f -y 1 w3@0x0E 0x0A 0x03

   # 控制i2c-1, 设备地址0x50, 寄存器0A, 写入0x21
   i2cset -y 1 0x50 0x0A 0x21

5 其他常用i2c调试命令
---------------------

.. code:: c

   # 查看所有的I2C设备(适配器 + 设备)
   ls /sys/bus/i2c/devices
   0-003c  1-001a  2-003c  3-003c  i2c-0   i2c-1   i2c-2   i2c-3
