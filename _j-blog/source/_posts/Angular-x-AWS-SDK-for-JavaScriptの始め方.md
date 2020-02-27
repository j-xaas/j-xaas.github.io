---
title: Angular x AWS SDK for JavaScriptの始め方
date: 2020-02-28 00:13:23
tags: 
- Angular
- AWS SDk for JavaScript
---

- angularで開発していたアプリケーションにaws-sdkを導入する際の備忘録
    - [AWS SDK](https://aws.amazon.com/jp/tools/)を入れることで、アプリケーションからAWSのリソースを操作可能になります
        - ex.) S3やDynamoDBへのデータの格納、Cognitoによる認証、Lambdaの実行...
- Angularの場合、公式の開発者ガイド通りにやると鬼のようにエラーが出るので共有しておきます。ひと手間必要でした。

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