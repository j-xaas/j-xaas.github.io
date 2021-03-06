---
title: Angular x AWS SDK for JavaScriptの始め方
date: 2020-02-28 00:13:23
categories:
- Serverless Application Dev
- AWS
tags: 
- Angular
- AWS SDK for JavaScript
toc: true
alias: /2020/02/28/Angular-x-AWS-SDK-for-JavaScriptの始め方/
thumbnail: https://user-images.githubusercontent.com/41946222/84601524-bf4f8b00-aebb-11ea-9bf6-33f5f99da852.png
---

- Angularで開発していたアプリケーションにaws-sdkを導入する際の備忘録
    - [AWS SDK](https://aws.amazon.com/jp/tools/)を入れることで、アプリケーションからAWSのリソースを操作可能になります
        - ex.) S3やDynamoDBへのデータの格納、Cognitoによる認証、Lambdaの実行...
        - SDK: Software Development Kit
          - 特定のソフトウェアを開発する際に必要なツールのセット
- Angularの場合、公式の開発者ガイド通りにやると鬼のようにエラーが出るので共有しておきます。ひと手間必要でした。

<!--toc-->

## 1. aws sdk for javascriptのinstall
- angular PJ直下で実行
```
npm install aws-sdk --save-dev
```
- この時点でAPを確認すると、以下のエラーが大量に出ます
    - APの画面もブラウザに表示されなくなります

```
>ng serve --open

ERROR in node_modules/aws-sdk/clients/acm.d.ts:141:37 - error TS2591: Cannot find name 'Buffer'. Do you need to install type definitions for node? Try `npm i @types/node` and then add `node` to the types field in your tsconfig.
```

## 2. @types/nodeをinstallする
- Angular PJ直下で以下を実行
```
npm install --save @types/node

+ @types/node@8.9.5
updated 1 package and audited 19123 packages in 41.418s
found 0 vulnerabilities
```

## 3. tsconfig.json ファイルに以下を追記
- Angular PJ直下にあります
```
  "compilerOptions": {
    "types": ["node"]
  }
```

- 場所は以下
```
> code .\tsconfig.json
```

- 4. tsconfig.app.json にも、同じく追記
    - Angular PJ直下にあります
```
 "compilerOptions": {
    "types": ["node"]
  }
```

- エラーの解消を確認
  - ng serve時に正しくAPの画面が表示される
- ここまででAngular APからAWSのリソースとAPI連携する下準備が整いました
    - 以下についても後日UP予定です
        - AWSの各リソースの利用方法、実装方法
        - Ampliyfy(BaaS)でAWSと連携させる手法

## 参考
### 関連記事
- [[AWS SDK for JavaScript] WEBアプリ→AWSへのリクエスト認証時のエラーハンドリング](/AWS-SDK-for-JavaScript-WEBアプリ→AWSへのリクエスト認証時のエラーハンドリング/)
- [[Angular mat-input] バリデーションまとめ](/Angular-mat-input-バリデーションまとめ/)
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)