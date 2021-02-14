---
title: '[Angular x AWS SDK for JavaScript] SPAからAWSのデータを取得する際の待ち合わせ処理(非同期処理)'
date: 2021-01-29 23:20:27
categories:
- Serverless Application Dev
- Angular
tags: 
- Angular
- AWS SDK for JavaScript
- 待ち合わせ処理
toc: true
thumbnail: https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/sdk-overview-v3.png
---

- SPAのプラットフォームにAWSを採用した際によく書くロジックについてのメモ
    - ex. クライアントからs3 bucketのobjectを取得して画面に表示

- AWS SDKでデータを取得するまではディベロッパーガイドを見れば容易だが、待ち合わせ処理の書き方が少し面倒
    - AWS SDKを導入するまでは以下を参照
        - [Angular x AWS SDK for JavaScriptの始め方](/Angular-x-AWS-SDK-for-JavaScriptの始め方/)

## 前提知識
- SPAであれば、基本的に以下の様な設計になる
    - 外部API(今回はAWS)へリクエストするロジックはServiceとして外だし
    - 各ComponentからServiceを呼び出し、返り値を受け取る

- TypeScript(JavaScript)で書いたプログラムは基本的に`非同期処理で動く`
    - Component側で返り値を扱うには`待ち合わせ処理にする必要がある`

- 待ち合わせ処理に用いるオブジェクトや構文についての詳細は以下にまとめた
    - [[Angular/TypeScript(JavaScript)] 非同期処理/待ち合わせ処理のまとめ (Observable/subscribe/forkJoin/Promise/async/await/then)](/Angular-TypeScript-JavaScript-非同期処理-待ち合わせ処理のまとめ-Observable-Promise-async-await/)

---

## 実装例
![AWS SDK for JavaScript](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/s3-photo-album-example.png)

- 想定する状況
    - Angularのコンポーネント生成時(画面遷移時)に自動でAWS(S3)から情報を取得して画面に表示する
    
- AWSからデータを取得して値を取り出すまでを待ち合わせ処理にする例
    1. ComponentからServiceのロジックを呼び出す
    2. Service内でAWSからデータを取得
      - 変数 = new Promise() の形式で、AWSへ問い合わせるロジックを書く
        - resolve(data)で変数に返り値を格納
      - 各関数間の待ち合わせ処理は async/await で実現する
    3. Promise型の返り値をComponentへ返す
    4. Component側で forkJoin().subscribe を使って受け取る  
    .then()で受け取ってもOK (Angularでは基本subscribeが推奨されている)

書き方は他にもあるかも

### AWS SDKを導入するまで
- 以下を参照
    - [Angular x AWS SDK for JavaScriptの始め方](/Angular-x-AWS-SDK-for-JavaScriptの始め方/)

### sampleの構成
- request.service.ts
    - AWSとの通信用のService

- sample.component.ts
    - Serviceを呼び出し、返り値を受け取るComponent

- sample.component.html    
    - 取得したデータを表示する

---

### AWSへリクエストするService
以下の様に書かないと、データが帰ってくる前にcomponentにundefinedを返してしまう

- AWSへのリクエストからの待ち合わせで返り値を取り出す方法
  - Promiseの中でリクエストする
  - GETしたデータをresolve()で返す
  - resolveで返された値をreturn
  
- async/await でService内の各関数間の待ち合わせ処理を実現する
    - 詳細は[別記事](/Angular-TypeScript-JavaScript-非同期処理-待ち合わせ処理のまとめ-Observable-Promise-async-await/)を参照


- Service記載例 (request.service.ts)
```
import { Injectable } from '@angular/core';

// aws-sdkを変数AWSとしてimportする必要がある
import * as AWS from 'aws-sdk' 

@Injectable({
  providedIn: 'root'
})

export class requestService {
  constructor() { }

  // 1. Componentから呼び出される関数
  async getAwsData() {
    return await this.requestData();
  }

  // 2. AWSに対してリクエストする関数
  async requestData() { 

    // 変数requestに返り値を格納　待ち合わせのためにPromiseを用いる
    const request = new Promise((resolve, reject) => { 

      // AWS SDKサービスのオブジェクトを宣言
      let bucket = new AWS.S3(({params: {Bucket: 'myBucket'}});

      // リクエストを実行 基本的に以下の様にdataに返り値が入る
      bucket.listObject(({}, (err, data ) => {
        if (err) {
          let errorMessage = String(err);
          alert(errorMessage);
        }
        else {
          // resolveでPromiseの処理を終了させる
          resolve(data); // データ加工が必要であれば、ここで関数を呼び出す
        }
      });
    });

    // エラーハンドリング
    request.catch((error) => {alert(error); } );
    // リクエスト結果を返す
    return request;
  }

}
```

- s3のデータの扱い方についてはディベロッパーガイドを参照。ここではあくまで共通した待ち合わせ処理の記法のみ

---

### データを受け取るComponent側

- Component側の受け取り方は２パターンある
  1. .then()
  2. .subscribe()

- Component側の実装例(sample.component.ts)
  - serviceを呼び、データを受け取る

```
//rxjs 待ち合わせ処理に必要
import { forkJoin } from 'rxjs';

let awsData; //データ受取用の変数を宣言

  // 起動時に取得 → ngOnInitから呼び出す
  ngOnInit() {
      getData();
  }

  getData() {

    // Credential認証 Service側でもいいかも
    AWS.config.credentials = new AWS.Credentials("<access key>","<secret key>")

    // thenで受け取る例
    this.requestService.getAwsData().then(response =>{
      console.log(response);
      this.awsData = response; // 取得データを代入
    });

    // subscribeで受け取る例 Promise型のため、forkJoinが必要
    forkJoin(this.grequestService.getAwsData()).subscribe(result => {
      this.awsData = result[0]; // 取得データを代入
    });
  }
```

- sample.compoent.html 取得したデータを画面に表示
```
{{getAwsData}}
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
- [AWS SDK for JavaScript](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/welcome.html)
- [Viewing photos in an Amazon S3 bucket from a browser](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/s3-example-photos-view.html)
- [s3から取得した画像を表示する](https://www.366service.com/jp/qa/f67f9e496ab8caf19381338f887a0552)