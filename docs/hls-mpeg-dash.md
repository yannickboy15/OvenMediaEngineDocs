# HLS & MPEG-DASH Streaming

OvenMediaEngine uses HLS and MPEG-DASH to support streaming even on older browsers that do not support WebRTC. Also, if you use OvenPlayer, it detects the browser status and performance, and automatically falls back using HLS or MPEG-DASH during streaming. HLS and MPEG-DASH are not Ultra-Low Latency protocols, so you can decide whether or not to use them.

{% hint style="info" %}
We will soon support CMAF, which can stream with Low Latency even when using HLS or MPEG-DASH in OvenMediaEngine and OvenPlayer. CMAF will work reliably on most browsers except iOS, and it can Low Latency Streaming from 1 to 3 seconds.
{% endhint %}

OvenMediaEngine provides the following features:

| Title | Functions |
| :--- | :--- |
| Delivery | HTTP |
| Security | TLS \(HTTPS\) |
| Format | `TS`: MPEG-TS \(HLS\), `M4S`: ISOBMFF \(MPEG-DASH\) |
| Codec | H.264, AAC |

## Configuration

If you want to use MPEG-DASH and HLS, you need to add the `<DASH>` and `<HLS>` element to the `<Publishers>` in the `Server.xml` configuration file as shown in the following example.

{% code title="Server.xml" %}
```markup
<Host>
    <Name>default</Name>
    <IP>*</IP>
    <Ports>
        <HLS>80</HLS>
        <DASH>80</DASH>
    </Ports>
    <Applications>
        <Application>
            <Publishers>
                <DASH>
                    <SegmentDuration>5</SegmentDuration>
                    <SegmentCount>3</SegmentCount>
                    <CrossDomain>
                        <Url>*<Url>
                    </CrossDomain>
                </DASH>
                <HLS>
                    <SegmentDuration>5</SegmentDuration>
                    <SegmentCount>3</SegmentCount>
                    <CrossDomain>
                        <Url>https://airensoft.com<Url>
                        <Url>airensoft.com<Url>
                    </CrossDomain>
                </HLS>
            </Publishers>
```
{% endcode %}

| Element | Decsription |
| :--- | :--- |
| Ports | Sets the HTTP port to provide MPEG-DASH and HLS. Also, MPEG-DASH and HLS can use the same port. |
| SegmentDuration | Sets the length of the segment in seconds. The shorter the `<SegmentDuration>`, the lower latency can be streamed, but it is less stable during streaming. Therefore, we are recommended to set it to 3 to 5 seconds. |
| SegmentCount | Sets the number of segments to be exposed to `*.m3u8` and `*.mpd`. The smaller the number of `<SegmentCount>`, the lower latency can be streamed, but it is less reliable during streaming. Therefore, it is recommended to set this value to 3. |
| CrossDomain | Controls the domain in which the player works through `<CorssDomain>`. For more information, please refer to the [CrossDomain](hls-mpeg-dash.md#crossdomain) section. |

## CrossDomain

Most browsers and players prohibit accessing other domain resources in the currently running domain. You can control this situation through Cross-Origin Resource Sharing \(CORS\) or Cross-Domain \(CrossDomain\). Moreover, OvenMediaEngine can set CORS and Cross-Domain as `<CrossDomain>` element.

{% code title="Server.xml" %}
```markup
<CrossDomain>
    <Url>*</Url>
    <Url>*.airensoft.com</Url>
    <Url>http://*.ovenplayer.com</Url>
    <Url>https://demo.ovenplayer.com</Url>
</CrossDomain>
```
{% endcode %}

You can set it using the `<Url>` element as shown above, and you can use the following values:

| Url Value | Decsription |
| :--- | :--- |
| \* | Allows requests from all Domains |
| domain | Allows both HTTP and HTTPS requests from the specified Domain |
| http://domain | Allows HTTP requests from the specified Domain |
| https://domain | Allows HTTPS requests from the specified Domain |

## Streaming

MPEG-DASH and HLS Streaming are ready when a live source is inputted and a stream is created. Viewers can stream using OvenPlayer or other players.

Also, you need to set H.264 and AAC in the Transcoding profile because MPEG-DASH and HLS use these codecs.

```markup
<Encodes>
   <Encode>
      <Name>HD_H264_AAC</Name>
      <Audio>
         <Codec>aac</Codec>
         <Bitrate>128000</Bitrate>
         <Samplerate>48000</Samplerate>
         <Channel>2</Channel>
      </Audio>
      <Video>
         <Codec>h264</Codec>
         <Width>1280</Width>
         <Height>720</Height>
         <Bitrate>2000000</Bitrate>
         <Framerate>30.0</Framerate>
      </Video>
   </Encode>
</Encodes>
<Streams>
   <Stream>
      <Name>${OriginStreamName}_o</Name>
      <Profiles>
         <Profile>HD_H264_AAC</Profile>
      </Profiles>
   </Stream>
</Streams>
```

When you create a stream as shown above, you can play MPEG-DASH and HLS through OvenPlayer with the following URL:

| Protocol | URL format |
| :--- | :--- |
| HLS | `http://<Server IP>[:<HLS Port]/<Application Name>/<Stream Name>/playlist.m3u8` |
| Secure HLS | `https://<Server IP>[:<HLS Port]/<Application Name>/<Stream Name>/playlist.m3u8` |
| MPEG-DASH | `http://<Server IP>[:<DASH Port]/<Application Name>/<Stream Name>/manifest.mpd` |
| Secure MPEG-DASH | `https://<Server IP>[:<DASH Port]/<Application Name>/<Stream Name>/manifest.mpd` |

{% hint style="info" %}
If you set TLS in the current version of OvenMediaEngine, you can only use Secure HLS and Secure MPEG-DASH. Please wait if you want to use regular HLS and MPEG-DASH at the same time.
{% endhint %}

If you use the default configuration, you can start streaming with the following URL:

* `https://[OvenMediaEngine IP]:80/app/stream_o/playlist.m3u8`
* `http://[OvenMediaEngine IP]:80/app/stream_o/playlist.m3u8`
* `https://[OvenMediaEngine IP]:80/app/stream_o/manifest.mpd`
* `http://[OvenMediaEngine IP]:80/app/stream_o/manifest.mpd`

We have prepared a test player that you can quickly see if OvenMediaEngine is working. Please refer to the [Test Player](test-player.md) for more information.

