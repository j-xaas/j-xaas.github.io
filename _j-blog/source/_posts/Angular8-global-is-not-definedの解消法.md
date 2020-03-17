---
title: Angular8:”global is not defined”の回避策
date: 2020-03-09 23:38:20
categories:
- Serverless Application Dev
- SPA (Angular)
tags: 
- Angular
- ”global is not defined”
- AWS SDK for JavaScript
toc: true
---

<!--toc-->

## 概要
- Angular8で以下のエラーにハマった際の解消法を解説します
```
”global is not defined”
```

- 上記のエラーについて
    - Angularでグローバルオブジェクトを参照する外部ライブラリを利用している環境で発生する事象
    - 今回はAngularとAWS間のAPI連携機能を実装した際に発生

- 環境の詳細
```
Angular CLI: 8.3.24
Node: 10.16.3
OS: win32 x64
Angular: 8.2.14
... animations, common, compiler, compiler-cli, core, forms
... language-service, platform-browser, platform-browser-dynamic
... router

Package                           Version
-----------------------------------------------------------
@angular-devkit/architect         0.803.24
@angular-devkit/build-angular     0.803.24
@angular-devkit/build-optimizer   0.803.24
@angular-devkit/build-webpack     0.803.24
@angular-devkit/core              8.3.24
@angular-devkit/schematics        8.3.24
@angular/cdk                      8.2.3
@angular/cli                      8.3.24
@angular/material                 8.2.3
@ngtools/webpack                  8.3.24
@schematics/angular               8.3.24
@schematics/update                0.803.24
rxjs                              6.4.0
typescript                        3.5.3
webpack                           4.39.2
```

## 解決策
- pollyfills.tsに設定が必要
    - 所在
        - "pj-name"\srcの配下
    - 以下を追記すると解決します
    ```
    // "global is not defined"の対応
    (window as any).global = window;
    ``` 

### 解説
- polyfillとは
    - JavaScriptのversion間の互換性を補うもの
    - 利用したい機能に未対応のブラウザでも使えるように、同等の機能をJavaScriptで供給できる
- pollyfills.ts
    - Angularにおけるpolyfillの設定ファイル
    - 例えば、Angularで開発したAPをIEでも動かしたい時には設定が必要
        - Angularは”デフォルトではIEに未対応”です