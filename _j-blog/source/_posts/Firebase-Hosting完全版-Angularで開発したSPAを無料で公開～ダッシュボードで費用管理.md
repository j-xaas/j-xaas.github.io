---
title: Firebase Hosting完全版(Angularで開発したSPAを無料で公開～ダッシュボードで費用管理)
date: 2020-04-16 02:09:36
toc: true
categories:
- Serverless Application Dev
- Firebase
tags: 
- Angular
- Firebase
- Hosting
- AngularFire
---

こんにちは。今回はFirebase Hostingについての解説です。
AngularとFirebaseの連携手法については[以前に詳しくまとめた記事](https://j-xaas.github.io/2020/03/05/Cloud9-x-Angular-x-Firebase%E3%81%A7AP%E9%96%8B%E7%99%BA%EF%BC%88%E5%B0%8E%E5%85%A5%E7%B7%A8%EF%BC%89/)をご覧ください

- [Cloud9 x Angular x FirebaseでAP開発（導入編）](https://j-xaas.github.io/2020/03/05/Cloud9-x-Angular-x-Firebase%E3%81%A7AP%E9%96%8B%E7%99%BA%EF%BC%88%E5%B0%8E%E5%85%A5%E7%B7%A8%EF%BC%89/)


<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79361347-24503880-7f80-11ea-846a-68a713792cc8.png" height="400px" width="400px" alt="Firebase Hosting">
</div>

<!-- toc -->

## 基礎知識
- [Firebase Hosting](https://firebase.google.com/docs/hosting?hl=ja)とは
    - Googleが提供している[BaaS](https://www.otsuka-shokai.co.jp/words/baas.html)である[Firebase](https://firebase.google.com/)のサービス群の一つ
    - Hostingサービス
        - 開発したアプリケーションをを保持してWEBに公開してくれます
        - 同様の機能を持つ競合サービス
            - [Amazon S3](https://aws.amazon.com/jp/s3/)
                - バックエンドにAWSを用いるのであれば基本的にこちら
            - [Github Pages](https://j-xaas.github.io/2020/03/23/Hexo-x-Github-Pages-5%E5%88%86%E4%BB%A5%E5%86%85%E3%81%AB%E3%83%96%E3%83%AD%E3%82%B0%E3%82%92%E8%87%AA%E5%8B%95%E7%94%9F%E6%88%90%E3%81%97%E3%81%A6%E7%84%A1%E6%96%99%E3%81%A7%E5%85%AC%E9%96%8B%E3%81%99%E3%82%8B%E3%81%BE%E3%81%A7/)
                - 本サイトはこれを使ってホスティングしています
                - 利用方法は以下に詳しくまとめています
                    - [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](https://j-xaas.github.io/2020/03/23/Hexo-x-Github-Pages-5%E5%88%86%E4%BB%A5%E5%86%85%E3%81%AB%E3%83%96%E3%83%AD%E3%82%B0%E3%82%92%E8%87%AA%E5%8B%95%E7%94%9F%E6%88%90%E3%81%97%E3%81%A6%E7%84%A1%E6%96%99%E3%81%A7%E5%85%AC%E9%96%8B%E3%81%99%E3%82%8B%E3%81%BE%E3%81%A7/)

    - 設定後はdeployコマンド一撃でサービスを公開可能です
    ```
    firebase deploy
    ```

## AP公開までの手順

### 本記事の前提
- 以下の作業が完了済み
    - Angular PJの生成
        ```
        ng new "ap-name"
        ```
    - Firebase PJの作成
        - Firebase Consoleでする作業
    - Firebase CLIの導入
        ```
        npm install -g firebase-tools
        ```
    - 開発環境からのFirebase Login
        ```
        firebase login
        ```
- ここまでの作業の詳細は[以前の記事](https://j-xaas.github.io/2020/03/05/Cloud9-x-Angular-x-Firebase%E3%81%A7AP%E9%96%8B%E7%99%BA%EF%BC%88%E5%B0%8E%E5%85%A5%E7%B7%A8%EF%BC%89/)をご確認ください
    - [Cloud9 x Angular x FirebaseでAP開発（導入編）](https://j-xaas.github.io/2020/03/05/Cloud9-x-Angular-x-Firebase%E3%81%A7AP%E9%96%8B%E7%99%BA%EF%BC%88%E5%B0%8E%E5%85%A5%E7%B7%A8%EF%BC%89/)


### 事前の設定作業：Firebase PJの初期化

- 以下のコマンドでFirebase PJを初期化可能です
```
firebase init
```

- 利用するリソースを選択
    - 上記を実行すると以下のように選択欄が表示されます
    - 今回はHostingだけを選択(スペース) ⇒ Enter
```
? Which Firebase CLI features do you want to set up for this folder? Press Space to select features, then Enter to confirm your choices. 
 ◯ Database: Deploy Firebase Realtime Database Rules
 ◯ Firestore: Deploy rules and create indexes for Firestore
 ◯ Functions: Configure and deploy Cloud Functions
❯◉ Hosting: Configure and deploy Firebase Hosting sites
 ◯ Storage: Deploy Cloud Storage security rules
 ◯ Emulators: Set up local emulators for Firebase features
```

- Hosting Setup
    - deployコマンドの実行時に公開したいディレクトリを尋ねられます
        - publicのまま変更無し ⇒ Enter
    - 変更
        - ディレクトリのパスを入力 ⇒ Enter 

- Angularの場合の設定
    - publicディレクトリを指定
        - dist/angular-ap-name
            - Angularでビルドを実行した際に、生成されたファイルが格納されるディレクトリのことです
            - ここをちゃんと設定しないと後にハマるので気を付けましょう
    - single-page appとして設定
        - y
        - AngularはSingle Page Applicationであるためyes

- 出力
```
✔  Wrote dist/firebase-sample/index.html

i  Writing configuration info to firebase.json...
i  Writing project information to .firebaserc...

✔  Firebase initialization complete!
```

- firebase.jsonというファイルが生成されていれば成功です

### APをFirebase Hostingへ公開

- deployコマンド
    - Firebase PJのデフォルトの Hosting サイトにデプロイされます
    ```
    firebase deploy
    ```

- deployを実行
```
firebase-sample (master) $ firebase deploy
```
- 出力
```
=== Deploying to 'fir-sample-3a2dc'...

i  deploying hosting
i  hosting[fir-sample-3a2dc]: beginning deploy...
i  hosting[fir-sample-3a2dc]: found 10 files in dist/firebase-sample
✔  hosting[fir-sample-3a2dc]: file upload complete
i  hosting[fir-sample-3a2dc]: finalizing version...
✔  hosting[fir-sample-3a2dc]: version finalized
i  hosting[fir-sample-3a2dc]: releasing new version...
✔  hosting[fir-sample-3a2dc]: release complete

✔  Deploy complete!

Project Console: https://console.firebase.google.com/project/fir-sample-3a2dc/overview
Hosting URL: https://fir-sample-3a2dc.firebaseapp.com
```

- Deploy completeと出れば成功です


- デフォルトのホスティングサイトのURLは以下のように規定されます
    - projectID.web.app
    - projectID.firebaseapp.com


- 今回のHosting URLにアクセスしてみます
```
Hosting URL: https://fir-sample-3a2dc.firebaseapp.com
```

- 空のままデプロイした場合のデフォルト画面
    - まだAPをビルドしていなければこのように表示されます
        - dist/angular-ap-nameの中が空であるため

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79250765-251e9700-7eba-11ea-8d6a-ea1e4380a7c7.png" height="300px" width="400px" alt="Firebase Hosting">
</div>

実際にAPをビルドしてからdeployして変化を確認しましょう  

### APをビルドして再度deploy
- Angular PJをビルド
    - dist/my-project-nameディレクトリが生成されます
    ```
    $ ng build --prod
    ```

- 出力
```
firebase-sample (master) $ ng build --prod
Generating ES5 bundles for differential loading...
ES5 bundle generation complete.

chunk {2} polyfills-es2015.5b10b8fd823b6392f1fd.js (polyfills) 36.2 kB [initial] [rendered]
chunk {3} polyfills-es5.8e50a9832860f7cf804a.js (polyfills-es5) 127 kB [initial] [rendered]
chunk {0} runtime-es2015.c5fa8325f89fc516600b.js (runtime) 1.45 kB [entry] [rendered]
chunk {0} runtime-es5.c5fa8325f89fc516600b.js (runtime) 1.45 kB [entry] [rendered]
chunk {1} main-es2015.3b7f37fdd4f3fcb1925c.js (main) 245 kB [initial] [rendered]
chunk {1} main-es5.3b7f37fdd4f3fcb1925c.js (main) 293 kB [initial] [rendered]
chunk {4} styles.09e2c710755c8867a460.css (styles) 0 bytes [initial] [rendered]
Date: 2020-04-15T15:05:31.948Z - Hash: e637fe92a3f41b4587c8 - Time: 21540ms
```

- Angular PJ直下にdistディレクトリが生成されます
```
firebase-sample (master) $ ls
angular.json         dist           karma.conf.js  package-lock.json  tsconfig.app.json   tslint.json
browserslist         e2e            node_modules   README.md          tsconfig.json
database.rules.json  firebase.json  package.json   src                tsconfig.spec.json
```

- もう一度deploy
```
firebase-sample (master) $ firebase deploy
```

- 出力
```
=== Deploying to 'fir-sample-3a2dc'...

i  deploying hosting
i  hosting[fir-sample-3a2dc]: beginning deploy...
i  hosting[fir-sample-3a2dc]: found 10 files in dist/firebase-sample
✔  hosting[fir-sample-3a2dc]: file upload complete
i  hosting[fir-sample-3a2dc]: finalizing version...
✔  hosting[fir-sample-3a2dc]: version finalized
i  hosting[fir-sample-3a2dc]: releasing new version...
✔  hosting[fir-sample-3a2dc]: release complete

✔  Deploy complete!

Project Console: https://console.firebase.google.com/project/fir-sample-3a2dc/overview
Hosting URL: https://fir-sample-3a2dc.firebaseapp.com
```

- Hosting URLを確認
    - アプリの画面が反映されている
    - 今回は特にAPを弄っていないので、ng newで生成したAPの初期画面が表示されています

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79353834-902da380-7f76-11ea-9f25-394f0139cdb6.png" height="400px" width="400px">
</div>

### 以降の開発の流れ
1. APを改修
2. ローカルで確認
```
ng serve --open
```
3. APをビルド
```
ng build --prod
```
4. 本番環境へデプロイ
```
firebase deploy
```

## Firebase Console解説(Hosting関連)

### Hosting ダッシュボード

- 以下のように

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79359141-29f84f00-7f7d-11ea-9212-3dc25c4334b8.png" height="400px" width="400px">
</div>


- カスタムドメインの追加
    - 本格的なサービスのリリースに向けてドメインを変えたい場合は、こちらの画面で簡単に登録可能です
<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79359307-675cdc80-7f7d-11ea-964f-a444bdb6ee8f.png" height="300px" width="400px">
</div>

- ドメインの取得
    - 簡単に取得できるサービスを二つ紹介しておきます
        - [Google Domain](https://domains.google.com/m/registrar/search?utm_medium=product&utm_source=firebase&utm_campaign=product%20inbound&searchTerm=) 
        - [お名前.com](https://www.onamae.com/)
            - 費用優先ならこれ

- Google Domains利用イメージ
    - 取得したいドメイン名で検索すると、利用可能なドメインと料金が出ます。以下の例では、.comはすでに使われているようです。

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79359856-0eda0f00-7f7e-11ea-9cfa-2a77da5575d4.png" height="300px" width="400px">
</div>

### 使用状況

- 以下のように時系列で使用状況がビジュアライズされます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79361347-24503880-7f80-11ea-846a-68a713792cc8.png" height="300px" width="300px">
</div>


### 使用料と請求額
- 上限と現在の使用料が分かります
- 無料のSparkプランでは"10GB/month"が上限になります
        - 重要なポイント
        - ビジネスをスモールスタートさせる段階では

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79356011-645fed00-7f79-11ea-95a3-c8578f97eab2.png" height="300px" width="400px">
</div>

- 容量について
    - 10 GB = 10487560 KB
    - 現在の使用料＝リクエスト×1
10485760

- 他のプランと料金を比較する際は以下をご確認ください
    - [Firebase 料金プラン](https://firebase.google.com/pricing?authuser=0)

- プランの変更
<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79357263-c10fd780-7f7a-11ea-97d4-a37ef188b40b.png" height="200px" width="400px">
</div>

### まとめ
今回の解説は以上になります。  

クラウドの最大のメリットはビジネスの規模に応じてPlatformを柔軟にスケールできることです。  
  
サービスの初期段階ではFirease HostingのSparkプランを使って無料でスモールスタートを切り、人気・収益を確保してから有料プランに移行というのが、スタートアップにありがちな手法です。  
  
ここまででその手法を身に着けることができたと思います。良いアイデアを思いついたら是非Hostingを使って広告でもつけて公開してみましょう。淡々と学習するよりも、遥かにモチベーションが向上します。

- 追記：ng deployやangular-fireを使って、firebaseコマンドを使わずにデプロイする手法もあるようです
    - [Angular入門: デプロイ](https://angular.jp/start/start-deployment)

### おまけ: firebase.jsonの中身
- 以下にデプロイの設定が記載されています
    - Angularなら以下のようになっていればOK
```
firebase.json
{
  "hosting": {
    "public": "dist/angular-pj-name",
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```
