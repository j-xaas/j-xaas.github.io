---
title: Cloud9 x Angular x FirebaseでAP開発（導入編）
date: 2020-03-05 22:28:41
toc: true
categories:
- Serverless Application Dev
- Firebase
tags: 
- cloud9
- Angular
- Firebase
- AngularFire
---
  
こんにちは。今回は上記の３つを組み合わせてWEB AP開発を行う際の手順を解説します。それぞれの使用経験があっても細かい所で沢山ハマったのでまとめておきました。初級者がこの記事だけで目標を達成できるように書いたつもりです。  

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79091968-4b451980-7d8a-11ea-9938-5c217d995d87.png" height="400px" width="500px">
</div>

<!-- toc-->

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
つまり、良いアイデアさえあれば、一人でもビジネスを始められます。  

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
    - 以下のコマンドでAngular PJ(AngularにおけるAPの単位)を作成しましょう
    ```
    ng new <AP名>
    ```
    - 今回は以下の名称で作成
    ```
    $ ng new "firebase-sample"
    ```
- Gitから開発途中のソースを持ってきた場合
    - APのディレクトリ直下にもnode_moduleを入れなければ動かないので注意です
    ```
    npm install
    ```

### 2. firebase projectの作成
- 初めは基本的にWEBのFirebaseコンソールで操作していきます
    - Googleアカウントを作成していればすぐに始められます

