# Getting Started

## Run with Docker

OvenMediaEngine provides docker image to [Docker Hub](https://hub.docker.com/r/airensoft/ovenmediaengine) in the **airensoft/ovenmediaengine** repository. After installing [Docker](https://www.docker.com/), you can simply run the following command:

```text
docker run -d \
-e OME_VIDEO_BITRATE=2000000 -e OME_VIDEO_WIDTH=1280 -e OME_VIDEO_HEIGHT=720 \
-p 1935:1935 -p 3333:3333 -p 8080:8080 -p 9000:9000/udp -p 10000-10005:10000-10005/udp \
airensoft/ovenmediaengine:latest
```

You can set the following environment variables.

### Ports

| Env | Default Value |
| :--- | :--- |
| OME\_ORIGIN\_PORT | 9000 |
| OME\_RTMP\_PROV\_PORT | 1935 |
| OME\_HLS\_PUB\_PORT | 8080 |
| OME\_DASH\_PUB\_PORT | 8080 |
| OME\_ICE\_CANDIDATES | \*:10000-10005/udp |
| OME\_SIGNALLING\_PORT | 3333 |

### Encoding Options

| Env | Default Value |
| :--- | :--- |
| OME\_AUDIO\_BITRATE | 128000 |
| OME\_AUDIO\_SAMPLERATE | 48000 |
| OME\_AUDIO\_CHANNEL | 2 |
| OME\_VIDEO\_WIDTH | 640 |
| OME\_VIDEO\_HEIGHT | 360 |
| OME\_VIDEO\_BITRATE | 500000 |
| OME\_VIDEO\_FRAMERATE | 30 |

## Manual Install and run

### Installing for dependencies

OvenMediaEngine works in concert with a variety of open source and library. First, install them on your clean Linux machine as described below. We think that OvenMediaEngine can support most Linux packages, but the tested platforms we use are Ubuntu 18+, Fedora 28+, and CentOS 7+.

```bash
(curl -LOJ https://github.com/AirenSoft/OvenMediaEngine/archive/v0.10.0.tar.gz && tar xvfz OvenMediaEngine-0.10.0.tar.gz)
OvenMediaEngine-0.10.0/misc/prerequisites.sh
```

{% hint style="info" %}
If the prerequisites.sh script fails, proceed with the [manual installation](troubleshooting.md#prerequisites-sh-script-failed).
{% endhint %}

### **Build & Run**

You can build OvenMediaEngine sources using the following command.

{% tabs %}
{% tab title="Ubuntu 18" %}
```bash
cd OvenMediaEngine-0.10.0/src
make release
sudo make install
systemctl start ovenmediaengine
# If you want automatically start on boot
systemctl enable ovenmediaengine.service 
```
{% endtab %}

{% tab title="Fedora 28" %}
```bash
cd OvenMediaEngine-0.10.0/src
make release
sudo make install
systemctl start ovenmediaengine
# If you want automatically start on boot
systemctl enable ovenmediaengine.service
```

In addition, we recommend that you permanently set environment variables as follows.

```bash
$ echo 'export PKG_CONFIG_PATH=${PKG_CONFIG_PATH}:/usr/local/lib/pkgconfig:/usr/local/lib64/pkgconfig' >> ~/.bashrc 
$ echo 'export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/lib:/usr/local/lib64' >> ~/.bashrc
```
{% endtab %}

{% tab title="CentOS 7" %}
```bash
source scl_source enable devtoolset-7
cd OvenMediaEngine-0.10.10/src
make release
sudo make install
systemctl start ovenmediaengine
# If you want automatically start on boot
systemctl enable ovenmediaengine.service
```

In addition, we recommend that you permanently set environment variables as follows.

```bash
$ echo 'source scl_source enable devtoolset-7' >> ~/.bashrc 
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
if `systemctl start ovenmediaengine` fails in Fedora, selinux may be the cause. See [Check SELinux section of Troubleshooing](troubleshooting.md#check-selinux).
{% endhint %}

## Ports used by default

The default configuration uses the following ports, so you need to open them in your firewall settings. 

| Port | Purpose |
| :--- | :--- |
| 1935/TCP | RTMP Input |
| 9000/UDP | Origin Listen |
| 80/TCP | HLS & MPEG-Dash Streaming |
| 3333/TCP | WebRTC Signalling |
| 10000 - 10005/UDP | WebRTC Ice candidate |

You can open the port of the firewall as shown in the following example.

```bash
$ sudo firewall-cmd --add-port=80/tcp
$ sudo firewall-cmd --add-port=1935/tcp
$ sudo firewall-cmd --add-port=3333/tcp
$ sudo firewall-cmd --add-port=9000/udp
$ sudo firewall-cmd --add-port=10000-10005/udp
```

## Hello Ultra-low Latency Streaming

### Start Stream

You can live streaming using live encoders such as [OBS](https://obsproject.com/), [XSplit](https://www.xsplit.com). Please set the RTMP URL as below:

`rtmp://<Server IP>[:<RTMP Port>]/<Application name>/<Stream name>`

The meanings of each item are as follows:

* `<Server IP>`: The IP address or domain of the OvenMediaEngine server.
* `<RTMP Port>`: You can use `<Port>` of `<Provider>` in the above `Server.xml` file. If you use the default  configuration, the RTMP default port \(1935\) is used. Also, If you set the default port, you can omit the port.
* `<Application name>`: This value corresponds to `<Name>` of `<Application>` in `conf/Server.xml`. If you use the default configuration, you can use the `app`.
* `<Stream name>`:  The name of the stream you defined.

After you enter the above RTMP URL into the encoder and start publishing, you will have an environment in which the player can view the live stream.

### **Example of using OBS Encoder**

The server address in OBS needs to use the `<Application name>` generated in `Server.xml`. If you use the default configuration, the `app` has already been created, and you can use it.

* Install OBS on your PC and run it.
* Click "File" in the top menu, then "Settings" \(or press "Settings" on the bottom-right\).
* Select the "Stream" tab and fill in the settings.

![If you press the &quot;Service&quot; and select to &quot;Custom&quot;, your screen will be the same as this image.](.gitbook/assets/image%20%288%29%20%281%29.png)

* Go to the "Output" tab.
* Set the following entries.

For lower latency we recommend you to use Hardware Encoder as follows. NVENC provides a Low-Latency preset, which encodes with very low latency. Setting the MAX B-bframe to 0 is also important for lower latency.

![](.gitbook/assets/image%20%2820%29.png)

If Hardware Encoder is not installed in your PC, it is recommended to set x264 as follows. We highly recommend setting bframes = 0 for lower latency. And set the threads= option to 8 or less. Chrome does not handle more than 10 Nal Units. Setting threads=8 is the best way to avoid this.

![](.gitbook/assets/image%20%2827%29.png)

We recommend checking the "Enable new networking code" and "Low latency mode" as follows.

![](.gitbook/assets/image%20%2824%29.png)

### Playback

We have prepared a test player so that you can easily check that the OvenMediaEngine is working well. For more information, see the[ Test Player](test-player.md) chapter.

Please note that the WebRTC Signalling URL is similar to the RTMP URL and consists of the following:

* `ws://<Server IP>:[<Signalling Port>/<Application name>/<Output Stream name>`
  * `<Server IP>`: The IP address or domain of the OvenMediaEngine server.
  * `<Signalling Port>`: You can use the value of `<Signalling><ListenPort>` in `Server.xml` above. If you use the default configuration, the WebRTC signalling default port \(3333\) is used. 
  * `<Application name>`: This value corresponds to `<Name>` of `<Application>` in `conf/Server.xml`. If you use the default configuration, you can use the `app`.
  * `<Output Stream name>`: You have to use an output stream name for streaming. If you are using the default configuration, an output stream named `<Stream Name>_o` is automatically generated when stream is input.

{% hint style="info" %}
 If you use the default configuration, and the RTMP publishing URL is `rtmp://192.168.0.1:1935/app/stream`, the WebRTC Signalling URL will be `ws://192.168.0.1:3333/app/stream_o.` 

In addition, 

HLS streaming URL : `http://192.168.0.1/app/stream_o/playlist.m3u8` 

MPEG-Dash streaming URL : `http://192.168.0.1/app/stream_o/manifest.mpd`
{% endhint %}

If you want to build OvenPlayer in your environment, see [OvenPlayer Quick Start](https://github.com/AirenSoft/OvenPlayer#quick-start).

