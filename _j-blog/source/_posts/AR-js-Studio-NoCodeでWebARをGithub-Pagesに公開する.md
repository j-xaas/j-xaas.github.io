---
title: '[AR.js Studio] NoCodeでWebARをGithub Pagesに公開する '
date: 2020-08-10 23:03:57
categories:
- AR
- WebAR
tags:
- AR
- WebAR
- AR.js
- AR.js Studio
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/89791905-d8dd1e00-db5e-11ea-804f-980baebe1ac9.png
---

AR.jsというフレームワークを用いたAR開発の一部をNoCodeで実現する、AR.js Studio（WEBサービス）について解説します。
- AR.js自体の解説は↓にまとめました
    - [[AR.js x A-Frame] WebAR入門～マーカーベースで3Dオブジェクトを表示するAPを開発する～](/ar-js-x-a-frame-WebAR入門/)

## 概要
- AR.js Studio
    - AR.jsを活用したWEB ARをコードを書かず、ブラウザ上の操作だけで実現できるサービス
        - AR.js
            - AR(拡張現実)をWebブラウザ上で実現する技術
                - [[AR.js x A-Frame] WebAR入門～マーカーベースで3Dオブジェクトを表示するAPを開発する～](/ar-js-x-a-frame-WebAR入門/)
    - 作成したARはGithub PagesでWEBにそのまま公開することができます
        - Github Pages
            - 無料で静的コンテンツをホスティングするGithubの機能
            - 詳細は以下にまとめました
                - [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)
        - 利用に際して、Githubのアカウントが必要
    - 通常のAR.jsと比較して制限があるため、エンジニアは使わなくても良さそう

- 通常のAR.jsと比較した制限
    - 利用可能なデータの種類
        - 3Dモデルのうち複数のデータ形式を利用できません（Ex .obj, .mtl...）
    - 画像トラッキングベースのAR
        - AR.js Studioではマーカーベースとロケーションベースに限定される
    - データ容量
        - データ毎に最大の容量が定まっています

## 使用方法
- AR.js Studioにアクセス
    - https://ar-js-org.github.io/studio/

- Project Typeを選択
    - Location-based
        - 緯度経度を指定して、そこにARを置くタイプ
        - 指定した場所にカメラを向けるとオブジェクトが表示される
    - Marker-basede
        - Marker（特定の白黒の画像）をカメラに写ると、その上にオブジェクトを表示する

### Marker-base
- TypeよりMarker-basedを選択
<img width="892" alt="AR js Studio Marker-based" src="https://user-images.githubusercontent.com/68212997/89714313-a0abd300-d9d8-11ea-8004-5b8b76467625.png">

- Start Buidingを押下

#### 設定
1. Use a premade marker or upload your own
    - マーカーをオリジナルのものに変更することができます
    - デフォルトでは以下
    <img width="1104" alt="AR js 1 change marker" src="https://user-images.githubusercontent.com/68212997/89714508-2419f400-d9da-11ea-9ccd-2661921f0304.png">
    - 変更する場合はupload imageから任意の画像をアップロード
        - どのような画像がマーカーに向いているかは[公式ガイド](https://github.com/AR-js-org/studio/blob/master/what-makes-a-good-marker.md)をご確認ください
            - 基本白黒でシンプルなものが望ましい

2. Choose the content
    - ARで表示したいコンテンツをアップロードします
<img width="784" alt="AR js choose the contents" src="https://user-images.githubusercontent.com/68212997/89714599-d8b41580-d9da-11ea-8e3f-bfa4e5a8d933.png">

- 表示可能なコンテンツは以下
    - 3D Object (.gltf, .glb .zip; max size 50MB)
        - 通常のAR.jsであれば利用可能なobj形式が使えないのは微妙
    - Image (.jpg, .png, .gif; max size 15MB)
    - Video (.mp4; max size 25MB)

- 今回は3Dオブジェクトをアップロードしてみます
    - glbファイルを選択

3. Export the project
    - 以下の二種の利用方法があります
        - Publish on Github
            - Gituhub Pagesに生成されたソースを公開
        - Download package
            - ソースをLocalにダウンロード
            - 利用シーン
                - カスタマイズする場合
                    - AR.js Studioの制限で書けない範囲を開発できます
                - Github Pages意外でホスティングする場合
                    - Github Pagesは10万アクセスで落ちる仕様なので、本格的な運用（大規模なアクセスが予想されるサイト）には向いていません
                    - AWSのS3やGCPのFirebase Hostingに載せることになると思います
                        - [[AWS S3 x Angular] 静的WEBサイトホスティングでSPAを公開/ng build/公開範囲の限定/CI/CD化](/AWS-S3-x-Angular-静的WEBサイトホスティングでSPAを公開-公開範囲の限定/)
                        - [Firebase Hosting完全版(Angularで開発したSPAを無料で公開～ダッシュボードで費用管理)](/Firebase-Hosting完全版-Angularで開発したSPAを無料で公開～ダッシュボードで費用管理/)

#### Publish the project
<img width="518" alt="AR js Studio Publish the project" src="https://user-images.githubusercontent.com/68212997/89796410-99193500-db64-11ea-8305-a66932e8ba01.png">

- 公開するARのプロジェクト名を入力します
    - そのままURLになるので注意
- Publishを押下

- Githubのアカウント連携の許可を求められます
    - Authorize
<img width="870" alt="Github連携" src="https://user-images.githubusercontent.com/68212997/89714732-d8684a00-d9db-11ea-95d2-610fb6c3fd82.png">

- Github Pagesの自動生成を開始
<img width="617" alt="AR js loading" src="https://user-images.githubusercontent.com/68212997/89714763-136a7d80-d9dc-11ea-88eb-31a61c604fa5.png">

- 自動生成が完了
<img width="713" alt="リンクが共有されます" src="https://user-images.githubusercontent.com/68212997/89714883-b28f7500-d9dc-11ea-88c0-45a336f7e775.png">

- URLでAPが公開されている状態
    - アクセスすればアプリが起動し、マーカーをカメラで写すことでARを表示できます


## 関連記事
- [[AR.js x A-Frame] WebAR入門～マーカーベースで3Dオブジェクトを表示するAPを開発する～](/ar-js-x-a-frame-WebAR入門/)
- [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)
- [[AWS S3 x Angular] 静的WEBサイトホスティングでSPAを公開/ng build/公開範囲の限定/CI/CD化](/AWS-S3-x-Angular-静的WEBサイトホスティングでSPAを公開-公開範囲の限定/)
- [Firebase Hosting完全版(Angularで開発したSPAを無料で公開～ダッシュボードで費用管理)](/Firebase-Hosting完全版-Angularで開発したSPAを無料で公開～ダッシュボードで費用管理/)