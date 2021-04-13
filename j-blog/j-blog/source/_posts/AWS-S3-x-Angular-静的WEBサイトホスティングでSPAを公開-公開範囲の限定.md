---
title: '[AWS S3 x Angular] 静的WEBサイトホスティングでSPAを公開/ng build/公開範囲の限定/CI/CD化'
date: 2020-06-16 16:34:06
categories:
- Serverless Application Dev
- AWS
tags: 
- AWS
- S3
- Angular
- 静的WEB Hosting
- CI/CD
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/84747475-4272fd00-aff2-11ea-9060-07136ed71892.png
---

<!-- toc -->

## 基礎知識
### S3の静的WEBサイトホスティング機能

- [静的WEB Hosting](https://docs.aws.amazon.com/ja_jp/AmazonS3/latest/dev/WebsiteHosting.html)とは
    - 静的コンテンツをWEBに公開するオンラインストレージの機能
    - 静的コンテンツとは
        - HTML/CSS/JavaScriptなどで構成されるWEBページやWEBアプリ
    - サーバーレスアーキテクチャを実現
        - Angularで開発したアプリは静的WEB Hostingを利用することで、WEBサーバー無しで公開できます
        - TypScript(JavaScript)で書いた機能は、アクセスしたユーザーのブラウザ側で動くため、公開する側にコンピューティングパワーが必要無い=WEBサーバーが必要無いわけです
        - こういったサービスを利用して、表向きのサーバー無しで構成するのが、サーバレスアプリケーションと呼ばれるており、最近の流行りです
    - メリット
        - サーバーの運用費がかからないため、インフラのコストを大幅に削減可能
            - 一般的にアクセス数が伸びるまでは無料で公開できるため、スタートアップが新しいサービスを立ち上げる際によく使う手法でもあります

- 静的WEB Hostingの代表的なサービス
    - Amazon S3
        - AWSでHosting。本記事で解説しています。
    - Firebase Hosting
        - GCP(Google Cloud Platform)でHosting
        - 初めからSSL化(Https化)される点は、S3よりイケてますが、通信が増えるとS3の方がコスパが良いイメージ
        - 以下の記事で解説しています
            - [Firebase Hosting完全版(Angularで開発したSPAを無料で公開～ダッシュボードで費用管理)](/Firebase-Hosting完全版-Angularで開発したSPAを無料で公開～ダッシュボードで費用管理/)
    - Github Pages
        - GithubでHosting。本サイトはこれで公開しています(2020/06/16時点)
        - 以下の記事で解説しています
            - [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)

- [S3の料金](https://aws.amazon.com/jp/s3/pricing/)
    - 料金はHostingする容量＋通信料で定まります
    - 無料枠
        - 初めの1年間以下を利用できます
            - Hosting：5GBまで
            - 通信
                - 20,000 GET リクエスト、2,000 PUT、COPY、POST、あるいは LIST リクエスト、データ送信 15 GB 
    - 計算方法
        - SPA(Single page Application)は初めにAP全体を読み込んで画面遷移は端末側で行います。つまり、画面遷移によって通信が発生しません。よって、初めにS3のAPにアクセスしたタイミングでAPの容量分の通信が発生します。
            - APアクセス数/1000 x 0.05USD + AP容量 x 0.023USD 
        - 料金の目安
            - 最初の 50 TB/月 0.023USD/GB
        - Angularで開発したAPの容量
            - がっつり機能を具備したAPで容量は20MB程度でした
            - 50TBを超えるには相当人気が出ないと無理です

    - Tips: コスト削減方法
        - APの設計により、通信を減らす
            - 前述したように、フロントをMPAではなく、SPAで開発することで画面遷移に伴う通信を削減可能です
        - CloudFrontでキャッシュ
            - アクセスの多いリージョンからS3へのアクセスを軽減できます
            - S3を使う場合はこれを利用するのが一般的です

### APをS3で公開するまでの流れ
以下の流れで解説します

1. Build
2. S3バケットを生成
3. デプロイ

APを更新する度に手動でデプロイするのはイケてないので、その後の自動化方法も別記事で解説しています。
- [CI/CD入門 Github Actionsでビルド/テスト/デプロイを自動化](/CI-CD入門-Github-Actionsでビルド-テスト-デプロイを自動化/)



## Angular APをBuild
まずはアプリを本番環境で動く状態にします

- ng buildコマンド
  - プロジェクト配下に/distフォルダを生成
  - コンパイルされたアプリ一式が格納される
```
ng build --prod
```

- 生成されたフォルダ
  - angular pj直下にdistフォルダができています
```
your-app> ls

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       2020/06/05     17:00                dist
d-----       2020/02/04     11:15                e2e
d-----       2020/06/05     15:42                node_modules
d-----       2020/05/08      9:25                src
-a----       2020/02/04     11:15            246 .editorconfig
-a----       2020/02/04     11:15            631 .gitignore
-a----       2020/04/13      9:53           3905 angular.json
-a----       2020/02/04     11:15            429 browserslist
-a----       2020/02/04     11:15           1025 karma.conf.js
-a----       2020/04/07     13:58         482680 package-lock.json
-a----       2020/04/07     13:58           1480 package.json
-a----       2020/02/04     11:15           1029 README.md
-a----       2020/02/27     21:35            276 tsconfig.app.json
-a----       2020/02/27     21:35            575 tsconfig.json
-a----       2020/02/04     11:15            270 tsconfig.spec.json
-a----       2020/02/04     11:15           1953 tslint.json
```
- 中身
```
your-app\dist\your-app> ls


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----       2020/06/16     18:52         445686 3rdpartylicenses.txt
-a----       2020/06/16     18:52            948 favicon.ico
-a----       2020/06/16     18:53           1001 index.html
-a----       2020/06/16     18:53        3344584 main-es2015.c5b9e425026ef7fc731a.js
-a----       2020/06/16     18:53        3478180 main-es5.c5b9e425026ef7fc731a.js
-a----       2020/06/05     17:01          37074 polyfills-es2015.b94e8feb7a16d6790318.js
-a----       2020/06/05     17:01         129383 polyfills-es5.b220e907c509a1aa6f0d.js
-a----       2020/06/05     17:00           1485 runtime-es2015.c5fa8325f89fc516600b.js
-a----       2020/06/05     17:00           1485 runtime-es5.c5fa8325f89fc516600b.js
-a----       2020/06/16     18:52          64154 styles.5a9f2f959a54b18f5f2f.css
```



## S3バケットの作成
Buildしたソースコードを置くバケットを作ります

- AWS Consoleにログイン
- サービスよりS3を選択

- バケットを作成するを選択
![S3画面](https://user-images.githubusercontent.com/41946222/84749017-63d4e880-aff4-11ea-8ab1-2375e6c79530.png)


- 一意なバケット名とリージョンを指定
![バケット作成/名前とリージョン](https://user-images.githubusercontent.com/41946222/84749511-07be9400-aff5-11ea-9af6-841e118ec247.png)


- バージョニング
    - Git等でソースを管理しているのであれば、無効でOK
    - Tags
        - AWS運用の基本的原則として、最低限以下は付けましょう。AWSのリソースのコストはTagをつけておくことで追跡しやすくなります。
            - PJ: PJ名
            - Own: リソースの管理者

    ![バケット作成/バージョニング](https://user-images.githubusercontent.com/41946222/84751384-78ff4680-aff7-11ea-8089-63151cc4f8f5.png)

- アクセス許可の設定
    - 一先ずデフォルトでOK

    ![バケット作成/アクセス許可の設定](https://user-images.githubusercontent.com/41946222/84752880-4bb39800-aff9-11ea-9810-3859c9904691.png)

- バケットを作成

アクセス権限を変更する際には適宜このバケットの設定を弄ってください

## S3にHosting（アプリを公開）

### S3にソースをアップロード
先ほど作成したS3バケットにBuildしたAngularのソース(/dist/your-app配下)を置きます

- AWS Console/サービス/S3より任意のバケットのページを開く
- アップロードを押下
![Bucket](https://user-images.githubusercontent.com/41946222/84753536-1491b680-affa-11ea-9706-94f150cd9839.png)

- ファイルを追加
  - /dist/your-app配下のファイル群をドラック&ドロップ or ファイルを追加で選択

![upload](https://user-images.githubusercontent.com/41946222/84753693-499e0900-affa-11ea-9ce2-ac66ea4695cf.png)

- アップロードを押下
    - 以上でアップロードが始まります


### 静的ウェブホスティング設定
S3にアップしたソースを公開します

- AWS Console/サービス/S3より任意のバケットを選択
- 右に表示されるバケット情報欄の”アクセス権限”を選択

#### ブロックパブリックアクセス
- ブロックパブリックアクセスタブの”編集”を押下
![Blockpublic access](https://user-images.githubusercontent.com/41946222/84755028-207e7800-affc-11ea-9374-5cb8ff7a3f56.png)

- 全てのブロックのチェックを外す
    - 保存

#### バケットポリシーを変更
- バケットポリシータブを選択
![policy](https://user-images.githubusercontent.com/41946222/84756004-380a3080-affd-11ea-8955-f4a48695fb0b.png)

- 画面のエディターにポリシーを記載して保存
    - 特に絞らず公開する場合の例
    - "Resource"のarnは自分のbucketのものに変更してください

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::angular-sample-bucket/*"
        }
    ]
}
```

#### プロパティの変更/Static website hosting

- バケット内のプロパティタブ ⇒ Static website hostingを選択
![property](https://user-images.githubusercontent.com/41946222/84758061-eca55180-afff-11ea-831a-675f0fa4ef26.png)

- ”このバケットを使用してウェブサイトをホストする”
    - インデックスドキュメントに”index.html”を入力して保存

![Static website hosting](https://user-images.githubusercontent.com/41946222/84758571-98e73800-b000-11ea-8497-c1b9368c7869.png)

- URLの確認方法
    - Static website hostingより確認
- URL
    - 以下のように一意に定まります
    ```
    http://<bucket-name>.s3-website-<select-region>.amazonaws.com
    ```


### S3で公開したAPのアクセス制限

　開発したAPを社内公開する際、イントラからのアクセスに絞るケースが多いと思うので解説しておきます。

- バケットポリシーにIPアドレスの制限をかけることで絞れます
    - Conditionで許可するIPを書き足すだけ
    ```
            "Condition" : {
                "IpAddress" : {
                    "aws:SourceIp": "xxx.xxx.xxx.xxx/xx" 
                }
    ```

- 記載例
    - アクセスを許可するIPをSourceIpとして定義
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::your-bucket/*",
            "Condition" : {
                "IpAddress" : {
                    "aws:SourceIp": "xxx.xxx.xxx.xxx/xx"
                }
        }
    ]
}
```



## デプロイの自動化(CI/CD)の検討

APを改修する度に手動でS3へデプロイするのは面倒なので自動化しましょう。やり方は沢山あり、ぱっと思いつくだけでも以下があります。

- 実現方法
    - Github Actionsを使用
    - Code PipelineとCode Buildを使用
    - ng deployに設定
    - JenkinsやCircleCIを使用

- 色々試した結果、最も使い勝手が良いのはGithub Actionsだと思います。詳細は以下の記事にまとめてあります。
    - [CI/CD入門 Github Actionsでビルド/テスト/デプロイを自動化](/CI-CD入門-Github-Actionsでビルド-テスト-デプロイを自動化/)

- 何故Github Actionsが優れているか？
    - Githubで完結
        - 他のツールと組み合わせる手間が無いのは大きいです
    - 費用面
        - 基本無料で使える
    - 再利用性
        - ymlファイルで自動化するワークフローを定義するのですが、これをgitでソースと一緒に管理できます。つまり、同じようなケースでファイルをコピーするだけで使いまわすことができます。
    - 学習コストの低さ
    - サードパーティーの公開Action
        - よくあるパターンのテンプレートは大体既に用意されています
        - まさに今回やりたいS3へのデプロイの自動化を実現するActionも既にありました


### Tourble Shooting: 404でAPにアクセスできない

![404](https://user-images.githubusercontent.com/41946222/84759051-304c8b00-b001-11ea-8dd0-d2cb7cdd57a0.png)

- 以下を確認してください 
    - bucket直下にindex.htmlが入っているか？
    - /dist配下のフォルダ毎まるまるupしてしまうと読み込めません
    - /dist/your-app配下のファイル群をUpしましょう

## 参考
### 関連記事
- [CI/CD入門 Github Actionsでビルド/テスト/デプロイを自動化](/CI-CD入門-Github-Actionsでビルド-テスト-デプロイを自動化/)

### S3
- [AWS S3にAngularアプリをデプロイする手順](https://qiita.com/BBA/items/4de0132e63a371da4626)
- [Angularで作ったWebアプリをGitHubで管理してS3に自動デプロイする](https://s8a.jp/angular-github-aws-s3-auto-deploy#%E5%8B%95%E4%BD%9C%E7%A2%BA%E8%AA%8D)
- [AWS初心者の私がAmazon S3のStatic website hostingを利用して静的Webページをホスティングしてみました](https://blog.web.nifty.com/engineer/2401)
- [Amazon S3のStatic Web Hostingにアクセス制限をかけて使ってみる](https://kray.jp/blog/amazon_s3_static_web_hosting/)