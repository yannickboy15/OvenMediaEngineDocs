# Troubleshooting

## `prerequisites.sh` Script Failed

If you have problems with the `prerequisites.sh` script we have provided, please install it manually as follows.

{% tabs %}
{% tab title="Ubuntu 18" %}
#### Install packages

```bash
$ sudo apt install build-essential nasm autoconf libtool zlib1g-dev libssl-dev libvpx-dev libopus-dev pkg-config libfdk-aac-dev tclsh cmake
```

#### libSRTP 2.2.0 [\[Download\]](https://github.com/cisco/libsrtp/archive/v2.2.0.tar.gz)

```bash
$ (curl -OL https://github.com/cisco/libsrtp/archive/v2.2.0.tar.gz && tar xvfz v2.2.0.tar.gz && cd libsrtp-2.2.0 && ./configure --enable-openssl && make && sudo make install)
```

#### OpenH264 1.8.0 [\[Download\]](http://ciscobinary.openh264.org/libopenh264-1.8.0-linux64.4.so.bz2)

```bash
$ (curl -OL http://ciscobinary.openh264.org/libopenh264-1.8.0-linux64.4.so.bz2 && bzip2 -d libopenh264-1.8.0-linux64.4.so.bz2 && sudo mv libopenh264-1.8.0-linux64.4.so /usr/lib && sudo ln -s /usr/lib/libopenh264-1.8.0-linux64.4.so /usr/lib/libopenh264.so && sudo ln -s /usr/lib/libopenh264-1.8.0-linux64.4.so /usr/lib/libopenh264.so.4)
```

#### SRT [\[Download\]](https://github.com/Haivision/srt/archive/v1.3.1.tar.gz)

```bash
$ (curl -OL https://github.com/Haivision/srt/archive/v1.3.1.tar.gz && tar xvf v1.3.1.tar.gz && cd srt-1.3.1 && ./configure && make && sudo make install)
```

#### FFmpeg 3.4.2 [\[Download\]](https://www.ffmpeg.org/releases/ffmpeg-3.4.2.tar.xz)

```bash
$ (curl -OL https://www.ffmpeg.org/releases/ffmpeg-3.4.2.tar.xz && xz -d ffmpeg-3.4.2.tar.xz && tar xvf ffmpeg-3.4.2.tar && cd ffmpeg-3.4.2 && ./configure \
--disable-static --enable-shared \
--extra-libs=-ldl \
--enable-ffprobe \
--disable-ffplay --disable-ffserver --disable-filters --disable-vaapi --disable-avdevice --disable-doc --disable-symver --disable-debug --disable-indevs --disable-outdevs --disable-devices --disable-hwaccels --disable-encoders \
--enable-zlib --enable-libopus --enable-libvpx --enable-libfdk_aac \
--enable-encoder=libvpx_vp8,libvpx_vp9,libopus,libfdk_aac \
--disable-decoder=tiff \
--enable-filter=asetnsamples,aresample,aformat,channelmap,channelsplit,scale,transpose,fps,settb,asettb && make && sudo make install && sudo ldconfig)
```
{% endtab %}

{% tab title="Fedora 28" %}
#### Install packages

```bash
$ sudo yum install gcc-c++ make nasm autoconf libtool zlib-devel openssl-devel libvpx-devel opus-devel tcl cmake
```

#### libSRTP 2.2.0 [\[Download\]](https://github.com/cisco/libsrtp/archive/v2.2.0.tar.gz)

```bash
$ (curl -OL https://github.com/cisco/libsrtp/archive/v2.2.0.tar.gz && tar xvfz v2.2.0.tar.gz && cd libsrtp-2.2.0 && ./configure --enable-openssl && make && sudo make install)
```

#### FDK-AAC [\[Download\]](https://github.com/mstorsjo/fdk-aac/archive/v0.1.5.tar.gz)

```bash
$ (curl -OL https://github.com/mstorsjo/fdk-aac/archive/v0.1.5.tar.gz && tar xvf v0.1.5.tar.gz && cd fdk-aac-0.1.5 && ./autogen.sh && ./configure && make && sudo make install)
```

#### FFmpeg 3.4.2 [\[Download\]](https://www.ffmpeg.org/releases/ffmpeg-3.4.2.tar.xz)

