# Configuration

OvenMediaEngine has an XML configuration file. If you start OvenMediaEngine with `systemctl start ovenmediaengine`, the config file is loaded from the following path.

```bash
/usr/share/ovenmediaengine/conf/Server.xml
```

If you run it directly from the command line, it loads the configuration file from:

```bash
/<OvenMediaEngine Binary Path>/conf/Server.xml
```

If you run it in Docker container, the path to the configuration file is:

```markup
# For Origin mode
/opt/ovenmediaengine/bin/origin_conf/Server.xml
# For Edge mode
/opt/ovenmediaengine/bin/edge_conf/Server.xml
```

## Server

The `Server` is the root element of the configuration file. The `version`attribute indicates the version of the configuration file. OvenMediaEngine uses this version information to check if the config file is a compatible version.

{% code title="" %}
```markup
<?xml version="1.0" encoding="UTF-8"?>
<Server version="7">
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

The `IP address` is OvenMediaEngine will bind to. If you set \*, all IP addresses of the system are used. If you enter a specific IP, the Host uses that IP only.

## Bind

The `Bind` is the configuration for the server port that will be used. Bind consists of `Providers` and `Publishers`. The Providers are the server for stream input, and the Publishers are the server for streaming. 

```markup
<Bind>
	<Providers>
		<RTMP>
			<Port>1935</Port>
		</RTMP>
		<MPEGTS>
			<Port>4000-4003,4004,4005/udp</Port>
		</MPEGTS>
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

The meaning of each element is shown in the following table:

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
      <td style="text-align:left">RTMP port for incoming RTMP stream.</td>
    </tr>
    <tr>
      <td style="text-align:left">MPEG-TS</td>
      <td style="text-align:left">MPEGTS ports for incoming MPEGTS/UDP stream.</td>
    </tr>
    <tr>
      <td style="text-align:left">OVT</td>
      <td style="text-align:left">
        <p>OVT port for an origin server.</p>
        <p>OVT is a protocol defined by OvenMediaEngine for Origin-Edge communication.
          For more information about Origin-Edge, see the <a href="origin-edge-clustering.md">Origin-Edge Clustering</a> chapter.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">HLS</td>
      <td style="text-align:left">HTTP(s) port for HLS streaming.</td>
    </tr>
    <tr>
      <td style="text-align:left">DASH</td>
      <td style="text-align:left">HTTP(s) port for MPEG-DASH streaming including Low-Latency MPEG-DASH.</td>
    </tr>
    <tr>
      <td style="text-align:left">WebRTC</td>
      <td style="text-align:left">Port for WebRTC. If you want more information on the WebRTC port, see
        the <a href="streaming/webrtc-publishing.md">WebRTC Streaming</a> chapter.</td>
    </tr>
  </tbody>
</table>

{% hint style="info" %}
HLS and DASH can be set to the same port, but all other protocols have to use different ports. The ability for all protocols to use the same port will be updated in the future. 
{% endhint %}

## Virtual Host

`VirtualHosts` are a way to run more than one streaming server on a single machine. OvenMediaEngine supports IP-based virtual host and Domain-based virtual host. "IP-based" means that you can separate streaming servers into multiples by setting different IP addresses, and "Domain-based" means that even if the streaming servers use the same IP address, you can split the streaming servers into multiples by setting different domain names.

`VirtualHosts`consist of `Domain`, `Origins`, and `Applications`.

```markup
<?xml version="1.0" encoding="UTF-8"?>
<Server version="7">
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

The Domain has `Names` and TLS. Names can be either a domain address or an IP address. Setting \* means it allows all domains and IP addresses. 

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

Origins \(also we called OriginMap\) are a feature to pull streams from external servers. It now supports OVT and RTSP for the pulling protocols. OVT is a protocol defined by OvenMediaEngine for Origin-Edge communication. It allows OvenMediaEngine to relay a stream from other OvenMediaEngines that have OVP Publisher turned on. Using RTSP, OvenMediaEngine pulls a stream from an RTSP server and creates a stream. RTSP stream from external servers can stream by WebRTC, HLS, and MPEG-DASH. 

The Origin has `Location` and `Pass` elements. Location is a URI pattern for incoming requests. If the incoming URL request matches Location, OvenMediaEngine pulls the stream according to a Pass element. In the Pass element, you can set the origin stream's protocol and URLs.

To run the Edge server, Origin creates application and stream if there isn't those when user request. For more learn about Origin-Edge, see the  [Live Source](live-source/) chapter.

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
		<Location>/rtsp/stream</Location>
		<Pass>
			<Scheme>rtsp</Scheme>
			<Urls><Url>rtsp-server.com:554/</Url></Urls>
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

`<Application>` consists of various elements that can define the operation of the stream, including Stream input, Encoding, and Stream output. In other words, you can create as many `<Application>` as you like and build various streaming environments.

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
* `<Type>` defines the operation of `<Application>`. Currently, there is only a `live` type. 

#### Providers

`Providers` ingest streams that come from a media source. 

```markup
<Application>
   <Providers>
      <RTMP/>
      <RTSPPull/>
      <OVT/>
      <MPEGTS>
         <StreamMap>
            ...
         </StreamMap>
      </MPEGTS>
   </Providers>
