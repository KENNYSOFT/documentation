---
categories:
- モニタリング
- network
- cloud
dependencies: []
description: Salesforce のリアルタイムプラットフォームイベントを Datadog ログとして収集します。
doc_link: https://docs.datadoghq.com/integrations/salesforce/
draft: false
git_integration_title: salesforce
has_logo: true
integration_id: ''
integration_title: セールスフォース・ドットコム
integration_version: ''
is_public: true
kind: integration
manifest_version: '1.0'
name: salesforce
public_title: セールスフォース・ドットコム
short_description: Salesforce のリアルタイムプラットフォームイベントを Datadog ログとして収集します。
team: web-integrations
version: '1.0'
---

## 概要

Salesforce は顧客関係管理サービスに加えて、カスタマーサービス、マーケティングオートメーション、アナリティクス、アプリケーション開発に特化したエンタープライズ向けアプリケーションスイートを無料で提供しています。

Salesforce と Datadog の統合により次のことが行えます:

- [Datadog ログ管理][1]を使用してログを表示およびパースする。
- Salesforce プラットフォームからの[イベント][3]に[モニター][2]を設定する。
- Datadogの[セキュリティプラットフォーム][4]を活用して、Salesforce プラットフォームに対する脅威を監視および検知する。
- Salesforce API の使用量を監視し、API の制限以下で運用を行っていることを確認する。

## セットアップ

### インストール

インストールは必要ありません。

### コンフィギュレーション

Datadog にデータを送信するよう Salesforce を構成するには、[Salesforce イベントモニタリング][5]にアクセスし、Salesforce イベント上のストレージを有効化して Salesforce の組織を Datadog に接続する必要があります。

#### アクセス許可

[Salesforce Shield][6] を使用している場合、すべてのイベントに対して必要な権限が設定されています。Shield をご利用でない場合は、[イベントモニタリングアドオン][7]が必要です。

#### イベントストレージを有効化する

プラットフォームまたはリアルタイムイベントの使用を計画している場合、イベントマネージャーでこれを設定する必要があります。イベントログファイルイベントの場合は、このステップは必要ありません。

1. (Lightning インターフェースを使用して) Salesforce のアカウントに[ログイン][8]します。
2. **Event Manager** を検索します。
3. Event Manager ページで、クロールしたい各イベントについて右矢印をクリックして **Enable Storage** を選択します。

| 名前                                     | サブスクリプションチャンネル             | タイプ             | 説明                                                                                          | データのストリーミング | データの保存   |
|------------------------------------------|----------------------------------|------------------|------------------------------------------------------------------------------------------------------|----------------|----------------|
| API Anomaly Event                        | /event/ApiAnomalyEvent           | イベントモニタリング | ユーザーが API コールを行う際の異常を追跡                                                          | {{< X >}}            | {{< X >}}            |
| API Event                                | /event/ApiEventStream            | イベントモニタリング | 組織内のユーザー API クエリを追跡                                                                   | {{< X >}}            | {{< X >}}            |
| Bulk API Result Event                    | /event/BulkApiResultEvent        | イベントモニタリング | ユーザーが Bulk API リクエストの結果をいつダウンロードしたかを追跡                                        | {{< X >}}            | {{< X >}}            |
| Concurrent Long Running Apex Error Event | /event/ConcurLongRunApexErrEvent | イベントモニタリング | Apex の並列長時間実行エラーの発生状況を追跡                                         | {{< X >}}            | 該当なし |
| Credential Stuffing Event                | /event/CredentialStuffingEvent   | イベントモニタリング | クレデンシャルスタッフィング攻撃が確認された際に、ユーザーが Salesforce へのログインに成功した際の追跡 | {{< X >}}            | {{< X >}}            |
| Identify Provider Event                  | 該当なし                   | イベントモニタリング | ID プロバイダーの活動を追跡                                                                   | 該当なし | {{< X >}}            |
| Identify Verification Event              | 該当なし                   | イベントモニタリング | ユーザーが本人確認を行った際の追跡                                                               | 該当なし | {{< X >}}            |
| Lightning URI Event                      | /event/LightningUriEventStream   | イベントモニタリング | ユーザーが Salesforce Lightning でレコードを作成、アクセス、更新、削除したときの追跡            | {{< X >}}            | {{< X >}}            |
| List View Event                          | /event/ListViewEventStream       | イベントモニタリング | リストビューでユーザーがデータにアクセスした際の追跡                                                      | {{< X >}}            | {{< X >}}            |
| Login Event                              | /event/LoginEventStream          | イベントモニタリング | ユーザーが組織にログインしたときの追跡                                                                | {{< X >}}            | {{< X >}}            |
| LoginAs Event                            | /event/LoginAsEventStream        | イベントモニタリング | 管理者が別のユーザーとして組織にログインしたときの追跡                                              | {{< X >}}            | {{< X >}}            |
| Logout Event                             | /event/LogoutEventStream         | イベントモニタリング | Salesforce UI でユーザーがログアウトをクリックしたときの追跡                                                | {{< X >}}            | {{< X >}}            |
| Permission Set Event (ベータ版)              | /event/PermissionSetEvent        | イベントモニタリング | 全データの変更または全データの閲覧の権限がユーザーに割り当てられた際の追跡                        | {{< X >}}            | {{< X >}}            |
| Report Anomaly Event                     | /event/ReportAnomalyEvent        | イベントモニタリング | ユーザーがレポートを実行したり、エクスポートしたりする際の異常の追跡                                                   | {{< X >}}            | {{< X >}}            |
| Report Event                             | /event/ReportEventStream         | イベントモニタリング | ユーザーがレポートでデータにアクセス、またはデータをエクスポートした際の追跡                                              | {{< X >}}            | {{< X >}}            |
| Session Hijacking Event                  | /event/SessionHijackingEvent     | イベントモニタリング | 盗難セッションを使用して、不正ユーザーが Salesforce ユーザーのセッションの所有権を獲得した際の追跡 | {{< X >}}            | {{< X >}}            |
| URI Event                                | /event/UriEventStream            | イベントモニタリング | ユーザーが Salesforce Classic でレコードを作成、アクセス、更新、削除したときの追跡              | {{< X >}}            | {{< X >}}            |

