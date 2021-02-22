# Configuration

OvenMediaEngine has an XML configuration file. If you start OvenMediaEngine by `systemctl start ovenmediaengine`, the configuration file is loaded from the following path.

```bash
/usr/share/ovenmediaengine/conf/Server.xml
```

If you run it directly from the command line, it loads the configuration file from the following location: 

```bash
/<OvenMediaEngine Binary Path>/conf/Server.xml
```

## Server

`Server`is the root element of the configuration file. The `version`attribute indicates the version of the configuration file. OvenMediaEngine uses this version information to check if the configuration file is a compatible version.

{% code title="" %}
```markup
<?xml version="1.0" encoding="UTF-8"?>
<Server version="2">
    <Name>OvenMediaEngine</Name>
    <Hosts>
        ...
    </Hosts>
</Server>
```
{% endcode %}

## Host

`Host` is a group that manages IP, Ports, and Applications. In the future, we plan to expand to the ability to manage multiple virtual hosts.

{% code title="" %}
```markup
<?xml version="1.0" encoding="UTF-8"?>
<Server version="2">
    <Name>OvenMediaEngine</Name>
    <Hosts>
        <Host>
            <IP>*</IP>
            <Ports>
            ...
            </Ports>
            <Applications>
            ...
            </Applications>
        </Host>
    </Hosts>
</Server>
```
{% endcode %}

### IP

{% code title="" %}
```markup
<Host>
    <IP>*</IP>
```
{% endcode %}

The IP address that the OvenMediaEngine will bind to. If you set \*, all IP addresses of the system are used. If you input a specific IP, the Host use only that IP.

### Ports

The Port to be used by the Host can be set as follows. 

```markup
<Host>
    <Ports>
        <Origin>9000/udp</Origin>
        <RTMPProvider>1935/tcp</RTMPProvider>
        <HLS>80/tcp</HLS>
        <DASH>80/tcp</DASH>
        <WebRTC>
            <IceCandidates>
                <IceCandidate>*:10000-10005/udp</IceCandidate>
            </IceCandidates>
            <Signalling>3333/tcp</Signalling>
        </WebRTC>
    </Ports>
```

The meanings of each element are shown in the following table.

| Element | Description |
| :--- | :--- |
| Origin | SRT port of the Origin server for communication with the Edge server. For more information about Origin-Edge, see the  [Origin-Edge Clustering](origin-edge-clustering.md) chapter. |
| RTMPProvider | RTMP Port for incoming RTMP stream.  |
| HLS | HTTP\(s\) Port for HLS Streaming |
| DASH | HTTP\(s\) Port for MPEG-Dash Streaming |
| WebRTC | Ports for WebRTC, For more information. If you want more information about WebRTC port, see [WebRTC Streaming](webrtc-publishing.md) chapter. |

{% hint style="danger" %}
Currently, only HLS and MPEG-Dash can use the same port, and other ports must not be duplicated. We will update to use the same port all in 1.0 release.
{% endhint %}

## Application

`<Application>` consists of various elements that can define the operation of the stream including Stream input, Encoding, and Stream output. In other words, you can create as many `<Application>` as you want and build a variety of streaming environments.

```markup
<Host>
    ...
    <Applications>
        <Application>
            ...
        </Application>
        <Application>
            ...
        </Application>
    </Applications>
</Host>
```

`<Application>` needs to set `<Name>` and `<Type>` as follows:

```markup
<Application>
    <Name>app</Name>
    <Type>live</Type>
    <Provider> ... </Provider>
    <Encodes> ... </Encodes>
    <Streams> ... </Streams>
    <Publishers> ... </Publishers>
</Application>
```

* `<Name>` is used to configure the Streaming URL. 
* `<Type>` defines the operation of the `<Application>`. Currently, there are the following types:

| Application Type | Description |
| :--- | :--- |
| live | Application works in Single Mode or Origin Mode. |
| liveedge | Edge Application receives a stream from Origin and retransmits it. |

{% hint style="danger" %}
In this version, only one application can work in one host. Multiple applications is planned to be updated soon with version 0.91.
{% endhint %}

### Providers

`Providers` ingests streams that comes from media source. 

```markup
<Application>
   <Providers>
      <RTMP/>
   </Providers>
</Application>
```

If you want to get more information about the `<Providers>`, please refer to the [Live Source](live-source.md) chapter.

### Encodes & Stream

`<Encode>` has profiles that encode the Input Stream coming in via `<Providers>`. You can available this only if The `<Type>` in `<Application>` is "live". Also, you can set up multiple profiles in `<Encode>`.

```markup
<Application>
    <Encodes>
        <Encode>
            <Name>HD_VP8_OPUS</Name>
            <Video>
                <Codec>vp8</Codec>
                ...
            </Video>
            <Audio>
                <Codec>opus</Codec>
                ...
            </Audio>
        </Encode>
        <Encode>
            ...
        </Encode>
    </Encodes>
</Application>
```

