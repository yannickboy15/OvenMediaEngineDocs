# WebRTC Streaming

OvenMediaEngine uses WebRTC to provide sub-second latency streaming. WebRTC uses RTP for media transmission and provides various extensions.

OvenMediaEngine provides the following features:

| Title | Functions |
| :--- | :--- |
| Delivery | RTP / RTCP |
| Security | DTLS,  SRTP |
| Connectivity | ICE |
| Error Correction | ULPFEC \(VP8, H.264\), In-band FEC \(Opus\) |
| Codec | VP8, H.264, Opus |
| Signalling | Self-Defined Signalling Protocol and Embedded Web Socket-Based Server |

## Configuration

If you want to use the WebRTC feature, you need to add `<WebRTC>` element to the `<Publishers>` and &lt;Ports&gt; in the `Server.xml` configuration file, as shown in the example below.

```markup
<Server version="7">
	<Bind>
		<Publishers>
			<WebRTC>
				<Signalling>
				  <Port>3333</Port>
				</Signalling>
				<IceCandidates>
					<IceCandidate>*:10000-10005/udp</IceCandidate>
				</IceCandidates>
			</WebRTC>
		</Publishers>
	</Bind>
</Server>
```

### ICE

WebRTC uses ICE for connections and specifically NAT traversal. The web browser or player exchanges the Ice Candidate with each other in the Signalling phase. Therefore, OvenMediaEngine provides an ICE for WebRTC connectivity.

If you set IceCandidate to `*: 10000-10005/udp`, as in the example above, OvenMediaEngine automatically gets IP from the server and generates `IceCandidate` using UDP ports from 10000 to 10005. If you want to use a specific IP as IceCandidate, specify a specific IP. You can also use only one 10000 UDP Port, not a range, by setting it to \*: 10000.

### Signalling

OvenMediaEngine has embedded a WebSocket-based signalling server and provides our defined signalling protocol. Also, OvenPlayer supports our signalling protocol. WebRTC requires signalling to exchange Offer SDP and Answer SDP, but this part isn't standardized. If you want to use SDP, you need to create your exchange protocol yourself. 

If you want to change the signaling port, change the value of  `<Ports><WebRTC><Signalling>`.

#### Signalling Protocol

The Signalling protocol is defined in a simple way:

![](../.gitbook/assets/image%20%283%29.png)

If you want to use a player other than OvenPlayer, you need to develop the signalling protocol as shown above and can integrate OvenMediaEngine.

## Streaming

### Publisher

Add `WebRTC` element to Publisher to provide streaming through WebRTC. 

```markup
<Server version="7">
	<VirtualHosts>
		<VirtualHost>
			<Applications>
				<Application>
					<Publishers>
					  <WebRTC>
							<Timeout>30000</Timeout>
						</WebRTC>
					</Publishers>
				</Application>
			</Applications>
		</VirtualHost>
	</VirtualHosts>
</Server>
```

### Encoding

WebRTC Streaming starts when a live source is inputted and a stream is created. Viewers can stream using OvenPlayer or players that have developed or applied the OvenMediaEngine Signalling protocol.

Also, the codecs supported by each browser are different, so you need to set the Transcoding profile according to the browser you want to support. For example, Safari for iOS supports H.264 but not VP8. If you want to support all browsers, please set up VP8, H.264, and Opus codecs in all transcoders.

WebRTC doesn't support AAC, so when trying to bypass transcoding RTMP input, audio must be encoded as opus. See the settings below.

```markup
<OutputProfiles>
	<OutputProfile>
		<Name>bypass_stream</Name>
		<OutputStreamName>${OriginStreamName}</OutputStreamName>
		<Encodes>
			<Audio>
				<Bypass>true</Bypass>
			</Audio>
			<Video>
				<Bypass>true</Bypass>
			</Video>
			<Video>
         <!-- vp8, h264 -->
         <Codec>vp8</Codec>
         <Width>1280</Width>
         <Height>720</Height>
         <Bitrate>2000000</Bitrate>
         <Framerate>30.0</Framerate>
      </Video>
			<Audio>
				<Codec>opus</Codec>
				<Bitrate>128000</Bitrate>
				<Samplerate>48000</Samplerate>
				<Channel>2</Channel>
			</Audio>
		</Encodes>
	</OutputProfile>
</OutputProfiles>
```

{% hint style="info" %}
Some browsers support both H.264 and VP8 to send Answer SDP to OvenMediaEngine, but sometimes H.264 can't be played. In this situation, if you write the VP8 above the H.264 code line in the Transcoding profile setting, you can increase the priority of the VP8.

Using this manner so that some browsers, support H.264 but can't be played, can stream smoothly using VP8. This means that you can solve most problems with this method.
{% endhint %}

### Playback

If you created a stream as shown in the table above, you can play WebRTC on OvenPlayer via the following URL:

| Protocol | URL format |
| :--- | :--- |
| WebRTC Signalling | ws://&lt;Server IP&gt;\[:&lt;Signalling Port\]/&lt;Application name&gt;/&lt;Stream name&gt; |
| Secure WebRTC Signalling | wss://&lt;Server IP&gt;\[:&lt;Signalling Port\]/&lt;Application name&gt;/&lt;Stream name&gt; |

If you use the default configuration, you can stream to the following URL:

* `ws://[OvenMediaEngine IP]:3333/app/stream`
* `wss://[OvenMediaEngine IP]:3333/app/stream`

We have prepared a test player to make it easy to check if OvenMediaEngine is working. Please see the [Test Player](../test-player.md) chapter for more information.

