---
title: '[AWS SDK for JavaScript] WEBアプリ→AWSへのリクエスト認証時のエラーハンドリング'
date: 2020-09-17 21:02:12
categories:
- Serverless Application Dev
- AWS
tags: 
- Angular
- AWS SDK for JavaScript
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/84601524-bf4f8b00-aebb-11ea-9bf6-33f5f99da852.png
---

- WEBアプリ(Angular)から[AWS SDK](https://aws.amazon.com/jp/tools/)経由でAWSにアクセスする際に、Credential(AccessKey/SecretAccessKey)の認証が必須。そこでの認証エラーを検知する仕組みを作る手法についてのメモ
    - AgularでAWS SDKを利用する方法自体は以下に記載 
        - [Angular x AWS SDK for JavaScriptの始め方](/Angular-x-AWS-SDK-for-JavaScriptの始め方/)

## 認証エラーを検知 → アラート発信 ＆ 処理の中止
![AWS SDK経由のリクエストのイメージ](https://user-images.githubusercontent.com/68212997/93470108-5b11ec80-f92c-11ea-8547-3e318a1314a3.png)

WEBアプリからAWSへの通信時のエラーハンドリングについて、AWS SDK開発者ガイドを基に以下を検討した

- 実現したい仕様
    - AWS SDK経由のリクエスト実行時にクレデンシャルエラーを検知したい
        - クレデンシャルの認証エラーであることをユーザにエラーメッセージで教えたい
            - Credentialの問題なのか、リクエストの問題（例えば指定したリソースが存在しない）なのか知りたい
            - ユーザは原因を判断できないと、次のアクションが分からない
    - クレデンシャルエラーを検知した場合に処理をストップ
        - クレデンシャルに誤りがある場合、AWSへの複数ののリクエストを実行しても全て失敗する
        - 一つエラーが出た時点で止めることでUXを向上させたい
  
- [AWS SDK](https://aws.amazon.com/jp/tools/)とは
  - WEBアプリからAWSにリクエストする際に利用するもの
  - AWS Credential(accessKeyId, secretAccessKey)が必要
    - 今回は↑が間違っていた場合に検知したい
  - サービスオブジェクトメソッドは呼び出されると、AWS.Response オブジェクトをコールバック関数に渡すことで返す
  - AWS.Response オブジェクトのプロパティを通じて、レスポンスの内容にアクセス可能
​
- AWS.Response オブジェクトのプロパティ x 2
    1. data プロパティ
    2. error プロパティ
        - ここの内容でエラーの種類を見分けられそう
​
## エラーメッセージの検証
- Credentialの認証エラー時のメッセージを確認してみる
```
// aws-sdkを変数AWSとしてimport
import * as AWS from 'aws-sdk';
​
region = XXXX;
​
// AWS SDK各サービスのオブジェクトをまとめて宣言
let ec2 = new AWS.EC2({region: region});
​
// Ex. VPCの情報をリクエスト（describeVpcs）
// AWSにリクエストを発信し、返り値(dataプロパティ ＆ errorプロパティ)に応じた処理を実行
ec2.describeVpcs({}, (err, data ) => {
  if (err) { // Error発生時の処理
    // 以下でエラーメッセージを見てみる
    alert(err); // AuthFailure: AWS was not able to validate the provided access credentials
  }
  else { // データを取得した際の処理
    resolve(data); // 問題無くresolveしたら()の値を返す
  }
});
​
```

- Credentialのエラーメッセージは以下が出た
  - alert(err)の結果
```
AuthFailure: AWS was not able to validate the provided access credentials
```

![alertで確認したエラーメッセージ](https://user-images.githubusercontent.com/68212997/93469930-243bd680-f92c-11ea-9495-ef18f088434a.png)
​
​
- パターンマッチで、上記のエラーメッセージの場合に以下を実行させる
  - エラーメッセージの表示
    - "AWS Credentialに誤りがあります"と表示する
  - 処理の停止
    - AWSへの他のリクエストも同時に実行しようとしていた場合、全てストップする
​
- errはString型ではない(AWS.Responseオブジェクト型)ので、パターンマッチをするには型変換が必要
```
let errorMessage = String(err);
​
if (errorMessage = "AuthFailure: AWS was not able to validate the provided access credentials") { 
  // credential error時の処理
​
}
```
​​
## 実装
```
// aws-sdkを変数AWSとしてimport
import * as AWS from 'aws-sdk';
​
region = XXXX;
​
// AWS SDK各サービスのオブジェクトをまとめて宣言
let ec2 = new AWS.EC2({region: region}); // リージョンを設定
let as = new AWS.AutoScaling({region: region});
let iam = new AWS.IAM({apiVersion: '2010-05-08'});
​
​
// VPCの情報をリクエストする（describeVpcs）場合
​
// AWSにリクエストを発信し、返り値(dataプロパティ ＆ errorプロパティ)に応じた処理を実行
​
ec2.describeVpcs({}, (err, data ) => {
  if (err) { // Error発生時の処理
    let errorMessage = String(err);
    // credential errorの場合
    if (errorMessage = "AuthFailure: AWS was not able to validate the provided access credentials") { 
      //credential error時の処理を実行する関数を呼ぶ
      stopRequest(errorMessage);
    }
    else { // Credential以外のエラーの場合
      alert(region +" describeVpcs Error");
    }
  }
  else { // データの取得に成功した際の処理
    resolve(data);　// 問題無くresolveしたら()の値を返す
  } 
});
​
// Credential Error時の処理　停止 エラーをfiledata-baseに返す？
let stopRequest= (errorMessage) => {
  // エラーメッセージを表示
  alert(errorMessage); // そのままアラート
  alert("AWS Credentialに誤りがあります"); // 日本語でアラート
  // 処理を強制終了してエラーメッセージを返す
  return("Credential Error");
}
```
​
- 補足：上記のアロー関数を普通に書くと以下
  - Angularでは基本的に無名関数では無く、アロー関数で書いた方が良い
```
function stopRequest (errorMessage) {
  // エラーメッセージを表示
  alert(errorMessage); // AuthFailure: AWS was not able to validate the provided access credentials
  // 処理を強制終了
  return("Credential Error");
}
```
​
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

### その他
- [AWS Javascript SDK レスポンスオブジェクトの使用](https://docs.aws.amazon.com/ja_jp/sdk-for-javascript/v2/developer-guide/the-response-object.html)
​
​
​