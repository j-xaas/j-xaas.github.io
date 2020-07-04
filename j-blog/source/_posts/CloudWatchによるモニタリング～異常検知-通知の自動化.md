---
title: CloudWatchまとめ モニタリング～異常検知/通知の自動化(SNS連携)/コスト管理
date: 2020-07-02 01:24:11
categories:
- Serverless Application Dev
- AWS
tags: 
- AWS
- Amazon CloudWatch
- Amazon SNS
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/86269649-03a19180-bc05-11ea-881c-c5250dc63e6b.png
---

AWSの一般的なサーバレス構成における、以下の手法を解説します
- 運用状況をCloudWatch Dashboardで可視化
- CloudWatch Alarm メトリクスの異常値を自動検知
- Amazon SNS連携による自動通知
- コスト超過でSNS通知

今回想定するアーキテクチャ
![静的コンテンツをS3でHostingする際の一般的なアーキテクチャ](https://user-images.githubusercontent.com/41946222/86269988-80347000-bc05-11ea-8a3d-752ef61f5ca7.png)


<!-- toc -->

## 必要な情報をCloudWatchに集める
- 今回のターゲット
    - ユーザがコンテンツを利用する際に接する、以下のリソースの利用状況
        - CloudFront
        - WAF

### Dashboardを作成
- Console/CloudWatch/Dashboard
- ダッシュボードの作成
  - 名称を設定(今回PJ名)
- ウィジェットの作成
  - タイプを選択
  - CloudFront
    - 欲しい項目を選択
    ![image](https://user-images.githubusercontent.com/41946222/85984207-d47ffa00-ba23-11ea-8b18-48ffcbef98e7.png)
    - ウィジェットの作成

- 次にWAFのエラー＝ソースIP外から試みたアクセスを可視化する
  - ウィジェットの追加
  - タイプを選択
  - AWS/WAFV2/Rule, WebAClから必要な項目を選択
  ![CloudWatchメトリクスの追加](https://user-images.githubusercontent.com/41946222/85984745-91725680-ba24-11ea-8bdc-e96362ad1fdd.png)
  - ウィジェットの作成

- 以下のようにウィジェットが表示される
![CloudFront Dashboard](https://user-images.githubusercontent.com/41946222/85985519-cd59eb80-ba25-11ea-827e-75b72cac871a.png)

- 分かり辛いので名称を変更
![CloudFront Dashboard](https://user-images.githubusercontent.com/41946222/85985224-60deec80-ba25-11ea-85e1-6d3334624e5e.png)

- ダッシュボードの保存


#### 各サービスのコンソールから設定する場合 
- 例：CloudFrontのメトリクスをダッシュボードに追加
    - aws console/CloudFront
    - 左のメニューバー/Monitoringを選択
    - 作成済みのディストリビューションのラジオボタンを選択
    - View distribution metricsを押下
    - ダッシュボードに追加を押下

----------------------------
## その他
### アラームを設定
- WAFで弾かれるリクエストが異常発生した際にSNSで通知が来るように設定
  - アラーム/アラームの作成
  - メトリクスの選択
    - AWS/WAFV2/Rule, WebACL
    - BlockedRequestを選択
  - メトリクスと条件の指定
    - 5分間に50以上弾かれていれば攻撃を受けている可能性があるのでアラートを出す
    ![CloudWatchアラームの作成](https://user-images.githubusercontent.com/41946222/85989561-aacad100-ba2b-11ea-8c85-9ad80f59b4e4.png)
    - 次へ
  - アクションの設定
    - 新しいトピックの作成
      - 通知を受け取るEメールエンドポイントに、開発チームのTeamsチャンネルのメールアドレスを指定
      - トピックの追加を押下
        - この時点でテストメールが発信されます
        - ”Confirm Subscription”と書かれたリンクを押すと有効化される
    - 次へ
  - 名前と説明を追加
  ![名前と説明を追加](https://user-images.githubusercontent.com/41946222/85990149-89b6b000-ba2c-11ea-8186-34038fc3dbeb.png)
  - 次へ
  - 問題なければアラームの作成

設定したいアラーム毎に以上の作業を行えばOK

--------

### コスト追跡 
- CloudWatchの請求アラーム
  - これはアカウント全体に対する請求のアラームであり、PJ毎にコスト監視することはできない
  - PJ毎にTagでフィルタリングして監視するにBudgetsで監視する


---------------------------
### おまけ：CloudWatch Logs
- CloudWatch Logs
  - EC2等からエージェント経由でログを集めるサービスなので今回は不要だが、ログをExportする手法を以下にメモとして残しておきます
  - 有料にはなるが、CloudTrailの証跡へのアクセスをLogsで取り込んで、改竄発生時にアラートを出すことも可能

#### ログの取得まで
- CloudWatch Logs エージェントをEC2にインストール
- ロググループを作成~サブスクライブ設定

#### S3にログをエキスポート
- 事前にログ格納用のS3バケットを作成

### Export設定
- 任意のロググループ/アクション/データをAmazon S3にエクスポート
![CloudWatch Export](https://user-images.githubusercontent.com/41946222/85986025-8caea200-ba26-11ea-9862-36d153f37562.png)

- データを Amazon S3 にエクスポート
  - 開始と終了を選択
  - S3バケットを選択
    - 事前に作成が必須
  - エクスポート

![CloudWatch Export](https://user-images.githubusercontent.com/41946222/85986375-1cece700-ba27-11ea-8f32-65652f66d3aa.png)


## 参考
### 関連記事
- [[AWS WAF] CloudFrontへアクセス可能なソースIPを社内イントラに制限](/AWS-WAF-CroudFrontへアクセス可能なソースIPを社内イントラに制限/)
- [AWS CloudFront~S3のアクセス制御まとめ/署名付きURL](/AWS-CroudFrontのアクセス制御まとめ-署名付きURL/)
- [s3 ssl化 https化(CloudFront/ACM/Route53)](/s3-ssl化-https化/) 
- [[AWS S3 x Angular] 静的WEBサイトホスティングでSPAを公開/ng build/公開範囲の限定/CI/CD化](/AWS-S3-x-Angular-静的WEBサイトホスティングでSPAを公開-公開範囲の限定/)

### その他
- [CloudFront開発者ガイド/Amazon CloudWatch による CloudFront のモニタリング](https://docs.aws.amazon.com/ja_jp/AmazonCloudFront/latest/DeveloperGuide/monitoring-using-cloudwatch.html)
- [Amazon S3 へのログデータのエクスポート](https://docs.aws.amazon.com/ja_jp/AmazonCloudWatch/latest/logs/S3Export.html)


