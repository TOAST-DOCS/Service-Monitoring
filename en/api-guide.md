## Management > Service Monitoring > API Guide

### Basic information
```
API Endpoint: https://api-service-monitoring.cloud.toast.com
```
## Single Batch Monitoring 

### Data Transfer
-  Transfers data which must be verified by the batch monitoring server.
- The JSON type data can be sent according to the verification information entered for batch monitoring. When verification of batch monitoring fails, it is registered as a failure.

[URL]
```
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
        "serviceCode": 0,
        "status": "beforeValidation"
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
| body.status | String | Request status |

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

## 시나리오 생성

### 데이터 전송
- 서비스 모니터링 서버로 시나리오 생성 요청 시 필요한 데이터를 전송합니다.

[URL]
```http
POST /open-api/v1.0/appkey/{appKey}/scenarios
Content-Type: application/json
```

[Path Variables]

| 값 |	타입 | 필수 여부 |	설명 |
|---|---|---|--|
| appKey | String | Required | 서비스 Appkey(**서비스 관리** 탭에서 확인 가능) |

[Request Header]
 헤더 이름 | 헤더 값
 --- | ---
 TOAST_PRODUCT_APPKEY | Service Monitoring 서비스 관리 메뉴에서 오른쪽 상단 URL & Appkey를 클릭하면 확인 가능한 Appkey

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

#### Request Body
값 | 타입 | 해당하는 scenarioType | 할당 가능한 값 | 필수 여부 | 기본값 | 설명
---|---|---|---|---|---|---
url | String | API | http또는 https로 시작하는 url | Y |  | 모니터링을 진행할 API의 URL
headers | Map\<String, String\> | API |  | N |  | API를 보낼 때 사용할 헤더값
httpMethod | Enum | API | GET, POST, DELETE, PUT | Y |  | API의 httpMethod
requestBody | String | API |  | N |  | API의 requestBody
browserOption | Map\<String, String\> | API | {"OPT_LOCALE" : "kr"} | Y | {"OPT_LOCALE" : "kr"} | 
[validation](#validation) | Object | API |  | Y |  | API의 검증 정보
scenarioType | Enum | API | API | Y |  | 시나리오 타입
scenarioName | String | API |  | Y |  | 시나리오 이름
description | String | API |  | Y |  | 시나리오 설명
monitoringRegion | Set\<Enum\> | API | KOR, US | Y | KOR | 시나리오를 모니터링할 지역
monitoringInterval | Integer | API |  | N(쓰지 않을 경우 monitoringCron이 필수) |  | 모니터링 간격(초)
monitoringCron | String | API | 5자리의 Cron 표현식 | N(쓰지 않을 경우 monitoringInterval이 필수) |  | 모니터링 간격(Cron 표현식)
errorLimitCount | Integer | API | 0 이상의 정수 | Y | 0 | 연속 오류 허용 횟수

#### validation
값 | 타입 | 해당하는 scenarioType | 할당 가능한 값 | 필수 여부 | 기본값 | 설명
---|---|---|---|---|---|---
[validation.textValidation](#textValidation) | Object | API |  | N |  | 문자열 검증 정보
validation.timeout | Integer | API | 0 이상의 정수(ms 단위) | N |  | 타임아웃 임곗값
validation.responseCodes | Set\<String\> | API | HTTP response code | N |  | 허용된 responseCode
validation.avoidingValidationText | String | API |  | N |  | body에 포함된 경우 전파를 제외할 문자열

#### textValidation
값 | 타입 | 해당하는 scenarioType | 할당 가능한 값 | 필수 여부 | 기본값 | 설명
---|---|---|---|---|---|---
validation.textValidation.textValidationType | Enum | API | JSON, HTML, XML | N |  | 문자열을 검증할 때 기반이 되는 body 타입
[validation.textValidation.textValidationInfos](#textValidationInfo) | List\<Object\> | API | | N |  | 문자열 검증 정보

#### textValidationInfo
값 | 타입 | 해당하는 scenarioType | 할당 가능한 값 | 필수 여부 | 기본값 | 설명
---|---|---|---|---|---|---
validation.textValidation.textValidationInfo.operator | Enum | API | CONTAINS, NOT_CONTAINS, EQ, NE, GT, GTE, LT, LTE | Y |  | 문자열 연산자
validation.textValidation.textValidationInfo.expression | String | API |  | Y |  | 검증이 필요한 문자열
validation.textValidation.textValidationInfo.operand | String | API |  | Y(N) |  | 기댓값

#### 응답
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

값 | 타입 | 설명
--- | --- | ---
header.isSuccessful | Boolean | 성공 여부
header.resultCode | Integer | 실패 코드(0은 정상)
header.resultMessage | String | 실패 메시지
body.scenarioId | UUID | 시나리오의 ID
body.url  |  String  |  모니터링을 진행할 API의 URL
headers  |  Map\<String, String\>  |  API를 보낼 때 사용할 헤더값
body.httpMethod  |  Enum  |  API의 httpMethod
body.requestBody  |  String  |  API의 requestBody
body.browserOption  |  Map\<String, String\>  |  
body.validation](#validation)  |  Object  |  API의 검증 정보
body.scenarioType  |  Enum  |  시나리오 타입
body.scenarioName  |  String  |  시나리오 이름
body.description  |  String  |  시나리오 설명
body.monitoringRegion  |  Set\<Enum\>  |  시나리오를 모니터링할 지역
body.monitoringInterval  |  Integer  |  모니터링 간격(초)
body.monitoringCron  |  String  |  모니터링 간격(Cron 표현식)
body.errorLimitCount  |  Integer  |  연속 오류 허용 횟수
body.registeredTime | Date | 등록 시각
body.amendedTime | Date | 수정 시각
body.status | String | 시나리오의 현재 상태

#### validation
값  |  타입  | 설명
--- | --- | ---
[body.validation.textValidation](#textvalidation)  |  Object  |  문자열 검증 정보
body.validation.timeout  |  Integer  |  타임아웃 임곗값
body.validation.responseCodes  | Set\<String\>  |  허용된 responseCode
body.validation.avoidingValidationText  |  String  |  body에 포함된 경우 전파를 제외할 문자열

#### textValidation
필드명(경로명)  |  타입  |  설명
--- | --- | ---
textValidationType  |  Enum  |  문자열을 검증할 때 기반이 되는 body 타입
[body.validation.textValidation.textValidationInfos](#textvalidationinfo)  |  List\<Object\>  |  문자열 검증 정보

#### textValidationInfo
값  |  타입  |  설명
--- | --- | ---
body.validation.textValidation.textValidationInfo.operator  |  Enum  |  문자열 연산자
body.validation.textValidation.textValidationInfo.expression  |  String  |  검증이 필요한 문자열
body.validation.textValidation.textValidationInfo.operand  |  String  |  기댓값

## 등록된 시나리오 조회

[URL]
```http
GET /open-api/v1.0/appkey/{appKey}/scenarios/{ScenarioId}
Content-Type: application/json
```

[Request Header]
 헤더 이름 | 헤더 값
 --- | ---
 TOAST_PRODUCT_APPKEY | Service Monitoring 서비스 관리 메뉴에서 오른쪽 상단 URL & Appkey를 클릭하면 확인 가능한 Appkey

[Path Variables]

| 값 |	타입 | 필수 여부 |	설명 |
|---|---|---|---|
| appKey | String | Required | 서비스 Appkey(**서비스 관리** 탭에서 확인 가능) |
| scenarioId | String | Required | 시나리오 ID |

#### 응답
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

값 | 타입 | 해당하는 scenarioType | 설명
--- | --- | --- | ---
header.isSuccessful | Boolean | - |성공 여부
header.resultCode | Integer | - |실패 코드(0은 정상)
header.resultMessage | String | - |실패 메시지
body.scenarioId | UUID | - | 시나리오의 ID
body.url | String | API, WEB, MODULE | 모니터링을 진행할 API의 URL
body.headers | Map\<String, String\> | API, WEB, MODULE | API를 보낼 때 사용할 헤더값
body.httpMethod | Enum | API, WEB, MODULE | API의 httpMethod
[body.validation](#validation) | Object | - | 시나리오의 검증 정보
body.requestBody | String | API, WEB, MODULE | API의 requestBody
body.browserOption | Map\<String, String\> | API, WEB, MODULE | 
body.ip | String | - | 모니터링을 진행할 대상의 IP
body.scenarioType | Enum | - | 시나티오 타입
body.scenarioName | String | - | 시나리오 이름
body.description | String | - | 시나리오 설명
body.monitoringRegion | Set\<Enum\> | - | 시나리오 모니터링 지역
body.registeredTime | Date | - | 등록 시각
body.amendedTime | Date | - | 수정 시각
body.monitoringInterval | Integer | - | 모니터링 간격(초 단위)
body.monitoringCron | String | - | 모니터링 간격(Cron 표현식)
body.status | String | - | 시나리오의 현재 상태
body.errorLimitCount | Integer | - | 연속 오류 허용 횟수
body.request | String | TCP, UDP | TCP, UDP 요청 시 리퀘스트 문자열
body.port | Integer | TCP,UDP | TCP, UDP 요청 시 포트 번호

#### validation
값  | 타입  |  해당하는 scenarioType |  설명
--- | --- | --- | ---
[body.validation.textValidation](#textvalidation) | Object  |  API, WEB, MODULE |  문자열 검증 정보
body.validation.timeout | Integer  |  - | 타임아웃 임곗값
body.validation.responseCodes  | Set\<String\>  |  - | 허용된 responseCode
body.validation.avoidingValidationText  | String  | API, WEB, MODULE | body에 포함된 경우 전파를 제외할 문자열
body.validation.imageValidationPaths | List\<String\> | API, WEB, MODULE | 이미지 검증 경로
[body.validation.responseValidation](#responsevalidation) | List\<Object\> | TCP,UDP | TCP, UDP 요청 시 Resoponse 검증 목록
body.validation.lengthValidation | Map\<String, String\> | TCP,UDP | Response의 길이 검증

#### textValidation
값 | 타입  |  해당하는 scenarioType |  설명
--- | --- | --- | ---
body.validation.textValidation.textValidationType  | Enum  |  API, WEB, MODULE |  문자열을 검증할 때 기반이 되는 body 타입
[body.validation.textValidation.textValidationInfos](#textvalidationinfo) | List\<Object\>  | API, WEB, MODULE | 문자열 검증 정보

#### textValidationInfo
값  | 타입  |  해당하는 scenarioType | 설명
--- | --- | --- | ---
body.validation.textValidation.textValidationInfo.operator | Enum  |  API, WEB, MODULE | 문자열 연산자
body.validation.textValidation.textValidationInfo.expression | String  |  API, WEB, MODULE |  검증이 필요한 문자열
body.validation.textValidation.textValidationInfo.operand | String  |  API, WEB, MODULE |  기댓값

#### responseValidation
값 | 타입 | 해당하는 scenarioType | 설명
--- | --- | --- | ---
body.validation.responseValidation.position | Integer | TCP,UDP | Response에서 검증할 문자열이 시작하는 위치
body.validation.responseValidation.validationText | String | TCP,UDP | Response에서 검증할 문자열

## 등록된 시나오 삭제

[URL]
```http
DELETE /open-api/v1.0/appkey/{appKey}/scenarios/{ScenarioId}
Content-Type: application/json
```

[Request Header]
 헤더 이름 | 헤더 값
 --- | ---
 TOAST_PRODUCT_APPKEY | Service Monitoring 서비스 관리 메뉴에서 오른쪽 상단 URL & Appkey를 클릭하면 확인 가능한 Appkey

[Path Variables]

| 값 |	타입 | 필수 여부 |	설명 |
|---|---|---|---|
| appKey | String | Required | 서비스 Appkey(**서비스 관리** 탭에서 확인 가능) |
| scenarioId | String | Required | 시나리오 ID |

#### 응답
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

값 | 타입 | 해당하는 scenarioType | 설명
--- | --- | --- | ---
header.isSuccessful | Boolean | - |성공 여부
header.resultCode | Integer | - |실패 코드(0은 정상)
header.resultMessage | String | - |실패 메시지
body.scenarioId | UUID | - | 시나리오의 ID
body.url | String | API, WEB, MODULE | 모니터링을 진행할 API의 URL
body.headers | Map\<String, String\> | API, WEB, MODULE | API를 보낼 때 사용할 헤더값
body.httpMethod | Enum | API, WEB, MODULE | API의 httpMethod
[body.validation](#validation) | Object | - | 시나리오의 검증 정보
body.requestBody | String | API, WEB, MODULE | API의 requestBody
body.browserOption | Map\<String, String\> | API, WEB, MODULE | 
body.ip | String | - | 모니터링을 진행할 대상의 IP
body.scenarioType | Enum | - | 시나티오 타입
body.scenarioName | String | - | 시나리오 이름
body.description | String | - | 시나리오 설명
body.monitoringRegion | Set\<Enum\> | - | 시나리오 모니터링 지역
body.registeredTime | Date | - | 등록 시각
body.amendedTime | Date | - | 수정 시각
body.monitoringInterval | Integer | - | 모니터링 간격(초 단위)
body.monitoringCron | String | - | 모니터링 간격(Cron 표현식)
body.status | String | - | 시나리오의 현재 상태
body.errorLimitCount | Integer | - | 연속 오류 허용 횟수
body.request | String | TCP, UDP | TCP, UDP 요청 시 리퀘스트 문자열
body.port | Integer | TCP,UDP | TCP, UDP 요청 시 포트 번호

#### validation
값  | 타입  |  해당하는 scenarioType |  설명
--- | --- | --- | ---
[body.validation.textValidation](#textvalidation) | Object  |  API, WEB, MODULE |  문자열 검증 정보
body.validation.timeout | Integer  |  - | 타임아웃 임곗값
body.validation.responseCodes  | Set\<String\>  |  - | 허용된 responseCode
body.validation.avoidingValidationText  | String  | API, WEB, MODULE | body에 포함된 경우 전파를 제외할 문자열
body.validation.imageValidationPaths | List\<String\> | API, WEB, MODULE | 이미지 검증 경로
[body.validation.responseValidation](#responsevalidation) | List\<Object\> | TCP,UDP | TCP, UDP 요청 시 Resoponse 검증 목록
body.validation.lengthValidation | Map\<String, String\> | TCP,UDP | Response의 길이 검증

#### textValidation
값 | 타입  |  해당하는 scenarioType |  설명
--- | --- | --- | ---
body.validation.textValidation.textValidationType  | Enum  |  API, WEB, MODULE |  문자열을 검증할 때 기반이 되는 body 타입
[body.validation.textValidation.textValidationInfos](#textvalidationinfo) | List\<Object\>  | API, WEB, MODULE | 문자열 검증 정보

#### textValidationInfo
값  | 타입  |  해당하는 scenarioType | 설명
--- | --- | --- | ---
body.validation.textValidation.textValidationInfo.operator | Enum  |  API, WEB, MODULE | 문자열 연산자
body.validation.textValidation.textValidationInfo.expression | String  |  API, WEB, MODULE |  검증이 필요한 문자열
body.validation.textValidation.textValidationInfo.operand | String  |  API, WEB, MODULE |  기댓값

#### responseValidation
값 | 타입 | 해당하는 scenarioType | 설명
--- | --- | --- | ---
body.validation.responseValidation.position | Integer | TCP,UDP | Response에서 검증할 문자열이 시작하는 위치
body.validation.responseValidation.validationText | String | TCP,UDP | Response에서 검증할 문자열
