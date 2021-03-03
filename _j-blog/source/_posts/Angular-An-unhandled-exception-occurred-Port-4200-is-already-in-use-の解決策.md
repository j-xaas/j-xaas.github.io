---
title: '[Angular] An unhandled exception occurred: Port 4200 is already in use の解決策'
date: 2021-03-03 14:47:47
categories:
- Serverless Application Dev
- Angular
tags: 
- Angular
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/104157486-8d40e000-542e-11eb-9c5d-786957fff2b4.png
---

<!-- toc -->

ng serve実行時のエラーについてのメモ

## Error
- 以下のエラーが出る
```
> ng serve
An unhandled exception occurred: Port 4200 is already in use. Use '--port' to specify a different port.
```

## 解決策
Port 4200のプロセスをkillする

- ポートが:4200と表示されているプロセスを探す
```
> netstat -a -n -o
プロトコル  ローカル アドレス  外部アドレス         状態            PID
TCP         127.0.0.1:62279   127.0.0.1:4200     ESTABLISHED     18492
```
- pidを指定してプロセスをkill
```
> taskkill -f /pid 8464
成功: PID 18492 のプロセスは強制終了されました。
```
- 以上でserve 可能になる
```
> ng serve
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
- https://stackoverflow.com/questions/39091735/port-4200-is-already-in-use-when-running-the-ng-serve-command

