
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
v0.10.42 // 后来改成了v0.11.12
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
export CCFLAGS="-march=armv7-a -mtune=cortex-a5 -mfpu=vfpv4 -mfloat-abi=soft"
export CXXFLAGS="-march=armv7-a -mtune=cortex-a5 -mfpu=vfpv4 -mfloat-abi=soft"

./configure --without-snapshot --dest-cpu=arm --dest-os=linux --with-arm-float-abi=soft
make
make install DESTDIR=./arm-node/
```
然后到把 ~/arm-node 
tar cvf arm-node.tar arm-node
tftp 到arm上 /usr/local 下
tar xvf arm-node.tar
切记将压缩包里的bin等文件放在linux的/usr/local 下（压缩包里自己就有/usr/local 这两层目录就不要了，不然npm会有问题）
### Step 3
下面要进行插件编译
```
安装nan： npm install -save nan
拿node-addon-examples做测试
npm install
node-gyp --arch=arm configure
node-gyp --arch=arm rebuild
```

### Step 4

```
  需要在arm的home路径下mkdir root，这样npm才能工作。
  如果node有ssl功能，npm的时候可以把npm关掉
  npm config set strict-ssl false
  否则会失败。
```
