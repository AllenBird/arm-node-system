
*****

在ARM上跑个JavaScript是件多么酷的事情。

*****

## arm-node-system

### Step 1
准备开发环境如下：

```
root@ubuntu:/# python --version
Python 2.7.6
root@ubuntu:/# cat /proc/version
Linux version 3.13.0-24-generic (buildd@roseapple) (gcc version 4.8.2 (Ubuntu 4.8.2-19ubuntu1) ) #46-Ubuntu SMP Thu Apr 10 19:08:14 UTC 2014
root@ubuntu:/# node --version
v0.10.42
root@ubuntu:/# npm --version
1.4.29
root@ubuntu:/# node-gyp --version
v3.3.1
root@ubuntu:/# arm-none-linux-gnueabi-gcc --version
arm-none-linux-gnueabi-gcc (Sourcery CodeBench Lite 2011.09-70) 4.6.1
Copyright (C) 2011 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

### Step 2
用的版本是node-v0.10.42.tar.gz，首先用arm-gcc编译一发Nodejs。

```
export AR=arm-none-linux-gnueabi-ar
export CC=arm-none-linux-gnueabi-gcc
export CXX=arm-none-linux-gnueabi-g++
export LINK=arm-none-linux-gnueabi-g++
export CCFLAGS="-static -march=armv7-a -mtune=cortex-a5 -mfpu=vfpv4 -mfloat-abi=soft"
export CXXFLAGS="-static -march=armv7-a -mtune=cortex-a5 -mfpu=vfpv4 -mfloat-abi=soft"
export LDFLAGS="-static"

./configure --without-snapshot --dest-cpu=arm --dest-os=linux --with-arm-float-abi=soft
make
make install DESTDIR=~/arm-node/
```
然后到把 ~/arm-node 
tar cvf arm-node.tar arm-node
tftp 到arm上 /usr/local 下
tar xvf arm-node.tar
  
### Step 3
下面要进行插件编译
```
安装nan： npm install -save nan
拿node-addon-examples做测试
npm install
node-gyp --arch=arm rebuild

tar cvf  hello.tar build/

```

### Step 4
下面要进行插件编译
```
安装nan： npm install -save nan
拿node-addon-examples做测试
npm install
node-gyp --arch=arm rebuild

tar cvf  hello.tar build/

```
