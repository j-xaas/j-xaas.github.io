---
title: '[Angular] ng e2e Test 証明書エラーでハマった際の回避策(unable to get local issuer certificate))'
date: 2021-01-11 16:18:54
categories:
- Serverless Application Dev
- SPA (Angular)
tags: 
- Angular
- 'e2e test'
thumbnail: https://user-images.githubusercontent.com/68212997/104157486-8d40e000-542e-11eb-9c5d-786957fff2b4.png
toc: true
---

AnglarのE2Eテストを会社の開発環境で試みたところ、証明書エラーでハマってしまった際のメモ

- E2Eテスト自体の概要は以下にまとめています
    - [[Angular] E2E Testの自動化](/Angular-E2E-Testの自動化/)

## Error詳細
- E2Eテストの実行時に以下の様なエラーが出る
```
\angular-pj> ng e2e
[21:20:51] I/config_source - curl -oD:\angular-pj\node_modules\protractor\node_modules\webdriver-manager\selenium\chrome-response.xml https://chromedriver.storage.googleapis.com/
events.js:174
      throw er; // Unhandled 'error' event
      ^

Error: unable to get local issuer certificate
    at TLSSocket.onConnectSecure (_tls_wrap.js:1058:34)
    at TLSSocket.emit (events.js:198:13)
    at TLSSocket._finishInit (_tls_wrap.js:636:8)
Emitted 'error' event at:
    at Request.onRequestError (D:\angular-pj\node_modules\request\request.js:877:8)
    at ClientRequest.emit (events.js:198:13)
    at TLSSocket.socketErrorListener (_http_client.js:392:9)
    at TLSSocket.emit (events.js:198:13)
    at emitErrorNT (internal/streams/destroy.js:91:8)
    at emitErrorAndCloseNT (internal/streams/destroy.js:59:3)
    at process._tickCallback (internal/process/next_tick.js:63:19)
```

- webdrive-managerのアップデートにより解決した事例を真似てupdateを試みたものの、同様のエラーで進めない状況
    - Error: unable to get local issuer certificate
```
PS D:\angular-pj>  webdriver-manager update --version.chrome=86.0.4240.75
webdriver-manager: using global installed version 12.1.7
[21:24:30] I/config_source - curl -oD:\Users\XXXXXXXXX\AppData\Roaming\npm\node_modules\webdriver-manager\selenium\standalone-response.xml https://selenium-release.storage.googleapis.com/
[21:24:30] I/config_source - curl -oD:\Users\XXXXXXXXX\AppData\Roaming\npm\node_modules\webdriver-manager\selenium\chrome-response.xml https://chromedriver.storage.googleapis.com/
[21:24:30] I/config_source - curl -oD:\Users\XXXXXXXXX\AppData\Roaming\npm\node_modules\webdriver-manager\selenium\gecko-response.json https://api.github.com/repos/mozilla/geckodriver/releases
events.js:174
      throw er; // Unhandled 'error' event
      ^

Error: unable to get local issuer certificate
    at TLSSocket.onConnectSecure (_tls_wrap.js:1058:34)
    at TLSSocket.emit (events.js:198:13)
    at TLSSocket._finishInit (_tls_wrap.js:636:8)
Emitted 'error' event at:
    at Request.onRequestError (D:\Users\XXXXXXXXX\AppData\Roaming\npm\node_modules\webdriver-manager\node_modules\request\request.js:877:8)
    at ClientRequest.emit (events.js:198:13)
    at TLSSocket.socketErrorListener (_http_client.js:392:9)
    at TLSSocket.emit (events.js:198:13)
    at emitErrorNT (internal/streams/destroy.js:91:8)
    at emitErrorAndCloseNT (internal/streams/destroy.js:59:3)
    at process._tickCallback (internal/process/next_tick.js:63:19)
```

## Protoractor/ng e2eについて基礎知識
- Protoractorは内部的にSelenium WebDriverというライブラリを利用しており、入力やボタン操作などの操作の仕組みを標準で備えている

- Protoractor(Webdriver)は単体では直接ブラウザーを操作できない
  - 内部的には、中計サーバーであるSelenium Serverを介して対象にアクセスする
![protoractor](protoractor.jpg)

- directConnect
  - 有効化した場合、ChromeとFirefoxはSeleniumServerを立ち上げずにアクセスできる

- angular-pj\node_modules\selenium-webdriver>
  - pj配下にseleniumは元から入っているように見受けられる


## 証明書エラー解決策: 証明書検証の回避
- オプションにより証明書の検証を回避できる
  - --ignore_ssl
```
angular-pj> .\node_modules\.bin\webdriver-manager update --proxy=XXXX --ignore_ssl
angular-pj> ng e2e --no-webdriver-update
```


## 環境情報
- package.json

