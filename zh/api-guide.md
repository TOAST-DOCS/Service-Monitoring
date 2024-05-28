## Monitoring > Service Monitoring > API 가이드

### 기본 정보
```
API Endpoint: https://api-service-monitoring.cloud.toast.com
```

## 배치 모니터링

### 데이터 전송
- 배치 모니터링 서버로 검증이 필요한 데이터를 전송합니다.
- 배치 모니터링에 입력한 검증 정보에 따른 JSON 타입의 데이터를 전송할 수 있으며, 배치 모니터링의 검증에 실패할 경우 장애로 등록됩니다.

[URL]
```
POST /v1.0/monitoring/batchmon/appkey/{appKey}/scenarios/{scenarioId}
Content-Type: application/json
```

[Path Variables]

| 값 |	타입 | 필수 여부 |	설명 |
|---|---|---|--|
| appKey | String | Required | 서비스 앱키(서비스 관리 탭에서 확인 가능) |
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
| body.status | String | 요청 상태 |