## Management > Service Monitoring > API 가이드

### 기본 정보
```
API Endpoint: https://api-service-monitoring.cloud.toast.com
```

## 시나리오 생성

### 데이터 전송
- 서비스 모니터링 서버로 시나리오 생성 요청시 필요한 데이터를 전송합니다.

[URL]
```http
POST /open-api/v1.0/services/{appKey}/scenario
Content-Type: application/json
```

[Path Variables]

| 값 |	타입 | 필수 여부 |	설명 |
|---|---|---|--|
| appKey | String | Required | 서비스 앱키(**서비스 관리** 탭에서 확인 가능) |

[Request Body]
```json
{
   "url":"http://nhn.com",
   "httpMethod":"GET",
   "validation": {
      "responseCodes":["200", "201"],
      "textValidationType":"JSON",
      "textValidations":[
         {
            "expression":"$.isSuccess",
            "operator":"EQ",
            "operand":"true"
         }
      ],
      "timeout":5000
   },
   "browserOption":{
      "OPT_LOCALE":"ko"
   },
   "ip":"nhn.com",
   "scenarioType":"API",
   "scenarioName":"API시나리오 테스트",
   "description":"API시나리오 테스트",
   "monitoringRegion":[
      "KOR"
   ],
   "monitoringInterval":30,
   "errorLimitCount":0
}
```

[RequestBody 설명]  
-RequestBody
타입 | 필드명(경로명) | 해당하는 scenarioType | 할당 가능한 값 | 필수 여부 | 기본값 | 필드 설명
---|---|---|---|---|---|---
String | url | API | http또는 https로 시작하는 url | Y |  | 모니터링을 진행할 api의 url
Map<String, String> | headers | API |  | N |  | api를 보낼 때 사용할 header값
Enum | httpMethod | API | GET, POST, DELETE, PUT | Y |  | api의 httpMethod
String | requestBody | API |  | N |  | api의 requestBody
Map<String, String> | browserOption | API | {"OPT_LOCALE" : "kr"} | Y | {"OPT_LOCALE" : "kr"} | 
Object | validation | API |  | Y |  | api의 검증 정보
Enum | scenarioType | API | API | Y |  | 시나리오 타입
String | scenarioName | API |  | Y |  | 시나리오 이름
String | description | API |  | N |  | 시나리오 설명
Set<Enum> | monitoringRegion | API | KOR, US | Y | KOR | 시나리오를 모니터링 할 지역
Integer | monitoringInterval | API |  | N |  | 모니터링 간격 (초)
String | monitoringCron | API | 5자리의 Cron표현식 | N |  | 모니터링 간격 (Cron표현식)
Integer | errorLimitCount | API | 0이상의 정수 | Y | 0 | 연속 에러 허용 횟수

-RequestBody.validation
타입 | 필드명(경로명) | 해당하는 scenarioType | 할당 가능한 값 | 필수 여부 | 기본값 | 필드 설명
---|---|---|---|---|---|---
Object | textValidation | API |  | N |  | 문자열 검증 정보
Enum | textValidations.operator | API | CONTAINS, NOT_CONTAINS, EQ, NE, GT, GTE, LT, LTE | Y |  | 문자열 연산자
String | textValidations.expression | API |  | Y |  | 검증이 필요한 문자열
String | textValidations.operand | API |  | Y(N) |  | 기댓값
Integer | timeout | API | 0이상의 정수(ms 단위) | N |  | 타임아웃 threshold
Set<String> | responseCodes | API | HTTP response code | N |  | 허용된 responseCode
String | avoidingValidationText | API |  | N |  | body 포함되어있을 경우 전파 제외 할 문자열

-RequestBody.validation.textValidation
타입 | 필드명(경로명) | 해당하는 scenarioType | 할당 가능한 값 | 필수 여부 | 기본값 | 필드 설명
---|---|---|---|---|---|---
Enum | textValidationType | API | JSON, HTML, XML | N |  | 문자열 검증을 할 때 기반이 되는 body 타입
List<Object> | textValidationInfos | API | | N |  | 문자열 검증정보

-RequestBody.validation.textValidation.textValidationInfos
타입 | 필드명(경로명) | 해당하는 scenarioType | 할당 가능한 값 | 필수 여부 | 기본값 | 필드 설명
---|---|---|---|---|---|---
Enum | operator | API | CONTAINS, NOT_CONTAINS, EQ, NE, GT, GTE, LT, LTE | Y |  | 문자열 연산자
String | expression | API |  | Y |  | 검증이 필요한 문자열
String | operand | API |  | Y(N) |  | 기댓값