```
{
  "name": "XXXXXXXXX",
  "version": "0.0.0",
  "scripts": {
    "ng": "ng",
    "start": "ng serve",
    "build": "ng build",
    "test": "ng test",
    "lint": "ng lint",
    "e2e": "ng e2e"
  },
  "private": true,
  "dependencies": {
    "@angular/animations": "~8.2.13",
    "@angular/cdk": "~8.2.3",
    "@angular/common": "~8.2.13",
    "@angular/compiler": "~8.2.13",
    "@angular/core": "~8.2.13",
    "@angular/forms": "~8.2.13",
    "@angular/material": "^8.2.3",
    "@angular/platform-browser": "~8.2.13",
    "@angular/platform-browser-dynamic": "~8.2.13",
    "@angular/router": "~8.2.13",
    "hammerjs": "^2.0.8",
    "jquery": "^3.4.1",
    "ng-material-treetable": "^0.5.5",
    "rxjs": "~6.4.0",
    "tslib": "^1.14.0",
    "zone.js": "~0.9.1"
  },
  "devDependencies": {
    "@angular-devkit/build-angular": "^0.803.29",
    "@angular/cli": "^8.3.29",
    "@angular/compiler-cli": "~8.2.13",
    "@angular/language-service": "~8.2.13",
    "@types/jasmine": "~3.3.8",
    "@types/jasminewd2": "~2.0.3",
    "@types/node": "^8.10.64",
    "aws-sdk": "^2.768.0",
    "codelyzer": "^5.2.2",
    "jasmine-core": "~3.4.0",
    "jasmine-spec-reporter": "~4.2.1",
    "karma": "~4.1.0",
    "karma-chrome-launcher": "~2.2.0",
    "karma-coverage-istanbul-reporter": "~2.0.1",
    "karma-jasmine": "~2.0.1",
    "karma-jasmine-html-reporter": "^1.5.4",
    "protractor": "^5.4.4",
    "ts-node": "~7.0.0",
    "tslint": "~5.15.0",
    "typescript": "~3.5.3"
  }
}
```

```
ng --version
> ng --version

     _                      _                 ____ _     ___
    / \   _ __   __ _ _   _| | __ _ _ __     / ___| |   |_ _|
   / △ \ | '_ \ / _` | | | | |/ _` | '__|   | |   | |    | |
  / ___ \| | | | (_| | |_| | | (_| | |      | |___| |___ | |
 /_/   \_\_| |_|\__, |\__,_|_|\__,_|_|       \____|_____|___|
                |___/
    

Angular CLI: 8.3.29
Node: 10.16.3
OS: win32 x64
Angular: 8.2.14
... animations, common, compiler, compiler-cli, core, forms
... language-service, platform-browser, platform-browser-dynamic
... router

Package                           Version
-----------------------------------------------------------
@angular-devkit/architect         0.803.29
@angular-devkit/build-angular     0.803.29
@angular-devkit/build-optimizer   0.803.29
@angular-devkit/build-webpack     0.803.29
@angular-devkit/core              8.3.29
@angular-devkit/schematics        8.3.29
@angular/cdk                      8.2.3
@angular/cli                      8.3.29
@angular/material                 8.2.3
@ngtools/webpack                  8.3.29
@schematics/angular               8.3.29
@schematics/update                0.803.29
rxjs                              6.4.0
typescript                        3.5.3
webpack                           4.39.2
npm
npm -v
6.9.0
```

## その他
- Angularに関するトラブルシューティングはコミュニティ(ng japan)に質問すると解決できることが多いです
    - https://angular-japan.discourse.group/


## 参考
### 関連記事
- [[Angular] E2E Testの自動化](/Angular-E2E-Testの自動化/)
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
- [[Angular Service入門] ロジックを切り出し、複数Componentで再利用可能にする](/Angular-Service入門-ロジックを切り出し、複数Componentで再利用可能にする/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)
- [[Angular JavaScript] JSONファイル(複数)の読み込み](/Angular-JavaScript-JSONファイルの読み込み/)
- [Angular x AWS SDK for JavaScriptの始め方](/Angular-x-AWS-SDK-for-JavaScriptの始め方/)
- [静的ホスティングサービスまとめ(Netlify/S3/Firebase Hosting/Github Pages)](/静的ホスティングサービスまとめ-Netlify-S3-Firebase-Hosting-Github-Pages/)
- [[Angular ReactiveForm x mat-input] 一つの入力欄のエラーメッセージを出し分ける](/Angular-ReactiveForm-x-mat-input-一つの入力欄のエラーメッセージを出し分ける/)
- [[Angular mat-input] バリデーションまとめ](/Angular-mat-input-バリデーションまとめ/)
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)

### その他
- [ng japan user group](https://angular-japan.discourse.group/)
- [ng e2e実行時のエラー(Error: unable to get local issuer certificate) 証明書の登録方法について](https://angular-japan.discourse.group/t/topic/246)