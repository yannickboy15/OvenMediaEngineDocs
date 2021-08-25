# Introduction

## What is OvenMediaEngine?

OvenMediaEngine \(OME\) is an open-source and streaming server with sub-second latency. OME receives video via RTMP, MPEG-TS, and RSTP Pull from live encoders such as OBS, FFMPEG, and more. Then OME transmits video using WebRTC, Low-Latency HTTP \(DASH\), MPEG-DASH, and HLS. This enables sub-second latency streaming from OME which plays back seamlessly in your browser without requiring any plug-ins. Also, we provide [OvenPlayer](https://github.com/AirenSoft/OvenPlayer), the most optimized HTML5 player for OME, as an open-source.

Our goal is to make it easier for you to build a stable broadcasting/streaming service with sub second latency.

## Features

* Ingest
  * [WebRTC Push](live-source/webrtc-beta.md), [RTMP Push](live-source/rtmp.md), [SRT Push](live-source/srt-beta.md), [MPEG-2 TS Push](live-source/mpeg-2-ts-beta.md), [RTSP Pull](live-source/rtsp-pull-beta.md)
* [WebRTC sub-second streaming](streaming/webrtc-publishing.md)
  * WebRTC over TCP \(with embedded TURN server\)
  * Embedded WebRTC Signaling Server \(Web Socket based\)
  * ICE \(Interactive Connectivity Establishment\)
  * DTLS \(Datagram Transport Layer Security\)
  * SRTP \(Secure Real-time Transport Protocol\)
  * ULPFEC \(Forward Error Correction\) with VP8, H.264
  * In-band FEC \(Forward Error Correction\) with Opus
* [Low-Latency MPEG-DASH streaming](streaming/hls-mpeg-dash.md) \(Chunked CMAF\)
* [Legacy HLS/MPEG-DASH streaming](streaming/hls-mpeg-dash.md)
* [Embedded Live Transcoder](transcoding/) \(VP8, H.264, Opus, AAC, Bypass\)
* [Origin-Edge structure](origin-edge-clustering.md)
* Access Control
  * [Signed Policy](access-control/signedpolicy.md)
  * [Admission Webhooks](access-control/admission-webhooks.md)
* [Monitoring](logs-and-statistics.md)
* Beta
  * [File Recording](recording-experiment.md)
  * [RTMP Push Publishing\(re-streaming\)](push-publishing.md)
  * [Thumbnail](thumbnail-experiment.md)
  * [REST API](rest-api/)
* Experiment
  * [P2P Traffic Distribution](p2p-delivery.md)

## Supported Platforms

We have tested OvenMediaEngine on platforms, listed below. However, we think it can work with other Linux packages as well:

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

OvenMediaEngine is under the [GPLv2 license](https://github.com/AirenSoft/OvenMediaEngineDocs/tree/30ee3b30408d99632b4c2af88b070d9e38e201db/LICENSE/README.md).

