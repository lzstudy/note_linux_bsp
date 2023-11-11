nfs配置
===========

.. code-block:: c

    ################################### 1 安装软件包
    sudo apt-get install -y nfs-kernel-server rpcbind
    sudo apt-get install openssh-server

    ################################### 2 创建nfs目录
    mkdir /home/zw/linux/nfs
    chmod 777 /home/zw/linux/nfs

    ################################### 3 修改配置文件
    sudo vi /etc/exports
    ---------------------------

    /home/zw/linux/nfs *(rw,sync,no_root_squash)

    ################################### 4 重启nfs服务
    sudo /etc/init.d/nfs-kernel-server restart