You can create a new Output Stream by grouping the transcoded results in the `<Stream>` setting. 

* `<Name>` is the Stream Name when you use to configure the Streaming URL.
* `<Stream>` can be combined with multiple `<Profile>` and output as Multi Codec or Adaptive.
* `<Profile>` is `<Encode><Name>` and maps the Transcoding profile. 

```markup
<Application>
   <Streams>
      <Stream>
         <Name>${OriginStreamName}_o</Name>
         <Profiles>
            <Profile>HD_VP8_OPUS</Profile>
            <Profile>HD_H264_AAC</Profile>
         </Profiles>
      </Stream>
   </Streams>
</Application>
```

For more information about the Encodes and Streams, please see the [Transcoding](transcoding.md) chapter.

### Publishers

You can configure the Output Stream operation in `<Publishers>`. `<ThreadCount>` is the number of threads used by each component responsible for the `<Publishers>` protocols.

{% hint style="info" %}
You need many threads to transmit streams to a large number of users at the same time. Therefore, it is recommended to use a higher core CPU and set the `ThreadCount` equal to the number of CPU cores.
{% endhint %}

```markup
<Application>
   <Publishers>
      <ThreadCount>2</ThreadCount>
      <HLS>
         ...
      </HLS>
      <DASH>
         ...
      </DASH>
      <WebRTC>
         ...
      </WebRTC>
   </Publishers>
</Application>
```

​OvenMediaEngine currently supports WebRTC, HLS, and MPEG-DASH as the . If you do not want to use any protocol then you can delete that protocol setting, the component for that protocol is not initialized. As a result, you can save system resources by deleting settings for unused protocol components.

If you want to learn more about WebRTC, please visit the [WebRTC Streaming](webrtc-publishing.md) chapter. And if you want to get more information about HLS and MPEG-DASH, please refer to the chapter on [HLS & MPEG-DASH Streaming](hls-mpeg-dash.md).

## Config Example

Finally, `Server.xml` will be configured as follows:

```markup
<?xml version="1.0" encoding="UTF-8"?>
<Server version="2">
    <Name>OvenMediaEngine</Name>
    <Hosts>
        <Host>
            <Name>default</Name>
            <IP>*</IP>
            <Ports>
                <Origin>9000</Origin>
                <RTMPProvider>1935</RTMPProvider>
                <HLS>80</HLS>
                <DASH>80</DASH>
                <WebRTC>
                    <IceCandidates>
                        <IceCandidate>*:10000-10005/udp</IceCandidate>
                    </IceCandidates>
                    <Signalling>3333</Signalling>
                </WebRTC>
            </Ports>
            <Applications>
                <Application>
                    <Name>app</Name>
                    <Type>live</Type>
                    <Encodes>
                        <Encode>
                            <Name>VP8</Name>
                            <Audio>
                                <Codec>opus</Codec>
                                <Bitrate>128000</Bitrate>
                                <Samplerate>48000</Samplerate>
                                <Channel>2</Channel>
                            </Audio>
                            <Video>
                                <Codec>vp8</Codec>
                                <Width>640</Width>
                                <Height>360</Height>
                                <Bitrate>500000</Bitrate>
                                <Framerate>30.0</Framerate>
                            </Video>
                        </Encode>
                        <Encode>
                            <Name>H264</Name>
                            <Audio>
                                <Codec>aac</Codec>
                                <Bitrate>128000</Bitrate>
                                <Samplerate>48000</Samplerate>
                                <Channel>2</Channel>
                            </Audio>
                            <Video>
                                <Codec>h264</Codec>
                                <Width>640</Width>
                                <Height>360</Height>
                                <Bitrate>500000</Bitrate>
                                <Framerate>30.0</Framerate>
                            </Video>
                        </Encode>
                    </Encodes>
                    <Streams>
                        <Stream>
                            <Name>${OriginStreamName}_o</Name>
                            <Profiles>
                                <Profile>VP8</Profile>
                                <Profile>H264</Profile>
                            </Profiles>
                        </Stream>
                    </Streams>
                    <Providers>
                        <RTMP />
                    </Providers>
                    <Publishers>
                        <ThreadCount>2</ThreadCount>
                        <HLS>
                            <SegmentDuration>5</SegmentDuration>
                            <SegmentCount>3</SegmentCount>
                            <CrossDomain>
                                <Url>*</Url>
                            </CrossDomain>
                        </HLS>
                        <DASH>
                            <SegmentDuration>5</SegmentDuration>
                            <SegmentCount>3</SegmentCount>
                            <CrossDomain>
                                <Url>*</Url>
                            </CrossDomain>
                        </DASH>
                        <WebRTC>
                            <Timeout>30000</Timeout>
                        </WebRTC>
                    </Publishers>
                </Application>
            </Applications>
        </Host>
    </Hosts>
</Server>
```

 ****

