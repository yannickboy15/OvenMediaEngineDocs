# Configuration

OvenMediaEngine has an XML configuration file. If you start OvenMediaEngine by `systemctl start ovenmediaengine`, the configuration file is loaded from the following path.

```bash
/usr/share/ovenmediaengine/conf/Server.xml
```

If you run it directly from the command line, it loads the configuration file from the following location: 

```bash
/<OvenMediaEngine Binary Path>/conf/Server.xml
```

If you run it in docker container, the configuration file is in the following path:

```markup
# For Origin mode
/opt/ovenmediaengine/bin/origin_conf/Server.xml
# For Edge mode
/opt/ovenmediaengine/bin/edge_conf/Server.xml
```

## Server

`Server`is the root element of the configuration file. The `version`attribute indicates the version of the configuration file. OvenMediaEngine uses this version information to check if the configuration file is a compatible version.

{% code title="" %}
```markup
<?xml version="1.0" encoding="UTF-8"?>
<Server version="5">
    <Name>OvenMediaEngine</Name>
    <IP>*</IP>
    <Bind>...</Bind>
    <VirtualHosts>...</VirtualHosts>
</Server>
```
{% endcode %}

### IP

{% code title="" %}
```markup
    <IP>*</IP>
```
{% endcode %}

The IP address that the OvenMediaEngine will bind to. If you set \*, all IP addresses of the system are used. If you input a specific IP, the Host use only that IP.

## Bind

`Bind`is configuration for server port that will be used. Bind consists of Providers and Publishers. The Providers is server for stream input, the Publishers is server for streaming. 

```markup
<Bind>
	<Providers>
		<RTMP>
			<Port>1935</Port>
		</RTMP>
	</Providers>

	<Publishers>
		<OVT>
			<Port>9000</Port>
		</OVT>
		<HLS>
			<Port>80</Port>
		</HLS>
		<DASH>
			<Port>80</Port>
		</DASH>
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
```

The meanings of each element are shown in the following table.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Element</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">RTMP</td>
      <td style="text-align:left">RTMP Port for incoming RTMP stream</td>
    </tr>
    <tr>
      <td style="text-align:left">OVT</td>
      <td style="text-align:left">
        <p>OVT Port for origin server</p>
        <p>OVT is a protocol defined by OvenMediaEngine for Origin-Edge communication.
          For more information about Origin-Edge, see the <a href="origin-edge-clustering.md">Origin-Edge Clustering</a> chapter.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">HLS</td>
      <td style="text-align:left">HTTP(s) Port for HLS Streaming</td>
    </tr>
    <tr>
      <td style="text-align:left">DASH</td>
      <td style="text-align:left">HTTP(s) Port for MPEG-Dash Streaming including Low-Latency MPEG-Dash</td>
    </tr>
    <tr>
      <td style="text-align:left">WebRTC</td>
      <td style="text-align:left">Ports for WebRTC, For more information. If you want more information about
        WebRTC port, see <a href="webrtc-publishing.md">WebRTC Streaming</a> chapter.</td>
    </tr>
  </tbody>
</table>

{% hint style="info" %}
HLS and DASH can be set to the same port, but all other protocols have to use different ports. The ability for all protocols to use the same port will be updated in the future. 
{% endhint %}

## VirtualHosts

`VirtualHosts` is a way of operating more than one streaming server on a single machine. OvenMediaEngine supports IP based virtual host and Domain based virtual host. The "IP based" means that you can separate the streaming servers into multiples by setting different IP addresses and the "Domain based" means that even if the streaming servers use the same IP address, you can split the streaming servers into multiples by setting different domain names.

`VirtualHosts`consists of Domain, Origins and Applications.

```markup
<?xml version="1.0" encoding="UTF-8"?>
<Server version="5">
    <Name>OvenMediaEngine</Name>
    <VirtualHosts>
        <VirtualHost>
            <Name>default</Name>
            <Domain>
            ...
            </Domain>
            
            <Origins>
            ...
            </Origins>
            
            <Applications>
            ...
            </Applications>
        </Host>
    </Hosts>
</Server>
```

### Domain

The Domain has Names and TLS. The Names can be either domain address or IP address. Setting \* means it can accept all domains and IP addresses. 

```markup
<Domain>
		<Names>
			<!-- Domain names
			<Name>stream1.airensoft.com</Name>
			<Name>stream2.airensoft.com</Name>
			<Name>*.sub.airensoft.com</Name>
			-->
			<Name>*</Name>
		</Names>
</Domain>
```

### Origins

Origins \(sometimes called OriginMap\) is a feature to pull streams from external servers. The protocols for pulling now supports OVT and RTSP. The OVT is a protocol defined by OvenMediaEngine for Origin-Edge communication. It allows OvenMediaEngine to relay a stream from other OvenMediaEngines that have OVP Publisher turned on. Using RTSP, OvenMediaEngine pulls a stream from a RTSP server and create the stream. RTSP stream from external server can stream by WebRTC, HLS and MPEG-Dash. 

The Origin has Location and Pass elements. The Location is a URI pattern for incoming request. If the incoming URL request matches the Location, OvenMediaEngine will pull the stream according to the Pass element. In the pass element, you can set the protocol and Urls of the origin stream. 

To run edge server, Origin creates application and stream if there isn't those when user request. For more information about Origin-Edge, see the  [Live Source](live-source.md) chapter.