#### 組織を接続する

1. Salesforce の組織で、固有のシステムアカウントを作成します。
2. [Salesforce インテグレーションタイル][9]の ** Configuration** タブにある **New Production Org** または **New Sandbox Org** をクリックします。
3. これらのイベントに付けたいカスタムタグを、カンマ区切りのリストとして設定します。有効にするイベントを選択することができます。

    {{< img src="integrations/salesforce/salesforce-1.png" alt="Datadog 上で Salesforce org の構成が完了した際の成功画面" popup="true" style="width:90%" >}}

4. **Save** をクリックします。これにより、Salesforce アカウントにログインし、Datadog のアクセス権限を付与するよう促されます。
5. ログインフローが完了したら、Datadog の [Salesforce インテグレーションタイル][9]に戻ります。組織には、すぐに使えるデフォルトのタグが含まれています。

    {{< img src="integrations/salesforce/salesforce-default-tags.png" alt="Datadog 上で Salesforce org の構成が完了した際の成功画面" popup="true" style="width:90%" >}}

6. 使用するタグを選択し、**Connect** をクリックします。
7. この手順を繰り返して、残りの組織を接続します。追加しようとする組織へのアクセス権が必要です。

**注**: Salesforce org ID にデフォルトタグが追加されますが、[このタグ][10]は企業にとってより意味の通りやすいものになるよう編集することができます。

#### 結果

しばらくすると、`salesforce` のソース下に[ログ][1]が表示されます。イベントログファイルベースのイベントが表示されるまでには、最大で 1 時間かかる場合があります。

## 収集データ

### メトリクス
{{< get-metrics-from-git "salesforce" >}}


### イベント

ログイベントの一覧は、[リアルタイムイベント監視データ保存][12]と [EventLogFile イベント][13]を参照してください。

### サービスのチェック

Salesforce インテグレーションには、サービスのチェック機能は含まれません。

## トラブルシューティング

Configuration タブで `The authenticated connection does not have access` エラーが発生した場合、要求されたイベントにアクセスするための権限が欠落している可能性があります。Salesforce の Datadog ロールの管理者権限を一時的に有効にして、不足しているアクセス権限を確認することができます。

ご不明な点は、[Datadog のサポートチーム][14]までお問合せください。

[1]: /ja/logs/
[2]: /ja/monitors/monitor_types/
[3]: /ja/events/
[4]: /ja/security_platform/
[5]: https://trailhead.salesforce.com/content/learn/modules/event_monitoring
[6]: https://www.salesforce.com/editions-pricing/platform/shield
[7]: https://help.salesforce.com/s/articleView?id=000339868&type=1
[8]: https://login.salesforce.com/
[9]: https://app.datadoghq.com/account/settings#integrations/salesforce
[10]: /ja/getting_started/tagging/using_tags/
[11]: https://github.com/DataDog/dogweb/blob/prod/integration/salesforce/salesforce_metadata.csv
[12]: https://developer.salesforce.com/docs/atlas.en-us.securityImplGuide.meta/securityImplGuide/real_time_event_monitoring_storage.htm#storage-events
[13]: https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_eventlogfile_supportedeventtypes.htm
[14]: https://docs.datadoghq.com/ja/help/