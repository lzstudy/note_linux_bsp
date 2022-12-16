curl
========

1. overview
------------

curl工具主要用于下载文件

参考资料
https://blog.csdn.net/cyd411/article/details/117950544
https://blog.csdn.net/u014421422/article/details/45533243


2. 软件移植
------------

2.1 下载源码
*************

======== =================================
官网下载  https://curl.se/download/
本地下载  curl-7.78.0.zip
======== =================================

2.2 配置源码
*************

.. code-block:: shell

    ./configure --prefix=$PWD/_install --host=arm CC=arm-xxcc CXX=arm-xxc++ --with-wolfssl
    make && make install

.. note:: 

    可选项 --enable-shared来支持动态库

3. 用户编程
--------------

- 如何获取下载目标文件大小
- 下载进度条
- 断点续传

.. warning:: 
    
    使用cmake时, 库的编译选项 ``set(libs curl zstd z pthread)``


3.1 参考代码
**************

.. code-block:: c

    static int ota_progress_event(void *clientp, double dltotal, double dlnow, double ultotal, double ulnow)
    {
        printf("%fk / %fk\n", dltotal/1024, dlnow/1024);
        return 0;
    }

    static size_t my_fwrite(void *buffer, size_t size, size_t nmemb, void *stream) 
    {
        return fwrite(buffer, size, nmemb, stream);
    }

    void ota_init(void)
    {
        FILE *fp;
        CURL *curl;

        fp = fopen("/home/zw/airdoc/airdoc_app_sdk/ota/_build/my_file.tar.gz", "w+");

        curl = curl_easy_init();
        curl_easy_setopt(curl, CURLOPT_URL, "http://120.48.82.24:9100/debug/ota/v0.0.1.tar.gz");
        curl_easy_setopt(curl, CURLOPT_NOPROGRESS, 0);
        curl_easy_setopt(curl, CURLOPT_PROGRESSFUNCTION, ota_progress_event);
        curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, &my_fwrite); 
        curl_easy_setopt(curl, CURLOPT_WRITEDATA, fp);
        curl_easy_setopt(curl, CURLOPT_USE_SSL, CURLUSESSL_ALL);

        /* 启动传输 */
        curl_easy_perform(curl);
    }