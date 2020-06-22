---
title: '[AWS WAF] CloudFrontへアクセス可能なソースIPを社内イントラに制限'
date: 2020-06-22 21:31:04
categories:
- Serverless Application Dev
- AWS
tags: 
- AWS
- CloudFront
- WAF
- S3
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/85305995-d2301400-b4e8-11ea-8cb8-3d265b176dd4.png
---

AWS WAFを用いて、CloudFrontへのアクセスを制御する手法を解説します。CloudFrontディストリビューションの生成やS3へのコンテンツのHostingは事前に完了している前提とします。それらの手順は以下の記事にまとめています。

- 参考記事
    - [[AWS S3 x Angular] 静的WEBサイトホスティングでSPAを公開/ng build/公開範囲の限定/CI/CD化](/AWS-S3-x-Angular-静的WEBサイトホスティングでSPAを公開-公開範囲の限定/)
        - S3でコンテンツを公開するまで
    - [s3 ssl化 https化(CloudFront/ACM/Route53)](/s3-ssl化-https化/)
        - s3とCloudFrontを連携させるまで
    - [AWS CloudFront~S3のアクセス制御まとめ/署名付きURL](/AWS-CloudFrontのアクセス制御まとめ-署名付きURL/)
        - S3へのアクセスをCloudFront経由に限定

<!-- toc -->

### 前提とする構成
![Serverless Architecture](https://user-images.githubusercontent.com/41946222/85308292-00fbb980-b4ec-11ea-885a-51ff1a5867d6.png)

S3単体であれば、バケットポリシーによるソースIP制限も可能ですが、CloudFrontを挟む場合は、アクセス元となるCloudForntがブロックされてしまいます。そこで、S3がアクセスを許可する対象をCloudFrontに絞り、CloudFront側でIP制限をかける必要があり、AWS WAFを利用することで実現可能です。

### AWS WAF(Web Application Firewall)
![AWS WAF](https://user-images.githubusercontent.com/41946222/85305995-d2301400-b4e8-11ea-8cb8-3d265b176dd4.png)

- [AWS WAF](https://aws.amazon.com/jp/waf/)
    - 名前の通り、Webアプリケーションを守るファイアウォールです
    - 主な機能
        1. SQL インジェクションやクロスサイトスクリプティングなどの一般的な攻撃パターンの防御
            - AWSにおける基本的な防御策の一つであり、CloudFrontとWAFをコンテンツ～ユーザ間に挟むのが定石です
            - 更に守りを固めるのであればAWS Shieldも付けるイメージ
        2. 定義した特定のトラフィックパターンを除外するルールを作成して、トラフィックがアプリケーションに到達する方法を制御
            - 今回利用するのはこちら

## 手順
WAFで以下のトラフィックパターンを実現します

- 社内イントラ(複数IP)からのアクセスのみを許可
- その他をブロック

### IP set設定
まず、アクセス可能なIPアドレスをリスト化します

- IP Sets
  - Create IPsetを選択

![IP Sets](https://user-images.githubusercontent.com/41946222/85279270-1efff480-b4c1-11ea-8c65-494dcae6bde6.png)


- Crate IP set
  - ip set name
    - 今回は会社のイントラネットのIPを登録するので”intranet”に設定
  - IP addressesに許可したいIPアドレスを列挙
  - Create IP set

![Create IP set](https://user-images.githubusercontent.com/41946222/85279530-86b63f80-b4c1-11ea-87ef-3a7eb0aec11e.png)


### WEB ACL設定
次に設定したIP setからのアクセスのみに絞るルールを作ります

- WEB ACLs/Create web ACLを選択

![WEB ACLs](https://user-images.githubusercontent.com/41946222/85280180-98e4ad80-b4c2-11ea-8b87-1e51aef758b6.png)


- WEB ACL設定
  - Resource type
    - CloudFront distributionsを指定
  - 他は任意の名称を設定
    - 今回はip-limit-acl

![Describe web ACL and associate it to AWS resources](https://user-images.githubusercontent.com/41946222/85277658-b44db980-b4be-11ea-9d35-646aef940484.png)

- Add rules and rule groups
  - ここでIPをルールとして設定します
  - Add my own rules and rule groupsを選択

![Add rules and rule groups](https://user-images.githubusercontent.com/41946222/85278623-25da3780-b4c0-11ea-8b94-3996b62f4e08.png)

- Add my own rules and rule groups
  - Rule type
    - IP setを選択
  - Rule
    - 任意の名称を設定
  - IP Set
    - 事前に作ったものを選択
  - Action
    - Allow
  - Add rule 

![Add my own rules and rule groups](https://user-images.githubusercontent.com/41946222/85280469-27f1c580-b4c3-11ea-8095-844ab2f93544.png)


- Default web ACL action for requests that don't match any rules
  - Blockに設定しておく

![Add rules and rule groups](https://user-images.githubusercontent.com/41946222/85280671-8e76e380-b4c3-11ea-9729-84885304a559.png)
  
- デフォルトのブロックと任意のIP setにアクセスを許可するルールが設定されたことを確認してNext

- Set rule priority
  - そのままでNext

- Configure metrics
  - そのままでNext

- Review and create web ACL
  - 設定に問題がなければCreate web ACL

### CloudFrontディストリビューションに適応
- WEBACL完成後にCloudFrontに適応
  - Associated AWS resourcesタブ
  - Add AWS resources

![Associated AWS resources](https://user-images.githubusercontent.com/41946222/85286358-70ae7c00-b4cd-11ea-96d1-fd933ff27469.png)

- 作成済みのCroudFrontディストリビューションを選択
  - Add

![Add AWS Resources](https://user-images.githubusercontent.com/41946222/85286603-e4e91f80-b4cd-11ea-89b8-09ac6c82ebec.png)


以上でOK。試しにCloudFrontのURLにアクセスすると、許可したIP以外からはブロックされます。



## 参考
### 関連記事
- [[AWS S3 x Angular] 静的WEBサイトホスティングでSPAを公開/ng build/公開範囲の限定/CI/CD化](/AWS-S3-x-Angular-静的WEBサイトホスティングでSPAを公開-公開範囲の限定/)
- [s3 ssl化 https化(CloudFront/ACM/Route53)](/s3-ssl化-https化/)
    - s3とCloudFrontを連携させるまで
- [AWS CloudFront~S3のアクセス制御まとめ/署名付きURL](/AWS-CloudFrontのアクセス制御まとめ-署名付きURL/)
    - S3へのアクセスをCloudFront経由に限定

### その他
- [AWS WAFを完全に理解する ~WAFの基礎からv2の変更点まで~](https://dev.classmethod.jp/articles/fully-understood-aws-waf-v2/)