```bash
$ (curl -OL https://www.ffmpeg.org/releases/ffmpeg-3.4.2.tar.xz && xz -d ffmpeg-3.4.2.tar.xz && tar xvf ffmpeg-3.4.2.tar && cd ffmpeg-3.4.2 && ./configure \
--disable-static --enable-shared \
--extra-libs=-ldl \
--enable-ffprobe \
--disable-ffplay --disable-ffserver --disable-filters --disable-vaapi --disable-avdevice --disable-doc --disable-symver --disable-debug --disable-indevs --disable-outdevs --disable-devices --disable-hwaccels --disable-encoders \
--enable-zlib --enable-libopus --enable-libvpx --enable-libfdk_aac \
--enable-encoder=libvpx_vp8,libvpx_vp9,libopus,libfdk_aac \
--disable-decoder=tiff \
--enable-filter=asetnsamples,aresample,aformat,channelmap,channelsplit,scale,transpose,fps,settb,asettb && make && sudo make install)
```

#### OpenH264 1.8.0 [\[Download\]](http://ciscobinary.openh264.org/libopenh264-1.8.0-linux64.4.so.bz2)

```bash
$ (curl -OL http://ciscobinary.openh264.org/libopenh264-1.8.0-linux64.4.so.bz2 && bzip2 -d libopenh264-1.8.0-linux64.4.so.bz2 && sudo mv libopenh264-1.8.0-linux64.4.so /usr/lib && sudo ln -s /usr/lib/libopenh264-1.8.0-linux64.4.so /usr/lib/libopenh264.so && sudo ln -s /usr/lib/libopenh264-1.8.0-linux64.4.so /usr/lib/libopenh264.so.4)
```

#### SRT [\[Download\]](https://github.com/Haivision/srt/archive/v1.3.1.tar.gz)

```bash
$ (curl -OL https://github.com/Haivision/srt/archive/v1.3.1.tar.gz && tar xvf v1.3.1.tar.gz && cd srt-1.3.1 && ./configure && make && sudo make install)
```
{% endtab %}

{% tab title="CentOS 7" %}
#### Install packages

```bash
$ sudo yum install centos-release-scl
$ sudo yum install devtoolset-7 bc gcc-c++ nasm autoconf libtool glibc-static zlib-devel git bzip2 tcl cmake
```

#### OpenSSL 1.1.0g [\[Download\]](https://www.openssl.org/source/openssl-1.1.0g.tar.gz)

```bash
$ (curl -OL https://www.openssl.org/source/openssl-1.1.0g.tar.gz && tar xvfz openssl-1.1.0g.tar.gz && cd openssl-1.1.0g && ./config shared no-idea no-mdc2 no-rc5 no-ec2m no-ecdh no-ecdsa && make && sudo make install)
```

#### libvpx 1.7.0 [\[Download\]](https://chromium.googlesource.com/webm/libvpx/+archive/v1.7.0.tar.gz)

```bash
$ (curl -OL https://chromium.googlesource.com/webm/libvpx/+archive/v1.7.0.tar.gz && mkdir libvpx-1.7.0 && cd libvpx-1.7.0 && tar xvfz ../v1.7.0.tar.gz && ./configure --enable-shared --disable-static --disable-examples && make && sudo make install)
```

#### Opus 1.1.3 [\[Download\]](https://archive.mozilla.org/pub/opus/opus-1.1.3.tar.gz)

```bash
$ (curl -OL https://archive.mozilla.org/pub/opus/opus-1.1.3.tar.gz && tar xvfz opus-1.1.3.tar.gz && cd opus-1.1.3 && autoreconf -f -i && ./configure --enable-shared --disable-static && make && sudo make install)
```

#### libSRTP 2.2.0 [\[Download\]](https://github.com/cisco/libsrtp/archive/v2.2.0.tar.gz)

```bash
$ (curl -OL https://github.com/cisco/libsrtp/archive/v2.2.0.tar.gz && tar xvfz v2.2.0.tar.gz && cd libsrtp-2.2.0 && ./configure --enable-openssl && make && sudo make install)
```

#### FDK-AAC [\[Download\]](https://github.com/mstorsjo/fdk-aac/archive/v0.1.5.tar.gz)

