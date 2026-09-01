<!-- pre-align:aligned sig=1a93f00c2c5a -->

<a id="monitoring-service-monitoring-release-notes"></a>
## Monitoring > Service Monitoring > リリースノート { #monitoring-service-monitoring-release-notes }

<a id="may-28-2024"></a>
### 2024. 05. 28. { #may-28-2024 }
* Service Monitoring サービスの表示位置がManagementからMonitoringカテゴリーに変更されました。

<a id="november-15-2022"></a>
### 2022. 11. 15. { #november-15-2022 }
* 障害発生時、配信チャンネルを通じて提供するアラームページURLのドメインがnh.nuに変更されました。

<a id="monitoring-service-monitoring-release-notes-november-15-2022"></a>
### 2022. 11. 15. { #monitoring-service-monitoring-release-notes-november-15-2022 }
* 障害発生時、配信チャンネルを通じて提供するアラームページURLのドメインがnh.nuに変更されました。

<a id="december-28-2021"></a>
### 2021. 12. 28. { #december-28-2021 }

<a id="december-28-2021-bug-fixes"></a>
#### バグ修正
* シナリオ名が31文字を超える場合、2つ以上のシナリオのヒストリーExcelをダウンロードできない問題を修正

<a id="july-27-2021"></a>
### 2021. 07. 27. { #july-27-2021 }

<a id="july-27-2021-feature-updates"></a>
#### 機能改善
* 新規ローディングバーの適用
* セキュリティ改善事項の適用

<a id="april-27-2021"></a>
### 2021. 04. 27. { #april-27-2021 }

<a id="april-27-2021-bug-fixes"></a>
#### バグ修正
* 特定のユーザーが含まれるプロジェクトで、配信対象者が照会できない問題を修正

<a id="march-23-2021"></a>
### 2021. 03. 23. { #march-23-2021 }

<a id="march-23-2021-bug-fixes"></a>
#### バグ修正
* 障害通知ページ検索条件に基づいて照会できない問題を修正
* 障害通知ページでリストを選択した時、満了案内が表示される問題を修正
* Webフックを編集した時、既存のHTTPヘッダが継続して表示される問題を修正
* ヒストリー検索を行った時、開始時間が含まれていない問題を修正

<a id="december-29-2020"></a>
### 2020. 12. 29. { #december-29-2020 }

<a id="december-29-2020-bug-fixes"></a>
#### バグ修正
* シナリオ管理オープンAPIのメソッド(POST、GET、PUT、DELETE)で、API呼び出しのresultCodeとresultMessageの表示が異なる問題を修正
* CloudTrailイベントが常にUSER_CONSOLEとして適用される問題を修正
* CloudTrail PM変更イベントがPM登録イベントとして登録される問題を修正

<a id="november-24-2020"></a>
### 2020. 11. 24. { #november-24-2020 }

