## Management > Service Monitoring > APIガイド

### 基本情報
```
API Endpoint: https://api-service-monitoring.cloud.toast.com
```

## バッチモニタリング

### データ転送
- バッチモニタリングサーバーに、検証が必要なデータを転送します。
- バッチモニタリングに入力した検証情報に基づいたJSONタイプのデータを転送することができ、バッチモニタリングの検証に失敗した場合は障害に登録されます。

[URL]
```
POST /v1.0/monitoring/batchmon/appkey/{appKey}/scenarios/{scenarioId}
Content-Type: application/json
```

[Path Variables]

| 値 |	タイプ | 必須かどうか |	説明 |
|---|---|---|--|
| appKey | String | Required | サービスアプリケーションキー(**サービス管理**タブで確認可能) |
| scenarioId | String | Required | サービスID |

[Request Body]
```json
{
    "issueDescription": "This is test message."
}
```


#### レスポンス
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

| 値 | タイプ | 説明 |
|---|---|---|
| header.isSuccessful | Boolean | 成否 |
| header.resultCode | Integer | 失敗コード(0は正常) |
| header.resultMessage | String | 失敗メッセージ |
| body.pk.serviceId | String | サービス固有ID |
| body.pk.requestId | String | リクエスト固有ID |
| body.scenarioId | String | シナリオ固有ID |
| body.requestData.body | Object | リクエストデータ |
| body.ipaddr | String | リクエスト者のIPアドレス |
| body.requestTime | String | リクエスト時刻(ISO 8601フォーマット) |
| body.serviceCode | Integer | サービス固有コード |
| body.status | String | リクエスト状態 |

## シナリオ作成

### データ転送
- サービスモニタリングサーバーへシナリオ作成をリクエストする時、必要なデータを転送します。

[URL]
```http
POST /open-api/v1.0/appkey/{appKey}/scenarios
Content-Type: application/json
```

[Path Variables]

| 値 |	タイプ | 必須か |	説明 
|---|---|---|---|
| appKey | String | Required | サービスAppkey(**サービス管理**タブで確認可能) |

[Request Header]

 ヘッダ名 | ヘッダ値
 --- | ---
 TOAST_PRODUCT_APPKEY | Service Monitoringサービス管理メニューで右上URL & Appkeyを押すと確認できるAppkey

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

