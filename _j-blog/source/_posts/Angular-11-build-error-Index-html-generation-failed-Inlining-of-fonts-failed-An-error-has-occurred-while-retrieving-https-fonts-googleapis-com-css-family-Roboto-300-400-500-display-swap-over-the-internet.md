---
title: >-
  Angular 11 buid error: Index html generation failed.Inlining of fonts failed.
  An error has occurred while retrieving
  https://fonts.googleapis.com/css?family=Roboto:300,400,500&display=swap over
  the internet.
date: 2021-03-03 15:23:24
categories:
- Serverless Application Dev
- Angular
tags: 
- Angular
- aot
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/104157486-8d40e000-542e-11eb-9c5d-786957fff2b4.png
---

- Angular 11で有効化されたフォント最適化機能について
- プロキシ環境下で開発をしている場合、ng buildでプロキシエラーが発生してしまう

## 発生した事象
- Anular11 へのアップデート後に、ng buildで以下のエラーが発生する
```
> ng build --prod
√ Browser application bundle generation complete.
√ ES5 bundle generation complete.
√ Copying assets complete.
× Index html generation failed.
Inlining of fonts failed. An error has occurred while retrieving https://fonts.googleapis.com/css?family=Roboto:300,400,500&display=swap over the internet.
connect ETIMEDOUT 
```

## 原因
- Angular 11からインラインフォントへの新機能が追加されている
- プロキシ環境下で開発を行っている場合は、build時にプロキシを通過できずエラーが発生してしまう
  - 正しくHTTP_PROXY/http_proxyの環境変数を設定していても無視されてしまう

## 回避策
- 幾つかGitのissueに同様の事象が挙がっていたが、以下が結論であった
```
ビルドサーバーはファイアウォールの外にはアクセスできません。それをオフにするのが唯一の選択肢だと思います。
```

- font optimizationをオフにする方法
  - [Angular workspace configuration](https://angular.io/guide/workspace-config#optimization-and-source-map-configuration)
    - [Fonts optimization options](https://angular.io/guide/workspace-config#fonts-optimization-options)

### 要約
- 設定ファイル(angular-pj/angular.json)でbuildの設定を変更することで解決可能
- デフォルトの設定
  - "optimization": trueとなっている
```
  "projects": {
    "app_name": {
      ...
      "architect": {
        "build": {
          "builder": "@angular-devkit/build-angular:browser",
          "options": {
            ...
          },
          "configurations": {
            "production": {
              "fileReplacements": [
                {
                  "replace": "src/environments/environment.ts",
                  "with": "src/environments/environment.prod.ts"
                }
              ],
              "optimization": true,
```

- angular.jsonの各項目について
  - "architect"
    - コンパイルやテスト実行などの複雑なタスクを実行するためにCLIが使用するツール
    - ターゲットの設定に従って、指定されたタスクを実行するために指定されたビルダーを実行するシェル
  - ["build"](https://angular.io/guide/workspace-config#build-target)
  - "configurration"
    - このセクションでは、異なる目的地のための代替構成を定義し、名前を付ける。このセクションには、それぞれの設定のためのセクションがあり、その環境でのデフォルトのオプションを設定する。
  - ["optimization"](https://angular.io/guide/workspace-config#optimization-configuration)
    - より細かい設定を行うために、ブール値かオブジェクトのいずれかを指定できる。このオプションを使用すると、ビルド出力のさまざまな最適化が可能になる
    - fonts
    ```
    フォントの最適化を有効にします。
    注意: これにはインターネットへのアクセスが必要です
    ```
    - [fonts optimsization options](https://angular.io/guide/workspace-config#fonts-optimization-options)
      - inline
        ```
        アプリケーションの HTML インデックス ファイル内の外部の Google フォントとアイコンの CSS 定義をインライン化することで、レンダリング ブロック要求を減らします。
        注意:これにはインターネットへのアクセスが必要です。
        ```
        - [レンダリングブロック要求について](https://web.dev/render-blocking-resources/)

- 以下に変更
```
"optimization": { 
  "fonts": true
}
```
- 以上でbuildが問題なく実行できた
```
> ng build --prod
√ Browser application bundle generation complete.
√ ES5 bundle generation complete.
√ Copying assets complete.
√ Index html generation complete.
```

同様の事象はgitのissueで複数確認できたが、無効化する以外の解決策は見つけられなかった。


## 参考
### 関連記事
- [Angular x AWS SDK for JavaScriptの始め方](/Angular-x-AWS-SDK-for-JavaScriptの始め方/)
- [[Angular/TypeScript(JavaScript)] 非同期処理/待ち合わせ処理のまとめ (Observable/subscribe/forkJoin/Promise/async/await/then)](/Angular-TypeScript-JavaScript-非同期処理-待ち合わせ処理のまとめ-Observable-Promise-async-await/)
- [[Angular JavaScript] JSONデータのファイル化と出力 取得したデータを任意の名称で保存するロジック (TypeScript)](/Angular-JSONデータのファイル化と出力-クラウドから取得したデータを任意の名称で保存する/)
- [[Angular] ng e2e Test 証明書エラーでハマった際の回避策(unable to get local issuer certificate))](/Angular-ng-e2e-Test-証明書エラーでハマった際の回避策/)
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
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
- [Angularコミュニティ](https://angular-japan.discourse.group/)
- [Automatic Inlining of Fonts breaks in Angular 11 #19350](https://github.com/angular/angular-cli/issues/19350)
- [Inlining fonts behind HTTP proxy #19401](https://github.com/angular/angular-cli/issues/19401)
- [Angular workspace configuration](https://angular.io/guide/workspace-config#optimization-and-source-map-configuration)
- [Port and Proxy Config on ng-build](https://stackoverflow.com/questions/40995791/port-and-proxy-config-on-ng-build)
