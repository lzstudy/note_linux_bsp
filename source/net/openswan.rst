openswan
===========

1. overview
----------------

1.1 参考资料
**************

============================= ==========================================================
ipesec + l2tpd移植            https://blog.csdn.net/u011425939/article/details/80728648
============================= ==========================================================

1.2 缩略语
***********

========= ============================= ===================================
缩写      全写                          描述
ipsec     internet protocol security    互联网安全协议
L2TP      layer two tunneling protocol  第二层隧道协议
========= ============================= ===================================

1.3 移植说明
*************

ipsec移植分为


.. warning:: 
    
    - 移植过程中注意编译路径的修改

2. 源码编译
------------

2.1 编译libgmp
******************

======== =================================
官网下载  https://gmplib.org/download/gmp/
本地下载  gmp-6.2.1.tar.bz2_
======== =================================

.. _gmp-6.2.1.tar.bz2: http://120.48.82.24:9100/note_linux_bsp/gmp-6.2.1.tar.bz2

.. code-block:: c

    ############################################# 1 进入源码, 并创建安装目录
    $ tar -vxf gmp-6.2.1.tar.xz && cd gmp-6.2.1
    $ mkdir _install

    ############################################# 2 配置源码
    $ ./configure --host=mips --with-pcap=linux --prefix=$PWD/_install/ CC=mipsel-openwrt-linux-gcc CFLAGS="-g -O2 -Wall -fPIC" CXXFLAGS="-g -O2 -Wall -fPIC"

    ############################################# 3 编译安装
    make && make install


2.2 编译openswan
-----------------

======== =================================================
官网下载  https://github.com/xelerance/Openswan/releases
本地下载  Openswan-2.6.50.tar.gz_
======== =================================================

.. _Openswan-2.6.50.tar.gz: http://120.48.82.24:9100/note_linux_bsp/Openswan-2.6.50.tar.gz

.. code-block:: c

    ############################################# 1 进入源码, 并创建安装目录
    $ tar -vxf openswan-2.6.50.tar.gz && cd penswan-2.6.50
    $ mkdir _install

    ############################################# 2 修改Makefile.inc
    $ vi Makefile.inc
    INC_USRLOCAL=/home/zw/zdlz/ipsec/openswan-2.6.50/_install
    LIBGMP =-L /home/zw/zdlz/ipsec/openswan-2.6.50/lib -lgmp

    ############################################# 3 修改parser.l, 添加GLOB宏定义
    $ vi openswan-2.6.50/lib/libipsecconf/parser.l
    #define GLOB_BRACE      (1 << 10)
    #define GLOB_NOMAGIC    (1 << 11)

    ############################################# 4 修改sysdep_linux.c, 添加sighandler_t定义
    $ vi openswan-2.6.50/programs/pluto/sysdep_linux.c
    typedef void (*__sighandler_t) (int);

    ############################################# 5 拷贝之前编译好的libgmp, 路径根据自己情况完善
    $ cp libgmp.a openswan/lib
    $ cp gmp.h openswan/include

    ############################################# 6 编译和安装, 因为会在/etc/目录下生成配置文件, 安装时要加sudo
    $ make CC=mipsel-openwrt-linux-gcc programs
    $ sudo make install


2.3 编译libpcap-1.8.1
------------------------------------

======== ======================================================================
官网下载  https://www.linuxfromscratch.org/blfs/view/svn/basicnet/libpcap.html
本地下载  libpcap-1.10.1.tar.gz_
======== ======================================================================

.. _libpcap-1.10.1.tar.gz: http://120.48.82.24:9100/note_linux_bsp/libpcap-1.10.1.tar.gz

.. code-block:: c

    ############################################# 1 进入源码, 并创建安装目录
    $ tar -vxf libpcap-1.10.1.tar.gz && cd libpcap-1.10.1
    $ mkdir _install

    ############################################# 2 配置源码
    ./configure --host=mips --with-pcap=linux --prefix=$PWD/_install CC=mipsel-openwrt-linux-gcc

    ############################################# 3 编译和安装
    make && make install

2.4 编译xl2tpd
----------------

======== ======================================================================
官网下载  
本地下载  xl2tpd-1.3.0.tar.gz_
======== ======================================================================

.. _xl2tpd-1.3.0.tar.gz: http://120.48.82.24:9100/note_linux_bsp/xl2tpd-1.3.0.tar.gz

.. code-block:: c

    ############################################# 1 进入源码, 并创建安装目录
    $ tar -vxf xl2tpd-1.3.0.tar.gz && cd xl2tpd-1.3.0
    $ mkdir _install

    ############################################# 2 链接libpcap, 修改121行pfc目标的生成规则
    $ vi Makefile
    pfc:
        $(CC) $(CFLAGS) -c contrib/pfc.c
        $(CC) $(LDFLAGS) -o pfc -L /home/zw/zdlz/ipsec/libpcap-1.10.1/_install/lib pfc.o -lpcap $(LDLIBS)

    ############################################# 3 编译源码, 编译后会在当前目录下生成xl2tpd, 无需安装
    make CC=mipsel-openwrt-linux-gcc KERNELSRC=/home/zw/zdlz/ipsec/libpcap-1.10.1/_install LIBSRC=/home/zw/zdlz/ipsec/libpcap-1.10.1/_install/lib



5 软件移植
-----------------

5.1 整理资源
****************

5.1 拷贝库和配置
*****************

5.2 添加配置文件
*****************

psec_conf.tar.gz_

.. _psec_conf.tar.gz: http://120.48.82.24:9100/note_linux_bsp/ipsec_conf.tar.gz
 
===================== =================================== ====================================
文件                  目录                                说明
ipsec.conf            /etc/ipsec.conf                     ipsec配置文件
ipsec.secrets         /etc/ipsec.secrets
xl2tpd.conf           /etc/xl2tpd/xl2tpd.conf
chap-secrets          /etc/ppp/chap-secrets
options.l2tpd.client  /etc/ppp/peers/options.l2tpd.client
===================== =================================== ====================================
