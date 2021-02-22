# TLS Encryption

Most browsers can not load resources via HTTP and WS \(WebSocket\) from HTTPS web pages secured with TLS. Therefore, if the player is on an HTTPS page, the player must request streaming through https and wss URLs secured with TLS. In this case, you must apply the TLS certificate to the OvenMediaEngine.

Add your certificate files to  as follows:

```markup
<Hosts>
	<Host>
		<IP>192.168.0.1</IP>
		<TLS>
			<ChainCertPath>/path/to/cert/filename.crt</ChainCertPath>
			<CertPath>/path/to/cert/filename.crt</CertPath>
			<KeyPath>/path/to/cert/filename.key</KeyPath>
		</TLS>
		<Applications>
		...
	</Host>
</Host>
```

If the certificate settings are completed correctly, WebRTC streaming can be played `wss://url` with HLS and DASH streaming `https://url`.

{% hint style="warning" %}
The current version of OvenMediaEngine does not allow the use of "Without TLS" and "With TLS" at the same time. This means that you can only use https and wss if you set up a TLS certificate.
{% endhint %}



