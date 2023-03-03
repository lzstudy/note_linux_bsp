wifi调试
==========

1 移植说明
-----------------

wifi调试需要两个软件包: 

- wireless-tool(iw) 扫描设备
- wpa_supplicant 配置连接wpa类路由器

.. note:: 
    
    iw支持驱动较多, 但只支持wep加密, 现代路由器几乎都是wap加密

.. tip:: wpa_supplicant

    开源项目, 被谷歌修改后加入android平台, 主要用于支持wpa/pwa2无线协议加密和认证, 
    本质上通过socket来控制wifi芯片. wpa_supplicant不支持所有驱动, 具体还要去官网
    查看支持驱动列表. 




2 移植wireless-tool
------------------------

3 移植wpa_supplicant
------------------------

4 配置连接wpa网络
------------------

- 假设要连接wifi账号: zwtest, 密码: 123456


4.1 生成conf文件
**********************

.. code-block:: c

    $ wpa_passphrase zwtest 123456 > /etc/wpa_supplicant.conf

    network={
            ssid="zwtest"
            #psk="123456"
            psk=95a34f64a0873deb9335ec55f8c3990c93b3dfe5089ea4af6d47c11e416b66ca
    }

4.2 设置配置文件
*******************

.. code-block:: c

    # 根据4.1 生成的配置文件做修改为如下内容
    vi /etc/wpa_supplicant.conf

    ctrl_interface=/var/run/wpa_supplicant
    ctrl_interface_group=0
    update_config=1

    network={
            ssid="zwtest"
            #psk="123456"
            psk=95a34f64a0873deb9335ec55f8c3990c93b3dfe5089ea4af6d47c11e416b66ca

            key_mgmt=WPA-PSK
            proto=WPA
            pairwise=TKIP
            group=TKIP
    }

4.3 使能配置
*******************

.. code-block:: c

    # 1 使能配置, 连接成功后才能上网
    $ wpa_supplicant -i wlan0 -B -Dwext -c /etc/wpa_supplicant.conf
    wlan: Send EAPOL pkt to 88:XX:XX:XX:e6:22
    mlan0:
    IPv6: ADDRCONF(NETDEV_CHANGE): mlan0: link becomes ready

    # 2 设置IP, 注意IP地址网段要和服务器一致
    $ ifconfig mlan0 10.100.1.21

    # 3 ping测试
    $ ping -I wlan0 10.100.1.0


5 wifi调试
-------------

5.1 查看控制器信息
**********************

.. code-block:: c

    $ iwconfig

    mlan0     IEEE 802.11-DS  ESSID:"Airdoc-RD" [54]  
            Mode:Managed  Frequency=2.462 GHz  Access Point: 88:2A:5E:6A:E6:22   
            Bit Rate:1 Mb/s   Tx-Power=24 dBm   
            Retry limit:9   RTS thr=2347 B   Fragment thr=2346 B   
            Encryption key:-****-****-*******-****-****-****-****-****-****   Security mode:open
            Power Management:off
            Link Quality=3/5  Signal level=-65 dBm  Noise level=-80 dBm
            Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:184340
            Tx excessive retries:127  Invalid misc:2212   Missed beacon:0


5.2 扫描wifi设备
*************************

.. code-block:: c

    $ iwlist mlan0 scan

    wlan: SCAN COMPLETED: scanned AP count=54
    mlan0     Scan completed :
          Cell 01 - Address: 88:2A:5E:6A:E8:92
                    ESSID:"Airdoc-RD"
                    Mode:Master
                    Frequency=2.412 GHz (Channel 1)
                    Quality:2/5  Signal level:-79 dBm  Noise level:-95 dBm
                    Encryption key:on
                    Bit Rates:1 Mb/s; 2 Mb/s; 5.5 Mb/s; 6 Mb/s; 9 Mb/s
                              11 Mb/s; 12 Mb/s; 18 Mb/s; 24 Mb/s; 36 Mb/s
                              48 Mb/s; 54 Mb/s
                    Extra:Beacon interval=100
                    IE: IEEE 802.11i/WPA2 Version 1
                        Group Cipher : TKIP
                        Pairwise Ciphers (2) : CCMP TKIP
                        Authentication Suites (1) : PSK
                    IE: WPA Version 1
                        Group Cipher : TKIP
                        Pairwise Ciphers (2) : CCMP TKIP
                        Authentication Suites (1) : PSK
                    IE: Unknown: DD180050F2020101800003A4000027A4000042435E0062322F00
                    Extra:band=bg
          Cell 02 - Address: 06:74:9C:30:FE:DE
                    ESSID:"CU-CYJG2" [2]
                    Mode:Master
                    Frequency=2.412 GHz (Channel 1)
                    Quality:3/5  Signal level:-62 dBm  Noise level:-95 dBm
                    Encryption key:on
                    Bit Rates:1 Mb/s; 2 Mb/s; 5.5 Mb/s; 6 Mb/s; 9 Mb/s
                              11 Mb/s; 12 Mb/s; 18 Mb/s; 24 Mb/s; 36 Mb/s
                              48 Mb/s; 54 Mb/s
                    Extra:Beacon interval=100
                    IE: IEEE 802.11i/WPA2 Version 1
                        Group Cipher : CCMP
                        Pairwise Ciphers (1) : CCMP
                        Authentication Suites (1) : PSK
                    IE: WPA Version 1
                        Group Cipher : CCMP
                        Pairwise Ciphers (1) : CCMP
                        Authentication Suites (1) : PSK
                    IE: Unknown: DD180050F2020101860003A4000027A4000042435E0062322F00
                    IE: Unknown: DD0900037F01010000FF7F


5.3 查看与设置速率
***********************

.. code-block:: c

    # 1 连接wifi
    $ wpa_supplicant -i wlan0 -B -Dwext -c /etc/wpa_supplicant.conf
    wlan0:
    IPv6: ADDRCONF(NETDEV_CHANGE): mlan0: link becomes ready

    # 2 获取速率
    $ iwlist mlan0 rate
    mlan0     12 available bit-rates :
            1 Mb/s
            2 Mb/s
            5.5 Mb/s
            6 Mb/s
            9 Mb/s
            11 Mb/s
            12 Mb/s
            18 Mb/s
            24 Mb/s
            36 Mb/s
            48 Mb/s
            54 Mb/s
            Current Bit Rate=2 Mb/s

    # 3 设置速率
    $ iwconfig mlan0 rate 54M

5.4 设置DHCP获取IP
***********************

.. code-block:: c

    udhcpc -i mlan0 -b



