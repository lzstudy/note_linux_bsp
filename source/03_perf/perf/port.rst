perf移植
==========

1. 基本介绍
-----------

由于perf功能强大好用, 已经集成到linux内核的tools路径下。移植过程分为两部分

- 编译perf
- 配置内核支持perf功能

2 编译perf
------------

perf工具依赖zlib, 所以有可能需要先编zlib库

2.1 编译perf
*************

.. code-block:: c

    # 进入perf所在目录
    cd tools/perf

    # 编译, 其中架构和交叉编译器根据自身情况选择
    make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- perf LDFLAGS+=--static NO_LIBELF=1 V=1 WERROR=0 NO_SLANG=1 NO_GTK2=1 NO_LIBAUDIT=1 NO_LIBNUMA=1 NO_LIBPERL=1 NO_STRLCPY=1

    # 编译完后会在当前目录下生成perf文件, 拷贝到开发板上运行即可

3 配置内核
------------



4 常见问题
------------

4.1 报找不到zlib的问题
***********************

.. code-block:: c

    # 1 交叉编译zlib包

    # 2 打开编译配置脚本
    cd tools/perf
    vi config/Makefile

    # 3 指定zlib所在库的路径
    CFLAGS += -L/xxx/lib/