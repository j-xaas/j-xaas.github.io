---
title: '[Ionic Framework] Web/iOS/AndroidアプリのBuild方法'
date: 2021-03-29 18:02:03
category:
- Serverless Application Dev
- Mobile (Ionic)
tags:
- Ionic
- Angular
- iOS
- Android
- クロスプラットフォーム
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/116813817-bda0bb80-ab90-11eb-8b5c-f1ca7fd6977e.png
---

- 開発したIonic ProjectからWEB/iOS/Androidアプリをビルドする方法についてのメモ
    - Ionicの概要と初期開発については以下を参照
        - [[Ionic入門] Angular/React/VueベースでWeb/iOS/Androidを同時開発可能なクロスプラットフォームフレームワーク](/Ionic入門-Angular-React-VueベースでWeb-iOS-Androidを同時開発するクロスプラットフォームフレームワーク/)

<!--toc-->

## 前提： モバイルの動作確認
- 以下のコマンドでiOS/Andoridのそれぞれの画面を動作確認できる
```
ionic serve --lab
```

![Ionic FirstApp Temaplate](https://user-images.githubusercontent.com/68212997/116815792-1cb6fe00-ab9a-11eb-8526-0076f79cafe1.png)

## Ionic ProjectのBuild
- 単一のIonic Projectから、三種のAPをビルドできる
    - WEB(SPA), iOS, Android
- 生成されたソースを各アプリストアで公開する

## WebアプリとしてBuild
- ここはベースとするフレームワークと同様
    - /dist配下にSPAとして動作するソースが払い出される（Angularベースの場合）
    - S3やFirebase Hostingに置けばサービス公開できる

- WEB APのビルドコマンド
```
ionic build --prod
```

## モバイルアプリとしてBuild

### buildに必要なパッケージをPJにインストール
- /wwwディレクトリが生成される
```
ionic integrations enable capacitor
```

### iOS
- buildしてiOSアプリとして動かす
    - /iosディレクトリが生成される
    - ここのソースをApple Storeに公開すれば良い
```
ionic cap add ios
```

以下はMac前提。IonicならMacでなくとも開発自体は問題なくできるが、動作確認をするにはMacである必要がある

- Xcodeを起動して実機確認
    - XCodeの概要とインストール方法は以下を参照
        - [モバイルアプリ開発用ツールのインストール](/Ionic入門-Angular-React-VueベースでWeb-iOS-Androidを同時開発するクロスプラットフォームフレームワーク/#モバイルアプリ開発用ツールのインストール)

- iPhoneがあれば連携可能

### Android
- /androidディレクトリが生成される
    - ここのソースをGoogle Playに公開すれば良い
```
ionic cap add android
```
- Android Studioを起動して実機確認
    - Android Studioの概要とインストール方法については以下を参照
        - [モバイルアプリ開発用ツールのインストール](/Ionic入門-Angular-React-VueベースでWeb-iOS-Androidを同時開発するクロスプラットフォームフレームワーク/#モバイルアプリ開発用ツールのインストール)

----
## 参考
### 関連記事
- [[Ionic入門] Angular/React/VueベースでWeb/iOS/Androidを同時開発可能なクロスプラットフォームフレームワーク](/Ionic入門-Angular-React-VueベースでWeb-iOS-Androidを同時開発するクロスプラットフォームフレームワーク/)
- [[Angular入門] AngularCLI環境構築~PJの開始,コンポーネントの生成,初めての関数開発](/Angular入門-AngularCLI環境構築-PJの開始-コンポーネントの生成-初めての関数開発/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
- [Angular x AWS SDK for JavaScriptの始め方](/Angular-x-AWS-SDK-for-JavaScriptの始め方/)
- [[Angular/TypeScript(JavaScript)] 非同期処理/待ち合わせ処理のまとめ (Observable/subscribe/forkJoin/Promise/async/await/then)](/Angular-TypeScript-JavaScript-非同期処理-待ち合わせ処理のまとめ-Observable-Promise-async-await/)
- [[Angular JavaScript] JSONデータのファイル化と出力 取得したデータを任意の名称で保存するロジック (TypeScript)](/Angular-JSONデータのファイル化と出力-クラウドから取得したデータを任意の名称で保存する/)
- [[Angular] ng e2e Test 証明書エラーでハマった際の回避策(unable to get local issuer certificate))](/Angular-ng-e2e-Test-証明書エラーでハマった際の回避策/)
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular Service入門] ロジックを切り出し、複数Componentで再利用可能にする](/Angular-Service入門-ロジックを切り出し、複数Componentで再利用可能にする/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)
- [[Angular JavaScript] JSONデータのファイル化と出力 ~取得したデータを任意の名称で保存するロジック~ (TypeScript)](/Angular-JSONデータのファイル化と出力-クラウドから取得したデータを任意の名称で保存する/)
- [[Angular JavaScript] JSONファイル(複数)の読み込み](/Angular-JavaScript-JSONファイルの読み込み/)
- [Angular x AWS SDK for JavaScriptの始め方](/Angular-x-AWS-SDK-for-JavaScriptの始め方/)
- [静的ホスティングサービスまとめ(Netlify/S3/Firebase Hosting/Github Pages)](/静的ホスティングサービスまとめ-Netlify-S3-Firebase-Hosting-Github-Pages/)
- [[Angular ReactiveForm x mat-input] 一つの入力欄のエラーメッセージを出し分ける](/Angular-ReactiveForm-x-mat-input-一つの入力欄のエラーメッセージを出し分ける/)
- [[Angular mat-input] バリデーションまとめ](/Angular-mat-input-バリデーションまとめ/)
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)

### その他
- [Ionicで作る モバイルアプリ制作入門[Angular版]<Web/iPhone/Android対応>](https://amzn.to/335cTj9)
- [Ionicで作る モバイルアプリ制作入門〈Web/iPhone/Android対応〉](https://amzn.to/3tbX5pA)
- [Ionic Framework - 日本語ドキュメンテーション](https://ionicframework.com/jp/docs/)
- [Ionic Framework - UIコンポーネント](https://ionicframework.com/jp/docs/components)
- [Ionic Framework - Native APIs](https://ionicframework.com/jp/docs/native)
