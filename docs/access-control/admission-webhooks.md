# Admission Webhooks

## Overview

AdmissionWebhooks are HTTP callbacks that query the server for admission for publishing and playback.

![](../.gitbook/assets/image%20%2832%29.png)

Users can use AdmissionWebhooks to control publishing and playback, as well as record when publishing and playback, or hide external exposure of app/stream names.

## Configuration

AdmissionWebhooks can be set up on VirtualHost, as shown below.

```markup
<VirtualHost>
	<AdmissionWebhooks>
		<ControlServerUrl>https://192.168.0.161:9595/v1/admission</ControlServerUrl>
		<SecretKey>1234</SecretKey>
		<Timeout>3000</Timeout>
		<Enables>
			<Providers>rtmp,webrtc,srt</Providers>
			<Publishers>webrtc,hls,dash,lldash</Publishers>
		</Enables>
	</AdmissionWebhooks>
</VirtualHost>
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">Key</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">ControlServerUrl</td>
      <td style="text-align:left">The HTTP Server to receive the query. HTTP and HTTPS are available.</td>
    </tr>
    <tr>
      <td style="text-align:left">SecretKey</td>
      <td style="text-align:left">
        <p>The secret key used when encrypting with HMAC-SHA1</p>
        <p>For more information, see <a href="admission-webhooks.md#security">Security</a>.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Timeout</td>
      <td style="text-align:left">Time to wait for a response after request (in milliseconds)</td>
    </tr>
    <tr>
      <td style="text-align:left">Enables</td>
      <td style="text-align:left">Enable Providers and Publishers to use AdmissionWebhooks</td>
    </tr>
  </tbody>
</table>

## Request

### Format

AdmissionWebhooks send HTTP/1.1 request message to the configured user's control server when an encoder requests publishing or a player requests playback. The request message format is as follows.

```http
POST /configured/tartget/url/ HTTP/1.1
Content-Length: 325
Content-Type: application/json
Accept: application/json
X-OME-Signature: f871jd991jj1929jsjd91pqa0amm1
{
  "client": 
  {
    "address": "211.233.58.86",
    "port": 29291
  },
  "request":
  {
    "direction": "incoming | outgoing",
    "protocol": "webrtc | rtmp | srt | hls | dash | lldash",
    "url": "scheme://host[:port]/app/stream/file?query=value&query2=value2",
    "time": ""2021-05-12T13:45:00.000Z"
  }
}
```

The message is sent by POST method and the payload is in application/json format. X-OME-Signature is a value obtained by encrypting payload with HMAC-SHA1 so that ControlServer can verify the validity of this message. For more information on X-OME-Signature, see the [Security ](admission-webhooks.md#security)section.

Here is a detailed explanation of each element of Json payload:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Element</th>
      <th style="text-align:left">Sub-Element</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">client</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Information of the client who requested the connection.</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">address</td>
      <td style="text-align:left">Client&apos;s IP address</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">port</td>
      <td style="text-align:left">Client&apos;s Port number</td>
    </tr>
    <tr>
      <td style="text-align:left">request</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Information about the client&apos;s request</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">direction</td>
      <td style="text-align:left">
        <p>incoming : A client requests to publish a stream</p>
        <p>outgoing : A client requests to play a stream</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">protocol</td>
      <td style="text-align:left">webrtc, srt, rtmp, hls, dash, lldash</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">url</td>
      <td style="text-align:left">url requested by the client</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left">time</td>
      <td style="text-align:left">time requested by the client (ISO8601 format)</td>
    </tr>
  </tbody>
</table>

### Security

The Control Server may need to validate incoming http requests for security purposes. To do this, the AdmissionWebhooks module puts the value `X-OME-Signature` in the HTTP request header. X-OME-Signature is a value encrypted by using HMAC-SHA1 and the secret key set in `<AdmissionWebhooks><SecretKey>` of configuration.

### Conditions that triggers the request

As shown below, the trigger condition of request is different for each protocol.

| Protocol  | Condition |
| :--- | :--- |
| WebRTC | When a client requests Offer SDP |
| RTMP | When a client sends a publish message |
| SRT | When a client send a [streamid](https://airensoft.gitbook.io/ovenmediaengine/live-source/srt-beta#encoders-and-streamid) |
| HLS | Every time a client requests a playlist |
| DASH | Every time a client requests a playlist |
| LL-DASH | Every time a client requests a playlist |

## Response 

### Format

ControlServer must respond with the following Json format. In particular, the `"allowed"` element is required.

```http
HTTP/1.1 200 OK
Content-Length: 102
Content-Type: application/json
Connection: Closed
{
  "allowed": true,
  "new_url": "scheme://host[:port]/app/stream/file?query=value&query2=value2",
  "lifetime": milliseconds,
  "reason": "authorized"
}
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">Element</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">allowed <b>(required)</b>
      </td>
      <td style="text-align:left">Allows or rejects the client&apos;s request.</td>
    </tr>
    <tr>
      <td style="text-align:left">new_url (optional)</td>
      <td style="text-align:left">Redirects the client to a new url. However, the <code>scheme</code>, <code>port</code>,
        and <code>file</code> cannot be different from the request. The host can
        only be changed to another virtual host on the same server.</td>
    </tr>
    <tr>
      <td style="text-align:left">lifetime (optional)</td>
      <td style="text-align:left">
        <p>The amount of time (in milliseconds) that a client can maintain a connection
          (Publishing or Playback)</p>
        <ul>
          <li>0 means infinity</li>
        </ul>
        <p>HTTP based streaming (HLS, DASH, LLDASH) does not keep a connection, so
          this value does not apply.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">reason (optional)</td>
      <td style="text-align:left">If allowed is false, it will be output to the log.</td>
    </tr>
  </tbody>
</table>

### new url and lifetime

`new_url` redirects the original request to another app/stream. This can be used to hide the actual app/stream name from the user or to authenticate the user by inserting additional information instead of the app/stream name.

For example, you can issue a WebRTC streaming URL by inserting the user ID as shown below. It will be more effective if you distribute the encrypted value with user ID, url expired time, and other information. `ws://domain.com:3333/user_id`

After the Control Server checks whether the user is authorized to play using `user_id`, and responds with `ws://domain.com:3333/app/sport-3` to `new_url`, the user can play app/sport-3.

If the user has only one hour of playback rights, the Control Server responds by putting 3600000 in the `lifetime`.

