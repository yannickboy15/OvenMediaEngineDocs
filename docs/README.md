# Introduction

## What is OvenMediaEngine?

OvenMediaEngine \(OME\) is an open source, streaming server with sub-second latency. OME receives video via RTMP or other protocols from live encoders such as OBS, XSplit and transmits it on WebRTC and Low-Latency DASH. So, sub-second latency streaming from OME can work seamlessly in your browser without plug-ins. Also, OME provides [OvenPlayer](https://github.com/AirenSoft/OvenPlayer), the HTML5 standard web player.

Our goal is to make it easier for you to build a stable broadcasting/streaming service with Ultra-Low Latency.

## Features

* RTMP Input
* WebRTC sub-second streaming
  * Embedded WebRTC Signalling Server \(WebSocket based\)
  * ICE \(Interactive Connectivity Establishment\)
  * DTLS \(Datagram Transport Layer Security\)
  * SRTP \(Secure Real-time Transport Protocol\)
  * ULPFEC \(Forward Error Correction\) with VP8, H.264
  * In-band FEC \(Forward Error Correction\) with Opus
* Low-Latency MPEG-DASH streaming \(Chunked CMAF\)
* Legacy HLS/MPEG-DASH streaming
* Embedded Live Transcoder \(VP8, H.264, Opus, AAC, Bypass\)
* Origin-Edge structure
* Monitoring
* Experiment
  * P2P Traffic Distribution
  * RTSP Pull, MPEG-TS Push Input

## Supported Platforms

We have tested OME on the platforms listed below. However, we think it can work with other Linux packages as well:

* Docker \([https://hub.docker.com/r/airensoft/ovenmediaengine](https://hub.docker.com/r/airensoft/ovenmediaengine)\)
* Ubuntu 18
* CentOS 7
* Fedora 28

## Getting Started

Please read [Getting Started](getting-started.md) chapter in tutorials.

## For more information

* [OvenMediaEngine Github](https://github.com/AirenSoft/OvenMediaEngine)
* [OvenMediaEngine Website](https://ovenmediaengine.com) 
  * Basic Information, FAQ, and Benchmark
* [OvenMediaEngine Tutorials](https://airensoft.gitbook.io/ovenmediaengine/)
  * Getting Started, Install, and Configuration
* Test Player
  * `Without TLS` : [http://demo.ovenplayer.com](http://demo.ovenplayer.com)
  * `Based on TLS` : [https://demo.ovenplayer.com](https://demo.ovenplayer.com)
* [OvenPlayer Github](https://github.com/AirenSoft/OvenPlayer)
* [OvenPlayer Website](https://ovenplayer.com/index.html)
* [AirenSoft Website](https://www.airensoft.com/)

## How to Contribute

Please see our [Guidelines ](https://github.com/AirenSoft/OvenMediaEngine/blob/master/CONTRIBUTING.md)and [Rules](https://github.com/AirenSoft/OvenMediaEngine/blob/master/CODE_OF_CONDUCT.md).

## License

OvenMediaEngine is under the [GPLv2 license](LICENSE).

