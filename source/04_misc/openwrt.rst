openwrt-luci库编译
==================

1 制作ubox.so
--------------

uci依赖ubox库, 因此需要先编译出此库

1.1 下载相关依赖
****************

.. code:: bash

   # 一下路径二选一
   git clone http://git.nbd.name/luci2/libubox.git
   git clone https://git.openwrt.org/project/libubox.git

1.2 编译ubox.so
***************

.. code:: bash

   cd libubox

   创建编译路径和安装路径
   mkdir build install
   cd build 

   # 编译
   cmake .. -DBUILD_LUA=off -DBUILD_EXAMPLES=off
   make 

2 制作uci.so
------------

2.1 下载源码
************

.. code:: bash

   git clone https://git.openwrt.org/project/uci.git

2.2 编译源码
************

.. code:: c

   cd uci
   
   # 创建编和安装路径
   mkdir _build _install

   # 编译源码
   cd _build
   cmake .. -DBUILD_LUA=off
   make

其他选项
--------


这个是格式化字符串, 我也不知道是否有效果。.
  如果换行了

http://www.pingtaimeng.com/article/detail/id/1567158