```bash
$ (curl -OL https://github.com/mstorsjo/fdk-aac/archive/v0.1.5.tar.gz && tar xvf v0.1.5.tar.gz && cd fdk-aac-0.1.5 && ./autogen.sh && ./configure && make && sudo make install)
```

#### FFmpeg 3.4.2 [\[Download\]](https://www.ffmpeg.org/releases/ffmpeg-3.4.2.tar.xz)

```bash
$ (curl -OL https://www.ffmpeg.org/releases/ffmpeg-3.4.2.tar.xz && xz -d ffmpeg-3.4.2.tar.xz && tar xvf ffmpeg-3.4.2.tar && cd ffmpeg-3.4.2 && ./configure \
--disable-static --enable-shared \
--extra-libs=-ldl \
--enable-ffprobe \
--disable-ffplay --disable-ffserver --disable-filters --disable-vaapi --disable-avdevice --disable-doc --disable-symver --disable-debug --disable-indevs --disable-outdevs --disable-devices --disable-hwaccels --disable-encoders \
--enable-zlib --enable-libopus --enable-libvpx --enable-libfdk_aac \
--enable-encoder=libvpx_vp8,libvpx_vp9,libopus,libfdk_aac \
--disable-decoder=tiff \
--enable-filter=asetnsamples,aresample,aformat,channelmap,channelsplit,scale,transpose,fps,settb,asettb && make && sudo make install)
```

#### OpenH264 1.8.0 [\[Download\]](http://ciscobinary.openh264.org/libopenh264-1.8.0-linux64.4.so.bz2)

```bash
$ (curl -OL http://ciscobinary.openh264.org/libopenh264-1.8.0-linux64.4.so.bz2 && bzip2 -d libopenh264-1.8.0-linux64.4.so.bz2 && sudo mv libopenh264-1.8.0-linux64.4.so /usr/lib && sudo ln -s /usr/lib/libopenh264-1.8.0-linux64.4.so /usr/lib/libopenh264.so && sudo ln -s /usr/lib/libopenh264-1.8.0-linux64.4.so /usr/lib/libopenh264.so.4)
```

#### SRT [\[Download\]](https://github.com/Haivision/srt/archive/v1.3.1.tar.gz)

```bash
$ (curl -OL https://github.com/Haivision/srt/archive/v1.3.1.tar.gz && tar xvf v1.3.1.tar.gz && cd srt-1.3.1 && ./configure && make && sudo make install)
```
{% endtab %}
{% endtabs %}

## **`systemctl start ovenmediaengine` failed**

### **Check SELinux** 

If SELinux is running on your system, SELinux can deny execution of OvenMediaEngine. 

```bash
# Example of SELinux disallow OvenMediaEngine execution
$ systemctl start ovenmediaengine
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ====
Authentication is required to start 'ovenmediaengine.service'.
Authenticating as: Jeheon Han (getroot)
Password:
==== AUTHENTICATION COMPLETE ====
Failed to start ovenmediaengine.service: Unit ovenmediaengine.service not found.
# Check if SELinux is enabled
$ sestatus
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Memory protection checking:     actual (secure)
Max kernel policy version:      31
# Check if SELinux denies execution
$ sudo tail /var/log/messages
...
May 17 12:44:24 localhost audit[1]: AVC avc:  denied  { read } for  pid=1 comm="systemd" name="ovenmediaengine.service" dev="dm-0" ino=16836708 scontext=system_u:system_r:init_t:s0 tcontext=system_u:object_r:default_t:s0 tclass=file permissive=0
May 17 12:44:24 localhost audit[1]: AVC avc:  denied  { read } for  pid=1 comm="systemd" name="ovenmediaengine.service" dev="dm-0" ino=16836708 scontext=system_u:system_r:init_t:s0 tcontext=system_u:object_r:default_t:s0 tclass=file permissive=0

```

You can choose between two methods of adding a policy to SELinux or setting SELinux to permissive mode. To add a policy, you must apply the SELinux policy file for the OvenMediaEngine service to your system as follows:

```bash
$ cd <OvenMediaEngine Git Clone Root Path>
$ sudo semodule -i misc/ovenmediaengine.pp
$ sudo touch /.autorelabel
# If you add a policy to SELinux, you must reboot the system.
$ sudo reboot
```

Setting SELinux to permissive mode is as simple as follows. But we do not recommend this method.

```bash
$ sudo setenforce 0
```









