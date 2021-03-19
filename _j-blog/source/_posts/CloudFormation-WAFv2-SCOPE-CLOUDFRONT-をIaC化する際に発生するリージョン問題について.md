---
title: '[CloudFormation] WAFv2(SCOPE: CLOUDFRONT)をIaC化する際に発生するリージョン問題について'
date: 2021-03-19 19:36:08
categories:
- Serverless Application Dev
- AWS
tags: 
- CloudFormation
- WAFv2
- CloudFront
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/111768125-de67c700-88ea-11eb-9aa6-e31391e9a14f.png
---

AWSでCloudFront Distributionに関連付けるWAFv2リソースをCFnテンプレート化する際の注意点についてのメモ

<!--toc-->

## 発生する事象
- CFnテンプレートの書式が正しくても、CFnを実行するリージョンによって以下のエラーが発生する
```
Error reason: The scope is not valid., 
field: SCOPE_VALUE, parameter: 
CLOUDFRONT (Service: Wafv2, Status Code: 400, Request ID: ...
```

## 原因: WAFv2のSCOPE: CLOUDFLONT
- WAFv2は`SCOPE`によって、us-east-1にしか生成できないことがある
  - CloudFormation Distributionに関連づけるWAFはコンソールであれば、Globalとするが、`実体はus-east-1に生成される`
  - CFnでは"SCOPE"というプロパティで、WAFv2がCloudFront向けか、各リージョンに生成するか定義する
    - 以下のように定義した場合、cloudFront Distributionにアタッチ可能なWAFv2リソースとなるが、
    ```
    Scope: "CLOUDFRONT"
    ```
- SCOPEについてのルールは[公式ドキュメント](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-resource-wafv2-webacl.html#cfn-wafv2-webacl-scope)に以下のように書かれていた
```
CLOUDFRONT の場合、米国東部 (バージニア北部) リージョン (us-east-1) で WAFv2 リソースを作成する必要があります。
```

- CloudFormtationは選択中のリージョンで実行される
  - `SCOPE: CLOUDFRONTのWAFv2リソースがあれば、us-east-1で実行する必要がある`
  - 同じテンプレートで定義した他のリソース群も、us-east-1に生成されてしまう
    - 払い出せるリージョンが実質ロックされるので、汎用性が欠けたテンプレートになってしまう...

## 回避策
以下が考えられる

- テンプレートの分割
  - WAFv2リソースのみ別にする
    - CloudFront Distributionとの関連付けが面倒ではある
- us-east-1専用テンプレートとしてルール付けする

同様の問題を経験した人も多いはずなので、そのうちAWS側で解消されるかもしれない


## 参考
### 関連記事
- [Angular x AWS SDK for JavaScriptの始め方](/Angular-x-AWS-SDK-for-JavaScriptの始め方/)

- [[Angular JavaScript] JSONデータのファイル化と出力 ~取得したデータを任意の名称で保存するロジック~ (TypeScript)](/Angular-JSONデータのファイル化と出力-クラウドから取得したデータを任意の名称で保存する/)
- [[Angular JavaScript] JSONファイル(複数)の読み込み](/Angular-JavaScript-JSONファイルの読み込み/)
- [静的ホスティングサービスまとめ(Netlify/S3/Firebase Hosting/Github Pages)](/静的ホスティングサービスまとめ-Netlify-S3-Firebase-Hosting-Github-Pages/)
- [[Angular ReactiveForm x mat-input] 一つの入力欄のエラーメッセージを出し分ける](/Angular-ReactiveForm-x-mat-input-一つの入力欄のエラーメッセージを出し分ける/)
- [[Angular mat-input] バリデーションまとめ](/Angular-mat-input-バリデーションまとめ/)
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)
