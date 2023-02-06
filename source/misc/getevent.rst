getevent
==================

1 工具说明
--------------

``getevent`` 可以用于检测所有的 ``input`` 事件, 平时我们使用 ``hexdump /dev/input/eventX``
来一个一个调试, 使用此工具后直接使用 ``getevent`` 命令即可检测所有的事件.

2 移植说明
-----------

2.1 源码下载
**************

========= =======================================
官方下载  https://github.com/ndyer/getevent
本地下载  getevent-master.zip_
========= =======================================

.. _getevent-master.zip: http://120.48.82.24:9100/note_linux_bsp/getevent-master.zip

2.2 源码编译
**************

.. code-block:: c

   # 1 解压源码
   unzip getevent-master.zip

   # 2 进入目录
   cd getevent-master

   # 3 修改py脚本, 由于默认的input路径与我们不一致
   # 需要将40行的路径调整为我们嵌入式环境的路径
   vi generate-input.h-labels.py
   
   # 4 编译
   make

   # 5 将生成的源码拷贝到开发板上执行即可

3 效果展示
**************

.. code-block:: c

   $ ./getevent
   could not open /dev/input/by-id, Is a directory
   add device 1: /dev/input/event15
   name:     "USB 2.0 Camera: USB Camera"
   add device 2: /dev/input/touchscreen0
   name:     "goodix-ts"
   could not open /dev/input/by-path, Is a directory
   add device 3: /dev/input/event14
   name:     "lightswitch9"
   add device 4: /dev/input/event13
   name:     "lightswitch8"
   add device 5: /dev/input/event12
   name:     "lightswitch7"
   add device 6: /dev/input/event11
   name:     "lightswitch6"
   ...

   ##### 事件触发
   /dev/input/event10: 0001 0010 00000000
   /dev/input/event10: 0001 0010 00000001
   /dev/input/event10: 0000 0000 00000000
   /dev/input/event10: 0001 0010 00000000
   /dev/input/event10: 0001 0010 00000001
   /dev/input/event10: 0000 0000 00000000
   /dev/input/event10: 0001 0010 00000000
   /dev/input/event10: 0001 0010 00000001
   /dev/input/event10: 0000 0000 00000000