<a id="november-24-2020-functions-added"></a>
#### 機能追加
* シナリオを管理するための[修正](/Monitoring/Service%20Monitoring/ja/api-guide/#_15)オープンApi機能を追加
* Webフックリクエストパラメータをサポート
* **LINE**のメッセージ送信Webフックテンプレートを追加

<a id="october-27-2020"></a>
### 2020. 10. 27. { #october-27-2020 }

<a id="october-27-2020-bug-fixes"></a>
#### バグ修正
* openAPIを活用してシナリオを作成する際、requestBodyのurlフィールドにハイフン(-)が含まれている場合に、シナリオが作成できない問題を修正
* バッチモニタリングシナリオを作成する時、検証情報を入力しなくても作成できた問題を修正
* Webフック情報を修正する時、リクエストデータの編集項目の内容が正常に表示されない問題を修正

<a id="september-22-2020"></a>
### 2020. 09. 22. { #september-22-2020 }

<a id="september-22-2020-more-features"></a>
#### 機能追加
* シナリオ管理用の[作成](/Monitoring/Service%20Monitoring/ja/api-guide/#_8)、[照会](/Monitoring/Service%20Monitoring/ja/api-guide/#_11)、[削除](/Monitoring/Service%20Monitoring/ja/api-guide/#_13) Open API機能を追加

<a id="august-25-2020"></a>
### 2020. 08. 25. { #august-25-2020 }

<a id="august-25-2020-bug-fixes"></a>
#### バグ修正
* バッチモニタリングシナリオを登録した後、すぐにAPIを呼び出した時、呼び出し結果が正常に反映されない問題を修正

<a id="july-28-2020"></a>
### 2020. 07. 28. { #july-28-2020 }

<a id="july-28-2020-feature-updates"></a>
#### 機能改善
* Webモニタリングの動作最小単位を変更
  * APIモニタリング：60秒 ---> 30秒
  * 仮想ブラウザ、モジュールモニタリング：120秒 ---> 60秒

<a id="july-28-2020-bug-fixes"></a>
#### バグ修正
* バッチモニタリング内容検証時、大文字/小文字を区別しない問題を修正

<a id="may-26-2020"></a>
### 2020. 05. 26. { #may-26-2020 }

<a id="may-26-2020-feature-updates"></a>
#### 機能改善
* Webモニタリングテストが30秒以上かかる場合、失敗する問題を修正
* Webモニタリングテキスト検証時、シナリオタイプ、レスポンスコンテンツタイプに応じて使用できる演算子を追加
  * API
    * レスポンスが_HTML_、_XML_の場合、contain、!containのみ使用可能
    * レスポンスが_JSON_の場合、_JsonPath_を活用して(==, !=, >, >=, <, <=)使用可能
  * Browser, Module
    * レスポンスが_HTML_、_XML_の場合、contain、!contain、_xPath_を活用して(==, !=, >, >=, <, <=)使用可能
    * レスポンスが_JSON_の場合、_JsonPath_を活用して(==, !=, >, >=, <, <=)使用可能
* TOAST CloudTrailサービス連携
  * Service Monitoringコンソールで発生したユーザーイベントをTOAST CloudTrailで確認可能
  
<a id="march-24-2020"></a>
### 2020. 03. 24. { #march-24-2020 }

<!-- TODO: translate body -->

<a id="march-24-2020-feature-updates"></a>
#### 機能改善

<!-- TODO: translate body -->

<a id="january-21-2020"></a>
### 2020. 01. 21. { #january-21-2020 }

<a id="january-21-2020-feature-updates"></a>
#### 機能改善
* Web/TCPモニタリングUSリージョンをサポート
* モニタリングヒストリー検索時、モニタリングリージョンオプションを追加

<a id="december-24-2019"></a>
### 2019. 12. 24. { #december-24-2019 }

<a id="december-24-2019-feature-updates"></a>
#### 機能改善
* 障害の配信に失敗した時、ヒストリー詳細履歴メッセージを改善
* Webページ性能が向上

<a id="december-24-2019-bug-fixes"></a>
#### バグ修正
* サービス配信担当者ではないユーザーに配信状況ページで配信履歴が表示されない問題を修正

<a id="november-26-2019"></a>
### 2019. 11. 26. { #november-26-2019 }

<a id="november-26-2019-feature-updates"></a>
#### 機能改善
* 配信状況ページで、配信状態情報を分離表示。
* ユーザー言語が英語の場合、一部のUIが崩れる現象を修正。
* Webモニタリングシナリオ編集機能を改善
  * テキスト検証時、演算子に応じてplaceholderが表示されるように改善。
  * [シナリオ検証タイプ]によってテキスト検証演算子表示を制限。
  * [シナリオ検証タイプ]がモジュールの場合にも**スクリプトエラー無視**、**イメージダウンロード除外**、**CSS有効化**オプションを使用できるように改善。

<a id="november-26-2019-bug-fixes"></a>
#### バグ修正
* [障害配信キャンセル案内]メールの送信時、各メール内のキャンセルしたユーザー名ではなく受信者名が伝達される問題を修正。


<a id="august-27-2019"></a>
### 2019. 08. 27. { #august-27-2019 }

<a id="august-27-2019-feature-updates"></a>
#### 機能改善
* 日本語のフォントをTOAST共通のフォントに変更しました。
* TCPモニタリングシナリオ編集画面で、IP、PORT入力ラベルを分離して可読性を向上させました。

<a id="august-27-2019-bug-fixes"></a>
#### バグの修正
* 配信詳細表示画面で、時間が正常に表示されない問題を修正しました。
* モニタリング障害登録の際、配信タイトルがシナリオ保存時に設定された言語で登録されるように修正しました。
* 障害配信メッセージ内の障害通知ページ接続時に、有効期限切れではないのにかかわらず、有効期限切れと表示される現象を修正しました。
* 日本語の障害配信メッセージのタイトルが誤って追加される問題を修正しました。

<a id="july-23-2019"></a>
### 2019. 07. 23. { #july-23-2019 }


<a id="july-23-2019-new-service-release"></a>
#### 新規サービスリリース
Service Monitoringは、安定的にサービスを運営するためのサービス障害管理プラットフォームです。 
	* サービスごとに配信担当者、配信チャンネルを管理
	* 多様なモニタリング方式をサポート。Webモニタリング、TCPモニタリング、バッチモニタリング 
