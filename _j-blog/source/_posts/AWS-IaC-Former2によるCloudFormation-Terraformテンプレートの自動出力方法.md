---
title: '[AWS/IaC] Former2によるCloudFormation/Terraformテンプレートの自動出力方法'
date: 2021-03-11 21:40:13
categories:
- Serverless Application Dev
- AWS
tags: 
- Angular
- CloudFormation
- Former2
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/110789693-82ca8780-82b3-11eb-931a-88629a8cfdd0.png
---

AWSアカウント内の既存のリソースから、IaCテンプレート(CloudFormation/Terraform)を自動で作成するツールの活用方法と手動で改修すべき範囲についてのメモ

<!-- toc -->

## 1. Former2の概要
![Former2](https://github.com/iann0036/former2/raw/master/img/screen1.png)

- [Former2](https://github.com/iann0036/former2)
  - ブラウザで動く[Webアプリ](https://former2.com/)
  - サードパーティ製のツール
  ```
  Former2では、AWSアカウント内の既存のリソースからInfrastructure-as-Codeの出力を生成することができます。AWS JavaScript SDKを使用して関連するコールを行うことで、Former2はインフラストラクチャをスキャンし、出力を生成するリソースのリストを表示します。
  ```
  - CFnテンプレートだけでなく、Terraform等も出力可能
    - CloudFormation
    - Terraform
    - Troposphere
    - CDK (Cfn Primitives) - TypeScript, Python, Java, C#
    - CDK for Terraform - TypeScript
    - Pulumi - TypeScript
    - Diagram - embedded version of draw.io
  - CloudFormerと比較して高機能で2021年現在主流になっている
  - 価格はOSSのため無料
  ```
  Former2はローカルでのアクセスや利用は無料ですが、一部のAWSサービスではAPIコールに関連して少額の料金が発生するため、AWSの利用料金に数セント余分に加算される可能性があります。
  ```
  - 出力したCFn Templateを`汎用的にするには少し手直しが必要であった`
    - ex. 固定値のパラメータを入力値を参照するように変更、formerでテンプレートに取り込めない細かいリソースの追記
  - 使用方法は二つ
    - Webアプリとして使用
      - https://former2.com/
    - セキュリティの関係でローカルホスティングしたい場合は、自分で動作環境を準備してそこで動かば良い

## 2. (Former2動作環境の起動)
- Former2は[こちら](https://former2.com/)でブラウザから利用可能だが、セキュリティの制約が厳しいPJの場合は、自分でgitからformer2をpullしてローカル環境で動作させることもできる

- Former2 ローカル動作環境の構築例
  - EC2インスタンスを生成/IPを付与
  - EC2でgitからFormer2をpull
  - EC2のパブリックIPからFormer2を利用

- 参考
  - https://qiita.com/miriwo/items/8d5b35950232c1126d36
  - https://qiita.com/jey0taka/items/237f8d62c6c171975960
  - [Former2の起動](https://www.inomaso.com/post/2020/12/former2-terraform-import/)

## 3. Former2 Setup
- Former2にアクセス
  - https://former2.com/ or 用意したローカル環境のIP

### 3.1. Introduction
![Former2 Introduction](https://user-images.githubusercontent.com/41946222/110564345-fd9a8200-818f-11eb-80cf-3f836d8e19ce.png)

- ブラウザにプラグインを導入
  - Install Former2 Helper for Google Chrome
  ![Former2 Google Chrome Plugin](https://user-images.githubusercontent.com/41946222/110561640-63383f80-818b-11eb-9123-37f3a142b9a8.png)

- Continue to Credentialsを押下

### 3.2. Credentials
![Former2 Credential](https://user-images.githubusercontent.com/41946222/110564433-2458b880-8190-11eb-9477-1ff984092a82.png)

事前にReadOnlyAccessを付与したIAM UserとAccess Key/Secret Access Keyの用意が必要

- IAM UserのCredentialを入力
  - Access Key ID
  - Secret Access Key
- Credentialを正しく認識できたら、以下が表示される
```
Logged in as: <IAM User Name>@<AWS Account>
```

- 公式説明
```
リクエストを認証するには、IAM キーのペアが必要です。リソースを直接インポートすることを計画していない場合は、これらの資格情報で読み取りアクセスのみを提供し、
ReadOnlyAccessポリシーを割り当てることをお勧めします。インポート機能を使用する予定がある場合は、スタックを作成するために適切な権限を付与する必要があります。
```

- Continue to Parametersを押下

### 3.3. Parameter
![Former2 Parameter](https://user-images.githubusercontent.com/41946222/110565025-27a07400-8191-11eb-8ad9-a201fe65a2b8.png)

- 独自のCloudFormation Parameterを設定する事ができる
  - 特になければスルー

```
オプションとして、以下にCloudFormationスタックのパラメータを追加することで、独自のCloudFormationスタックパラメータを含めることができます。デフォルト値が設定されている場合、値が一致していれば、出力でこれらのパラメータを参照するために !Ref または !Sub を使用することができます。
```

- Continue to Settingsを押下

### 3.4. Settings

- CFnテンプレートを出力するだけであればデフォルトでもOK
  - 設定後にGo to Dashboardを押下

- CloudFormation Spacing
  - 出力のスペース数を変更
- Logical ID Strategy
  - 論理ID名の付け方を設定
- Default Output
  - 出力言語の設定
  - デフォルトではCloudFormation
  - ここで、TerraformやCDK, Draw.ioのDiagram等に変更可能

- Irrelevant Resources
```
有効にすると、この設定は無関係とみなされるリソースをスキップします（現在はCloudWatchログストリームのみ）
```
- Enable Related Resources
  - 関連リソースを有効化
- Add All Resources
```
以下のボタンを使用して、スキャンしたすべてのリソースを出力に追加します（非推奨）
```
- Save / Load Settings
```
すべての設定されたパラメータと設定（資格情報を除く）を含むファイルを保存またはロードします。
```
- Programming Language 
```
CDKやPulumiの出力プログラミング言語の好みを変更してください。
```
- Default Resources
```
この設定を有効にすると、デフォルトのVPCやそのサブネットなどのデフォルトのリソースが含まれます。
```

以上でセットアップは完了

## 4. Former2によるテンプレートの出力
テンプレートに含めたいリソースを選択していく

- Former2のDashboardからサービスを選択

![Former2 Dashboard](https://user-images.githubusercontent.com/41946222/110568664-ae0b8480-8196-11eb-8177-595694632576.png)


- 表示されたオブジェクトから、テンプレート化したいリソースを選択
- +Add Selected

- 関連するリソースがタブでまとまっており、直感的に操作できる
![Former2 Select Objects](https://user-images.githubusercontent.com/41946222/110569353-be702f00-8197-11eb-9075-08cfb131bbee.png)

- Generate Template
  - オブジェクトの追加完了後に、Generate
  - CFnテンプレートが出力される
  - オブジェクト追加時に確認出来なかった項目も反映されていた
    - ex. tag

![generate template](https://user-images.githubusercontent.com/41946222/110570187-e0b67c80-8198-11eb-8b36-25155a293006.png)


## 5. Former2で出力されるテンプレートについて
- Former2では、一部CFnテンプレートに含まれない情報を見受けられた
- また、各Propertyが固定された状態で出力されてしまうため、汎用的なテンプレートにするには入力値(Properties)の設定が必要になる

### 例とするPF構成
- 以下のPF構成をformer2でテンプレート化したケースを考える
  - S3 Bucket
    - アプリをホスティング
  - CloudFront Distribution
    - S3 Bucketをソースとしてアプリを配信
  - AWS WAF
    - CloudFrontにアタッチ
    - アクセスを制限

### 出力したCFn Temlateに含まれない情報/注意点

- s3 bucket
  - LifecycleConfiguration
    - Rules
      - Id指定のみで、ルール自体はCFn Templateに取り込めていなかった
      - 別アカウントでテンプレートを実行する場合は同名のルールが無いためエラーになってしまうと思われる
      - ライフサイクル未指定であれば無関係

- WAFに関する注意点
  - CloudFront用に設定したWAFのリソースは`Global(CloudFront)として扱われる`
  - `former2では画面右上のボタンでリージョンを選択するが、Globalが無い`
  - 調査したところ、`リージョンをUS East(N. Virginia)に設定することで確認可能`であった
    - `US East(N. Virginia)`で生成される仕様になっていため、former2の画面右上のボタンでリージョンを変更するだけで良い

### 汎用的なテンプレートにするには
- `Parametersで各固定値を自由に設定可能な入力値に変更する`
  - CFn Template実行時の入力値としてユーザが設定可能なパラメータを規定できる
    - テンプレートが汎用的に利用可能になり、別Projectやアカウントでも使い回し可能になる
  - 各パラメータには以下を定めることで利便性を高めることができる
    - Description
      - 説明を定義
    - 初期値とバリデーションも定義できる
      - 参考となるような名称を予め定めておくことで、Templateの利便性が高まる
  - 記載例
  ```
  Parameters: ＃ 入力値を定義 
    BucketName: 
        # CFn実行時に入力欄に表示されるユーザ向けの説明
        Description: Please write bucket name 
        # デフォルト値
        Default: sample-bucket # 
        # 入力値のタイプ
        Type: String
    XXXXName: 
      ...
  ```

- 各リソースのPropertiesの各項目から入力値(Parameters)を参照する
  - !RefでParametesで定めた入力値を参照できる
  - 入力値の規定/参照例
  ```
  Parameters: ＃ 入力値を定義 
    BucketName: 
        # CFn実行時に入力欄に表示されるユーザ向けの説明
        Description: Please write bucket name 
        # デフォルト値
        Default: sample-bucket # 
        # 入力値のタイプ
        Type: String
  ...
  Resources: # 生成するResourceを定義
    S3Bucket:
        Type: "AWS::S3::Bucket"
        Properties:
            BucketName: !Ref BucketName # 入力値を参照
  ```

- 入力値の参照にはRefを用いる
  - 指定した論理IDのパラメータやリソースを参照する
```
!Ref <PropertyName>
```
[CloudFormationRef](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-ref.html)
- Subを使うと変数と文字列を組み合わせられる？   
  - [Cloudformation Fn::Subを使って文字列内に変数をいれる](https://chariosan.com/2019/08/11/cfn_fnsub/)
- 

- !Subを活用すると、入力値と固定値を組み合わせることができる
  - 名称の一貫性や、入力の手間の省力化を見込める
  - 書式
    ```
    !Sub "${PJName}-Bucket"
    !Sub "${PJName}-Policy"
    ...
    ```

## 所感
- Former2で出力したテンプレートの活用方法
  - そのまま固定値で使用しても構わないが、他のPJでも使いまわせる汎用的なテンプレートにするには、paramaterで変数化する必要がある。CFnの記法については最低限知識が必要

- CFnに精通したメンバがいない場合は、以下の流れでIaC化を図ると良さそう
  1. 0から無理にCFnテンプレを作らず、コンソールやCLIで検証しながらPFを構築
    - 初級者が0からCFnテンプレを書くと細かい内容でハマってしまう(実体験)
  2. ある程度定まってからFormer2で固定値のテンプレートを出力
  3. 固定値を変数化
  4. 以降はCFnテンプレートとGitでPFを管理していく

- テンプレートの分割について
  - 大規模なPFの場合はテンプレートを分割して、他のテンプレートと入れ子構造にした方が良い
    - Former2でホスティング環境/IAM関連/運用監視関係と分割してテンプレートを出力することで、管理が円滑になる
  - 分割テンプレートを連携させるには、パラメータの受け渡しなどの手間が増えるため、CFnの知識がある程度必要


## 参考
### 関連記事
- [Angular x AWS SDK for JavaScriptの始め方](/Angular-x-AWS-SDK-for-JavaScriptの始め方/)

- [[Angular JavaScript] JSONデータのファイル化と出力 ~取得したデータを任意の名称で保存するロジック~ (TypeScript)](/Angular-JSONデータのファイル化と出力-クラウドから取得したデータを任意の名称で保存する/)
- [[Angular JavaScript] JSONファイル(複数)の読み込み](/Angular-JavaScript-JSONファイルの読み込み/)
- [静的ホスティングサービスまとめ(Netlify/S3/Firebase Hosting/Github Pages)](/静的ホスティングサービスまとめ-Netlify-S3-Firebase-Hosting-Github-Pages/)
- [[Angular ReactiveForm x mat-input] 一つの入力欄のエラーメッセージを出し分ける](/Angular-ReactiveForm-x-mat-input-一つの入力欄のエラーメッセージを出し分ける/)
- [[Angular mat-input] バリデーションまとめ](/Angular-mat-input-バリデーションまとめ/)
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)

### その他
- [AWS EC2 AmazonLinux2 Gitをインストールする](https://qiita.com/miriwo/items/8d5b35950232c1126d36)
- [Docker, Docker Composeのインストール](https://qiita.com/jey0taka/items/237f8d62c6c171975960)
- [[Former2][Terraform] AWS既存環境を楽にコード化したい](https://www.inomaso.com/post/2020/12/former2-terraform-import/)

- [開発者ガイド/CloudFormer (ベータ) を使用して既存の AWS リソースから AWS CloudFormation テンプレートを作成する](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/cfn-using-cloudformer.html)
