# RTMP

## Configuration

`Providers` ingest streams that come from a media source. OvenMediaEngine supports RTMP protocol. You can set it in the configuration as follows:

```markup
<Server>
	...
	<Bind>
		<Providers>
			<RTMP>
			  <Port>1935</Port>
			</RTMP>
		</Providers>
	</Bind>
	...
	<VirtualHosts>
		<VirtualHost>
			<Application>
				<Providers>
					<RTMP>
			    	...
			    </RTMP>
			    ...
				</Providers>
			<Application>
		</VirtualHost>
	</VirtualHosts>
</Server>
```

When a live source inputs to `<Application>`, a stream is automatically created in the `<Application>`. The generated stream is passed to Encoder and Publisher.

## RTMP live stream

If you set up a live stream using RTMP-based encoder, you need to set the following in `Server.xml`:

```markup
<Application>
   <Providers>
      <RTMP>
         <BlockDuplicateStreamName>true</BlockDuplicateStreamName>
      </RTMP>
   </Providers>
<Application>
```

* `<BlockDuplicateStreamName>` is a policy for streams that are inputted as overlaps.

`<BlockDuplicateStreamName>` works with the following rules:

| Value | Description |
| :--- | :--- |
| true | `Default` rejects a new stream inputted as overlap, and it maintains the existing stream. |
| false | Accepts a new stream inputted as overlap, and it disconnects the existing stream. |

{% hint style="info" %}
Allowing the duplicated stream name feature can cause several problems. Player connections may be lost when a new stream is inputted. Most encoders can automatically reconnect when it is disconnected from the server. As a result, the two encoders can compete and disconnect from each other, causing serious problems with streaming.
{% endhint %}

## Publish

If you want to publish the source stream, you need to set the following in the Encoder:

* **`URL`**        `RTMP://<OvenMediaEngine IP>[:<RTMP Listen Port>]/<App Name]>` 
* **`Stream Key`** `Stream Name`                                                  

If you use the default configuration, `<RTMP><ListenPort>` is 1935, which is the default port for RTMP. So it can be omitted. Also, since the Application named `app` is created by default in the default configuration, you can enter the `app` in the `[App Name]`. You can define a Stream Key and use it in the Encoder, and the Streaming URL will change according to the Stream Key.

Moreover, some encoders may include a stream key in the URL, and if you use these encoders, you need to set it as follows:

* **`URL`** `RTMP://<OvenMediaEngine IP>[:<RTMP Listen Port>/<App Name>/<Stream Name>`

### Example using OBS

If you use the default configuration, set up OBS as follows:

![](../.gitbook/assets/image%20%282%29.png)

You can set the Stream Key that can be set to any name, you like, at any time.

### Streaming URL

If you use the default configuration, you can stream with the following streaming URLs when you start broadcasting to OBS:

* **`WebRTC`**   `ws://192.168.0.1:3333/app/stream`
* **`HLS`**      `http://192.168.0.1:8080/app/stream/playlist.m3u8`
* **`MPEG-DASH`**`http://192.168.0.1:8080/app/stream/manifest.mpd`



