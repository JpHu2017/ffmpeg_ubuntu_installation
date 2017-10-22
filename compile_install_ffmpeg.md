# 安装用于华为平台取流的ffmpeg

## 1、安装x264

### libx264需要yasm，所以先安装yasm(已弃用，改安装nasm)

```sh
sudo apt-get install yasm
```

但是从2014年开始yasm不再更新维护了，所以最新版的libx264使用nasm，而不用yasm了，所以需要安装nasm

### 安装nasm

```sh
cd nasm-2.13
./configure
make -j
sudo make install
```

### 安装libx264-dev（可省略）

```sh
sudo apt-get install libx264-dev
```

### 编译安装libx264

```sh
cd x264-snapshot-20171012-2245
./configure --enable-shared --enable--pic
make -j
sudo make install
```



## 2、安装依赖包

```sh
sudo apt-get install build-essential git-core checkinstall texi2html libfaac-dev \
libopencore-amrnb-dev libopencore-amrwb-dev libsdl1.2-dev libmp3lame-dev libtheora-dev \
libvorbis-dev libx11-dev libxvidcore-dev libxext-dev libxfixes-dev zlib1g-dev libopus-dev libavdevice-dev 
```



## 3、编译安装ffmpeg

因为华为平台取流时，视频流的数据量比较大，采用ffmpeg源码默认的数据量大小会导致解码错误，所以需要将这个数据量限制改大，更改方式为：

```C++
// 找到 udp.c 文件，打开，找到
#define UDP_MAX_PKT_SIZE 65536
// 将这一行改为
#define UDP_MAX_PKT_SIZE 131072   // 65536
```

然后开始编译：

```sh
./configure --enable-shared --enable-gpl --enable-libx264
make -j
sudo make install
```