## Monitoring > Service Monitoring > API Guide

### Basic information
```
API Endpoint: https://api-service-monitoring.cloud.toast.com
```

## Single Batch Monitoring

### Data Transfer
-  Transfers data which must be verified by the batch monitoring server.
- The JSON type data can be sent according to the verification information entered for batch monitoring. When verification of batch monitoring fails, it is registered as a failure.

[URL]
```http
POST /v1.0/monitoring/batchmon/appkey/{appKey}/scenarios/{scenarioId}
Content-Type: application/json
```

[Path Variables]

| Value |	Type | Required? |	Description |
|---|---|---|--|
| appKey | String | Required | Service App Key(You can view this on Manage Service tab) |
| scenarioId | String | Required | Service ID |

[Request Body]
```json
{
    "issueDescription": "This is test message."
}
```


#### Response
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "pk": {
            "serviceId": "d781dafc-2d5e-3d11-a87e-529b0868ea4f",
            "requestId": "3f136280-9d6a-11e9-83b4-ff39d67ddecf"
        },
        "scenarioId": "b00699c0-96f4-11e9-a68b-a7aaea9ae346",
        "ipaddr": "192.168.0.1",
        "requestTime": "2099-12-31T00:00:00.000",
        "requestData": {
            "body": "{\"issueDescription\": \"This is test message.\"}"
        },
        "serviceCode": 0
    }
}
```

| Value | Type | Description |
|---|---|---|
| header.isSuccessful | boolean | Success or Fail |
| header.resultCode | Integer | Fail code(0 is normal) |
| header.resultMessage | String | Fail message |
| body.pk.serviceId | String | Unique ID of the service |
| body.pk.requestId | String | Unique ID of the request |
| body.scenarioId | String | Unique ID of the scenario |
| body.requestData.body | Object | Request data |
| body.ipaddr | String | IP address of the requestor |
| body.requestTime | String | Request time (ISO 8601 format) |
| body.serviceCode | Integer | Unique code of the service |


## Multiple Batch Monitoring
- Verify multiple services and scenarios, with a single request.
- Not verify, when normal appkey or scenario ID exist.

### Data Transfer
- Send data in need of verification to bach monitoring server.
- Send JSON-type data according to verification information entered at batch monitoring; if verification fails for batch monitoring, register as failure.

[URL]
```http
POST /v1.0/monitoring/batchmon
Content-Type: application/json
```

[Request Body]
```json
{
    "target" : [
        {
            "appkey" : "81713f1cc96b4eae834f7535ec4fae7a",
            "scenarioIdList" : [
                "12962600-5c39-11ea-bd8f-0bbd7856015c"
            ]
        },
        {
            "appkey" : "2aac90b8f3c64fcb8083d1ed0218192c",
            "scenarioIdList" : [
                "1b648d20-5c39-11ea-bd8f-0bbd7856015c"
            ]
        }
    ],
    "data" : "{\"key\": \"value\"}"
}
```


#### Response
```json
{
    "body": [
        {
            "ipaddr": "127.0.0.1",
            "pk": {
                "requestId": "1958be80-5c3b-11ea-bd8f-0bbd7856015c",
                "serviceId": "128248e0-ff3b-34cd-be87-2b5bd173febb"
            },
            "requestData": {
                "body": "{\"key\": \"value\"}"
            },
            "requestTime": "2099-12-31T00:00:00.000",
            "scenarioId": "12962600-5c39-11ea-bd8f-0bbd7856015c",
            "serviceCode": 0
        },
        {
            "ipaddr": "127.0.0.1",
            "pk": {
                "requestId": "1959a8e0-5c3b-11ea-bd8f-0bbd7856015c",
                "serviceId": "1ab4e137-a158-346b-bacc-7e0c76466ac0"
            },
            "requestData": {
                "body": "{\"key\": \"value\"}"
            },
            "requestTime": "2099-12-31T00:00:00.000",
            "scenarioId": "1b648d20-5c39-11ea-bd8f-0bbd7856015c",
            "serviceCode": 1
        }
    ],
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```
| Value | Type | Description |
|---|---|---|
| header.isSuccessful | Boolean | Successful or Not |
| header.resultCode | Integer | Failure Code (0 is normal) |
| header.resultMessage | String | Failure Message |
| body.pk.serviceId | String | Original Service ID |
| body.pk.requestId | String | Original Request ID |
| body.scenarioId | String | Original Scenario ID |
| body.requestData.body | Object | Request Data |
| body.ipaddr | String | IP Address of Requestor |
| body.requestTime | String | Request Time (ISO 8601 format) |
| body.serviceCode | Integer | Original Service Code |

## Create scenario

### Data transfer
- Send data required for scenario creation request to the service monitoring server.

[URL]
```http
POST /open-api/v1.0/appkey/{appKey}/scenarios
Content-Type: application/json
```

[Path Variables]

| Value |	Type | Necessity |	Description 
|---|---|---|---|
| appKey | String | Required | Service Appkey (Viewable in **Manage Service** tab) |

[Request Header]

 Header Name | Header value
 --- | ---
 TOAST_PRODUCT_APPKEY | Service Monitoring: in the Manage Service menu, click URL & Appkey on the upper right side to check this Appkey.

[Request Body]
```json
{
   "url":"http://toast.com",
   "httpMethod":"GET",
   "validation":{
      "textValidation":{
         "textValidationType":"JSON",
         "textValidationInfos":[
            {
               "expression":"$.isSuccess",
               "operator":"EQ",
               "operand":"true"
            }
         ]
      },
      "responseCodes":[
         "200",
         "201"
      ]
   },
   "browserOption":{
      "OPT_LOCALE":"ko"
   },
   "ip":"toast.com",
   "scenarioType":"API",
   "scenarioName":"API test",
   "description":"API test",
   "monitoringRegion":[
      "KOR"
   ],
   "monitoringInterval":30,
   "errorLimitCount":0
}
```

- Request Body

Value | Type | Corresponding scenarioType | Assignable Value | Necessity | Default | Description
---|---|---|---|---|---|---
url | String | API | http or url starting with https | Y |  | The URL of API to monitor
headers | Map\<String, String\> | API |  | N |  | Header value to use to send the API
httpMethod | Enum | API | GET, POST, DELETE, PUT | Y |  | API의 httpMethod
requestBody | String | API |  | N |  | API의 requestBody
browserOption | Map\<String, String\> | API | {"OPT_LOCALE" : "kr"} | N | {"OPT_LOCALE" : "kr"} |
[validation](#validation1) | Object | API |  | Y |  | Validation info of API
scenarioType | Enum | API | API | Y |  | Scenario type
scenarioName | String | API |  | Y |  | Scenario name
description | String | API |  | Y |  | Scenario description
monitoringRegion | Set\<Enum\> | API | KOR, US | Y | KOR | The region to monitor a scenario
monitoringInterval | Integer | API |  | N (monitoringCron required if this is not used) |  | Monitoring intervals (sec)
monitoringCron | String | API | [6-digit Cron expression](#cronExpression) | N (monitoringInterval required if this is not used) |  | Monitoring intervals (Cron expression)
errorLimitCount | Integer | API | 0 or higher integer | Y | 0 | Number of repeat errors allowed

<div id='validation1'></div>
- validation

Value | Type | Corresponding scenarioType | Assignable Value | Necessity | Default | Description
---|---|---|---|---|---|---
[validation.textValidation](#textValidation1) | Object | API |  | N |  | String validation info
validation.timeout | Integer | API | 0 or higher integer (in ms) | N |  | Timeout threshold
validation.responseCodes | Set\<String\> | API | HTTP response code | N |  | Allowed responseCode
validation.avoidingValidationText | String | API |  | N |  | String to exclude from propagation, if included in the body

<div id='textValidation1'></div>
- textValidation

Value | Type | Corresponding scenarioType | Assignable Value | Necessity | Default | Description
---|---|---|---|---|---|---
validation.textValidation.textValidationType | Enum | API | JSON, HTML, XML | N |  | Base body type to validate a string
[validation.textValidation.textValidationInfos](#textValidationInfo1) | List\<Object\> | API | | N |  | String validation info

<div id='textValidationInfo1'></div>
- textValidationInfo

Value | Type | Corresponding scenarioType | Assignable Value | Necessity | Default | Description
---|---|---|---|---|---|---
validation.textValidation.textValidationInfo.operator | Enum | API | CONTAINS, NOT_CONTAINS, EQ, NE, GT, GTE, LT, LTE | Y |  | String operator
validation.textValidation.textValidationInfo.expression | String | API |  | Y |  | String that requires validation
validation.textValidation.textValidationInfo.operand | String | API |  | Y(N) |  | Expected Value

<div id='cronExpression'></div>

- cronExpression
  - A Cron expression is a string of six fields separated by spaces.
  - 'Day' and 'Day of week' cannot be set at the same time; one of the two fields must always be `?`.

Order | Item | Required | Allowed value | Allowed special characters
---|---|---|---|---
1 | minute | Y | 0-59 |  , - * /
2 | hour | Y | 0-23 | , - * /
3 | day | Y | 1-31 | , - * ? / L W
4 | month | Y | 1-12 or JAN-DEC | , - * /
5 | day of week | Y | 1-7 or SUN-SAT | , - * ? / L #
6 | year | N | 1970-2099 | , - * /

#### Response
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "scenarioId": "0c96cff0-edc2-11ea-9760-8d94f461e6d4",
        "url": "http://toast.com",
        "httpMethod": "GET",
        "validation": {
            "textValidation": {
                "textValidationType": "JSON",
                "textValidationInfos": [
                    {
                        "operator": "EQ",
                        "expression": "$.isSuccess",
                        "operand": "true"
                    }
                ]
            },
            "responseCodes": [
                "200",
                "201"
            ]
        },
        "browserOption": {
            "OPT_LOCALE": "ko"
        },
        "ip": "toast.com",
        "scenarioType": "API",
        "scenarioName": "API test",
        "description": "API test",
        "monitoringRegion": [
            "KOR"
        ],
        "amendedTime": "2020-09-03T08:49:01.197+0000",
        "monitoringCron": "7 * * * * ? *",
        "status": "temporary",
        "errorLimitCount": 0
    }
}
```

Value | Type | Description
--- | --- | ---
header.isSuccessful | Boolean | Success
header.resultCode | Integer | Failure Code (0: Normal)
header.resultMessage | String | Failure message
body.scenarioId | UUID | Scenario ID
body.url  |  String  |  The URL of API to monitor
headers  |  Map\<String, String\>  |  Header value to use to send the API
body.httpMethod  |  Enum  |  API의 httpMethod
body.requestBody  |  String  |  API의 requestBody
body.browserOption  |  Map\<String, String\>  |
[body.validation](#validation2)  |  Object  |  Validation info of API
body.scenarioType  |  Enum  |  Scenario type
body.scenarioName  |  String  |  Scenario name
body.description  |  String  |  Scenario description
body.monitoringRegion  |  Set\<Enum\>  |  The region to monitor a scenario
body.monitoringInterval  |  Integer  |  Monitoring intervals (sec)
body.monitoringCron  |  String  |  Monitoring intervals (Cron expression)
body.errorLimitCount  |  Integer  |  Number of repeat errors allowed
body.registeredTime | Date | Registered time
body.amendedTime | Date | Amended time
body.status | String | Scenario's current status

<div id='validation2'></div>
- validation

Value | Type | Description
--- | --- | ---
[body.validation.textValidation](#textValidation2)  |  Object  |  String validation info
body.validation.timeout  |  Integer  |  Timeout threshold
body.validation.responseCodes  | Set\<String\>  |  Allowed responseCode
body.validation.avoidingValidationText  |  String  |  String to exclude from propagation, if included in the body

<div id='textValidation2'></div>
- textValidation

Field name(directory name)  |  Type  |  Description
--- | --- | ---
textValidationType  |  Enum  |  Base body type to validate a string
[body.validation.textValidation.textValidationInfos](#textValidationInfo2)  |  List\<Object\>  |  String validation info

<div id='textValidationInfo2'></div>
- textValidationInfo

Value | Type | Description
--- | --- | ---
body.validation.textValidation.textValidationInfo.operator  |  Enum  |  String operator
body.validation.textValidation.textValidationInfo.expression  |  String  |  String that requires validation
body.validation.textValidation.textValidationInfo.operand  |  String  |  Expected Value

## Query registered scenario

[URL]
```http
GET /open-api/v1.0/appkey/{appKey}/scenarios/{ScenarioId}
Content-Type: application/json
```

[Request Header]

 Header Name | Header value
 --- | ---
 TOAST_PRODUCT_APPKEY | Service Monitoring: in the Manage Service menu, click URL & Appkey on the upper right side to check this Appkey.

[Path Variables]

| Value |	Type | Necessity |	Description |
|---|---|---|---|
| appKey | String | Required | Service Appkey (Viewable in **Manage Service** tab) |
| scenarioId | String | Required | Scenario ID |

#### Response
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "scenarioId": "be50d2a0-e353-11ea-a3c2-ebd9a267dbb2",
        "validation": {
            "timeout": 5000,
            "responseValidation": [
                {
                    "position": 0,
                    "validationText": "Hello World!"
                },
                {
                    "position": 13,
                    "validationText": "It's new world!"
                }
            ],
            "lengthValidation": {
                "ENDIAN": "BIG",
                "POS": "4",
                "TYPE": "INT",
                "BASE": "15"
            }
        },
        "ip": "127.0.0.1",
        "scenarioType": "TCP",
        "scenarioName": "TCP Scenario Test",
        "description": "TCP Scenario Test",
        "monitoringRegion": [
            "KOR"
        ],
        "amendedTime": "2020-09-01T05:54:58.861+0000",
        "monitoringCron": "9 * * * * ? *",
        "status": "temporary",
        "errorLimitCount": 0,
        "request": "Hello World!",
        "port": 8080
    }
}
```

Value | Type | Corresponding scenarioType | Description
--- | --- | --- | ---
header.isSuccessful | Boolean | - |Success
header.resultCode | Integer | - |Failure Code (0: Normal)
header.resultMessage | String | - |Failure message
body.scenarioId | UUID | - | Scenario ID
body.url | String | API, WEB, MODULE | The URL of API to monitor
body.headers | Map\<String, String\> | API, WEB, MODULE | Header value to use to send the API
body.httpMethod | Enum | API, WEB, MODULE | API의 httpMethod
[body.validation](#validation3) | Object | - | Scenario validation info
body.requestBody | String | API, WEB, MODULE | API의 requestBody
body.browserOption | Map\<String, String\> | API, WEB, MODULE |
body.ip | String | - | IP of monitoring target
body.scenarioType | Enum | - | Scenario type
body.scenarioName | String | - | Scenario name
body.description | String | - | Scenario description
body.monitoringRegion | Set\<Enum\> | - | Scenario monitoring region
body.registeredTime | Date | - | Registered time
body.amendedTime | Date | - | Amended time
body.monitoringInterval | Integer | - | Monitoring intervals (unit: secs)
body.monitoringCron | String | - | Monitoring intervals (Cron expression)
body.status | String | - | Scenario's current status
body.errorLimitCount | Integer | - | Number of repeat errors allowed
body.request | String | TCP, UDP | Request string for TCP, UDP requests
body.port | Integer | TCP,UDP | Port number for TCP, UDP requests

<div id='validation3'></div>
- validation

Value | Type | Corresponding scenarioType | Description
--- | --- | --- | ---
[body.validation.textValidation](#textValidation3) | Object  |  API, WEB, MODULE |  String validation info
body.validation.timeout | Integer  |  - | Timeout threshold
body.validation.responseCodes  | Set\<String\>  |  - | Allowed responseCode
body.validation.avoidingValidationText  | String  | API, WEB, MODULE | String to exclude from propagation, if included in the body
body.validation.imageValidationPaths | List\<String\> | API, WEB, MODULE | Image verification path
[body.validation.responseValidation](#responseValidation3) | List\<Object\> | TCP,UDP | Response verification list for TCP, UDP requests
body.validation.lengthValidation | Map\<String, String\> | TCP,UDP | Response length verification

<div id='textValidation3'></div>
- textValidation

Value | Type | Corresponding scenarioType | Description
--- | --- | --- | ---
body.validation.textValidation.textValidationType  | Enum  |  API, WEB, MODULE |  Base body type to validate a string
[body.validation.textValidation.textValidationInfos](#textValidationInfo3) | List\<Object\>  | API, WEB, MODULE | String validation info

<div id='textValidationInfo3'></div>
- textValidationInfo

Value | Type | Corresponding scenarioType | Description
--- | --- | --- | ---
body.validation.textValidation.textValidationInfo.operator | Enum  |  API, WEB, MODULE | String operator
body.validation.textValidation.textValidationInfo.expression | String  |  API, WEB, MODULE |  String that requires validation
body.validation.textValidation.textValidationInfo.operand | String  |  API, WEB, MODULE |  Expected Value

<div id='responseValidation3'></div>
- responseValidation

Value | Type | Corresponding scenarioType | Description
--- | --- | --- | ---
body.validation.responseValidation.position | Integer | TCP,UDP | Starting location of string to be verified in response
body.validation.responseValidation.validationText | String | TCP,UDP | String to be verified in response

## Delete registered scenario

[URL]
```http
DELETE /open-api/v1.0/appkey/{appKey}/scenarios/{ScenarioId}
Content-Type: application/json
```

[Request Header]

 Header Name | Header value
 --- | ---
 TOAST_PRODUCT_APPKEY | Service Monitoring: in the Manage Service menu, click URL & Appkey on the upper right side to check this Appkey.

[Path Variables]

| Value |	Type | Necessity |	Description |
|---|---|---|---|
| appKey | String | Required | Service Appkey (Viewable in **Manage Service** tab) |
| scenarioId | String | Required | Scenario ID |

#### Response
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "scenarioId": "be50d2a0-e353-11ea-a3c2-ebd9a267dbb2",
        "validation": {
            "timeout": 5000,
            "responseValidation": [
                {
                    "position": 0,
                    "validationText": "Hello World!"
                },
                {
                    "position": 13,
                    "validationText": "It's new world!"
                }
            ],
            "lengthValidation": {
                "ENDIAN": "BIG",
                "POS": "4",
                "TYPE": "INT",
                "BASE": "15"
            }
        },
        "ip": "127.0.0.1",
        "scenarioType": "TCP",
        "scenarioName": "TCP Scenario Test",
        "description": "TCP Scenario Test",
        "monitoringRegion": [
            "KOR"
        ],
        "amendedTime": "2020-09-01T05:54:58.861+0000",
        "monitoringCron": "9 * * * * ? *",
        "status": "temporary",
        "errorLimitCount": 0,
        "request": "Hello World!",
        "port": 8080
    }
}
```

Value | Type | Corresponding scenarioType | Description
--- | --- | --- | ---
header.isSuccessful | Boolean | - |Success
header.resultCode | Integer | - |Failure Code (0: Normal)
header.resultMessage | String | - |Failure message
body.scenarioId | UUID | - | Scenario ID
body.url | String | API, WEB, MODULE | The URL of API to monitor
body.headers | Map\<String, String\> | API, WEB, MODULE | Header value to use to send the API
body.httpMethod | Enum | API, WEB, MODULE | API의 httpMethod
[body.validation](#validation4) | Object | - | Scenario validation info
body.requestBody | String | API, WEB, MODULE | API의 requestBody
body.browserOption | Map\<String, String\> | API, WEB, MODULE |
body.ip | String | - | IP of monitoring target
body.scenarioType | Enum | - | Scenario type
body.scenarioName | String | - | Scenario name
body.description | String | - | Scenario description
body.monitoringRegion | Set\<Enum\> | - | Scenario monitoring region
body.registeredTime | Date | - | Registered time
body.amendedTime | Date | - | Amended time
body.monitoringInterval | Integer | - | Monitoring intervals (unit: secs)
body.monitoringCron | String | - | Monitoring intervals (Cron expression)
body.status | String | - | Scenario's current status
body.errorLimitCount | Integer | - | Number of repeat errors allowed
body.request | String | TCP, UDP | Request string for TCP, UDP requests
body.port | Integer | TCP,UDP | Port number for TCP, UDP requests

<div id='validation4'></div>
- validation

Value | Type | Corresponding scenarioType | Description
--- | --- | --- | ---
[body.validation.textValidation](#textValidation4) | Object  |  API, WEB, MODULE |  String validation info
body.validation.timeout | Integer  |  - | Timeout threshold
body.validation.responseCodes  | Set\<String\>  |  - | Allowed responseCode
body.validation.avoidingValidationText  | String  | API, WEB, MODULE | String to exclude from propagation, if included in the body
body.validation.imageValidationPaths | List\<String\> | API, WEB, MODULE | Image verification path
[body.validation.responseValidation](#responseValidation4) | List\<Object\> | TCP,UDP | Response verification list for TCP, UDP requests
body.validation.lengthValidation | Map\<String, String\> | TCP,UDP | Response length verification

<div id='textValidation4'></div>
- textValidation

Value | Type | Corresponding scenarioType | Description
--- | --- | --- | ---
body.validation.textValidation.textValidationType  | Enum  |  API, WEB, MODULE |  Base body type to validate a string
[body.validation.textValidation.textValidationInfos](#textValidationInfo4) | List\<Object\>  | API, WEB, MODULE | String validation info

<div id='textValidationInfo4'></div>
- textValidationInfo

Value | Type | Corresponding scenarioType | Description
--- | --- | --- | ---
body.validation.textValidation.textValidationInfo.operator | Enum  |  API, WEB, MODULE | String operator
body.validation.textValidation.textValidationInfo.expression | String  |  API, WEB, MODULE |  String that requires validation
body.validation.textValidation.textValidationInfo.operand | String  |  API, WEB, MODULE |  Expected Value

<div id='responseValidation4'></div>
- responseValidation

Value | Type | Corresponding scenarioType | Description
--- | --- | --- | ---
body.validation.responseValidation.position | Integer | TCP,UDP | Starting location of string to be verified in response
body.validation.responseValidation.validationText | String | TCP,UDP | String to be verified in response

## Scenario Modification

### Data transfer
- Send data required for scenario modification request to the service monitoring server.

[URL]
```http
POST /open-api/v1.0/appkey/{appKey}/scenarios/{scenarioId}
Content-Type: application/json
```

[Path Variables]

| Value |Type | Necessity |Description |
|---|---|---|---|
| appKey | String | Required | Service Appkey (Viewable in **Manage Service** tab) |
| scenarioId | String | Required | Scenario ID (Viewable in **Edit Scenario** window) |

[Request Header]

 Header Name | Header Value 
 --- | ---
 TOAST_PRODUCT_APPKEY | Service Monitoring: In the **Manage Service** menu, click **URL & Appkey** on the upper right side to check this Appkey. 

[Request Body]
```json
{
   "url":"http://toast.com",
   "httpMethod":"GET",
   "validation":{
      "textValidation":{
         "textValidationType":"JSON",
         "textValidationInfos":[
            {
               "expression":"$.isSuccess",
               "operator":"EQ",
               "operand":"true"
            }
         ]
      },
      "responseCodes":[
         "200",
         "201"
      ]
   },
   "browserOption":{
      "OPT_LOCALE":"ko"
   },
   "ip":"toast.com",
   "scenarioType":"API",
   "scenarioName":"API test",
   "description":"API test",
   "monitoringRegion":[
      "KOR"
   ],
   "monitoringInterval":30,
   "errorLimitCount":0
}
```

- Request Body

Value | Type | Corresponding scenarioType | Assignable Value | Necessity | Default | Description
---|---|---|---|---|---|---
url | String | API | url starting with http or https | Y |  | The URL of API to monitor 
headers | Map&lt;String, String&gt; | API |  | N |  | Header value to use to send the API
httpMethod | String | API | GET, POST, DELETE, PUT | Y |  | httpMethod of API
requestBody | String | API |  | N |  | requestBody of API
browserOption | Map&lt;String, String&gt; | API | {"OPT_LOCALE" : "kr"} | N | {"OPT_LOCALE" : "kr"} |
[validation](#validation1) | Object | API |  | Y |  | Validation info of API
scenarioType | String | API | API | Y |  | Scenario type
scenarioName | String | API |  | Y |  | Scenario name
description | String | API |  | Y |  | Scenario description
monitoringRegion | Set&lt;String&gt; | API | KOR, US | Y | KOR | The region to monitor a scenario
monitoringInterval | Integer | API |  | N (monitoringCron required if this is not used) |  | Monitoring intervals (sec)
monitoringCron | String | API | [6-digit Cron expression](#cronExpression)  | N (monitoringInterval required if this is not used) |  | Monitoring intervals (Cron expression)
errorLimitCount | Integer | API | 0 or higher integer | Y | 0 | Number of repeat errors allowed

<div id='validation1'></div>
- validation

Value | Type | Corresponding scenarioType | Assignable Value | Necessity | Default | Description
---|---|---|---|---|---|---
[validation.textValidation](#textValidation1) | Object | API |  | N |  | String validation info
validation.timeout | Integer | API | 0 or higher integer (in ms) | N |  | Timeout threshold
validation.responseCodes | Set&lt;String&gt; | API | HTTP response code | N |  | Allowed response code
validation.avoidingValidationText | String | API |  | N |  | String to exclude from propagation, if included in the body

<div id='textValidation1'></div>
- textValidation

Value | Type | Corresponding scenarioType | Assignable Value | Necessity | Default | Description
---|---|---|---|---|---|---
validation.textValidation.textValidationType | String | API | JSON, HTML, and XML | N |  | Base body type to validate a string
[validation.textValidation.textValidationInfos](#textValidationInfo1) | List&lt;Object&gt; | API | | N |  | String validation info

<div id='textValidationInfo1'></div>
- textValidationInfo

Value | Type | Corresponding scenarioType | Assignable Value | Necessity | Default | Description
---|---|---|---|---|---|---
validation.textValidation.textValidationInfo.operator | String | API | CONTAINS, NOT_CONTAINS, EQ, NE, GT, GTE, LT, LTE | Y |  | String operator
validation.textValidation.textValidationInfo.expression | String | API |  | Y |  | String that requires validation
validation.textValidation.textValidationInfo.operand | String | API |  | Y(N) |  | Expected Value

#### Response
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "scenarioId": "0c96cff0-edc2-11ea-9760-8d94f461e6d4",
        "url": "http://toast.com",
        "httpMethod": "GET",
        "validation": {
            "textValidation": {
                "textValidationType": "JSON",
                "textValidationInfos": [
                    {
                        "operator": "EQ",
                        "expression": "$.isSuccess",
                        "operand": "true"
                    }
                ]
            },
            "responseCodes": [
                "200",
                "201"
            ]
        },
        "browserOption": {
            "OPT_LOCALE": "ko"
        },
        "ip": "toast.com",
        "scenarioType": "API",
        "scenarioName": "API test",
        "description": "API test",
        "monitoringRegion": [
            "KOR"
        ],
        "amendedTime": "2020-09-03T08:49:01.197+0000",
        "monitoringCron": "7 * * * * ? *",
        "status": "temporary",
        "errorLimitCount": 0
    }
}
```

Value | Type | Description
--- | --- | ---
header.isSuccessful | Boolean | Success
header.resultCode | Integer | Failure Code (0: Normal)
header.resultMessage | String | Failure message
body.scenarioId | String | Scenario ID
body.url  |  String  | The URL of API to monitor 
headers  |  Map&lt;String, String&gt;  |  Header value to use to send the API
body.httpMethod  |  String  |  httpMethod of API
body.requestBody  |  String  |  requestBody of API
body.browserOption  |  Map&lt;String, String&gt;  |
[body.validation](#validation2)  |  Object  |  Validation info of API
body.scenarioType  |  String  |  Scenario type
body.scenarioName  |  String  |  Scenario name
body.description  |  String  |  Scenario description
body.monitoringRegion  |  Set&lt;String&gt;  |  The region to monitor a scenario
body.monitoringInterval  |  Integer  |  Monitoring intervals (sec)
body.monitoringCron  |  String  |  Monitoring intervals (Cron expression)
body.errorLimitCount  |  Integer  |  Number of repeat errors allowed
body.registeredTime | String | Registered time (yyyy-MM-dd'T'HH:mm:ss.SSSz)
body.amendedTime | String | Modified time (yyyy-MM-dd'T'HH:mm:ss.SSSz)
body.status | String | Scenario's current status

<div id='validation2'></div>
- validation

Value  |  Type  | Description
--- | --- | ---
[body.validation.textValidation](#textValidation2)  |  Object  |  String validation info
body.validation.timeout  |  Integer  |  Timeout threshold
body.validation.responseCodes  | Set&lt;String&gt;  |  Allowed response code
body.validation.avoidingValidationText  |  String  |  String to exclude from propagation, if included in the body

<div id='textValidation2'></div>
- textValidation

Field name (directory name)  |  Type  |  Description
--- | --- | ---
textValidationType  |  String  |  Base body type to validate a string
[body.validation.textValidation.textValidationInfos](#textValidationInfo2)  |  List&lt;Object&gt;  |  String validation info

<div id='textValidationInfo2'></div>
- textValidationInfo

Value  |  Type  |  Description
--- | --- | ---
body.validation.textValidation.textValidationInfo.operator  |  String  |  String operator
body.validation.textValidation.textValidationInfo.expression  |  String  |  String that requires validation
body.validation.textValidation.textValidationInfo.operand  |  String  |  Expected Value