</Application>
```

If you want to get more information about the `<Providers>`, please refer to the [Live Source](live-source/) chapter.

#### Encodes & Stream

`<Encode>` has a profile that encodes the input stream coming through `<Providers>`.  You can set up multiple profiles in `<Encode>`.

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

* `<Name>` is the Stream Name when using to configure the Streaming URL.
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

You can configure the Output Stream operation in `<Publishers>`. `<ThreadCount>` is the number of threads used by each component responsible for the `<Publishers>` protocol.

{% hint style="info" %}
You need many threads to transmit streams to a large number of users at the same time. So it's better to use a higher core CPU and set `<ThreadCount>` equal to the number of CPU cores.
{% endhint %}

```markup
<Application>
   <Publishers>
      <ThreadCount>8</ThreadCount>
      <OVT />
      <HLS />
      <DASH />
      <LLDASH />
      <WebRTC />
   </Publishers>
</Application>
```

​OvenMediaEngine currently supports WebRTC, Low-Latency DASH, MEPG-DASH, and HLS. If you don't want to use any protocol then you can delete that protocol setting, the component for that protocol isn't initialized. As a result, you can save system resources by deleting the settings of unused protocol components.

If you want to learn more about WebRTC, visit the [WebRTC Streaming](streaming/webrtc-publishing.md) chapter. And if you want to get more information on Low-Latency DASH, MPEG-DASH, and HLS, refer to the chapter on [HLS & MPEG-DASH Streaming](streaming/hls-mpeg-dash.md).

## Configuration Example

Finally, `Server.xml` is configured as follows:

```markup
<?xml version="1.0" encoding="UTF-8" ?>

<Server version="7">
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
			<MPEGTS>
				<!--
					Listen on port 4000,4001,4004,4005
					This is just a demonstration to show that you can configure the port in several ways
				-->
				<Port>4000-4001,4004,4005/udp</Port>
			</MPEGTS>
		</Providers>

		<Publishers>
			<OVT>
				<Port>9000</Port>
			</OVT>
			<HLS>
				<Port>80</Port>
				<!-- If you want to use TLS, specify the TLS port -->
				<!-- <TLSPort>443</TLSPort> -->
			</HLS>
			<DASH>
				<Port>80</Port>
				<!-- If you want to use TLS, specify the TLS port -->
				<!-- <TLSPort>443</TLSPort> -->
			</DASH>
			<WebRTC>
				<Signalling>
					<Port>3333</Port>
					<!-- If you want to use TLS, specify the TLS port -->
					<!-- <TLSPort>3334</TLSPort> -->
				</Signalling>
				<IceCandidates>
					<IceCandidate>*:10000-10005/udp</IceCandidate>
				</IceCandidates>
			</WebRTC>
		</Publishers>
	</Bind>

	<!-- P2P works only in WebRTC -->
	<!--
	<P2P>
		<MaxClientPeersPerHostPeer>2</MaxClientPeersPerHostPeer>
	</P2P>
	-->

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
			<!--
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
					<Location>/edge/</Location>
					<Pass>
						<Scheme>ovt</Scheme>
						<Urls><Url>origin.com:9000/app/</Url></Urls>
					</Pass>
				</Origin>
			</Origins>
			-->
			<!-- Settings for applications -->
			<Applications>
				<Application>
					<Name>app</Name>
					<!-- Application type (live/vod) -->
					<Type>live</Type>
					<Encodes>
						<Encode>
							<Name>bypass</Name>
							<Audio>
								<Bypass>true</Bypass>
							</Audio>
							<Video>
								<Bypass>true</Bypass>
							</Video>
						</Encode>
						<Encode>
							<Name>opus</Name>
							<Audio>
								<Codec>opus</Codec>
								<Bitrate>128000</Bitrate>
								<Samplerate>48000</Samplerate>
								<Channel>2</Channel>
							</Audio>
						</Encode>
					</Encodes>
					<Streams>
						<Stream>
							<Name>${OriginStreamName}_o</Name>
							<Profiles>
								<Profile>bypass</Profile>
								<Profile>opus</Profile>
							</Profiles>
						</Stream>
					</Streams>
					<Providers>
						<OVT />
						<RTMP />
						<MPEGTS>
							<StreamMap>
								<!--
									Set the stream name of the client connected to the port to "stream_${Port}"
									
									For example, if a client connets to port 4000, OME creates a "stream_4000" stream
								-->
								<Stream>
									<Name>stream_${Port}</Name>
									<Port>4000-4001,4004</Port>
								</Stream>
								<Stream>
									<Name>stream_name_for_4005_port</Name>
									<Port>4005</Port>
								</Stream>
							</StreamMap>
						</MPEGTS>
						<RTSPPull />
					</Providers>
					<Publishers>
						<ThreadCount>4</ThreadCount>
						<OVT />
						<WebRTC>
							<Timeout>30000</Timeout>
						</WebRTC>
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
					</Publishers>
				</Application>
			</Applications>
		</VirtualHost>
	</VirtualHosts>
</Server>
```

