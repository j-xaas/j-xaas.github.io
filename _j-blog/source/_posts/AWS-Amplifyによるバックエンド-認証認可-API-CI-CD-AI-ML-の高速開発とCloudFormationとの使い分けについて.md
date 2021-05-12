---
title: 'AWS Amplifyによるバックエンド(認証認可,API,CI/CD,AI/ML)の高速開発とCloudFormationとの使い分けについて'
date: 2021-05-12 16:50:18
categories:
- Serverless Application Dev
- AWS
tags: 
- AWS
- AWS Amplify
- AppSync
- DynamoDB
- Recognition
- Cognito
- Serverless
- GraphQL
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/117929143-67254100-b337-11eb-8d6d-60a767aa18d9.png
---

- WEB/Mobileアプリ開発のフレームワークである[AWS Amplify](https://aws.amazon.com/jp/amplify/)について、実開発で学んだことや、以下の2021 AWS Summitのセッション/デモの個人的まとめ
    - 2021 AWS Summit Online 
        - [Web・モバイルアプリ開発を加速させる AWS Amplify](https://summits-japan.virtual.awsevents.com/media/Web%E3%83%BB%E3%83%A2%E3%83%90%E3%82%A4%E3%83%AB%E3%82%A2%E3%83%97%E3%83%AA%E9%96%8B%E7%99%BA%E3%82%92%E5%8A%A0%E9%80%9F%E3%81%95%E3%81%9B%E3%82%8B%20AWS%20Amplify(AWS-47)/1_2arm4e2m)
        - [Amplify x AppSyncを使ったフルスタックアプリケーション](https://summits-japan.virtual.awsevents.com/media/AWS%20Amplify%20%E3%81%A8%20AWS%20AppSync%20%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%9F%E3%83%95%E3%83%AB%E3%82%B9%E3%82%BF%E3%83%83%E3%82%AF%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%AE%E9%96%8B%E7%99%BA(AWS-26)/1_mp6wwyzf)
        - [AWS Amplify と Amazon Rekognitionを使って施設の混雑状況をモニタリングするアプリを作ってみよう !](https://summits-japan.virtual.awsevents.com/media/AWS%20Amplify%20%E3%81%A8%20Amazon%20Rekognition%20%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6%E6%96%BD%E8%A8%AD%E3%81%AE%E6%B7%B7%E9%9B%91%E7%8A%B6%E6%B3%81%E3%82%92%E3%83%A2%E3%83%8B%E3%82%BF%E3%83%AA%E3%83%B3%E3%82%B0%E3%81%99%E3%82%8B%E3%82%A2%E3%83%97%E3%83%AA%E3%82%92%E4%BD%9C%E3%81%A3%E3%81%A6%E3%81%BF%E3%82%88%E3%81%86%20!/1_275jxzi3/207423013)
- AWS Amplifyを活用すれば、コマンドでバックエンドや連携部分を自動構築して開発の超高速化を図ることができる

<!-- toc -->

## AWS Amplify 概要
![AWS Amplify](https://user-images.githubusercontent.com/68212997/117931793-97221380-b33a-11eb-93ff-471993898112.png)

- AWSを用いたWEB/Mobileアプリを高速にリリースするための開発プラットフォーム
    - 競合はGCPのFirebase
    - どちらも利用経験があるが、Amplifyの方ができることが多く、連携部分も自動構築できるのが強みだと感じている
    ```
    Amplify を使用するお客様は、数分の内にバックエンドを構成しアプリケーションと接続でき、
    また、静的なウェブアプリケーションのデプロイは数クリックだけで実行できます。
    さらに、AWS コンソールの外部でも、簡単にアプリケーションコンテンツの管理が行えます。
    ```

- 目的：`差別化につながらない重労働の排除`
    - 認証周りのようなバックエンド構築やアプリへの組み込みをショートカットできる
        - 特にサーバレス構成に適している
    - バックエンドをCLIから対話形式で手軽に自動構築
        - AWSのベストプラクティスが反映された、フルマネージドなサービスを数分で即活用できる

- Amplifyで提供されるもの
![Amplify Console](https://user-images.githubusercontent.com/68212997/117932020-dd777280-b33a-11eb-82fa-37306bf70aef.png)
1. Amplify CLI
    ```
    Web/Mobile APのバックエンドをインタラクティブに
    作成・管理するためのOSSツールチェーン
    ```
    - やりたいことから宣言的にバックエンドを構築できる
        - 自動構築言えばCFnもあるが、Amplifyは`対話形式`で`細かい設定を決めずに`べスプラに沿った構成を瞬時に構築できる

2. Amplify ライブラリ
    ```
    Web/Mobile APとAWSを統合するためのOSSライブラリ
    ```
    - AWSのバックエンドとの連携に利用。SDKと比較して短く直感的にかける
3. Amplify Console
    ```
    フルスタックサーバレスWebアプリをビルド、テスト、デプロイ、
    ホスティングするためのOSSライブラリ
    ```
4. Amplify Admin UI
    ```
    Web/Mobile APのバックエンドとコンテンツを管理するための
    GUIツール
    ```
    - ユーザーとコンテンツの管理
    - GUIを用いたデータモデリング
        - GraphQLスキーマを自動生成できる
        - シンプルなデータモデルで完結するAPのバックエンドを手軽に開発できる
        - Admin UI (GUI)で作成されたデータをpullすることも可能

### ユースケース
![Amplify Usecase](https://user-images.githubusercontent.com/68212997/117932288-22030e00-b33b-11eb-9b1e-b417283782f8.png)


### AmplifyはCloudFormationと競合するか？
- Amplifyを活用する前に分かっていなかったポイント
    - CloudFormationでPFを管理する場合、Amplifyが邪魔になってしまう？

- 競合しなかった
    - Amplifyの構成もCFnテンプレートに取り込める
    - PF構成がパラメータレベルで定まっていれば最初からCFnを、試行錯誤しながら進めていくのであればまずはAmplifyが適している
    - 個人的には以下の開発フローが望ましいと感じた
        - 初期はAmplifyで高速開発＆検証　→　PF構成が確定　→　Former2などを活用してCFnテンプレート化　→　Gitで差分管理　→　他環境やPJに同一構成を展開

- 新規開発（Serverless or PF構成未確定）
    - Amplifyによる自動構築が適する
    - 認証周りやCI/CD, モニタリング、DB, Serverlessサービスなどをコマンドで俊敏に構築していく

- 新規開発（PF構成が確定済、サーバレスや認証周りを使わない）
    - こういったシンプルなケースは、はじめからCloudFormationで構築してOK
    - 無理にAmplifyを使う必要はない

- 開発が進み、PF構成が確定
    - ここでCFnテンプレート化を図れば良い
    - Former2を活用して実環境をCFnテンプレートに落とし込む
    - 固定値のテンプレートを汎用化（変数化など）する
    - GitでPF構成の差分管理を実施

- CloudFormtionとは用途が異なる
    - CFnはPFの管理、再現性の確保を重点においている
    - Amplifyはどちらかというと、`俊敏性`/高速開発に重点が置かれている
        - `差別化を図れない重労働の削減が目的`
        - CFnと異なり、細かい設定を規定しなくともAWSのベストプラクティスに即したPFを作れる
            - 使ったことがないサービスをいきなりCFnテンプレに起こすのはしんどい
        - 個人的にはスタートダッシュを図る為のものというイメージ

---
## 利用手順
### Amplifyによる自動構築の流れ  
基本的に以下の順序でバックエンドを数分で自動構築できる

1. Amplify CLIのインストール＆設定
2. PJの作成、Amplify初期化コマンドを実行
3. API追加コマンドを実行
4. デプロイコマンドの実行 

### Amplify CLI インストール/設定
- インストール
```
npm install -g @aws-amplify/cli  
```
- 設定
    - 以下のコマンドを実行するとブラウザにAWS Consoleのログイン画面ができる
```
> amplify configure
Follow these steps to set up access to your AWS account:

Sign in to your AWS administrator account:
https://console.aws.amazon.com/
Press Enter to continue
```

- 立ち上がったブラウザでログインを実施
- CLI上でEnter
- 次にデフォルトリージョンを選択する
    - 東京であれば ap-north-east-1にカーソルを合わせてEnter
```
Specify the AWS Region
? region:  (Use arrow keys)
❯ us-east-1 
  us-east-2 
  us-west-2 
  eu-west-1 
  eu-west-2 
  eu-central-1 
  ap-northeast-1 
(Move up and down to reveal more choices)
```

- Amplifyが使うIAM Userを設定
    - これは新規に作成するユーザー
```
Specify the username of the new IAM user:
? user name: 
```

- ブラウザでAWS Consoleのユーザーを追加画面が立ち上がる
    - ここでIAM Userを作成する
    - 別のIAMがあれば、プログラムによるアクセスのみでOK
    - 権限はデフォでAdmin, AdministoratorAccess-Amplifyがおすすめ

- ユーザーが生成されたら、アクセスキー/シークレットキーを控えておく
- CLIに戻りEnter
- accessKeyId/secretAccessKeyを入力
```
Enter the access key of the newly created user:
? accessKeyId:  ********************
? secretAccessKey:  ****************************************
```
- profile名はデフォルトでOK
```
This would update/create the AWS Profile in your local machine
? Profile Name:  (default) 
```
- 以下が表示されれば完了
```
Successfully set up the new user.
```

---
#### アプリケーションのプロジェクトにAmplifyを導入する

- アプリのProjectのトップディレクトリに移動
- コマンドで初期化処理を実行
    - いくつか質問される。基本defaultでOK
    ```
    amplify init
    ```
- amplifyフォルダが生成される
    - この中にバックエンドに関する設定ファイルが入る
    - つまり、テキストベースで設定変更が可能。Gitで差分管理もできる

---
### Amplify CLIによるバックエンドの構築
- 基本的に以下のコマンドの後ろにカテゴリ名を入れて構築する
```
amplify add XXXX
```

---
### 認証周りの自動構築
![Amazon Cognito](https://user-images.githubusercontent.com/68212997/117950896-9abf9580-b34e-11eb-951b-495274cbe6f8.png)

- [Amazon Cognito](https://aws.amazon.com/jp/cognito/)
    - 標準的な認証基板に求められる機能とAPIを提供。認証トークンの発行もできる

- AmplifyではUIの構築済みコンポーネントも用意されている
    - 構築をコマンドだけで済ませ、認証のフォームも数行書くだけ済む
        - 数分で認証周りの実装が完了する
    - クロスプラットフォームにも対応

- Cognitoの自動構築
    - 以下のコマンドを実行して、Defaultの設定でEnterを押していくだけで、Cognitoのバックエンドの設定が完了する
    ```
    amplify add auth
    ```

- Amplify UI Componentsを使ってSign-Up画面を追加
    - 以下の要素を使って数行で実装できる
    ```
    <AmplifyAuthenticator>...
    ```

![Amplify UI Component Auth](https://user-images.githubusercontent.com/68212997/117942977-9e4f1e80-b346-11eb-86a2-d78fdfad7065.png)

---
### API(GraphQL/REST)の自動構築
![AppSync DynamoDB](https://user-images.githubusercontent.com/68212997/117947627-57aff300-b34b-11eb-91f2-6ec588effa16.png)

- AmplifyではGraphQLとRESTに対応しており、バックエンドとクエリや設定ファイルを含めて自動構築できる
    - GraphQL API
        - AppSync, Dynamo DB
    - REST API
        - API Gateway, Lambda

#### AppSyncとは
![AWS AppSync](https://user-images.githubusercontent.com/68212997/117948029-b83f3000-b34b-11eb-9e36-a2b341d24bf7.png)

- [AWS AppSync](https://aws.amazon.com/jp/appsync/) API基盤(GraphQL)
    - フルマネージドGraphQLサービス
    - DynamoDBを初めとする様々なAWSデータリソースにアクセスするためのマネージドなGraphQL基盤を提供
    ```
    アプリケーション開発が加速できるという理由から、各組織では API の構築に GraphQL を選択しています。この API により、フロントエンドのデベロッパーは、複数のデータベースやマイクロサービス、そして API に対し、単一の GraphQL エンドポイントから迅速にクエリができるようになります。
    AWS AppSync は、GraphQL API の開発を容易にする、完全マネージド型サービスです。このサービスは、AWS DynamoDB や Lambda、その他のデータソースとの安全な接続に必要な、面倒な作業を自動的に処理します。パフォーマンスを向上させるためのキャッシュや、リアルタイムの更新を可能にするためのサブスクリプション、そして、オフラインのクライアントを簡単に同期できるようにするクライアント側のデータストアなどが、簡単に利用できるようになります。デプロイが完了すると、API リクエストのボリュームに合わせた GraphQL API 実行エンジンの自動的なスケールアップとダウンが、AWS AppSync により行われます。
    ```

- マイクロサービスへの統合されたアクセス
    ![マイクロサービスへの統合されたアクセス](https://user-images.githubusercontent.com/68212997/117948677-5206dd00-b34c-11eb-9fef-c1bd2d093d04.png)
    ```
    単一的なインターフェースを使用して、(REST API や GraphQL API のエンドポイントなどが置かれている) VPC 内のコンテナで実行中の複数のマイクロサービスからのデータにアクセスし、それらを組み合わせて使用できます。
    ```

- Subscriptionを用いる事で、データの更新（Mutation）をリアルタイムにクライアントAPに通知可能
    ![AppSyncリアルタイム同期](https://user-images.githubusercontent.com/68212997/117948431-1835d680-b34c-11eb-9e7c-6137477baf2a.png)
    ```
    データは、バックエンドと接続されたすべてのクライアントの間 (1 から多)、もしくは、クライアント間 (多から多) でブロードキャストされます。同じデータを 1 秒以内にすべてのクライアントにブロードキャストし、それらのクライアントから応答を得るシナリオが実現できます。
    ```
    
- シンプルかつ安全なデータアクセス
    - 短くコードを記述できる。セキュリティについてはWAFをアタッチできる

#### GraphQLとは
- API用のクエリ言語およびサーバーサイドランタイム
- 機能
    - 3種類のデータ処理形態
    1. 取得(Query)
    2. 変更(Mutation)
    3. 購読(Subscription): リアルタイムにデータを受け取るための存続期間の長い接続
- 特徴
    - 型指定されたスキーマ
    - `サブスクリプションを利用したリアルタイム処理`

#### Amplify CLIによるAPIの作成
1. API追加コマンドを実行
    - デフォルトではシンプルなTodoデータのスキーマを作成
    - 任意のGraphQLスキーマによるAPIを作成することも可能
2. デプロイコマンドを実行

- GraphQL APIを追加する手順
    - Step1 Amplify CLIでAPIカテゴリを追加
    - Step2 schema.grapfqlにスキーマを定義
    - AppSync経由でDynamoやLambdaと連携できる
        - @model @keyディレクティブ
            - DynamoDBのスキーマを意識しなくても必要なデータモデルの定義が可能
        - @connectionディレクティブ
            - DynamoDB間のリレーションを定義する

#### GraphQLのスキーマを定義
定義方法は二つある

- PJ/amplify/backend/apiにGraphQLの設定が入っている
    - schema.grapfql が定義ファイル
    - デフォルトのスキーマが定義されているため、APに即した形式に変更する
    - property名に対して型を決めて、スキーマを定義していく
        - 型の右に！をつけると必須(required)のpropertyになる
    - @model
        - このスキーマに即したテーブルをDynamoDBに作成する
    - このスキーマを定義することで、それに即したAppSyncとDynamoDBのテーブルが生成される

- Amplify Consoleでのスキーマの定義
    - クライアント側のコードの自動生成
    - 一からGraphQLをクエリするコードを書く必要がなくなる
    - Queryの深さには注意


#### GraphQL APIのローカルテスト
- Step1 Amplify GrapfQL Explorerの立ち上げ
    ```
    amplify mock api
    ```
- Step2 deploy
    ```
    amplify push api
    ```

---

### Hosting環境の自動構築
- add hostingコマンドでホスティング環境を構築できる
    - ここで amplify console コマンドを叩くと、このアプリの管理コンソールがブラウザで立ち上がります。
```
$ amplify add hosting
? Select the plugin module to execute (Use arrow keys)
? Hosting with Amplify Console (Managed hosting with custom domains, Continuous deployment)
? Choose a type Manual deployment

You can now publish your app using the following command:

Command: amplify publish
```

- amplify publish でアプリをアップロード
```
amplify publish
```

- 参考
    - [AWS Amplify コンソールを使用した継続的デプロイによる高速で簡単な静的ウェブホスティング](https://aws.amazon.com/jp/amplify/hosting/)

### AIサービス（Recognition）との連携
- AI/ML系のサービスを利用する際のコマンドは以下
```
amplify add predictions
```
- Recognitionによる物体検知は以下
```
Identify
Identity Labels
```
- 他の設定はDefaultを利用
- Recognitionの機能を使えるのは、認証されたユーザーのみかゲストユーザーか？
```
? Who should have access
> Auth users only
```

- 以上でRecognitionに関するバックエンドの設定は完了

### Storageの追加
- 以下のコマンドでstorageカテゴリを追加狩野
```
amplify add storage
```

### 開発端末上でのAmplifyの設定をAWSに反映
- amplify addを実行した時点で設定は作成されているが、AWSの実環境にはまだ構築されない
- 以下のコマンドで実環境に反映できる
```
amplify push
```
- push前にどのような設定がされるか確認画面が出る
- ContinueでYes
- GraphQLのスキーマにアクセスするためのクエリを自動生成してくれる
    - 言語をTypescriptに指定
    - AppSyncのスキーマに投げるためのクエリ＆設定ファイルがローカルに生成される
- クラウドに設定が反映されると同時にローカルにAWSの設定ファイルが吐き出される
- graphqlフォルダも作成される
    - AppSyncのスキーマに投げるためのクエリのソースコードが自動生成されている

## 運用周りについて

### CI/CD
- amplify envコマンド
    - 複数バックエンド環境を管理可能

- Amplify Console: PR　Preview
    - プルリク時の一時的なバックエンド環境
        - Step1. Amplify CLIでHostingカテゴリを追加
        ```
        amplify add hosting
        ```
        - Step2. Gitリポジトリと紐付け
            - 関連付けるリポジトリやbranchを選択
        - Step3. Pull Request Preview
        - Step4. Merge PR & Deploy

- Amplify Console E2E Test
    - cypressを使用したE2Eテストとの統合によるフルスタックCI/CDパイプラインの作成
    - Step1 cypressのインストール
    ```
    yran add -D cypress
    ```
    - Step2 amplify.ymlにtestフェーズを追加

### モニタリング
- Amplify Console Monitoring Metrics
    - CloudWatchアラーム・SNSトピックの作成、アクセスログの閲覧をAmplifyから一元管理
    - Amplify HostingがAmazon CloudWatchの統合でモニタリング機能をサポート
    - リクエスト数やデータ転送長、エラー数、TTFB(レスポンスまでの時間の平均)

- AWS X-Ray
    - Step1 AppSyncコンソールからXーRayを有効化
    - Step2 LambdaのコンソールからXーRayを有効か

### セキュリティ
- AWS WAF with AppSync
    - Web ACLを作成
    - AppSyncコンソールからWAFを有効化

---
## その他
### Amplifyのラーニングパス
- AWS Summit 2021 セッション
    - AmplifとAWS App Syncを使ったフルスタックアプリケーションの開発
    - ChimeSDKを使ったオンラインミーティングのサンプルAP

- AWS Amplify Social Network App Workshop
    - CI/CDやE2Eテストにも触れられている

- AWS Samplesの活用
    - GitでそれぞれのAWSサービスを利用したシステムのリファレンスアーキテクチャとソースが公開されている

- AWS Amplifyに関連するコミュニティ
    - Amplify Japan User Group
    - AWS Front-end Community


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
- [AWS AmplifyのUI Componentsを利用する](https://zenn.dev/ssshun/articles/e6c827f46640a9a657ed)
- [AWS Amplify Console で素の React アプリをホスティングしてみよう](https://aws.amazon.com/jp/builders-flash/202005/amplify-react-hosting/?awsf.filter-name=*all)