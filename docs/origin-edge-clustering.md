# Clustering

OvenMediaEngine supports Clustering and ensures High Availability \(HA\) and Scalability.

![](.gitbook/assets/image.png)

OvenMediaEngine supports the Origin-Edge structure for configuring Cluster and provides Scalability. Also, you can set Origin as `Primary` and `Secondary` in OvenMediaEngine for HA.

## Origin-Edge Configuration

Origin and Edge synchronize the `<Application>`. Moreover, Origin can connect with hundreds of Edge.

When delivering media data from Origin to Edge, we used the SRT protocol to reduce latency and improve reliability. For more information on the SRT Protocol, please visit the [SRT Alliance](https://www.srtalliance.org/) site.

### Origin Application

The role of the Origin Application is to transcode the inputted Live source and transmit it to the Edge. Origin works as `live` that sets in `<Type>` in `<Application>` just like Single Mode, and Edge connects to the port that sets in `<ListenPort>` element in `<Origin>`.

You can operate Origin with the following settings:

```markup
<?xml version="1.0" encoding="UTF-8"?>
<Server version="1">
   <Name>OvenMediaEngine</Name>
   <Hosts>
      <Host>
         <Name>default</Name>
         <IP>*</IP>
         <Ports>
            <Origin>9000</Origin>
            ...
         </Ports>
         <Applications>
            <Application>
               <Name>app</Name>
               <Type>live</Type>
               <Encodes>
                  ...
               </Encodes>
               <Streams>
                  ...
               </Streams>
               <Providers>
                  ...
               </Providers>
               <Publishers>
                  ...
               </Publishers>
            </Application>
         </Applications>
      </Host>
   </Hosts>
</Server>
```

### Edge Application

The role of the Edge is to receive and distribute transcoded streams from Origin. You can configure hundreds of Edge to distribute traffic to your players. As a result of testing, a single Edge can stream 4-5Gbps traffic based on AWS C5.2XLarge. If you need to stream more traffic, you can configure and use multiple Edge. If you set the   element to liveedge and Origin  and  to  , they will synchronize with the application with the same name as the Origin server.

{% code title="Server.xml" %}
```markup
<?xml version="1.0" encoding="UTF-8"?>

<Server version="1">
	<Name>OvenMediaEngine</Name>
	<Hosts>
		<Host>
			<Name>default</Name>
			<IP>*</IP>
			<Ports>
                ...
            </Ports>
			<Applications>
				<Application>
					<Name>app</Name>
					<Type>liveedge</Type>
					<Origin>
						<Primary>192.168.0.183:9000</Primary>
						<Secondary>192.168.0.184:9000</Secondary>
					</Origin>
					<Publishers>
						<ThreadCount>2</ThreadCount>
						<HLS/>
						<DASH/>
						<WebRTC/>
					</Publishers>
				</Application>
			</Applications>
		</Host>
	</Hosts>
</Server>
```
{% endcode %}

####  High-Availability

If you set `<Origin><Secondary>` element, OvenMediaEngine automatically switches to `<Secondary>` Origin when there is a problem with the `<Primary>` Origin. You need to configure `<Primary>` and `<Secondary>` Origin in `<Application>` on the same stream to use this function. 

## Load Balancer

When you are configuring Load Balancer, you need to use third-party solutions such as L4 Switch, LVS, or GSLB, but we recommend using DNS Round Robin. Also, services such as cloud-based [AWS Route53](https://aws.amazon.com/route53/?nc1=h_ls), [Azure DNS](https://azure.microsoft.com/en-us/services/dns/), or [Google Cloud DNS](https://cloud.google.com/dns/) can be a good alternative.

