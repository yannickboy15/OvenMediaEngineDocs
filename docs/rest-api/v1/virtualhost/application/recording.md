# Recording

{% api-method method="post" host="http://<OME\_HOST>:<API\_PORT>" path="/v1/vhosts/{vhost\_name}/apps/{app\_name}:startRecord" %}
{% api-method-summary %}
/v1/vhosts/{vhost\_name}/apps/{app\_name}:startRecord
{% endapi-method-summary %}

{% api-method-description %}
This API performs a recording start request operation.  for recording, the output stream name must be specified. file path, information path, recording interval and schedule parameters can be specified as options.  
  
Request Example:  
`POST http://1.2.3.4:8081/v1/vhosts/default/apps/app:startRecord                         
{  
  "id": "CustomId",  
  "stream": {  
    "name": "stream_o",  
    "tracks": [ 100, 200 ]  
  },  
  "filePath" : "{/path/to/save/recorded/file.ts}",  
  "infoPath" : "{/Path/to/save/information/file.xml}",  
  "interval" : 60000,         # Split it every 60 seconds  
  "schedule" : "0 0 */1"      # Split it at second 0, minute 0, every hours.   
}`
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter required=true name="vhost\_name" type="string" %}
A name of `VirtualHost`
{% endapi-method-parameter %}

{% api-method-parameter required=true name="app\_name" type="string" %}
A name of `Application`
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter required=true name="authorization" type="string" %}
A string for authentication in `Basic Base64(AccessToken)` format.  
For example, `Basic b21lLWFjY2Vzcy10b2tlbg==` if access token is `ome-access-token`
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="id" type="string" required=true %}
An unique identifier for recording job.
{% endapi-method-parameter %}

{% api-method-parameter name="stream" type="string" required=true %}
Output stream.
{% endapi-method-parameter %}

{% api-method-parameter name="name" type="string" required=true %}
Output stream name.
{% endapi-method-parameter %}

{% api-method-parameter name="tracks" type="array" required=false %}
Default is all tracks. It is possible to record only a specific track using the track Id.
{% endapi-method-parameter %}

{% api-method-parameter name="schedule" type="string" required=false %}
Schedule based split recording.  set only &lt;second minute hour&gt; using crontab method.  
It cannot be used simultaneously with interval.
{% endapi-method-parameter %}

{% api-method-parameter name="interval" type="number" required=false %}
Interval based split recording. millisecond unit  
It canoot be used sumultaneously with schedule
{% endapi-method-parameter %}

{% api-method-parameter required=false name="filePath" type="string" %}
Set the path of the file to be recorded. Same as setting macro pattern in Config file.
{% endapi-method-parameter %}

{% api-method-parameter required=false name="infoPath" type="string" %}
Set the path to the information file to be recorded. Same as setting macro pattern in Config file.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "message": "OK",
    "response": [
        {
            "id": "lotte_shop2",
            "vhost": "default",
            "app": "app",
            "stream": {
                "name" : "stream_o"
                "tracks": [100, 200]
            }
        }
    ],
    "statusCode": 200
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
	"statusCode": 400,
	"message": "There is no required parameter [{id}, {stream.name}] (400)"
}
	
{
	"statusCode": 400,
	"message": "Duplicate ID already exists [{id}] (400)"
}

{
  "statusCode": 400,
  "message" : "[Interval] and [Schedule] cannot be used at the same time"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="http://<OME\_HOST>:<API\_PORT>" path="/v1/vhosts/{vhost\_name}/apps/{app\_name}:stopRecord" %}
{% api-method-summary %}
/v1/vhosts/{vhost\_name}/apps/{app\_name}:stopRecord
{% endapi-method-summary %}

{% api-method-description %}
This API performs a recording stop request.   
  
Request Example:  
`POST http://1.2.3.4:8081/v1/vhosts/default/apps/app:stopRecord                           
  
{  
  "id": "CustomId"  
}`
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter required=true name="vhost\_name" type="string" %}
A name of `VirtualHost`
{% endapi-method-parameter %}

{% api-method-parameter required=true name="app\_name" type="string" %}
A name of `Application`
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter required=true name="authorization" type="string" %}
A string for authentication in `Basic Base64(AccessToken)` format.  
For example, `Basic b21lLWFjY2Vzcy10b2tlbg==` if access token is `ome-access-token`.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter required=true name="id" type="string" %}
An unique identifier for recording ㅓㅐ
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
	"statusCode": 400,
	"message": "There is no required parameter [id] (400)"
}
	
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
	"statusCode": 404,
	"message": "There is no record information related to the ID [{id}] (404)"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="http://<OME\_HOST>:<API\_PORT>" path="/v1/vhosts/{vhost\_name}/apps/{app\_name}:records" %}
{% api-method-summary %}
/v1/vhosts/{vhost\_name}/apps/{app\_name}:records
{% endapi-method-summary %}

{% api-method-description %}
You can view all the recording lists that are being recorded in the application. Information such as recording status, file path, size, and recording time can be found in the inquired record item.  
  
Request Example:  
`POST http://1.2.3.4:8081/v1/vhosts/default/apps/app:records`                           
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter required=true name="vhost\_name" type="string" %}
A name of `VirtualHost`
{% endapi-method-parameter %}

{% api-method-parameter required=true name="app\_name" type="string" %}
A name of `Application`
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter required=true name="authorization" type="string" %}
A string for authentication in `Basic Base64(AccessToken)` format.  
For example, `Basic b21lLWFjY2Vzcy10b2tlbg==` if access token is `ome-access-token`.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "message": "OK",
    "response": [
		{
			"id": "UserDefinedUniqueId",
			"app": "app",
			"createdTime": "2021-01-18T03:27:16.019+09:00",
			"finishTime": "1970-01-01T09:00:00+09:00",
			"recordBytes": 0,
			"recordTime": 0,
			"sequence": 0,
			"startTime": "1970-01-01T09:00:00+09:00",
			"state": "ready",
      "filePath" : "{/path/to/save/recorded/file.ts}",
      "infoPath" : "{/Path/to/save/information/file.xml}"			 
			"stream": {
				"name": "stream2_o",
				"tracks": [
					101,
					102
				]
			},
			"totalRecordBytes": 0,
			"totalRecordTime": 0,
			"vhost": "default"
		},
		{
			"id": "UserDefinedUniqueId2",
			"app": "app",
			"createdTime": "2021-01-18T03:24:31.812+09:00",
			"finishTime": "1970-01-01T09:00:00+09:00",
			"recordBytes": 0,
			"recordTime": 0,
			"sequence": 0,
			"startTime": "1970-01-01T09:00:00+09:00",
			"state": "ready",
      "filePath" : "{/path/to/save/recorded/file.ts}",
      "infoPath" : "{/Path/to/save/information/file.xml}"			
			"stream": {
				"name": "stream_o",
				"tracks": [
					101,
					102
				]
			},
			"totalRecordBytes": 0,
			"totalRecordTime": 0,
			"vhost": "default"
		}
	]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=204 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
	"statusCode": 204,
	"message": "There is no record information (204)"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

