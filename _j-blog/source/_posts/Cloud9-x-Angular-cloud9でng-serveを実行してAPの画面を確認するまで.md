---
title: '[Cloud9 x Angular] cloud9でng serveを実行してAPの画面を確認するまで'
date: 2020-04-14 02:04:12
categories:
- Serverless Application Dev
- AWS
tags: 
- Angular
- AWS
- cloud9
toc: true
---

Angularの開発であれば、小まめにWEB APの画面を確認しながら進めるのが一般的だと思います。ただ、開発環境にcloud9を採用する場合は、ng serveを実行する際に一手間必要になります。初級者が必ずと言っていいほどハマるポイントなので解説をおいておきます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79135318-1d8fbd00-7dea-11ea-9e6d-31134f1ad284.png" height="500px" width="500px">
</div>

<!-- toc -->

## cloud9上でのserve
- Localとcloud9の比較
|開発環境|ng serveでAPが動作する場所|
|--|--|
|Local端末|localhost(ローカルの端末上)|
|cloud9|実態はAWSのEC2インスタンス(仮想サーバー)|

- cloud9上でserveを実行しても、EC2インスタンス上でAPが動きます
    - いつものようにlocalhostを見ても、WEB APを確認できるはずが無いわけです
    - そこでCloud9のAPの画面を見るにはPreviewという機能を使用します
- Preview
    - トップの画像の様にcloud9の画面の一枠にweb APの画面を表示できます

- cloud9でpreviewにAPの画面を表示する為に必要なこと
    - portの指定(8080)
    - ホストの指定
        - PreviewのURLを指定します
    - disableHostCheck

## 1. previewの表示

- 上部タブから"Preview"を選択後"Preview Runnig Application"を選択するとCloud9上に表示できます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79140551-246efd80-7df3-11ea-96fb-53cf6c52fb14.png" height="120px" width="500px">
</div>

- PreviewのURL欄を控えてください
<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79140788-7dd72c80-7df3-11ea-95dc-1b8aa05ae4ff.png" height="90px" width="500px">
</div>


## 2. angular.jsonファイルを書き換えてポートを変更
- cloud9の場合はportを8080に設定する必要があります
    - 設定せずとも”--port 8080”を毎回オプションとして付ける手もあります
```
        "serve": {
          "builder": "@angular-devkit/build-angular:dev-server",
          "options": {
            "browserTarget": "NgTororo:build",
            "port": 8080 //ここを追加
          }
```

## 3. ng serveの実行（オプション指定）
```
ng serve --disableHostCheck  --public-host <cloud9のPreviewのURL> 
```

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79135318-1d8fbd00-7dea-11ea-9e6d-31134f1ad284.png" height="500px" width="500px">
</div>

以上です。このようなつまらないポイントで”半日失いました”は可哀そうなので、cloud9のビギナーを見かけたら教えてあげてください。