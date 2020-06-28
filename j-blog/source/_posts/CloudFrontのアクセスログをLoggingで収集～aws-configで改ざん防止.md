---
title: CloudFrontのアクセスログをLoggingで収集～aws configで改ざん防止
date: 2020-06-24 21:00:46
categories:
- Serverless Application Dev
- AWS
tags: 
- AWS
- CloudFront
- aws config
- S3
- Logging
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/85531367-ab3d2380-b649-11ea-8efe-cd98bb73dc5f.png
---

CloudFrontのLoggin機能を有効化して、任意のS3バケットに格納するよう設定します。
だれが何時何をしたのか？確認可能にすることで、後の監査対策となります。
今回の手法はAWSのベストプラクティスの一つなので覚えておきましょう（資格の試験で良く出ます）

<!-- toc -->

## 今回の構成
## CloudFrontのLogging機能
![ログ記録の仕組み](https://user-images.githubusercontent.com/41946222/85531367-ab3d2380-b649-11ea-8efe-cd98bb73dc5f.png)

- ClouFrontの設定からLoggingを有効化するだけで、上記の図のように、S3にアクセスログファイルを自動で格納するように設定可能です。CloudFrontを用いるのであれば、基本的に使う機能として覚えてください。

### 前提条件
- CloudFrontコントリビューションを生成済み
    - 未実施であれば、以下の記事をご参照ください
        - [s3 ssl化 https化(CloudFront/ACM/Route53)](/s3-ssl化-https化/) 
        - [[AWS WAF] CloudFrontへアクセス可能なソースIPを社内イントラに制限](/AWS-WAF-CroudFrontへアクセス可能なソースIPを社内イントラに制限/)
        - [AWS CloudFront~S3のアクセス制御まとめ/署名付きURL](/AWS-CroudFrontのアクセス制御まとめ-署名付きURL/)
        - [[AWS S3 x Angular] 静的WEBサイトホスティングでSPAを公開/ng build/公開範囲の限定/CI/CD化](/AWS-S3-x-Angular-静的WEBサイトホスティングでSPAを公開-公開範囲の限定/)    

## 実装手順
### Log格納用のs3 bucketを生成/Policy設定
- aws console/s3
- バケットを作成する
  - 任意の名称を付ける

![バケットの作成](https://user-images.githubusercontent.com/41946222/85527641-040abd00-b646-11ea-8216-c77ea309e464.png)


### 権限設定
- LogをためるにはCloudFrontからのアクセス許可が必要
- 必要な権限
  - s3:GetBucketAcl
  - s3:PutBucketAcl

- s3/任意のbucket/アクセス権限タブ/アクセスコントロール
  - S3 ログ配信グループを選択
  - オブジェクトへのアクセス
    - 以下をチェック
      - オブジェクトの一覧
      - オブジェクトの書き込み
    - 保存
  - ロックパブリックアクセスを無効化

- バケット用のs3 ACLでFULL_CONTROLを付与する必要もあります。






```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "MakeLogs",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity E2TINTLAJ7TK2U"
            },
            "Action": "*",
            "Resource": "arn:aws:s3:::reacq-bucket-for-logs/*"
        }
    ]
}
```







- その他のS3 Bucketのプロパティ
  - オブジェクトレベルのログ記録
    - 追加料金がかかっても良ければ、CroudTrailデータイベント機能を使用して記録可能

### ログ削除時にSNSトピックにより通知させる
S3 bucket内のログの削除イベントを元に、Amazon SNSトピックを利用して危険通知を自動で出せるように設定します

- SNSトピックを作成する
  - SNSトピックを利用することで設定した宛先に通知を送ることができます
  - 手順
    - AWS Console/Amazon Simple Notification Service
    - トピックの作成
      - トピック名
        - DeleteAlert
      - 次のステップ
      - 表示名
        - Log-Delete-Alert
      - 他の設定はスルーでOK
        - ※AWSを利用する際の原則として、タグにOwnerとPJ名程度は付けておきましょう
      - トピックの作成
    - トピックができたらARNを控えます
      - ARNはAWSのリソースを一意にに藩閥するための番号
      - 他のサービスと連携する時に必要になります
    - 生成したトピックを選択
    - ”サブスクリプションの作成”
      - プロトコル
        - 今回はEメールを選択
        - 例えばSMSで電話番号を設定するば、よくあるWEBサービスのSMS通知を簡単に実現できます
      - エンドポイント
        - 今回はTeamsの開発チームのチャンネルに流したいので、そのメールアドレスを設定（Teamsはチャンネル毎にメールアドレスを取得できます）
      - サブスクリプションの作成を押下
    - 以上でこのSNSトピックが呼び出された際に、サブスクリプションで規定した連絡先に通知がと届くようになります

![Amazon SNS サブスクリプション](https://user-images.githubusercontent.com/41946222/85717764-cfbbfd00-b728-11ea-8762-276babecb850.png)

- S3側でイベントを規定
  - S3 Bucket/プロパティタブ
  - イベント
    - ログの削除を検知できるようにイベントを作成します
      - 名前
        - DeleteLog
      - イベント
        - 全てのオブジェクトの削除イベント
      - プレフィックス/サフィックスはスルーでOK
      - 送信先
        - SNSトピック
      - SNSトピックのARN
        - 事前に作成しておいたSNSトピックのARNをコピペ



### Loggingの有効化
- aws console/CloudFront
- 任意のディストリビューションを選択して、GeneralタブのEditを押下
- Edit Distribution
  - Loggingをonに変更
  - Bucket for Logs
    - 事前に作成したbucketを選択
  - Yes, Edit

![Edit Distribution](https://user-images.githubusercontent.com/41946222/85528394-c6f2fa80-b646-11ea-8db0-4a0360861dfa.png)

### ログ記録を確認
- S3バケットを確認するとログの記録が開始されたことを確認できます



### Cloud Trail


### aws configで改ざんを防止
S3にせっかくログを貯めても、削除されてしまっては無意味であるため、改ざん防止策が必須となります。

- 改ざん対策
    - aws configでログの格納先であるs3 bucketをトラッキング
    - ログデータの削除/編集を検知

### 料金
- アクセスログの料金
    - つまりS3の料金対策だけでOK
```
アクセスログの作成は、CloudFront のオプション機能です。アクセスログの作成を有効にしても追加料金はかかりません。ただし、Amazon S3 でのファイルの保存とアクセスについて通常の Amazon S3 料金が発生します (ファイルの削除はいつでもできます)。
```

- 対策例
    - ログを非常事態以外は見ない場合
        - Glacier等の廉価版を利用
    - 直近のログは素早く確認したい場合
        - S3のライフサイクルポリシーを利用して、一定期間の経過後にGlacierに移行させる





## 参考
### 関連記事
- [[AWS WAF] CloudFrontへアクセス可能なソースIPを社内イントラに制限](/AWS-WAF-CroudFrontへアクセス可能なソースIPを社内イントラに制限/)
- [AWS CloudFront~S3のアクセス制御まとめ/署名付きURL](/AWS-CroudFrontのアクセス制御まとめ-署名付きURL/)
- [s3 ssl化 https化(CloudFront/ACM/Route53)](/s3-ssl化-https化/) 
- [[AWS S3 x Angular] 静的WEBサイトホスティングでSPAを公開/ng build/公開範囲の限定/CI/CD化](/AWS-S3-x-Angular-静的WEBサイトホスティングでSPAを公開-公開範囲の限定/)
### その他
- [CloudFront開発者ガイド/アクセスログの設定および使用](https://docs.aws.amazon.com/ja_jp/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html)

- [CloudFrontのアクセスログ設定方法](https://oji-cloud.net/2019/08/16/post-2740/)


- [CloudFront開発者ガイド/Amazon CloudFront のログ記録とモニタリング](https://docs.aws.amazon.com/ja_jp/AmazonCloudFront/latest/DeveloperGuide/logging-and-monitoring.html)



- [CloudFront開発者ガイド/Amazon CloudWatch による CloudFront のモニタリング](https://docs.aws.amazon.com/ja_jp/AmazonCloudFront/latest/DeveloperGuide/monitoring-using-cloudwatch.html)
- [CloudFront開発者ガイド/AWS CloudTrail を使用して CloudFront API に送信されたリクエストをキャプチャする](https://docs.aws.amazon.com/ja_jp/AmazonCloudFront/latest/DeveloperGuide/logging_using_cloudtrail.html)