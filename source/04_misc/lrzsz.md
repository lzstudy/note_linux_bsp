# RZSZ

## 1、介绍

>> RZSZ可以通过串口进行收发数据



## 2、下载lrzsz包

>> [lrzsz-0.12.20](http://120.48.82.24:9100/note_linux_bsp/lrzsz-0.12.20.tar.gz)

## 3、使用命令解压
 
```bash
# 解压命令
tar -vxf lrrzsz-0.12.20.tar.gz

# 然后进入解压的文件夹
cd lrzsz-0.12.20

# 创建安装目录
mkdir _install

# 配置安装目录
CFLAGS=-O2 CC=arm-linux-gnueabi-gcc ./configure --prefix=/home/zw/airdoc/work/test/lrzsz-0.12.20/_install

# 编译和安装
make && make install
```


## 4、更新文件

- 编译完会在_install目录下生成bin目录, 将lrz和lsz重命令为rz和sz

- 将rz、sz推送到嵌入式板子/usr/bin路径下.

- lrb和lrx走的协议不一样，需要串口软件支持才能使用

## 5、使用方法

```
# rz 接收PC数据
rz

# sz 向PC发数据
sz <文件名>
```


