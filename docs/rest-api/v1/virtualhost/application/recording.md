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
  "id": "custom_id",  
  "stream": {  
    "name": "stream_o",  
    "tracks": [ 100, 200 ]  
  },  
  "filePath" : "/path/to/save/recorded/file_${Sequence}.ts",  
  "infoPath" : "/path/to/save/information/file.xml",  
  "interval" : 60000,    # Split it every 60 seconds  
  "schedule" : "0 0 */1" # Split it at second 0, minute 0, every hours.   
  "segmentationRule" : "continuity"  
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
{% api-method-parameter name="segmentationRule" type="string" required=false %}
Define the policy for continuously or discontinuously generating timestamp in divided recorded files.  
  
- continuity   
- discontinuity  \(default\)
{% endapi-method-parameter %}

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
  
- default is all tracks
{% endapi-method-parameter %}

{% api-method-parameter name="schedule" type="string" required=false %}
Schedule based split recording.  set only &lt;second minute hour&gt; using crontab method.  
It cannot be used simultaneously with interval.
{% endapi-method-parameter %}

{% api-method-parameter name="interval" type="number" required=false %}
Interval based split recording. It cannot be used simultaneously with schedule.
{% endapi-method-parameter %}

{% api-method-parameter required=false name="filePath" type="string" %}
Set the path of the file to be recorded. same as setting macro pattern in Config file.
{% endapi-method-parameter %}

{% api-method-parameter required=false name="infoPath" type="string" %}
Set the path to the information file to be recorded. same as setting macro pattern in Config file.
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
            "state": "ready",
            "id": "stream_o",
            "vhost": "default",
            "app": "app",
            "stream": {
                "name": "stream_o",
                "tracks": []
            },
            "filePath": "/path/to/save/recorded/file_${Sequence}.ts",
            "infoPath": "/path/to/save/information/file.xml",            
            "interval": 60000,  
            "schedule": "0 0 */1",       
            "segmentationRule": "continuity",
            "createdTime": "2021-08-31T23:44:44.789+0900"
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
  "id": "custom_id"  
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
An unique identifier for recording job.
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
            "state": "stopping",
            "id": "custom_id",            
            "vhost": "default",
            "app": "app",
            "stream": {
                "name": "stream_o",
                "tracks": []
            },
            "filePath": "app_stream_o_0.ts",
            "infoPath": "app_stream_o.xml",
            "segmentationRule": "discontinuity",
            "recordBytes": 1200503,
            "recordTime": 4272,
            "totalRecordBytes": 1204775,
            "totalRecordTime": 4272,
            "createdTime": "2021-08-31T23:44:44.789+0900",
            "startTime": "2021-08-31T23:44:44.849+0900"
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
This API performs a query of the job being recorded. Provides job inquiry function for all or custom Id.   
  
Request Example:  
`POST http://1.2.3.4:8081/v1/vhosts/default/apps/app:records         
{  
   "id" : "custom_id"  
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
{% api-method-parameter name="id" type="string" required=false %}
An unique identifier for recording job. If no value is specified, the entire recording job is requested.
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
            "state": "ready",
            "id": "custom_id_1",
            "vhost": "default",
            "app": "app",
            "stream": {
                "name": "stream_o",
                "tracks": []
            },
            "filePath": "app_stream_o_0.ts",
            "infoPath": "app_stream_o.xml",
            "segmentationRule": "discontinuity",
            "createdTime": "2021-08-31T21:05:01.171+0900",
        },
        {
            "state": "recording",
            "id": "custom_id_2",
            "vhost": "default",                    
            "app": "app",
            "stream": {
                "name": "stream_o",
                "tracks": []
            },
            "filePath": "app_stream_o_0.ts",
            "infoPath": "app_stream_o.xml",
            "segmentationRule": "discontinuity",
            "sequence": 0,
            "recordBytes": 1907351,
            "recordTime": 6968,
            "totalRecordBytes": 1907351,
            "totalRecordTime": 6968,
            "createdTime": "2021-08-31T21:05:01.171+0900",            
            "startTime": "2021-08-31T21:05:01.567+0900",            
        },
        {
            "state": "stopping",
            "id": "custom_id_3",
            "vhost": "default",
            "app": "app",
            "stream": {
                "name": "stream_o",
                "tracks": []
            },
            "filePath": "app_stream_o_0.ts",
            "infoPath": "app_stream_o.xml",
            "segmentationRule": "discontinuity",
            "sequence": 0,
            "recordBytes": 1907351,
            "recordTime": 6968,
            "totalRecordBytes": 1907351,
            "totalRecordTime": 6968,
            "createdTime": "2021-08-31T21:05:01.171+0900",
            "startTime": "2021-08-31T21:05:01.567+0900",
        },
        {
            "state": "stopped",
            "id": "custom_id_4",
            "vhost": "default",        
            "app": "app",
            "stream": {
                "name": "stream_o",
                "tracks": []
            },
            "filePath": "app_stream_o_0.ts",
            "infoPath": "app_stream_o.xml",
            "segmentationRule": "discontinuity",
            "sequence": 0,
            "recordBytes": 1907351,
            "recordTime": 6968,
            "totalRecordBytes": 1907351,
            "totalRecordTime": 6968,
            "startTime": "2021-08-31T21:05:01.567+0900",            
            "createdTime": "2021-08-31T21:05:01.171+0900",            
            "finishTime": "2021-08-31T21:15:01.567+0900"
        },
        {
            "state": "error",
            "id": "custom_id_5",
            "vhost": "default",        
            "app": "app",
            "stream": {
                "name": "stream_o",
                "tracks": []
            },
            "filePath": "app_stream_o_0.ts",
            "infoPath": "app_stream_o.xml",
            "segmentationRule": "discontinuity",
            "sequence": 0,
            "recordBytes": 1907351,
            "recordTime": 6968,
            "totalRecordBytes": 1907351,
            "totalRecordTime": 6968,
            "createdTime": "2021-08-31T21:05:01.171+0900",
            "startTime": "2021-08-31T21:05:01.567+0900",
            "finishTime": "2021-08-31T21:15:01.567+0900"
        }
    ],
    "statusCode": 200
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

