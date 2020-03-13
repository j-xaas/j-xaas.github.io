---
title: Cloud9 x Angular x FirebaseでAP開発（導入編）
date: 2020-03-05 22:28:41
categories:
- Single Page Application Dev
- Cloud Service
tags: 
- cloud9
- Angular
- Firebase
- AngularFire
---
  
こんにちは。今回は上記の３つを組み合わせてWEB AP開発を行う際の手順を解説していこうと思います。それぞれの使用経験があっても組み合わせると細かい所で沢山ハマってしまうので注意しましょう。（新設の開発チームの大半がこの段階で躓く印象です）  

## 各技術についての基礎知識  

簡単にポイントだけを解説します。分かっている方は飛ばしてください  
手順を細かく書いているので分からない言葉があっても、一先ず触りなから理解していきましょう  

- [cloud9](https://aws.amazon.com/jp/cloud9/)
    - AWSのブラウザ型IDE
    - 自端末のOSに依存せず開発可能
        - iPadでも開発可能（よく使ってます）
    - 社内プロキシ問題に悩まされない
        - 個人的には最重要ポイント
    - 環境構築を省略可能
        - node_module, Git, AWS CLI等の便利なものが元から入っている
    - 複数ユーザーで同時編集可能
        - リモートワークやレビューに便利です
        - 対抗の[Google Cloud Shell](https://cloud.google.com/shell/docs?hl=ja)との差分
            - 後に同様の機能が付きそうではあります

- [Angular](https://angular.jp/)
    - Googleが出しているWEB APのフレームワーク
        - 言語はHTML, TypeScript, scss
    - [SPA (Single Page Application)](https://digitalidentity.co.jp/blog/creative/about-single-page-application.html)を開発可能
    - APの画面はAngular Materialでほぼ自動構築可能
    - [静的WEBサイトホスティング](https://docs.aws.amazon.com/ja_jp/AmazonS3/latest/dev/WebsiteHosting.html)サービスを用いれば、[サーバレス](https://qiita.com/y-some/items/e9b78468428d1c481fac)を実現可能
        - 代表例として[Firebase Hosting](https://firebase.google.com/docs/hosting?hl=ja)や[Amazon S3](https://aws.amazon.com/jp/s3/)等が挙げられます

- [Firebase](https://firebase.google.com/)
    - Googleの[BaaS(Backend as a Service)](https://boxil.jp/mag/a3651/#3651-2)
    - 認証機能やデータベースなどのバックエンドの機能を提供してくれるサービス
        - 開発工数を大幅削減可能
        - 本質的な機能の開発に集中可能
    - APのHostingや顧客分析、収益管理まで可能  
  
この３つを抑えれば開発環境の用意からAPの公開まで高速でできます。  
    
## 前提条件
- cloud9環境を作成済み
    - 指示に従って設定していくだけなので参考サイトを載せておきます
        - [初めてのAWS Cloud9導入](https://qiita.com/tu-kun/items/d7b4f1fa19cc93bc5b75)
- Googleアカウントを作成済み
    - [Googelアカウントの作成](https://support.google.com/accounts/answer/27441?hl=ja)

## 手順

### 1. Angular開発環境の準備

- Angularが動くために必要なもの
    - node_module
    - Angular CLI
  
cloud9には初めからnode_moduleが入っている為、Angular CLIを入れるだけでOKです。（Windowsにnode_moduleを入れようとすると、それだけでだいぶ工数をロスします）  
  
- 1.1. Angular CLIのinstall
```
npm install -g @angular cli
```
- 確認
    - 以下のようにngコマンドを使えるようになればOKです
        - ngはangularの略です
```
ng --version

     _                      _                 ____ _     ___
    / \   _ __   __ _ _   _| | __ _ _ __     / ___| |   |_ _|
   / △ \ | '_ \ / _` | | | | |/ _` | '__|   | |   | |    | |
  / ___ \| | | | (_| | |_| | | (_| | |      | |___| |___ | |
 /_/   \_\_| |_|\__, |\__,_|_|\__,_|_|       \____|_____|___|
                |___/
    

Angular CLI: 8.3.20
Node: 10.16.2
OS: linux x64
Angular: 8.2.14
```

- ０から開発する場合
    - 以下のコマンドでAngular PJを開始しましょう
    ```
    ng new <AP名>
    ```
- Gitから開発途中のソースを持ってきた場合
    - APのディレクトリ直下にもnode_moduleを入れなければ動かないので注意です
    ```
    npm install
    ```

### 2. firebaseの登録～project作成
- 初めは基本的にWEBのFirebaseコンソールで操作していきます
    - Googleアカウントを作成していればすぐに始められます

- 2.1. [公式ページ](https://firebase.google.com/?hl=ja)から右上の”コンソールへ移動"を押下

- 2.2. projectを作成します（APの単位です）
  - AP名を設定
- 2.3. アプリを登録
    - ios/android/webからwebを選択
    - ニックネームを設定
- 2.4. Firebase SDKの追加
    - 公式の説明は以下
```
これらのスクリプトをコピーして <body> タグの下部に貼り付けます。この作業は Firebase サービスを使用する前に行ってください。
```

- Angularの場合はindex.htmlのbody配下に置きます
  - SPAなので、単一ページであるindex.htmlがJSで書き換わっていくのがAngularの特徴です
```
    <body>
    <app-root></app-root>
  <!----firebaseSDKの追加-->
    <!-- The core Firebase JS SDK is always required and must be listed first -->
    <script src="/__/firebase/7.8.2/firebase-app.js"></script>
  
    <!-- TODO: Add SDKs for Firebase products that you want to use
       https://firebase.google.com/docs/web/setup#available-libraries -->
    <script src="/__/firebase/7.8.2/firebase-analytics.js"></script>
  
    <!-- Initialize Firebase -->
    <script src="/__/firebase/init.js"></script>
  </body>
  </html>
```


### 3. firebaseとAPの関連付け(Angular Fireの導入)
- Angular APとfirebaseを関連付けるまでに必要な作業の一覧は以下になります
    - angularfireのinstall
    - Firebase CLIのinstal
    - (firebase側でアプリを追加　 2で先に行った内容です)
        - Firebase SDKの追加
        - FirebaseのAPIキーを取得
    - Angular環境設定ファイルにAPIキーを貼り付け
        - ★二点に設定が必要。よくはまるポイントです
        - 開発環境
            - src/environments/environment.ts
        - 本番環境
            - src/environments/environment.prod.ts
    - firebase login/init 
    - 環境設定ファイルをapp.module.tsに読み込む
        - @angular/fireからAngularFireModuleを呼び出して、
        上記のenvironmentオブジェクトを使ってAPIキーをインストール
    - （特定の機能を利用する場合の作業：認証機能を利用する際の例）
        - @angular/fire/authからAngularFireModuleを呼び出して、NgModuleに登録
            - Firebase Authenticationを利用するために必要な工程
            - @angular/fireは全機能をinstallすると重くなる為、上記の様に必要なものだけを登録する仕様になっているらしい
  
それぞれ詳しく解説していきます  

#### 3.1. angular fireのinstall
- [angularfire](https://github.com/angular/angularfire)を入れます
    - AngularとFirebase連携用のLibraryです
```
npm install firebase @angular/fire
```
- 出力結果
```
+ @angular/fire@5.4.2
+ firebase@7.9.3
added 140 packages from 98 contributors and audited 19218 packages in 17.919s
found 5 vulnerabilities (3 moderate, 2 high)
```
- ここまでではまだ、Firebaseコマンドが使えない状態です

#### 3.2. [Firebase CLI](https://firebase.google.com/docs/cli?hl=ja#install-cli-mac-linux)の導入
- 以下を実行することでinstallできます
    - firebase コマンドが有効になります
```
npm install -g firebase-tools
```

### 3.3. Angular環境設定ファイルにAPIキーを貼り付け
- 開発環境向けと本番環境向けの２つの設定ファイルの改修が必要。よくはまるポイントです
    - 各値はfirebase consoleより確認

- src/environments/environment.ts
    - 開発環境用の設定ファイル
```
export const environment = {
  production: false
  // Firebaseの設定情報を登録
    firebase: {
      apiKey: '<your-key>',
      authDomain: '<your-project-authdomain>',
      databaseURL: '<your-database-URL>',
      projectId: '<your-project-id>',
      storageBucket: '<your-storage-bucket>',
      messagingSenderId: '<your-messaging-sender-id>'
    }
  };
};
```

- src/environments/environment.prod.ts
    - 本番環境用の設定ファイル
```
export const environment = {
  production: false
  // Firebaseの設定情報を登録
    firebase: {
      apiKey: '<your-key>',
      authDomain: '<your-project-authdomain>',
      databaseURL: '<your-database-URL>',
      projectId: '<your-project-id>',
      storageBucket: '<your-storage-bucket>',
      messagingSenderId: '<your-messaging-sender-id>'
    }
  };
};
```





#### 3.4. CLIとFirebaseの関連付け
- 次にCloud9のCLIとfirebaseを関連付けます
  - c9の場合--nolocalhostが必須
    - ★社内Local環境でloginをやろうとするとFirebaseの認証がProxyに阻まれて突破できず詰むので気を付けましょう
        - 調べた解決策を全て講じてもこれだけは解決できませんでした...

```
firebase login --no-localhost --reauth

i  Firebase optionally collects CLI usage and error reporting information to help improve our products. Data is collected in accordance with Google's privacy policy (https://policies.google.com/privacy) and is not used to identify you.

? Allow Firebase to collect CLI usage and error reporting information? Yes
i  To change your data collection preference at any time, run `firebase logout` and log in again.

Visit this URL on any device to log in:
<認証用のURL>

```
- 上記のURLから、ブラウザで認Googleアカウント証を進めるとコードが表示される
- 貼り付けてSuccessとでれば成功です
```
? Paste authorization code here: <Code>

✔  Success! Logged in as <googleアカウント名>
```
- Cloud9のCLIからFirebase(=Google Cloud)上のリソースにアクセス可能になりました

#### 3.5. APとfirebaseのpjの関連付け

- Angular PJ直下で実行
```
firebase init
```
- 以下のように出力されれば成功です
    - 使いたいサービスを選択すると簡単にCloud側と連携できます
```

     ######## #### ########  ######## ########     ###     ######  ########
     ##        ##  ##     ## ##       ##     ##  ##   ##  ##       ##
     ######    ##  ########  ######   ########  #########  ######  ######
     ##        ##  ##    ##  ##       ##     ## ##     ##       ## ##
     ##       #### ##     ## ######## ########  ##     ##  ######  ########

You're about to initialize a Firebase project in this directory:

  /home/ec2-user/environment/test-app

? Which Firebase CLI features do you want to set up for this folder? Press Space to select features, then Enter to confirm your choices. (Press
 <space> to select, <a> to toggle all, <i> to invert selection)
❯◯ Database: Deploy Firebase Realtime Database Rules
 ◯ Firestore: Deploy rules and create indexes for Firestore
 ◯ Functions: Configure and deploy Cloud Functions
 ◯ Hosting: Configure and deploy Firebase Hosting sites
 ◯ Storage: Deploy Cloud Storage security rules
 ◯ Emulators: Set up local emulators for Firebase features
```

## おまけ：cloud9でng serveを実行してAPの画面を確認する際の注意
- cloud9で必要なこと
    - portの指定(8080)
    - browserのURLの指定
    - disableHostCheck
- 大抵の人が必ず一度はハマるポイントです
- 上部タブから"Preview"を選択後"Preview Runnig Application"を選択するとCloud9上に表示できます
    - Previewの中のURL欄をコピーして控えてください

```
ng serve --public-host <cloud9のPreviewのURL> --disableHostCheck --port 8080
```

## 後書き
如何だったでしょうか？ここまでで本格的な実装に入る準備が整いました。Firebaseを使いこなせば、認証機能もデータのCRUD機能も１日で簡単に実装することができます。モダンな手法を使いこなして素早くAPを開発していきましょう。
  
- 最後に、この先の機能実装の際に参考になりそうなページを共有しておきます。
  - [AngularFireでFirestoreのCRUD処理を実装する【Angular + Firebase】](https://mae.chab.in/archives/60256)
  - [Angular8でFirebaseを使ってGoogleアカウント認証機能を実装する](https://satolabo.net/2019/11/30/angular8-fire-google-auth/)

