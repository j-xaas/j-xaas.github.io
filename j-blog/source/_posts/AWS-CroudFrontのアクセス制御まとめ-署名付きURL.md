---
title: AWS CroudFront~S3のアクセス制御まとめ/署名付きURL
date: 2020-06-17 16:51:29
categories:
- Serverless Application Dev
- AWS
tags: 
- AWS
- CroudFront
- S3
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/84862620-714ea900-b0ae-11ea-9900-40c1bbc7b7c3.png
---

- AWSのCloudFront x S3の構成におけるアクセス制御の手法について解説します
    - ソースIPの絞り方
    - コンテンツへのアクセスを経路をCloudFrontに限定

## CroudFrontのアクセス制限
![CroudFront Image](https://user-images.githubusercontent.com/41946222/84876574-54bc6c00-b0c2-11ea-83e9-d3a43a5629ff.png)

- 2ルートを考える必要があります
    - ①CroudFrontにキャッシュされたコンテンツへのアクセス
        - 署名付き URL または署名付き Cookie の使用が求められるように CloudFront を設定可能です。
        - 以下の制限を指定
            - 最終日時。この日時以降、URLが無効化
                - 解約したユーザーずっとアクセス可能な状態を防げます
            - (オプション) URL が有効になる日時。
            - (オプション) コンテンツへのアクセスに使用可能なコンピュータの IP アドレスまたはアドレス範囲

    - ②オリジンへの直接のアクセス
        - CroudFrontを介した通信に限定する事が可能です
        - オリジン
            - S3やEC2等のコンテンツを実際にHostingしている場所のこと

- 詳細
    - [プライベートコンテンツ供給の概要](https://docs.aws.amazon.com/ja_jp/AmazonCloudFront/latest/DeveloperGuide/private-content-overview.html)



### CroudFront経由以外のS3バケットへのアクセスをシャットアウトする

- CroudFrontの大きな役割の一つが攻撃者とコンテンツの間の緩衝材となることです
    - 直接S3へアクセス可能な状態では上記の効果を生めないため、基本的に遮断したほうが良いです

#### Distributionの作成時に設定
- Origin Settings
    - Restrict Bucket Access
        - Yesに設定 ⇒ CloudFront経由でのみ閲覧可能に絞る
        - Grant Read Permissions on Bucket
            - Yesに設定 ⇒ S3バケットのPolicyを自動で変更


#### Distribution作成後に設定
- AWS Console/CroudFront
- 対象のDistributionを選択
- Origins and Origin Groupsタブ
    - 対象のOrigin Domainを選択肢てEditを押下

![Origins and Origin Groups](https://user-images.githubusercontent.com/41946222/84873012-8ed73f00-b0bd-11ea-8e85-53ae14beec95.png)

- Restrict Bucket Access
    - Yesに変更
        - Grant Read Permissions on Bucket
            - Yesに設定 ⇒ S3バケットのPolicyを自動で変更
- Yes, Edit

![Edit Origin](https://user-images.githubusercontent.com/41946222/84875056-6bfa5a00-b0c0-11ea-9bf1-c167dfb1442b.png)


- バケットポリシーの変化を確認
    - 以下が自動的に追記されていた
```
        {
            "Sid": "2",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity XXXXXXXXXXX"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::XXXXXXXXXX/*"
        }
```

- 注意点
    - 元々書いていたポリシーに追記される形式になるため、どちらもEffectで書かれていれば二通りの通信を許可することになります


## CloudFront エッジキャッシュ内のファイルへのアクセス制限

- [署名付き URL の使用](https://docs.aws.amazon.com/ja_jp/AmazonCloudFront/latest/DeveloperGuide/private-content-signed-urls.html)

### 署名付きURLでソースIPを限定する


- [カスタムポリシーを使用して署名付きURLを作成する](https://docs.aws.amazon.com/ja_jp/AmazonCloudFront/latest/DeveloperGuide/private-content-creating-signed-url-custom-policy.html)ことでIPを制限できます


- (オプション) コンテンツへのアクセスに使用可能なコンピュータの IP アドレスまたはアドレス範囲。


- ポリシーステートメントの例
    - 任意のIPアドレスからのみディレクトリ内の全ファイルにアクセス可能

```
{ 
   "Statement": [
      { 
         "Resource":"http://d111111abcdef8.cloudfront.net/training/*", 
         "Condition":{ 
            "IpAddress":{"AWS:SourceIp":"192.0.2.0/24"}, 
            "DateLessThan":{"AWS:EpochTime":1357034400}
         }
      }
   ] 
}
```



## S3 Bucket Policyの記載例
アクセス制限内容
- ソースIP
    - SourceIpで定めた二点のみ
- アクセス経路
    - Principalで定めたCroudFrontディストリビューション経由のみ
- アクション
    - GetObject＝読み込みのみ

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity XXXXXXXX"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::xxxxxxxxxx/*",
            "Condition": {
                "IpAddress": {
                    "aws:SourceIp": [
                        "XXX.XXX.XX.X/24",
                        "XXX.XXX.XX.X/24"
                    ]
                }
            }
        }
    ]
}
```



## 参考

- [カスタムポリシーを使用する署名付き URL のポリシーステートメントの例](https://docs.aws.amazon.com/ja_jp/AmazonCloudFront/latest/DeveloperGuide/private-content-creating-signed-url-custom-policy.html#private-content-custom-policy-statement-examples)