#### 응답
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "body": {
        "scenarioId": "c0853b80-ec2a-11ea-ac23-87d32e8a7512",
        "url": "http://nhn.com",
        "httpMethod": "GET",
        "validation": {
            "textValidations": [],
            "responseCodes": [
                "200",
                "201"
            ]
        },
        "browserOption": {
            "OPT_LOCALE": "ko"
        },
        "ip": "nhn.com",
        "scenarioType": "API",
        "scenarioName": "API시나리오 테스트",
        "description": "API시나리오 테스트",
        "monitoringRegion": [
            "KOR"
        ],
        "registeredTime": "2020-09-01T08:11:35.160+0000",
        "amendedTime": "2020-09-01T08:11:35.160+0000",
        "monitoringInterval": 30,
        "status": "enable",
        "errorLimitCount": 0
    }
}
```

타입 | 필드명(경로명) | 필드 설명
--- | --- | ---
UUID | scenarioId | 시나리오의 ID
String | url | 모니터링을 진행할 api의 url
Map<String, String> | headers | api를 보낼 때 사용할 header값
Enum | httpMethod | api의 httpMethod
Object | validation | api의 검증 정보
Enum | validation.textValidationType | 문자열 검증을 할 때 기반이 되는 body 타입
List<Object> | validation.textValidations | 문자열 검증 정보
Enum | operator | 문자열 연산자
String | expression | 검증이 필요한 문자열
String | operand | 기댓값
Integer | validation.timeout | 타임아웃 threshold
Set<String> | validation.responseCodes | 허용된 responseCode
String | validation.avoidingValidationText | body 포함되어있을 경우 전파 제외 할 문자열
String | requestBody | api의 requestBody
Map<String, String> | browserOption | 
String | ip | 모니터링을 진행할 api의 ip
Enum | scenarioType | 시나티오 타입
String | scenarioName | 시나리오 이름
String | description | 시나리오 설명
Set<Enum> | monitoringRegion | 시나리오 모니터링 지역
Date | registeredTime | 등록 시각
Date | amendedTime | 수정 시각
String | status | 시나리오의 현재 상태
Integer | errorLimitCount | 연속 에러 허용 횟수
Integer | monitoringInterval | 모니터링 간격(초단위)
String | monitoringCron | 모니터링 간격(Cron 표현식)

## 등록된 시나리오 조회

[URL]
```http
GET /open-api/v1.0/services/{appKey}/scenario/{ScenarioId}
Content-Type: application/json
```

[Path Variables]

| 값 |	타입 | 필수 여부 |	설명 |
|---|---|---|---|
| appKey | String | Required | 서비스 앱키(**서비스 관리** 탭에서 확인 가능) |
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
            "validationCheckList": [
                {
                    "position": 0,
                    "validationText": "Hello world!"
                },
                {
                    "position": 15,
                    "validationText": "OK!"
                }
            ],
            "validationLength": {
                "ENDIAN": "BIG",
                "POS": "4",
                "TYPE": "INT",
                "BASE": "15"
            }
        },
        "ip": "127.0.0.1",
        "scenarioType": "TCP",
        "scenarioName": "Example Scenario",
        "description": "Example Scenario",
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

타입 | 필드명(경로명) | 해당하는 scenarioType | 필드 설명
--- | --- | --- | ---
UUID | scenarioId | - | 시나리오의 ID
String | url | API, WEB, MODULE | 모니터링을 진행할 api의 url
Map<String, String> | headers | API, WEB, MODULE | api를 보낼 때 사용할 header값
Enum | httpMethod | API, WEB, MODULE | api의 httpMethod
Object | validation | - | 시나리오의 검증 정보
Enum | validation.textValidationType | API, WEB, MODULE | 문자열 검증을 할 때 기반이 되는 body 타입
List<Object> | validation.textValidations | API, WEB, MODULE | 문자열 검증 정보
Enum | validation.textValidations.operator | API, WEB, MODULE | 문자열 연산자
String | validation.textValidations.expression | API, WEB, MODULE | 검증이 필요한 문자열
String | validation.textValidations.operand | API, WEB, MODULE | 기댓값
Integer | validation.timeout | - | 타임아웃 threshold(ms 단위)
Set<String> | validation.responseCodes | API, WEB, MODULE | 허용된 responseCode
String | validation.avoidingValidationText | API, WEB, MODULE | body 포함되어있을 경우 전파 제외 할 문자열
List<String> | validation.imageValidationPaths | API, WEB, MODULE | 이미지 검증 경로
List<Object> | validation.validationCheckList | TCP,UDP | TCP, UDP요청시 Resoponse 검증 목록
Integer | validation.validationCheckList.position | TCP,UDP | Response에서 검증할 문자열이 시작하는 위치
String | validation.validationCheckList.validationText | TCP,UDP | Response에서 검증할 문자열
Map<String, String> | validation.validationLength | TCP,UDP | Response의 길이 검증
String | requestBody | API, WEB, MODULE | api의 requestBody
Map<String, String> | browserOption | API, WEB, MODULE | 
String | ip | - | 모니터링을 진행할 대상의 ip
Enum | scenarioType | - | 시나티오 타입
String | scenarioName | - | 시나리오 이름
String | description | - | 시나리오 설명
Set<Enum> | monitoringRegion | - | 시나리오 모니터링 지역
Date | registeredTime | - | 등록 시각
Date | amendedTime | - | 수정 시각
Integer | monitoringInterval | - | 모니터링 간격(초단위)
String | monitoringCron | - | 모니터링 간격(Cron 표현식)
String | status | - | 시나리오의 현재 상태
Integer | errorLimitCount | - | 연속 에러 허용 횟수
String | request | TCP, UDP | TCP, UDP요청시 리퀘스트 문자열
Integer | port | TCP,UDP | TCP, UDP요청시 포트 번호

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
| appKey | String | Required | 서비스 앱키(**서비스 관리** 탭에서 확인 가능) |
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
- 비정상적인 앱키, 시나리오 ID 존재할 경우 검증을 진행하지 않습니다.

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