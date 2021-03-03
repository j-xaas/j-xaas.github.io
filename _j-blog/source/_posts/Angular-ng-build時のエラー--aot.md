---
title: '[Angular] ng build時のエラー --aotについて'
date: 2021-03-03 15:20:28
categories:
- Serverless Application Dev
- Angular
tags: 
- Angular
- aot
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/104157486-8d40e000-542e-11eb-9c5d-786957fff2b4.png
---

- Angular 9以降でデフォルト化されたAOTについてのメモ
    - 8以前のバージョンからアップデートした際に注意すべきポイント

<!--toc-->

## 発生する事象
- Angularのバージョンアップ後、build実行時にエラーが発生する
    - --aot オプションが必要になる
    ```
    ng build --aot
    ```

## 原因/AOT
- Angular9 以降ではahead-of-time (AOT)コンパイラがデフォルトに採用されている

- [Ahead-of-time (AOT) コンパイラ](https://angular.jp/guide/aot-compiler)
    - 引用
    ```
    Angular の ahead-of-time (AOT) コンパイラ は、ブラウザがそのコードをダウンロードして実行する前に、
    ビルドフェーズ中にAngular HTML コードと TypeScript コードを効率的な JavaScript コードに変換します。
    ビルドプロセス中にアプリケーションをコンパイルすると、ブラウザでのレンダリングが速くなります。
    ```

- Angularには、アプリケーションをコンパイルする2つの方法がある
  - Just-in-Time (JIT) 
    - 実行時にブラウザ内でアプリケーションをコンパイルします。This was the default until Angular 8.
  - Ahead-of-Time (AOT) 
    - ビルド時にアプリとライブラリをコンパイルします。This is the default since Angular 9.

- ng build (build only)またはng serve (build and serve locally)のCLIコマンドを実行すると、
コンパイルの種類（JITかAOTか）は、angular.jsonで指定したビルド設定のaotプロパティの値に依存する
  - Angular 8以前で開始した場合はデフォルトでtrueではない

これらの要因により今回のエラーが発生したと思われる

## aotの有効化
- 設定を変更することで、buildの度にオプション(--aot)を付与せずともエラーを回避可能になる

- 以下の設定ファイルでbuildの設定を変更することで解決できる
  - aotをtrueとするだけ

- angular-pj/angular.json
```
        "build": {
          "builder": "@angular-devkit/build-angular:browser",
          "options": {
            "outputPath": "dist/reacq-pj",
            "index": "src/index.html",
            "main": "src/main.ts",
            "polyfills": "src/polyfills.ts",
            "tsConfig": "tsconfig.app.json",
            "aot": true, //こちらを変更
```


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