- 2.1. [公式ページ](https://firebase.google.com/?hl=ja)から右上の”コンソールへ移動"を押下

- 2.2. projectを追加（APの単位です）
<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79091762-8abf3600-7d89-11ea-8299-3fe69a9ff84a.png" height="400px" width="500px">
</div>

- 2.3. Project名を設定
<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79091817-b9d5a780-7d89-11ea-86cf-69d608904466.png" height="400px" width="500px">
</div>

- 2.4. Googleアナリティクス(Firebaseプロジェクト向け)
    - 特別な理由が無ければ有効のままでOK
<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79091837-d1ad2b80-7d89-11ea-9f02-c218bfebd70f.png" height="400px" width="400px">
</div>

- 2.6. 以下が表示されたら完成
    - 続行を押すとPJの画面に飛びます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79091916-22bd1f80-7d8a-11ea-8c5c-f0db39638fc1.png" height="400px" width="500px">
</div>

- 2.7. Firebase Project画面
    - 以下が表示されればOK
    - 基本的にこの画面で設定を行います

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79091968-4b451980-7d8a-11ea-9938-5c217d995d87.png" height="450px" width="500px">
</div>


### 3. アプリの登録
- 次にFirebase Projectにアプリを登録します
    - Firebase PJには複数のアプリを登録可能です
    - 例えばWEB版、ios版といった形で複数をAP間で認証機能やデータベースを共有するイメージです

- 3.1. アプリを登録
    - ios/android/webからwebのアイコンを選択
<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79092138-e2aa6c80-7d8a-11ea-9440-8bf24e2a2219.png" height="300px" width="500px">
</div>

- 3.2. アプリの追加
    - 以下のように入力
    - アプリを登録
<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79092234-3452f700-7d8b-11ea-996c-043db263989b.png" height="400px" width="500px">
</div>

- 3.3. Firebase SDK の追加
    - 以下のようにスクリプトが表示されます

    <div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/41946222/79092282-61070e80-7d8b-11ea-9c67-d2c9a38e27fb.png" height="400px" width="400px">
    </div>


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


### 4. firebaseとAPの関連付け(Angular Fireの導入)
- Angular APとfirebaseを関連付けるまでに必要な作業の一覧は以下になります
    - Firebase CLIのinstall
        - 環境単位で必要な作業
    - angularfireのinstall
        - AP単位で毎回必要な作業
    - (firebase側でアプリを追加　 2で先に行った内容です)
        - Firebase SDKの追加
        - FirebaseのAPIキーを取得
    - Angularの環境設定ファイルにAPIキーを設定
        - ★二点に設定が必要。よくはまるポイントです
            - 開発環境
                - src/environments/environment.ts
            - 本番環境(prodは本番という意味)
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

#### 4.1. Firebase CLIの導入
- [Firebase CLI](https://firebase.google.com/docs/cli?hl=ja#install-cli-mac-linux)
    - 環境に一度入れればOKです
    - 以下を実行することでinstallできます
        - これでfirebase コマンドが有効になります
    ```
    npm install -g firebase-tools
    ```

#### 4.2. angular fireのinstall
- [angularfire](https://github.com/angular/angularfire)を入れます
    - AngularとFirebaseの連携用Libraryです
    - こちらは環境単位ではなくAP単位で入れる必要があります
- 以下を実行
    - ※最新版であればng addでもOK
```
firebase-sample (master) $ npm install @angular/fire firebase --save
```
- 出力結果
```
+ @angular/fire@5.4.2
+ firebase@7.9.3
added 140 packages from 98 contributors and audited 19218 packages in 17.919s
found 5 vulnerabilities (3 moderate, 2 high)
```


### 4.3. Angular環境設定ファイルにAPIキーを貼り付け
- 開発環境向けと本番環境向けの２つの設定ファイルの改修が必要。よくはまるポイントです
    - 各値はfirebase consoleより確認

- src/environments/environment.ts
    - 開発環境用の設定ファイルに以下の形式で設定します
```
export const environment = {
  production: false,
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
    - 本番環境用の設定ファイルも同様です
```
export const environment = {
  production: false,
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

- Firebaseの設定情報を確認
    - FirebaseコンソールのSettingsから確認可能

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79093020-ccea7680-7d8d-11ea-916f-0b2a4108baa4.png" height="370px" width="500px">
</div>

- 上記のIDとAPIキーをenvironmentsの該当箇所にコピペ

- 設定より、IDとAPIキー、送信者IDを確認する
        - 各設定項目は以下のように一意に定まる

```
export const environment = {
    production: false,
    firebase: {
      apiKey: '<ウェブAPIキー>',
      authDomain: "<プロジェクトID>.firebaseapp.com",
      databaseURL: "https://<プロジェクトID>.firebaseio.com",
      projectId: "<プロジェクトID>",
      storageBucket: "<プロジェクトID>.appspot.com",
      messagingSenderId: "<送信者ID>",
    }
  };
```

- 送信者IDはクラウドメッセージングTabより確認可能
<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79093197-7f223e00-7d8e-11ea-82ef-90078b05c160.png" height="300px" width="500px">
</div>

- 両方のファイルに以下を設定
```
export const environment = {
    production: false, // ,を忘れないよう注意
    // Firebaseの設定情報を登録
      firebase: {
        apiKey: 'XXXXXXXXXXXXXXXXXXXXXXXXXXX',
        authDomain: 'fir-sample-3a2dc.firebaseapp.com',
        databaseURL: 'https://fir-sample-3a2dc.firebaseio.com',
        projectId: 'fir-sample-3a2dc',
        storageBucket: 'fir-sample-3a2dc.appspot.com',
        messagingSenderId: '408620068768'
      }
  };
```

#### 4.4. CLIとFirebaseの関連付け(firebase login)
- 次に開発環境(Cloud9)のCLIとfirebaseを関連付けます
  - c9の場合--nolocalhostが必須
    - ★社内Local環境でloginをやろうとするとFirebase認証がProxyに阻まれて突破できず詰むので気を付けましょう
        - 頑張っても解決できませんでした...

```
firebase login --no-localhost --reauth

i  Firebase optionally collects CLI usage and error reporting information to help improve our products. Data is collected in accordance with Google's privacy policy (https://policies.google.com/privacy) and is not used to identify you.

? Allow Firebase to collect CLI usage and error reporting information? Yes
i  To change your data collection preference at any time, run `firebase logout` and log in again.

Visit this URL on any device to log in:
<認証用のURL>

```
- 上記のURLから、ブラウザでGoogleアカウント認証を進めるとコードが表示されます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79242034-c0aa0a80-7eae-11ea-8305-aef46bd36bed.png" height="200px" width="400px">
</div>


- 貼り付けてSuccessと表示されれば成功です
```
? Paste authorization code here: <Code>

✔  Success! Logged in as <googleアカウント名>
```
- Cloud9のCLIからFirebase(=Google Cloud)上のリソースにアクセス可能になりました

#### 4.5. APとfirebaseのPJの関連付け(firebase init)

- Angular PJ直下で実行
```
firebase init
```
- 以下のように出力されれば成功です
    - 使いたいサービスを選択すると簡単にCloud側と連携できます
        - スペースキーで各サービスを選択できます
```

     ######## #### ########  ######## ########     ###     ######  ########
     ##        ##  ##     ## ##       ##     ##  ##   ##  ##       ##
     ######    ##  ########  ######   ########  #########  ######  ######
     ##        ##  ##    ##  ##       ##     ## ##     ##       ## ##
     ##       #### ##     ## ######## ########  ##     ##  ######  ########

You're about to initialize a Firebase project in this directory:

  /home/ec2-user/environment/firebase-sample

? Which Firebase CLI features do you want to set up for this folder? Press Space to select features, then Enter to confirm your choices. (Press
 <space> to select, <a> to toggle all, <i> to invert selection)
❯◯ Database: Deploy Firebase Realtime Database Rules
 ◯ Firestore: Deploy rules and create indexes for Firestore
 ◯ Functions: Configure and deploy Cloud Functions
 ◯ Hosting: Configure and deploy Firebase Hosting sites
 ◯ Storage: Deploy Cloud Storage security rules
 ◯ Emulators: Set up local emulators for Firebase features
```

- Project Setup
    - Enterを押すと以下が出力されます
    - 今回は事前にプロジェクトを作成しているので”use an exciting project”を選択

```
=== Project Setup

First, let's associate this project directory with a Firebase project.
You can create multiple project aliases by running firebase use --add, 
but for now we'll just set up a default project.

? Please select an option: 
❯ Use an existing project 
  Create a new project 
  Add Firebase to an existing Google Cloud Platform project 
  Don't set up a default project 
```

- 自身のアカウントで作成済みのFirebase Projectが選択肢としてでてきます
    - 事前に作った”fir-sample-3a2dc (firebase-sample)”を選択
```
? Select a default Firebase project for this directory: (Use arrow keys)
❯ fir-sample-3a2dc (firebase-sample) 
```
- 以降は、先ほど選択したサービスのSetupが順に出てきます。いくつか例として書いておきます

- Hosting Setup
    - 公開したいディレクトリを尋ねられます
    - Angularであればビルドしたい際の生成物が格納されるを入力しましょう
        - dits/angular-pj-name
    - Hostingについての詳細は以下にまとめています
        - [Firebase Hosting完全版(Angularで開発したSPAを無料で公開～ダッシュボードで費用管理)](https://j-xaas.github.io/2020/04/16/Firebase-Hosting完全版-Angularで開発したSPAを無料で公開～ダッシュボードで費用管理/)

- Database Setup   
    - ひとまずデフォルトでEnter
```
=== Database Setup

Firebase Realtime Database Rules allow you to define how your data should be
structured and when your data can be read from and written to.

? What file should be used for Database Rules? (database.rules.json) 
```

- Firestore Set up
    - リソース(firestore)のlocationを設定していないとErrorが出ます
        - firebaseコンソール側で設定する必要があります
```
=== Firestore Setup

Error: Cloud resource location is not set for this project but the operation you are attempting to perform in Cloud Firestore requires it. Please see this documentation for more details: https://firebase.google.com/docs/projects/locations
```

- 以上で一先ずセットアップは完了です

- 次にのStepとしてFirebase HostingでAPを公開してみましょう（5分できます）
    - [Firebase Hosting完全版(Angularで開発したSPAを無料で公開～ダッシュボードで費用管理)](https://j-xaas.github.io/2020/04/16/Firebase-Hosting完全版-Angularで開発したSPAを無料で公開～ダッシュボードで費用管理/)
    - 実際にAuthentication(認証機能)やFirestore(ストレージ)を利用する手順は別記事に記載します

## 後書き
ここまでで本格的な実装に入る準備が整いました。Firebaseを使いこなせば、認証機能もデータのCRUD機能も1日で簡単に実装することができます。モダンな手法を使いこなして素早くサービスを開発していきましょう。
  
- 最後に、この先の機能実装の際に参考になりそうなページをまとめておきます。
    - APをFirebase Hostingで公開
        - [Firebase Hosting完全版(Angularで開発したSPAを無料で公開～ダッシュボードで費用管理)](https://j-xaas.github.io/2020/04/16/Firebase-Hosting完全版-Angularで開発したSPAを無料で公開～ダッシュボードで費用管理/)
    - CRUD機能を実装
        - [AngularFireでFirestoreのCRUD処理を実装する【Angular + Firebase】](https://mae.chab.in/archives/60256)
    - 認証機能を実装
        - [Angular8でFirebaseを使ってGoogleアカウント認証機能を実装する](https://satolabo.net/2019/11/30/angular8-fire-google-auth/)


## おまけ：cloud9でng serveする際の注意
- 詳細は以下の記事にまとめてあります
    - [[Cloud9 x Angular] cloud9でng serveを実行してAPの画面を確認するまで](https://j-xaas.github.io/2020/04/14/Cloud9-x-Angular-cloud9%E3%81%A7ng-serve%E3%82%92%E5%AE%9F%E8%A1%8C%E3%81%97%E3%81%A6AP%E3%81%AE%E7%94%BB%E9%9D%A2%E3%82%92%E7%A2%BA%E8%AA%8D%E3%81%99%E3%82%8B%E3%81%BE%E3%81%A7/)
- cloud9で必要なこと
    - portの指定(8080)
    - browserのURLの指定
    - disableHostCheck

- 1. angular.jsonファイルを書き換えてポートを変更
    - cloud9の場合はportを8080にする必要がある
```
        "serve": {
          "builder": "@angular-devkit/build-angular:dev-server",
          "options": {
            "browserTarget": "NgTororo:build",
            "port": 8080 //ここを追加
          }
```

- 2. portの設定後であれば以下でOK
```
ng serve --disableHostCheck --public-host <cloud9のPreviewのURL> 
```


<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79135318-1d8fbd00-7dea-11ea-9e6d-31134f1ad284.png" height="500px" width="500px">
</div>
