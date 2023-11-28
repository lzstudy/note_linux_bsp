alsa 
==============

1 介绍
-------------

   alsa库用来方便控制音频设备

2 软件下载
----------------

=========================== ==========================================================
官网下载                     https://www.alsa-project.org/main/index.php/Main_Page
alsa-lib-1.2.2.tar.bz2_     alsa库
alsa-utils-1.2.2.tar.bz2_   alsa工具
=========================== ==========================================================

.. _alsa-lib-1.2.2.tar.bz2: http://120.48.82.24:9100/note_linux_bsp/01_hw/02_alsa/alsa-lib-1.2.2.tar.bz2
.. _alsa-utils-1.2.2.tar.bz2: http://120.48.82.24:9100/note_linux_bsp/01_hw/02_alsa/alsa-utils-1.2.2.tar.bz2

3 alsa-lib编译
---------------------

.. code-block:: c

    # 创建文件夹
    mkdir /usr/share/arm-alsa
    mkdir ${alsa-lib}/_install

    # 配置
    ./configure --host=arm-linux-gnueabihf --prefix=${alsa-lib}/_install --with-configdir=/usr/share/arm-alsa

    # 编译
    make

    # 安装需要root权限
    sudo -s
    make install


4 alsa-utils编译
----------------------

.. code-block:: c

    ./configure --host=arm-linux-gnueabihf --prefix=${alsa-utils}/_install \
    --with-alsa-inc-prefix=${alsa-lib}/include/ \
    --with-alsa-prefix=${alsa-lib}lib/ --disable-alsamixer --disable-xmlto
