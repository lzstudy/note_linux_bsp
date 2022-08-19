i2c-tools
=========

介绍
----

i2c-tools可以通过shell控制i2c直接设备

资源链接
--------

===================== =============================================================
i2c-tools包           https://mirrors.edge.kernel.org/pub/software/utils/i2c-tools/
i2c-tools-4.2.tar.gz_ i2c-tools-4.2版本
===================== =============================================================

.. _i2c-tools-4.2.tar.gz: http://120.48.82.24:9100/note_linux_bsp/i2c-tools-4.2.tar.gz

.. warning::

   i2c-tools-4.2以前的版本不支持i2ctransfer工具

程序编译
--------

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
   make _install


工具使用
--------

- **i2cdetect - 检测工具**

`i2cdetect -l`

.. code:: shell

   buildroot:~# i2cdetect -l
   i2c-0   i2c             twi0                                    I2C adapter
   i2c-1   i2c             twi1                                    I2C adapter
   i2c-2   i2c             twi2                                    I2C adapter
   i2c-3   i2c             twi3                                    I2C adapter


i2cdetect -r -y 0

.. code:: bash

   -- --

- **i2ctransfer - 读写工具**

`读寄存器`

i2ctransfer -f -y <i2c adapter id> w2@<i2c addr> <reg_h> <reg_l> r<val>

.. code:: bash

   # 读取i2c-2, 设备地址0x1A, 地址0x0001, 3个值
   i2ctransfer -f -y 2 w2@0x1A 0x00 0x01 r3

`写寄存器`

i2ctransfer -f -y <i2c adapter id> w3@<i2c addr> <reg_h> <reg_l> <val>

.. code:: shell

   # 控制i2c-2, 设备地址0x1A, 寄存器地址0x0001, 写入0x03
   i2ctransfer-f -y 2 w3@0x1A 0x00 0x01 0x03

其他常用i2c调试命令
-------------------

.. code:: c

   # 查看所有的I2C设备
   ls /sys/bus/i2c/devices
