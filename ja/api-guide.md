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
