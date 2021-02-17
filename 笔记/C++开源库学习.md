# 日志

## log4cpp

### 1.安装

```shell
$ tar -zxvf log4cpp-1.1.3.tar.gz
$ cd log4cpp
$ ./configure
$ make
$ make install
```

默认lib库路径： /usr/local/lib

默认头文件位置：/usr/local/include/log4cpp

### 2.使用

1. 编译使用log4cpp库的cpp文件时，要加上库文件，才能顺利的编译通过

例：

```shell
g++ log4test.cpp -llog4cpp -lptread
```

2. 运行时，如若提示缺少log4cpp库文件，表示找不到log4cpp的动态库，需要进行以下设置以root身份执行

   ``` shell
   $ sudo vim /etc/ld.so.conf
   /usr/local/lib
   $ sudo ldconfig 	//更新库文件的缓存信息
   ```

3. 



