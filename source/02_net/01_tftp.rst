tftp配置
=============

1 tftp配置
---------------

.. code-block:: c

    ############################# 1 安装软件包
    sudo apt-get install tftp-hpa tftpd-hpa
    sudo apt-get install xinetd

    ############################# 2 创建tftp目录
    mkdir /home/zw/linux/tftp
    chmod 777 /home/zw/linux/tftp

    ############################# 3 修改tftp文件
    sudo vi /etc/xinetd.d/tftp
    -----------------------------

    server tftp
    {
        socket_type = dgram
        protocol = udp
        wait = yes
        user = root
        server = /usr/sbin/in.tftpd
        server_args = -s /home/zw/linux/tftp
        disable = no
        per_source = 11
        cps = 100 2
        flags = IPv4
    }

    ############################# 4 启动tftpd
    sudo service tftpd-hpa start

    ############################# 5 修改tftpd-hpa文件
    sudo vi /etc/default/tftpd-hpa
    -------------------------------------

    # /etc/default/tftpd-hpa

    TFTP_USERNAME="tftp"
    TFTP_DIRECTORY="/home/zw/linux/tftp"
    TFTP_ADDRESS=":69" 
    TFTP_OPTIONS="-l -c -s"

    ############################# 6 重启tftp服务
    sudo service tftpd-hpa restart
