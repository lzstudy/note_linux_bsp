openswan
===========

参考资料
https://blog.csdn.net/u011425939/article/details/80728648

1. overview
----------------

openswan移植需要两部分内容, libgmp和openswan

2. 移植libgmp
---------------

2.1 下载源码
***************

======== =================================
官网下载  https://gmplib.org/download/gmp/
本地下载  gmp-6.2.1.tar.bz2_
======== =================================

.. _gmp-6.2.1.tar.bz2: http://120.48.82.24:9100/note_linux_bsp/gmp-6.2.1.tar.bz2

2.2 配置源码
*************

.. code-block:: shell

    $ tar -vxf gmp-6.2.1.tar.xz && cd gmp-6.2.1
    $ ./configure --host=mips --with-pcap=linux --prefix=/home/zw/zdlz/work/ipsec/gmp-6.2.1/_install/ CC=mipsel-openwrt-linux-gcc CFLAGS="-g -O2 -Wall -fPIC" CXXFLAGS="-g -O2 -Wall -fPIC"

    # 配置完最后会打印如下内容
    configure: summary of build options:

    Version:           GNU MP 6.2.1
    Host type:         mips-unknown-elf
    ABI:               o32
    Install prefix:    /home/zw/zdlz/work/ipsec/gmp-6.2.1/_install
    Compiler:          mipsel-openwrt-linux-gcc
    Static libraries:  yes
    Shared libraries:  no


.. warning:: 
    
    这里的host、prefxi、CC根据自己的平台修改

2.3 编译
*************

.. code-block:: c

    make
    make install

2. 移植openswan
---------------

2.1 下载源码
***************

======== =================================================
官网下载  https://github.com/xelerance/Openswan/releases
本地下载  
======== =================================================

2.2 修改源码
*************

.. code-block:: c

    # 1 修改安装路径
    INC_USRLOCAL=/work/my/code/vpn/L2TP/openswan_client/install

    # 2 修改编译库
    LIBGMP =-L /openswan-2.6.50/lib -lgmp

2.3 编译源码
*************

.. code-block:: c

    # 1 拷贝之前编译号的libgmp到openswan/lib目录下
    cp libgmp.a openswan/lib
    cp gmp.h openswan/include

    # 2 编译
    make CC=mipsel-openwrt-linux-gcc programs

    # 3 安装
    make install

3 移植xl2tpd的依赖库libpcap-1.8.1
------------------------------------

3.1 下载源码
***************

======== ======================================================================
官网下载  https://www.linuxfromscratch.org/blfs/view/svn/basicnet/libpcap.html
本地下载  libpcap-1.10.1.tar.gz_
======== ======================================================================

.. _libpcap-1.10.1.tar.gz: http://120.48.82.24:9100/note_linux_bsp/libpcap-1.10.1.tar.gz


3.2 编译源码
***************

.. code-block:: c

    # 1 配置
    ./configure --host=mips --with-pcap=linux --prefix=$PWD/_install CC=mipsel-openwrt-linux-gcc

    # 2 编译
    make && make install

4 移植xl2tpd
-------------

4.1 下载源码
***************

======== ======================================================================
官网下载  
本地下载  xl2tpd-1.3.0.tar.gz
======== ======================================================================

.. _xl2tpd-1.3.0.tar.gz: http://120.48.82.24:9100/note_linux_bsp/xl2tpd-1.3.0.tar.gz

4.2 配置源码
***************

.. code-block:: c

    # 在pfc下添加如下内容, 另外bsd/signal.h可能会报错, 需要手动屏蔽掉

    pfc:
        $(CC) $(LDFLAGS) -o pfc -L /home/zw/zdlz/ipsec/libpcap-1.10.1/_install/lib pfc.o -lpcap $(LDLIBS)

4.3 编译源码
**************

    .. code-block:: c

        make CC=mipsel-openwrt-linux-gcc KERNELSRC=/home/zw/zdlz/ipsec/libpcap-1.10.1/_install LIBSRC=/home/zw/zdlz/ipsec/libpcap-1.10.1/_install/lib
    