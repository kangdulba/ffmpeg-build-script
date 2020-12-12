[![Build Status](https://travis-ci.org/markus-perl/ffmpeg-build-script.svg?branch=master)](https://travis-ci.org/markus-perl/ffmpeg-build-script)

![FFmpeg build script](https://raw.github.com/markus-perl/ffmpeg-build-script/master/ffmpeg-build-script.png)


build-ffmpeg
==========

The FFmpeg build script provides an easy way to build a static FFmpeg on **OSX** and **Linux** with **non-free codecs** included.


[![How-To build FFmpeg on MacOS](http://img.youtube.com/vi/Z9p3mM757cM/0.jpg)](http://www.youtube.com/watch?v=Z9p3mM757cM "How-To build FFmpeg on OSX")

*Youtube: How-To build and install FFmpeg on MacOS*

## Disclaimer
Use this script at your own risk. I maintain this script in my spare time. 
Please do not file bug reports for systems other than Debian 10 and macOS 10.15.x
because I don't have the resources and the time to maintain other systems.


## Supported Codecs
* `x264`: H.264 (MPEG-4 AVC)
* `x265`: H.265 Video Codec
* `aom`: AV1 Video Codec (Experimental and very slow!)
* `fdk_aac`: Fraunhofer FDK AAC Codec 
* `xvidcore`: MPEG-4 video coding standard
* `VP8/VP9/webm`: VP8 / VP9 Video Codec for the WebM video file format
* `mp3`: MPEG-1 or MPEG-2 Audio Layer III
* `ogg`: Free, open container format
* `vorbis`: Lossy audio compression format
* `theora`: Free lossy video compression format
* `opus`: Lossy audio coding format
* `nv-codec`: [NVIDIA's GPU accelerated video codecs](https://devblogs.nvidia.com/nvidia-ffmpeg-transcoding-guide/). Installation is triggered only if CUDA installation is detected, follow [these](#Cuda-installation) instructions for installation. Supported codecs in nvcodec:
    * Decoders
        * H264 `h264_cuvid`
        * H265 `hevc_cuvid`
        * Motion JPEG `mjpeg_cuvid`
        * MPEG1 video `mpeg1_cuvid`
        * MPEG2 video `mpeg2_cuvid`
        * MPEG4 part 2 video `mepg4_cuvid`
        * VC-1 `vc1_cuvid`
        * VP8 `vp8_cuvid`
        * VP9 `vp9_cuvid`
    * Encoders
        * H264 `nvenc nvenc_h264`
        * H265 `nvenc_hevc` 

## Continuos Integration
ffmpeg-build-script is rockstable. Every commit runs against Linux and MacOS with https://travis-ci.org just to make sure everything works as expected.

Requirements MacOS
------------

* XCode 10.x or greater

Requirements Linux
------------
* Debian >= Buster, Ubuntu => Focal Fossa, other Distributions might work too
* build-essentials installed:

```
# Debian and Ubuntu
sudo apt-get install build-essential curl g++
sudo apt-get -y install autoconf automake build-essential libass-dev libtool pkg-config texinfo zlib1g-dev libva-dev cmake mercurial libdrm-dev libvorbis-dev libogg-dev git libx11-dev libperl-dev libpciaccess-dev libpciaccess0 xorg-dev

# Fedora
sudo dnf install @development-tools
```

Installation
------------

### Quick install and run

Open your command line and run (needs curl to be installed):

```bash
bash <(curl -s "https://raw.githubusercontent.com/markus-perl/ffmpeg-build-script/master/web-install.sh?v1")
```
This command downloads the build script and automatically starts the build process.

### Run with Docker

```bash
git clone https://github.com/markus-perl/ffmpeg-build-script.git
cd ffmpeg-build-script
docker build --tag=ffmpeg .
docker run  ffmpeg -i http://files.coconut.co.s3.amazonaws.com/test.mp4 -f webm -c:v libvpx -c:a libvorbis - > /tmp/test.mp4
```

### Common installation

```bash
git clone https://github.com/markus-perl/ffmpeg-build-script.git
cd ffmpeg-build-script
./build-ffmpeg --help
```


### Cuda installation

CUDA is a parallel computing platform developed by NVIDIA.
To be able to compile ffmpeg with CUDA support, you first need a compatible NVIDIA GPU. 
- Ubuntu: To install the CUDA toolkit on Ubuntu, simply run "sudo apt install nvidia-cuda-toolkit"
- Other Linux distributions: Once you have the GPU and display driver installed, you can follow the 
[official instructions](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html) 
or [this blog](https://www.pugetsystems.com/labs/hpc/How-To-Install-CUDA-10-1-on-Ubuntu-19-04-1405/) 
to setup the CUDA toolkit.

Usage
------

```bash
./build-ffmpeg --help       Display usage information
./build-ffmpeg --build      Starts the build process
./build-ffmpeg --cleanup    Remove all working dirs
```

Contact
-------

* Github: [http://www.github.com/markus-perl/](https://github.com/markus-perl/ffmpeg-build-script)


Tested on
---------

* MacOS 10.15
* Debian 10
* Ubuntu 20.04

Example
-------

```
./build-ffmpeg --build

building yasm
=======================
Downloading http://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz
 ... Done
$ ./configure --prefix=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace
$ make -j 4
$ make install

building opencore
=======================
Downloading http://downloads.sourceforge.net/project/opencore-amr/opencore-amr/opencore-amr-0.1.3.tar.gz?r=http%3A%2F%2Fsourceforge.net%2Fprojects%2Fopencore-amr%2Ffiles%2Fopencore-amr%2F&ts=1442256558&use_mirror=netassist
 ... Done
$ ./configure --prefix=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace --disable-shared --enable-static
$ make -j 4
$ make install

building libvpx
=======================
Downloading https://github.com/webmproject/libvpx/archive/v1.5.0.tar.gz
 ... Done
sed: -i may not be used with stdin
$ ./configure --prefix=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace --disable-unit-tests --disable-shared
$ make -j 4
$ make install

building lame
=======================
Downloading http://kent.dl.sourceforge.net/project/lame/lame/3.99/lame-3.99.5.tar.gz
 ... Done
$ ./configure --prefix=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace --disable-shared --enable-static
$ make -j 4
$ make install

building xvidcore
=======================
Downloading http://downloads.xvid.org/downloads/xvidcore-1.3.3.tar.gz
 ... Done
$ ./configure --prefix=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace --disable-shared --enable-static
$ make -j 4
$ make install
$ rm /Volumes/Daten/dev/mac/ffmpeg-build-script/workspace/lib/libxvidcore.4.dylib

building x264
=======================
Downloading ftp://ftp.videolan.org/pub/x264/snapshots/last_x264.tar.bz2
 ... Done
$ ./configure --prefix=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace --disable-shared --enable-static
$ make -j 4
$ make install
$ make install-lib-static

building libogg
=======================
Downloading http://downloads.xiph.org/releases/ogg/libogg-1.3.2.tar.gz
 ... Done
$ ./configure --prefix=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace --disable-shared --enable-static
$ make -j 4
$ make install

building libvorbis
=======================
Downloading http://downloads.xiph.org/releases/vorbis/libvorbis-1.3.5.tar.gz
 ... Done
$ ./configure --prefix=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace --with-ogg-libraries=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace/lib --with-ogg-includes=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace/include/ --enable-static --disable-shared --disable-oggtest
$ make -j 4
$ make install

building libtheora
=======================
Downloading http://downloads.xiph.org/releases/theora/libtheora-1.1.1.tar.gz
 ... Done
$ ./configure --prefix=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace --with-ogg-libraries=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace/lib --with-ogg-includes=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace/include/ --with-vorbis-libraries=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace/lib --with-vorbis-includes=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace/include/ --enable-static --disable-shared --disable-oggtest --disable-vorbistest --disable-examples --disable-asm
$ make -j 4
$ make install

building pkg-config
=======================
Downloading http://pkgconfig.freedesktop.org/releases/pkg-config-0.29.1.tar.gz
 ... Done
$ ./configure --silent --prefix=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace --with-pc-path=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace/lib/pkgconfig --with-internal-glib
$ make -j 4
$ make install

building cmake
=======================
Downloading https://cmake.org/files/v3.5/cmake-3.5.0.tar.gz
 ... Done
$ ./configure --prefix=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace
$ make -j 4
$ make install

building vid_stab
=======================
Downloading https://codeload.github.com/georgmartius/vid.stab/legacy.tar.gz/release-0.98b
 ... Done
$ cmake -DCMAKE_INSTALL_PREFIX:PATH=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace .
$ make -s install

building x265
=======================
Downloading https://bitbucket.org/multicoreware/x265/downloads/x265_1.9.tar.gz
 ... Done
$ cmake -DCMAKE_INSTALL_PREFIX:PATH=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace -DENABLE_SHARED:bool=off .
$ make -j 4
$ make install

building fdk_aac
=======================
Downloading http://downloads.sourceforge.net/project/opencore-amr/fdk-aac/fdk-aac-0.1.4.tar.gz?r=https%3A%2F%2Fsourceforge.net%2Fprojects%2Fopencore-amr%2Ffiles%2Ffdk-aac%2F&ts=1457561564&use_mirror=kent
 ... Done
$ ./configure --prefix=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace --disable-shared --enable-static
$ make -j 4
$ make install

building ffmpeg
=======================
Downloading http://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2
 ... Done
$ ./configure --arch=64 --prefix=/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace --extra-cflags=-I/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace/include --extra-ldflags=-L/Volumes/Daten/dev/mac/ffmpeg-build-script/workspace/lib --extra-version=static --disable-debug --disable-shared --enable-static --extra-cflags=--static --disable-ffplay --disable-ffserver --disable-doc --enable-version3 --enable-libvpx --enable-libmp3lame --enable-libtheora --enable-libvorbis --enable-libx264 --enable-avfilter --enable-gpl --enable-libopencore_amrwb --enable-libopencore_amrnb --enable-nonfree --enable-filters --enable-libvidstab --enable-libx265 --enable-runtime-cpudetect --enable-libfdk-aac
$ make -j 4
$ make install

Building done. The binary can be found here: /Volumes/Daten/dev/mac/ffmpeg-build-script/workspace/bin/ffmpeg

Install the binary to your /usr/local/bin folder? [Y/n] y
Password:
```
    

Other Projects Of Mine
------------
- [Pushover CLI Client](https://github.com/markus-perl/pushover-cli)
- [Gender API](https://gender-api.com): [Genderize A Name](https://gender-api.com)
- [Gender API Client PHP](https://github.com/markus-perl/gender-api-client)
- [Gender API Client NPM](https://github.com/markus-perl/gender-api-client-npm)
- [Genderize Names](https://www.youtube.com/watch?v=2SLIAguaygo)
- [Genderize API](https://gender-api.io)
