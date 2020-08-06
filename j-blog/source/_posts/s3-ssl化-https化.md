---
title: s3 ssl化 https化(CloudFront/ACM/Route53)
date: 2020-06-17 13:15:02
categories:
- Serverless Application Dev
- AWS
tags: 
- AWS
- S3
- CloudFront
- Route53
- ACM
- CDN
- Angular
- 静的WEB Hosting
- SSL
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/84855780-ebc3fc80-b09f-11ea-921c-d413fa17627b.png
---

　S3の静的WEBホスティングで公開中のアプリのURLはデフォルトではhttp~になります。Cloud Frontを用いてhttps~にしてセキュリティを向上させます。

- 静的WEBホスティングで公開するまでは以下の記事をご参照ください
    - [[AWS S3 x Angular] 静的WEBサイトホスティングでSPAを公開/ng build/公開範囲の限定/CI/CD化](/AWS-S3-x-Angular-静的WEBサイトホスティングでSPAを公開-公開範囲の限定/)



## 基礎知識

### 静的コンテンツ配信を行うAWSアーキテクチャのベストプラクティス
![architecture](https://user-images.githubusercontent.com/41946222/84855780-ebc3fc80-b09f-11ea-921c-d413fa17627b.png)

- S3でコンテンツをHostingして、ユーザーとの間にCloud FrontとRoute53を挟むのが基本です
    - それぞれの説明は後述します

### SSL
- [SSL(Secure Sockets Layer)](https://jp.globalsign.com/ssl-pki-info/ssl_beginner/aboutssl.html)とは
    - データ通信を暗号化し、盗聴や改ざんを防ぐプロトコル
    - httpsから始まるWEBサイトはSSLを導入しています
    - 非SSL(http)の場合、Googleがデフォルトで警告マークを出すので、サイトやWEB APの信頼度が大幅に下がります

### Cloud Front
- CDN
    - 配信パフォーマンスの向上
    - コスト削減
- セキュリティの向上

### Route53


## 手順
- 想定する状況
    - AWS Consoleで設定
    - CloudFormationやSAMで作るケースもあると思います

### Cloud FrontとS3を連携
- まずは以下の構成を作ります
![CloudFront x s3](https://user-images.githubusercontent.com/41946222/84862620-714ea900-b0ae-11ea-9900-40c1bbc7b7c3.png)


- AWS Console/サービス/CloudFront

- Create Distributionを押下
![Cloud Front Menu](https://user-images.githubusercontent.com/41946222/84858364-f97c8080-b0a5-11ea-92a5-2cc13d7be57b.png)


- 配信方法はWEBを選択
![Delivery Method](https://user-images.githubusercontent.com/41946222/84858521-537d4600-b0a6-11ea-9caf-83774afab46b.png)


#### ディストリビューションの作成
- Origin Settings
    - Origin Domain Name
        - Hosting中のs3バケットを選択
    - Restrict Bucket Access
        - YesにするとCloudFront経由でのみ閲覧可能になる
        - Grant Read Permissions on Bucket
            - これを設定しておくとS3のバケット側の設定を自動でやってくれる
- Default Cache Behavior Settings
    - そのままでもOK
    - Viewer Protocol Policy
        - Redirect HTTP to HTTPS
            - これにしておくとhttpからhttpsへリダイレクトさせてくれる
- Distribution Settings
    - Alternate Domain Name(CNAMEs)
        - 証明書を取得したドメイン名を設定
            - 複数のドメイン名を使用する場合は、改行区切り
        - ドメインを取得していなければスルー
    - SSL Certificate
        - Defaultの証明書でOK
        - 独自SSLを使用する場合
            - 事前にIAMに証明書をアップロードしておく
    - Default Root Object
        - S3の静的Hostingの設定で指定した、インデックスドキュメント(Angularであればindex.html)を入力
    - Create Distributionを押下            

- 20分程度でStatusがDeployedになるので、完了後にアクセスしてみる
- URL
    - XXXX以降はディストリビューションのGeneralタブのDamain Nameに表示されているものを確認
    ```
    https://xxxxx.cloudfront.net
    ```
    ![image](https://user-images.githubusercontent.com/41946222/84861034-2d0dd980-b0ab-11ea-8042-0688711d2207.png)


- Route53や独自ドメインを利用しないのであれば以上で完了


#### CloudFrontの注意点：キャッシュコントロール

- デフォルト
    - オリジンサーバのコンテンツを 24 時間キャッシュ
    - 検証中など頻繁にS3のオブジェクト内容を更新する際にはキャッシュがききすぎて辛い。
- Cache-Controlヘッダーを設定してCloudFrontのキャッシュ時間を制御する等、キャッシュコントロールを適切に行う必要がある。
    - [ヘッダーを使用した個々のオブジェクトのキャッシュ保持期間の制御](https://docs.aws.amazon.com/ja_jp/AmazonCloudFront/latest/DeveloperGuide/Expiration.html#expiration-individual-objects)

-----------------------------------

## 独自ドメインを活用する場合
- 前提
    - 独自ドメインを取得してRoute53に登録済み
    - これから取得する人はお名前.com等を使ってみてください

### ACM(AWS Certificate Manager)で証明書を作成
- AWS Console/AWS Certificate Manager/証明書のリクエスト

- ドメイン名を入力
    - ”この証明書に別の名前を追加”よりドメインの別名を設定

![ACM](https://user-images.githubusercontent.com/41946222/84869260-c2639a80-b0b8-11ea-825b-fe0b932fb7d1.png)


- 対象ドメインに登録されているメールアドレス宛に検証確認メールが送付される
- 検証確認メールに記載されているhttps://us-east-1.certificates.amazon.com/approvals...にアクセス
- 表示された画面で" I Approve"を押下
    - 以上で証明書のリクエストが完了します


![requst](https://user-images.githubusercontent.com/41946222/84869629-4027a600-b0b9-11ea-9241-6a2253ed448e.png)

- CloudFrontにACMで作成した証明書を設定
- Ditribution/Generalタブ/Edit
- SSL Certificate
    - Custom SSL Certificateを選択
    - プルダウンから先ほどACMに登録した証明書を選択
- 保存

### Route53の独自ドメインのエイリアスの向き先を変更する

- AWS Console/サービス/Route53
- ホストゾーンから登録済みのドメインを選択

- 対象の行を選択
    - タイプ：Aのもの
- レコードセットの編集
    - エイリアス先を変更
        - [CloudFront ディストリビューション] - 対象のドメイン名 を選択
- レコードセットを保存


- 確認
    - 以下にアクセスしてみる
    ```
    https://{独自ドメイン} 
    ```


## 参考
- [AWSにおける静的コンテンツ配信パターンカタログ（アンチパターン含む）](https://dev.classmethod.jp/articles/static-contents-delivery-patterns/)

- [S3を最速でhttps化する手順と全設定項目](https://qiita.com/horiuchie/items/fa47ae921f9aac237689)
- [CloudFrontでS3のウェブサイトをSSL化する](https://qiita.com/jasbulilit/items/73d70a01a5d3b520450f)
- [AWSでWebサイトをHTTPS化 その9：S3のみ編](https://recipe.kc-cloud.jp/archives/11489)