```markup
<Origins>
	<Origin>
		<Location>/app/stream</Location>
		<Pass>
			<Scheme>ovt</Scheme>
			<Urls><Url>origin.com:9000/app/stream_720p</Url></Urls>
		</Pass>
	</Origin>
	<Origin>
		<Location>/app/</Location>
		<Pass>
			<Scheme>ovt</Scheme>
			<Urls><Url>origin.com:9000/app/</Url></Urls>
		</Pass>
	</Origin>
	<Origin>
		<Location>/</Location>
		<Pass>
			<Scheme>ovt</Scheme>
			<Urls><Url>origin2.com:9000/</Url></Urls>
		</Pass>
	</Origin>
</Origins>
```

### Application

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
* `<Type>` defines the operation of the `<Application>`. Currently, there is only `live` type. 

#### Providers

`Providers` ingests streams that comes from media source. 

```markup
<Application>
   <Providers>
      <RTMP/>
   </Providers>
</Application>
```

If you want to get more information about the `<Providers>`, please refer to the [Live Source](live-source.md) chapter.

#### Encodes & Stream

`<Encode>` has profiles that encode the Input Streams coming in via `<Providers>`.  You can set up multiple profiles in `<Encode>`.

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

You can create a new Output Stream by grouping the encoded results in the `<Stream>` setting. 

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

#### Publishers

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
      <LLDASH>
         ...
      </LLDASH>
      <WebRTC>
         ...
      </WebRTC>
   </Publishers>
</Application>
```

​OvenMediaEngine currently supports WebRTC, HLS, MPEG-DASH and Low-Latency MPEG-DASH. If you do not want to use any protocol then you can delete that protocol setting, the component for that protocol is not initialized. As a result, you can save system resources by deleting settings for unused protocol components.

If you want to learn more about WebRTC, please visit the [WebRTC Streaming](webrtc-publishing.md) chapter. And if you want to get more information about HLS, MPEG-DASH, Low-Latency MPEG-DASH, please refer to the chapter on [HLS & MPEG-DASH Streaming](hls-mpeg-dash.md).

## Config Example

Finally, `Server.xml` will be configured as follows:

```markup
<?xml version="1.0" encoding="UTF-8" ?>

<Server version="5">
	<Name>OvenMediaEngine</Name>
	<!-- Host type (origin/edge) -->
	<Type>origin</Type>
	<!-- Specify IP address to bind (* means all IPs) -->
	<IP>*</IP>

	<!-- Settings for the ports to bind -->
	<Bind>
		<Providers>
			<RTMP>
				<Port>1935</Port>
			</RTMP>
		</Providers>

		<Publishers>
			<OVT>
				<Port>9000</Port>
			</OVT>
			<HLS>
				<Port>80</Port>
			</HLS>
			<DASH>
				<Port>80</Port>
			</DASH>
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

	<VirtualHosts>
		<!-- You can use wildcard like this to include multiple XMLs -->
		<VirtualHost include="VHost*.xml" />
		<VirtualHost>
			<Name>default</Name>

			<!-- Settings for multi domain and TLS -->
			<Domain>
				<Names>
					<!--
						Domain names
						<Name>stream1.airensoft.com</Name>
						<Name>stream2.airensoft.com</Name>
						<Name>*.sub.airensoft.com</Name>
					-->
					<Name>*</Name>
				</Names>
				<!--
				<TLS>
					<CertPath>path/to/file.crt</CertPath>
					<KeyPath>path/to/file.key</KeyPath>
					<ChainCertPath>path/to/file.crt</ChainCertPath>
				</TLS>
				-->
			</Domain>

			<Origins>
				<!--
				<Origin>
					<Location>/app/stream</Location>
					<Pass>
						<Scheme>ovt</Scheme>
						<Url>origin.com:9000/app/stream_720p</Url>
					</Pass>
				</Origin>
				<Origin>
					<Location>/app/</Location>
					<Pass>
						<Scheme>ovt</Scheme>
						<Url>origin.com:9000/app/</Url>
					</Pass>
				</Origin>
				-->
				<Origin>
					<Location>/edge/</Location>
					<Pass>
						<Scheme>ovt</Scheme>
						<Url>origin.com:9000/app/</Url>
					</Pass>
				</Origin>
			</Origins>

			<!-- Settings for applications -->
			<Applications>
				<Application>
					<Name>app</Name>
					<!-- Application type (live/vod) -->
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
								<Framerate>30</Framerate>
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
								<Framerate>30</Framerate>
							</Video>
						</Encode>
						<Encode>
							<Name>BYPASS</Name>
							<!-- If you want to use bypass. For browser compatibility, the h264.bframe option must be fixed to 0.-->
							<Video>
								<Bypass>true</Bypass>
							</Video>							
							<Audio>
								<Bypass>true</Bypass>
							</Audio>
						</Encode>						
					</Encodes>
					<Streams>
						<Stream>
							<Name>${OriginStreamName}_o</Name>
							<Profiles>
								<Profile>VP8</Profile>
								<Profile>BYPASS</Profile>
							</Profiles>
						</Stream>
					</Streams>
					<Providers>
						<RTMP />
					</Providers>
					<Publishers>
						<ThreadCount>4</ThreadCount>
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
						<LLDASH>
							<SegmentDuration>5</SegmentDuration>
							<CrossDomain>
								<Url>*</Url>
							</CrossDomain>
						</LLDASH>
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

