minicom
=========

安装包
-------------

====================== ===========================
ncurses-6.0.tar.gz_    图形库依赖包
minicom-2.7.1.tar.gz_  串口工具
====================== ===========================

.. _ncurses-6.0.tar.gz: http://120.48.82.24:9100/note_linux_bsp/01_hw/01_minicom/ncurses-6.0.tar.gz
.. _minicom-2.7.1.tar.gz: http://120.48.82.24:9100/note_linux_bsp/01_hw/01_minicom/minicom-2.7.1.tar.gz

1 移植ncurse
-----------------

.. code-block:: shell

    # 配置软件包
    ./configure --prefix=/home/zw/tools/minicom/ncurses-6.0/_install \
    --host=arm-linux-gnueabihf --target=arm-linux-gnueabihf \
    --with-shared --without-profile --disable-stripping --without-progs --with-manpages --without-tests

    # 编译
    make 

    # 修改源码, 解决报错, 然后再次编译
    # 删除1621行的注释 /* generate */

    # 安装
    make install

    # 拷贝到文件系统
    cp lib/* nfs/usr/lib -rfa
    cp share/* nfs/usr/share/ -rfa
    cp include/* nfs/usr/include/ -rfa

2 移植minicom
-----------------

.. code-block:: shell

    # 配置软件包
    ./configure --prefix=/home/zw/tools/minicom/minicom-2.7.1/_install \
    --host=arm-linux-gnueabihf --target=arm-linux-gnueabihf \
    CPPFLAGS=-I/home/zw/tools/minicom/ncurses-6.0/include \
    LDFLAGS=-L/home/zw/tools/minicom/ncurses-6.0/lib \
    -enable-cfg-dir=/etc/minicom

    # 编译
    make && make install

    # 拷贝到文件系统
    cp bin/* /usr/bin -rfa

3 其他配置
-------------------

.. code-block:: shell

    # 1 如果运行时缺少其他库, 可以从sysroot中拷贝

    # 2 添加配置 /etc/profile
    LD_LIBRARY_PATH=/lib:/usr/lib:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH
    export TERM=vt100
    export TERMINFO=/usr/share/terminfo


4 开发板运行
-------------------

.. code-block:: shell

    # 设置基本参数
    minicom -s
       Serial port setup
          A - 设置串口设备 - ttymxc2
          E - 设置波特率   - 115200
          F - 设置硬件流控 - No
          G - 设置软件流控 - No

    # 配置回显
    ctrl + a , 再按z进入配置页面, 按下e配置回显

    # 此时按esc退出后, 输入就可以看到回显了

