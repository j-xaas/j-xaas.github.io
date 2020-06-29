---
title: '[AWS CloudTrail] 証跡/内部監査用のログを貯める'
date: 2020-06-30 00:37:10
categories:
- Serverless Application Dev
- AWS
tags: 
- AWS
- CloudTrail
- Logging
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/86027113-416ab280-ba6b-11ea-977f-90edbc38dc22.png
---

　監査対策としてCloudTrailでログを取得する方式を解説します。

<!-- toc -->

## CloudTrailとは？
![CloudTrail](https://user-images.githubusercontent.com/41946222/86027012-1c763f80-ba6b-11ea-9996-fb27b5c38e2a.png)

- 単一のAWS アカウントにおける、全APIリクエストを記録するサービス
  - 公式説明
  ```
  CloudTrail を使用すると、AWS アカウントのイベントを表示できます。これらのイベントの記録を保持するには、証跡を作成します。証跡により、イベントメトリクスを作成し、アラートをトリガーして、イベントワークフローを作成することもできます。 また、AWS Organizations のマスターアカウントでログインすると、組織の証跡を作成することができます。
  ```
  - 一つのAPではなく、アカウント全体の話
  - 外向きよりも、内向きの監視が目的
    - AWSアカウントを共有する内部のIAMユーザの悪意を持った行動を抑えることができる
    - 外部からのリクエストも記録できる
- デフォルト
  - 90日間の記録は初めから有効
  - 無料
  - 永続的に保持する際には有料となる
- ”証跡情報の作成”
  - S3
  - 本番稼働でなければ、ひとまず対処しなくてもOK？
- CloudTrail Insights
  - 異常検知機能
    - 公式説明
    ```
    CloudTrailが異常なアクティビティを検出した場合、Insightsイベントはトレイルの送信先のS3バケットに配信されます。また、CloudTrailコンソールでInsightsイベントを表示すると、インサイトの種類やインシデント期間を確認することができます。CloudTrailのトレイルでキャプチャされる他のタイプのイベントとは異なり、インサイトイベントは、アカウントの通常の使用パターンとは大きく異なるアカウントのAPI使用状況の変化をCloudTrailが検出した場合にのみログに記録されます。
    ```
  - 有料

- Organizationのマスターアカウントを使えば、マルチアカウント運用でも組織の証跡を作成可能
  - 監査チームとの連携に活用


## CloudTrailで内部監査用のログを貯める
デフォルトでは90日間の情報しか確認出来ないため、証跡を作成してS3 bucketに貯める必要がある。

- AWS Console/CloudTrail
- ダッシュボード/”証跡の作成”を押下
![CloudTrail](https://user-images.githubusercontent.com/41946222/85975263-89a9b680-ba12-11ea-90ae-efc82c19fd05.png)

### 証跡の作成
- 使い方は2種類ありそう
  - アカウント統一の証跡を作成
  - サービス毎に証跡を分ける
    - 複数の認跡の作成では、追加コストが発生
  
リージョン毎に一つまでの証跡を作成しても無料であり、S3のみの課金となる。
CloudWatch側でサービス毎のログは取るので、CloudTrailはアカウント統一の証跡を作成することにする

![CloudTrail 証跡の作成](https://user-images.githubusercontent.com/41946222/85976260-1ce3eb80-ba15-11ea-8f80-fe4e7fb86a21.png)

- 証跡情報の作成
  - 証跡名
    - アカウント名-logsで設定してみる
  - 証跡情報を全てのリージョンに適応
    - はい    
  - 管理イベント
    - そのまま
  - Insightsイベント
    - いいえ
      - 追加料金を避けるため
      - CloudWatchからのAlartで充分だと考える
  - データイベント
    - S3, Lambda共に全てでOK
  - ストレージの場所
    - 新しいS3バケットを作成しますか？
      - はい
    - S3バケット
      - 適当に命名
        - bucket-for-cloudtrail-logs
  - 詳細
    - ログファイルの検証
      - CloudTrailからログファイルを送信後に、編集/削除/変更がないか確認できる
      - Cloud Configが必要だと考えていたが、利用せずとも改ざん対策できた
  - タグ
    - Own
      - 証跡の作成者だけ登録
  - 作成

以上で証跡とS3 Bucketが生成される。内部監査対策は基本的にこれだけでOK。