値 | タイプ | 該当するscenarioType | 割当可能な値 | 必須か | デフォルト値 | 説明
---|---|---|---|---|---|---
url | String | API | httpまたはhttpsで始まるurl | Y |  | モニタリングを進行するAPIのURL
headers | Map&lt;String、String&gt; | API |  | N |  | APIを送る時に使用するヘッダ値
httpMethod | String | API | GET、POST、DELETE、PUT | Y |  | APIのhttpMethod
requestBody | String | API |  | N |  | APIのrequestBody
browserOption | Map&lt;String, String&gt; | API | {"OPT_LOCALE" : "kr"} | Y | {"OPT_LOCALE" : "kr"} | 
[validation](#validation1) | Object | API |  | Y |  | APIの検証情報
scenarioType | String | API | API | Y |  | シナリオタイプ
scenarioName | String | API |  | Y |  | シナリオ名
description | String | API |  | Y |  | シナリオの説明
monitoringRegion | Set&lt;String&gt; | API | KOR、US | Y | KOR | シナリオをモニタリングする地域
monitoringInterval | Integer | API |  | N(使用しないの場合monitoringCronが必須) |  | モニタリング間隔(秒)
monitoringCron | String | API | 5桁のCron式 | N(使用しないの場合monitoringIntervalが必須) |  | モニタリング間隔(Cron式)
errorLimitCount | Integer | API | 0以上の整数 | Y | 0 | 連続エラー許容回数

<div id='validation1'></div>
- validation

値 | タイプ | 該当するscenarioType | 割当可能な値 | 必須か | デフォルト値 | 説明
---|---|---|---|---|---|---
[validation.textValidation](#textValidation1) | Object | API |  | N |  | 文字列検証情報
validation.timeout | Integer | API | 0以上の整数(ms単位) | N |  | タイムアウトしきい値
validation.responseCodes | Set&lt;String&gt; | API | HTTP response code | N |  | 許可されたresponseCode
validation.avoidingValidationText | String | API |  | N |  | bodyに含まれる場合、配信を除く文字列

<div id='textValidation1'></div>
- textValidation

値 | タイプ | 該当するscenarioType | 割当可能な値 | 必須か | デフォルト値 | 説明
---|---|---|---|---|---|---
validation.textValidation.textValidationType | String | API | JSON、HTML、XML | N |  | 文字列を検証する時、ベースになるbodyタイプ
[validation.textValidation.textValidationInfos](#textValidationInfo1) | List&lt;Object&gt; | API | | N |  | 文字列検証情報

<div id='textValidationInfo1'></div>
- textValidationInfo

値 | タイプ | 該当するscenarioType | 割当可能な値 | 必須か | デフォルト値 | 説明
---|---|---|---|---|---|---
validation.textValidation.textValidationInfo.operator | String | API | CONTAINS、NOT_CONTAINS、EQ、NE、GT、GTE、LT、LTE | Y |  | 文字列演算子
validation.textValidation.textValidationInfo.expression | String | API |  | Y |  | 検証が必要な文字列
validation.textValidation.textValidationInfo.operand | String | API |  | Y(N) |  | 期待値

#### レスポンス
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

値 | タイプ | 説明
--- | --- | ---
header.isSuccessful | Boolean | 成否
header.resultCode | Integer | 失敗コード(0は正常)
header.resultMessage | String | 失敗メッセージ
body.scenarioId | String | シナリオのID
body.url  |  String  | モニタリングを進行するAPIのURL
headers  |  Map&lt;String, String&gt;  |  APIを送る時に使用するヘッダ値
body.httpMethod  |  String  |  APIのhttpMethod
body.requestBody  |  String  |  APIのrequestBody
body.browserOption  |  Map&lt;String, String&gt;  |  
[body.validation](#validation2)  |  Object  |  APIの検証情報
body.scenarioType  |  String  | シナリオタイプ
body.scenarioName  |  String  | シナリオ名
body.description  |  String  | シナリオの説明
body.monitoringRegion  |  Set&lt;String&gt;  | シナリオをモニタリングする地域
body.monitoringInterval  |  Integer  | モニタリング間隔(秒)
body.monitoringCron  |  String  | モニタリング間隔(Cron式)
body.errorLimitCount  |  Integer  | 連続エラー許容回数
body.registeredTime | String | 登録時刻(yyyy-MM-dd'T'HH:mm:ss.SSSz)
body.amendedTime | String | 修正時刻(yyyy-MM-dd'T'HH:mm:ss.SSSz)
body.status | String | シナリオの現在状態

<div id='validation2'></div>
- validation

値 | タイプ | 説明
--- | --- | ---
[body.validation.textValidation](#textValidation2)  |  Object  | 文字列検証情報
body.validation.timeout  |  Integer  | タイムアウトしきい値
body.validation.responseCodes  | Set&lt;String&gt;  |  許可されたresponseCode
body.validation.avoidingValidationText  |  String  |  bodyに含まれる場合、配信を除く文字列

<div id='textValidation2'></div>
- textValidation

フィールド名(パス名)  | タイプ | 説明
--- | --- | ---
textValidationType  |  String  | 文字列を検証する時、ベースになるbodyタイプ
[body.validation.textValidation.textValidationInfos](#textValidationInfo2)  |  List&lt;Object&gt;  | 文字列検証情報

<div id='textValidationInfo2'></div>
- textValidationInfo

値 | タイプ | 説明
--- | --- | ---
body.validation.textValidation.textValidationInfo.operator  |  String  | 文字列演算子
body.validation.textValidation.textValidationInfo.expression  |  String  | 検証が必要な文字列
body.validation.textValidation.textValidationInfo.operand  |  String  |  期待値

## 登録されたシナリオ照会

[URL]
```http
GET /open-api/v1.0/appkey/{appKey}/scenarios/{ScenarioId}
Content-Type: application/json
```

[Request Header]

 ヘッダ名 | ヘッダ値
 --- | ---
 TOAST_PRODUCT_APPKEY | Service Monitoringサービス管理メニューで右上URL & Appkeyを押すと確認できるAppkey

[Path Variables]

| 値 |	タイプ | 必須か |	説明 |
|---|---|---|---|
| appKey | String | Required | サービスAppkey(**サービス管理**タブで確認可能) |
| scenarioId | String | Required | シナリオID |

#### レスポンス
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

値 | タイプ | 該当するscenarioType | 説明
--- | --- | --- | ---
header.isSuccessful | Boolean | - |成否
header.resultCode | Integer | - |失敗コード(0は正常)
header.resultMessage | String | - |失敗メッセージ
body.scenarioId | String | - | シナリオのID
body.url | String | API、WEB、MODULE | モニタリングを進行するAPIのURL
body.headers | Map&lt;String、String&gt; | API、WEB、MODULE | APIを送る時に使用するヘッダ値
body.httpMethod | String | API、WEB、MODULE | APIのhttpMethod
[body.validation](#validation3) | Object | - | シナリオの検証情報
body.requestBody | String | API、WEB、MODULE | APIのrequestBody
body.browserOption | Map&lt;String、String&gt; | API、WEB、MODULE | 
body.ip | String | - | モニタリングを進行する対象のIP
body.scenarioType | String | - | シナリオタイプ
body.scenarioName | String | - | シナリオ名
body.description | String | - | シナリオの説明
body.monitoringRegion | Set&lt;String&gt; | - | シナリオモニタリング地域
body.registeredTime | String | - | 登録時刻(yyyy-MM-dd'T'HH:mm:ss.SSSz)
body.amendedTime | String | - | 修正時刻(yyyy-MM-dd'T'HH:mm:ss.SSSz)
body.monitoringInterval | Integer | - | モニタリング間隔(秒単位)
body.monitoringCron | String | - | モニタリング間隔(Cron式)
body.status | String | - | シナリオの現在状態
body.errorLimitCount | Integer | - | 連続エラー許容回数
body.request | String | TCP、UDP | TCP、UDPリクエスト時のリクエスト文字列
body.port | Integer | TCP、UDP | TCP、UDPリクエスト時のポート番号

<div id='validation3'></div>
- validation

値 | タイプ | 該当するscenarioType | 説明
--- | --- | --- | ---
[body.validation.textValidation](#textValidation3) | Object  |  API、WEB、MODULE | 文字列検証情報
body.validation.timeout | Integer  |  - | タイムアウトしきい値
body.validation.responseCodes  | Set&lt;String&gt;  |  - | 許可されたresponseCode
body.validation.avoidingValidationText  | String  | API、WEB、MODULE | bodyに含まれる場合、配信を除く文字列
body.validation.imageValidationPaths | List&lt;String&gt; | API、WEB、MODULE | イメージ検証パス
[body.validation.responseValidation](#responseValidation3) | List&lt;Object&gt; | TCP、UDP | TCP、UDPリクエスト時、Resoponse検証リスト
body.validation.lengthValidation | Map&lt;String、String&gt; | TCP、UDP | Responseの長さ検証

<div id='textValidation3'></div>
- textValidation

値 | タイプ | 該当するscenarioType | 説明
--- | --- | --- | ---
body.validation.textValidation.textValidationType  | String  |  API, WEB, MODULE | 文字列を検証する時、ベースになるbodyタイプ
[body.validation.textValidation.textValidationInfos](#textValidationInfo3) | List&lt;Object&gt;  | API、WEB、MODULE | 文字列検証情報

<div id='textValidationInfo3'></div>
- textValidationInfo

値 | タイプ | 該当するscenarioType | 説明
--- | --- | --- | ---
body.validation.textValidation.textValidationInfo.operator | String  |  API, WEB, MODULE | 文字列演算子
body.validation.textValidation.textValidationInfo.expression | String  |  API、WEB、MODULE | 検証が必要な文字列
body.validation.textValidation.textValidationInfo.operand | String  |  API、WEB、MODULE |  期待値

<div id='responseValidation3'></div>
- responseValidation

値 | タイプ | 該当するscenarioType | 説明
--- | --- | --- | ---
body.validation.responseValidation.position | Integer | TCP、UDP | Responseで検証する文字列が始まる位置
body.validation.responseValidation.validationText | String | TCP、UDP | Responseで検証する文字列

## 登録されたシナリオ削除

[URL]
```http
DELETE /open-api/v1.0/appkey/{appKey}/scenarios/{ScenarioId}
Content-Type: application/json
```

[Request Header]

 ヘッダ名 | ヘッダ値
 --- | ---
 TOAST_PRODUCT_APPKEY | Service Monitoringサービス管理メニューで右上URL & Appkeyを押すと確認できるAppkey

[Path Variables]

| 値 |	タイプ | 必須か |	説明 |
|---|---|---|---|
| appKey | String | Required | サービスAppkey(**サービス管理**タブで確認可能) |
| scenarioId | String | Required | シナリオID |

#### レスポンス
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

値 | タイプ | 該当するscenarioType | 説明
--- | --- | --- | ---
header.isSuccessful | Boolean | - |成否
header.resultCode | Integer | - |失敗コード(0は正常)
header.resultMessage | String | - |失敗メッセージ
body.scenarioId | String | - | シナリオのID
body.url | String | API、WEB、MODULE | モニタリングを進行するAPIのURL
body.headers | Map&lt;String、String&gt; | API、WEB、MODULE | APIを送る時に使用するヘッダ値
body.httpMethod | String | API、WEB、MODULE | APIのhttpMethod
[body.validation](#validation4) | Object | - | シナリオの検証情報
body.requestBody | String | API、WEB、MODULE | APIのrequestBody
body.browserOption | Map&lt;String、String&gt; | API、WEB、MODULE | 
body.ip | String | - | モニタリングを進行する対象のIP
body.scenarioType | String | - | シナリオタイプ
body.scenarioName | String | - | シナリオ名
body.description | String | - | シナリオの説明
body.monitoringRegion | Set&lt;String&gt; | - | シナリオモニタリング地域
body.registeredTime | String | - | 登録時刻(yyyy-MM-dd'T'HH:mm:ss.SSSz)
body.amendedTime | String | - | 修正時刻(yyyy-MM-dd'T'HH:mm:ss.SSSz)
body.monitoringInterval | Integer | - | モニタリング間隔(秒単位)
body.monitoringCron | String | - | モニタリング間隔(Cron式)
body.status | String | - | シナリオの現在状態
body.errorLimitCount | Integer | - | 連続エラー許容回数
body.request | String | TCP、UDP | TCP、UDPリクエスト時のリクエスト文字列
body.port | Integer | TCP、UDP | TCP、UDPリクエスト時のポート番号

<div id='validation4'></div>
- validation

値 | タイプ | 該当するscenarioType | 説明
--- | --- | --- | ---
[body.validation.textValidation](#textValidation4) | Object  |  API, WEB, MODULE | 文字列検証情報
body.validation.timeout | Integer  |  - | タイムアウトしきい値
body.validation.responseCodes  | Set&lt;String&gt;  |  - | 許可されたresponseCode
body.validation.avoidingValidationText  | String  | API、WEB、MODULE | bodyに含まれる場合、配信を除く文字列
body.validation.imageValidationPaths | List&lt;String&gt; | API、WEB、MODULE | イメージ検証パス
[body.validation.responseValidation](#responseValidation4) | List&lt;Object&gt; | TCP、UDP | TCP、UDPリクエスト時のResoponse検証リスト
body.validation.lengthValidation | Map&lt;String、String&gt; | TCP、UDP | Responseの長さ検証

<div id='textValidation4'></div>
- textValidation

値 | タイプ | 該当するscenarioType | 説明
--- | --- | --- | ---
body.validation.textValidation.textValidationType  | String  |  API, WEB, MODULE | 文字列を検証する時、ベースになるbodyタイプ
[body.validation.textValidation.textValidationInfos](#textValidationInfo4) | List&lt;Object&gt;  | API, WEB, MODULE | 文字列検証情報

<div id='textValidationInfo4'></div>
- textValidationInfo

値 | タイプ | 該当するscenarioType | 説明
--- | --- | --- | ---
body.validation.textValidation.textValidationInfo.operator | String  |  API, WEB, MODULE | 文字列演算子
body.validation.textValidation.textValidationInfo.expression | String  |  API、WEB、MODULE | 検証が必要な文字列
body.validation.textValidation.textValidationInfo.operand | String  |  API、WEB、MODULE |  期待値

<div id='responseValidation4'></div>
- responseValidation

値 | タイプ | 該当するscenarioType | 説明
--- | --- | --- | ---
body.validation.responseValidation.position | Integer | TCP、UDP | Responseで検証する文字列が始まる位置
body.validation.responseValidation.validationText | String | TCP、UDP | Responseで検証する文字列

## 시나리오 수정

### 데이터 전송
- 서비스 모니터링 서버로 시나리오 수정을 요청할 때 필요한 데이터를 전송합니다.

[URL]
```http
POST /open-api/v1.0/appkey/{appKey}/scenarios/{scenarioId}
Content-Type: application/json
```

[Path Variables]

| 값 |	타입 | 필수 여부 |	설명 
|---|---|---|---|
| appKey | String | Required | 서비스 Appkey(**서비스 관리** 탭에서 확인 가능) |
| scenarioId | String | Required | 시나리오 ID(**시나리오 편집** 창에서 확인 가능) |

[Request Header]

 헤더 이름 | 헤더값
 --- | ---
 TOAST_PRODUCT_APPKEY | Service Monitoring **서비스 관리** 메뉴에서 오른쪽 상단 **URL & Appkey**를 클릭하면 확인 가능한 Appkey

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
url | String | API | http 또는 https로 시작하는 url | Y |  | 모니터링할 API의 URL
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
body.scenarioId | String | 시나리오 ID
body.url  |  String  |  모니터링할 API의 URL
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