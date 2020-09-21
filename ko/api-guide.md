## Management > Service Monitoring > API 가이드

### 기본 정보
```
API Endpoint: https://api-service-monitoring.cloud.toast.com
```

## 단일 배치 모니터링

### 데이터 전송
- 배치 모니터링 서버로 검증이 필요한 데이터를 전송합니다.
- 배치 모니터링에 입력한 검증 정보에 따른 JSON 타입의 데이터를 전송할 수 있으며, 배치 모니터링의 검증에 실패할 경우 장애로 등록됩니다.

[URL]
```http
POST /v1.0/monitoring/batchmon/appkey/{appKey}/scenarios/{scenarioId}
Content-Type: application/json
```

[Path Variables]

| 값 |	타입 | 필수 여부 |	설명 |
|---|---|---|---|
| appKey | String | Required | 서비스 Appkey(**서비스 관리** 탭에서 확인 가능) |
| scenarioId | String | Required | 서비스 ID |

[Request Body]
```json
{
    "issueDescription": "This is test message."
}
```


#### 응답
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
        "ipaddr": "127.0.0.1",
        "requestTime": "2099-12-31T00:00:00.000",
        "requestData": {
            "body": "{\"issueDescription\": \"This is test message.\"}"
        },
        "serviceCode": 0
    }
}
```

| 값 | 타입 | 설명 |
|---|---|---|
| header.isSuccessful | Boolean | 성공 여부 |
| header.resultCode | Integer | 실패 코드(0은 정상) |
| header.resultMessage | String | 실패 메시지 |
| body.pk.serviceId | String | 서비스 고유 ID |
| body.pk.requestId | String | 요청 고유 ID |
| body.scenarioId | String | 시나리오 고유 ID |
| body.requestData.body | Object | 요청 데이터 |
| body.ipaddr | String | 요청자의 IP 주소 |
| body.requestTime | String | 요청 시각(ISO 8601 포맷) |
| body.serviceCode | Integer | 서비스 고유 코드 |


## 다중 배치 모니터링
- 한 번의 요청으로 다중 서비스, 시나리오를 검증할 수 있습니다.
- 비정상적인 앱키나 시나리오 ID가 존재할 경우 검증을 진행하지 않습니다.

### 데이터 전송
- 배치 모니터링 서버로 검증이 필요한 데이터를 전송합니다.
- 배치 모니터링에 입력한 검증 정보에 따른 JSON 타입의 데이터를 전송할 수 있으며, 배치 모니터링의 검증에 실패할 경우 장애로 등록됩니다.

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


#### 응답
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
| 값 | 타입 | 설명 |
|---|---|---|
| header.isSuccessful | Boolean | 성공 여부 |
| header.resultCode | Integer | 실패 코드(0은 정상) |
| header.resultMessage | String | 실패 메시지 |
| body.pk.serviceId | String | 서비스 고유 ID |
| body.pk.requestId | String | 요청 고유 ID |
| body.scenarioId | String | 시나리오 고유 ID |
| body.requestData.body | Object | 요청 데이터 |
| body.ipaddr | String | 요청자의 IP 주소 |
| body.requestTime | String | 요청 시각(ISO 8601 포맷) |
| body.serviceCode | Integer | 서비스 고유 코드 |

## 시나리오 생성

### 데이터 전송
- 서비스 모니터링 서버로 시나리오 생성 요청 시 필요한 데이터를 전송합니다.

[URL]
```http
POST /open-api/v1.0/appkey/{appKey}/scenarios
Content-Type: application/json
```

[Path Variables]

| 값 |	타입 | 필수 여부 |	설명 
|---|---|---|---|
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

- Request Body

값 | 타입 | 해당하는 scenarioType | 할당 가능한 값 | 필수 여부 | 기본값 | 설명
---|---|---|---|---|---|---
url | String | API | http또는 https로 시작하는 url | Y |  | 모니터링을 진행할 API의 URL
headers | Map&lt;String, String&gt; | API |  | N |  | API를 보낼 때 사용할 헤더값
httpMethod | String | API | GET, POST, DELETE, PUT | Y |  | API의 httpMethod
requestBody | String | API |  | N |  | API의 requestBody
browserOption | Map&lt;String, String&gt; | API | {"OPT_LOCALE" : "kr"} | Y | {"OPT_LOCALE" : "kr"} | 
[validation](#validation1) | Object | API |  | Y |  | API의 검증 정보
scenarioType | String | API | API | Y |  | 시나리오 타입
scenarioName | String | API |  | Y |  | 시나리오 이름
description | String | API |  | Y |  | 시나리오 설명
monitoringRegion | Set&lt;String&gt; | API | KOR, US | Y | KOR | 시나리오를 모니터링할 지역
monitoringInterval | Integer | API |  | N(쓰지 않을 경우 monitoringCron이 필수) |  | 모니터링 간격(초)
monitoringCron | String | API | 5자리의 Cron 표현식 | N(쓰지 않을 경우 monitoringInterval이 필수) |  | 모니터링 간격(Cron 표현식)
errorLimitCount | Integer | API | 0 이상의 정수 | Y | 0 | 연속 오류 허용 횟수

<div id='validation1'></div>
- validation

값 | 타입 | 해당하는 scenarioType | 할당 가능한 값 | 필수 여부 | 기본값 | 설명
---|---|---|---|---|---|---
[validation.textValidation](#textValidation1) | Object | API |  | N |  | 문자열 검증 정보
validation.timeout | Integer | API | 0 이상의 정수(ms 단위) | N |  | 타임아웃 임곗값
validation.responseCodes | Set&lt;String&gt; | API | HTTP response code | N |  | 허용된 responseCode
validation.avoidingValidationText | String | API |  | N |  | body에 포함된 경우 전파를 제외할 문자열

<div id='textValidation1'></div>
- textValidation

값 | 타입 | 해당하는 scenarioType | 할당 가능한 값 | 필수 여부 | 기본값 | 설명
---|---|---|---|---|---|---
validation.textValidation.textValidationType | String | API | JSON, HTML, XML | N |  | 문자열을 검증할 때 기반이 되는 body 타입
[validation.textValidation.textValidationInfos](#textValidationInfo1) | List&lt;Object&gt; | API | | N |  | 문자열 검증 정보

<div id='textValidationInfo1'></div>
- textValidationInfo

값 | 타입 | 해당하는 scenarioType | 할당 가능한 값 | 필수 여부 | 기본값 | 설명
---|---|---|---|---|---|---
validation.textValidation.textValidationInfo.operator | String | API | CONTAINS, NOT_CONTAINS, EQ, NE, GT, GTE, LT, LTE | Y |  | 문자열 연산자
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
body.scenarioId | String | 시나리오의 ID
body.url  |  String  |  모니터링을 진행할 API의 URL
headers  |  Map&lt;String, String&gt;  |  API를 보낼 때 사용할 헤더값
body.httpMethod  |  String  |  API의 httpMethod
body.requestBody  |  String  |  API의 requestBody
body.browserOption  |  Map&lt;String, String&gt;  |  
[body.validation](#validation2)  |  Object  |  API의 검증 정보
body.scenarioType  |  String  |  시나리오 타입
body.scenarioName  |  String  |  시나리오 이름
body.description  |  String  |  시나리오 설명
body.monitoringRegion  |  Set&lt;String&gt;  |  시나리오를 모니터링할 지역
body.monitoringInterval  |  Integer  |  모니터링 간격(초)
body.monitoringCron  |  String  |  모니터링 간격(Cron 표현식)
body.errorLimitCount  |  Integer  |  연속 오류 허용 횟수
body.registeredTime | String | 등록 시각(yyyy-MM-dd'T'HH:mm:ss.SSSz)
body.amendedTime | String | 수정 시각(yyyy-MM-dd'T'HH:mm:ss.SSSz)
body.status | String | 시나리오의 현재 상태

<div id='validation2'></div>
- validation

값  |  타입  | 설명
--- | --- | ---
[body.validation.textValidation](#textValidation2)  |  Object  |  문자열 검증 정보
body.validation.timeout  |  Integer  |  타임아웃 임곗값
body.validation.responseCodes  | Set&lt;String&gt;  |  허용된 responseCode
body.validation.avoidingValidationText  |  String  |  body에 포함된 경우 전파를 제외할 문자열

<div id='textValidation2'></div>
- textValidation

필드명(경로명)  |  타입  |  설명
--- | --- | ---
textValidationType  |  String  |  문자열을 검증할 때 기반이 되는 body 타입
[body.validation.textValidation.textValidationInfos](#textValidationInfo2)  |  List&lt;Object&gt;  |  문자열 검증 정보

<div id='textValidationInfo2'></div>
- textValidationInfo

값  |  타입  |  설명
--- | --- | ---
body.validation.textValidation.textValidationInfo.operator  |  String  |  문자열 연산자
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
body.scenarioId | String | - | 시나리오의 ID
body.url | String | API, WEB, MODULE | 모니터링을 진행할 API의 URL
body.headers | Map&lt;String, String&gt; | API, WEB, MODULE | API를 보낼 때 사용할 헤더값
body.httpMethod | String | API, WEB, MODULE | API의 httpMethod
[body.validation](#validation3) | Object | - | 시나리오의 검증 정보
body.requestBody | String | API, WEB, MODULE | API의 requestBody
body.browserOption | Map&lt;String, String&gt; | API, WEB, MODULE | 
body.ip | String | - | 모니터링을 진행할 대상의 IP
body.scenarioType | String | - | 시나티오 타입
body.scenarioName | String | - | 시나리오 이름
body.description | String | - | 시나리오 설명
body.monitoringRegion | Set&lt;String&gt; | - | 시나리오 모니터링 지역
body.registeredTime | String | - | 등록 시각(yyyy-MM-dd'T'HH:mm:ss.SSSz)
body.amendedTime | String | - | 수정 시각(yyyy-MM-dd'T'HH:mm:ss.SSSz)
body.monitoringInterval | Integer | - | 모니터링 간격(초 단위)
body.monitoringCron | String | - | 모니터링 간격(Cron 표현식)
body.status | String | - | 시나리오의 현재 상태
body.errorLimitCount | Integer | - | 연속 오류 허용 횟수
body.request | String | TCP, UDP | TCP, UDP 요청 시 리퀘스트 문자열
body.port | Integer | TCP,UDP | TCP, UDP 요청 시 포트 번호

<div id='validation3'></div>
- validation

값  | 타입  |  해당하는 scenarioType |  설명
--- | --- | --- | ---
[body.validation.textValidation](#textValidation3) | Object  |  API, WEB, MODULE |  문자열 검증 정보
body.validation.timeout | Integer  |  - | 타임아웃 임곗값
body.validation.responseCodes  | Set&lt;String&gt;  |  - | 허용된 responseCode
body.validation.avoidingValidationText  | String  | API, WEB, MODULE | body에 포함된 경우 전파를 제외할 문자열
body.validation.imageValidationPaths | List&lt;String&gt; | API, WEB, MODULE | 이미지 검증 경로
[body.validation.responseValidation](#responseValidation3) | List&lt;Object&gt; | TCP,UDP | TCP, UDP 요청 시 Resoponse 검증 목록
body.validation.lengthValidation | Map&lt;String, String&gt; | TCP,UDP | Response의 길이 검증

<div id='textValidation3'></div>
- textValidation

값 | 타입  |  해당하는 scenarioType |  설명
--- | --- | --- | ---
body.validation.textValidation.textValidationType  | String  |  API, WEB, MODULE |  문자열을 검증할 때 기반이 되는 body 타입
[body.validation.textValidation.textValidationInfos](#textValidationInfo3) | List&lt;Object&gt;  | API, WEB, MODULE | 문자열 검증 정보

<div id='textValidationInfo3'></div>
- textValidationInfo

값  | 타입  |  해당하는 scenarioType | 설명
--- | --- | --- | ---
body.validation.textValidation.textValidationInfo.operator | String  |  API, WEB, MODULE | 문자열 연산자
body.validation.textValidation.textValidationInfo.expression | String  |  API, WEB, MODULE |  검증이 필요한 문자열
body.validation.textValidation.textValidationInfo.operand | String  |  API, WEB, MODULE |  기댓값

<div id='responseValidation3'></div>
- responseValidation

값 | 타입 | 해당하는 scenarioType | 설명
--- | --- | --- | ---
body.validation.responseValidation.position | Integer | TCP,UDP | Response에서 검증할 문자열이 시작하는 위치
body.validation.responseValidation.validationText | String | TCP,UDP | Response에서 검증할 문자열

## 등록된 시나리오 삭제

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
body.scenarioId | String | - | 시나리오의 ID
body.url | String | API, WEB, MODULE | 모니터링을 진행할 API의 URL
body.headers | Map&lt;String, String&gt; | API, WEB, MODULE | API를 보낼 때 사용할 헤더값
body.httpMethod | String | API, WEB, MODULE | API의 httpMethod
[body.validation](#validation4) | Object | - | 시나리오의 검증 정보
body.requestBody | String | API, WEB, MODULE | API의 requestBody
body.browserOption | Map&lt;String, String&gt; | API, WEB, MODULE | 
body.ip | String | - | 모니터링을 진행할 대상의 IP
body.scenarioType | String | - | 시나티오 타입
body.scenarioName | String | - | 시나리오 이름
body.description | String | - | 시나리오 설명
body.monitoringRegion | Set&lt;String&gt; | - | 시나리오 모니터링 지역
body.registeredTime | String | - | 등록 시각(yyyy-MM-dd'T'HH:mm:ss.SSSz)
body.amendedTime | String | - | 수정 시각(yyyy-MM-dd'T'HH:mm:ss.SSSz)
body.monitoringInterval | Integer | - | 모니터링 간격(초 단위)
body.monitoringCron | String | - | 모니터링 간격(Cron 표현식)
body.status | String | - | 시나리오의 현재 상태
body.errorLimitCount | Integer | - | 연속 오류 허용 횟수
body.request | String | TCP, UDP | TCP, UDP 요청 시 리퀘스트 문자열
body.port | Integer | TCP,UDP | TCP, UDP 요청 시 포트 번호

<div id='validation4'></div>
- validation

값  | 타입  |  해당하는 scenarioType |  설명
--- | --- | --- | ---
[body.validation.textValidation](#textValidation4) | Object  |  API, WEB, MODULE |  문자열 검증 정보
body.validation.timeout | Integer  |  - | 타임아웃 임곗값
body.validation.responseCodes  | Set&lt;String&gt;  |  - | 허용된 responseCode
body.validation.avoidingValidationText  | String  | API, WEB, MODULE | body에 포함된 경우 전파를 제외할 문자열
body.validation.imageValidationPaths | List&lt;String&gt; | API, WEB, MODULE | 이미지 검증 경로
[body.validation.responseValidation](#responseValidation4) | List&lt;Object&gt; | TCP,UDP | TCP, UDP 요청 시 Resoponse 검증 목록
body.validation.lengthValidation | Map&lt;String, String&gt; | TCP,UDP | Response의 길이 검증

<div id='textValidation4'></div>
- textValidation

값 | 타입  |  해당하는 scenarioType |  설명
--- | --- | --- | ---
body.validation.textValidation.textValidationType  | String  |  API, WEB, MODULE |  문자열을 검증할 때 기반이 되는 body 타입
[body.validation.textValidation.textValidationInfos](#textValidationInfo4) | List&lt;Object&gt;  | API, WEB, MODULE | 문자열 검증 정보

<div id='textValidationInfo4'></div>
- textValidationInfo

값  | 타입  |  해당하는 scenarioType | 설명
--- | --- | --- | ---
body.validation.textValidation.textValidationInfo.operator | String  |  API, WEB, MODULE | 문자열 연산자
body.validation.textValidation.textValidationInfo.expression | String  |  API, WEB, MODULE |  검증이 필요한 문자열
body.validation.textValidation.textValidationInfo.operand | String  |  API, WEB, MODULE |  기댓값

<div id='responseValidation4'></div>
- responseValidation

값 | 타입 | 해당하는 scenarioType | 설명
--- | --- | --- | ---
body.validation.responseValidation.position | Integer | TCP,UDP | Response에서 검증할 문자열이 시작하는 위치
body.validation.responseValidation.validationText | String | TCP,UDP | Response에서 검증할 문자열