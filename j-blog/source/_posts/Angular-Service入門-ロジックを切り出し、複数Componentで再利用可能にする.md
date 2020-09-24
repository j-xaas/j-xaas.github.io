---
title: '[Angular Service入門] ロジックを切り出し、複数Componentで再利用可能にする'
date: 2020-09-25 01:03:49
categories:
- Serverless Application Dev
- SPA (Angular)
tags: 
- Angular
thumbnail: https://user-images.githubusercontent.com/68212997/94169447-c5490500-fec9-11ea-9bca-837c27462e2f.png
toc: true
---

Angularでは基本的にComponent単位で開発を行いますが、複数のComponentで使い回す機能については”Service”という形で切り出すのが一般的です。
初めは各Componentのtsファイルに機能をそのまま書いてしまうと思います。そこから切り出す過程と注入(Injection)の手順や、Serviceについて解説します。

<!-- toc -->

## AngularにおけるServiceという概念
![About Angular Service](https://user-images.githubusercontent.com/68212997/94169447-c5490500-fec9-11ea-9bca-837c27462e2f.png)

- 機能をサービスクラス(tsファイル)として切り出し、複数のComponentから使用させることが可能
    - Serviceから各Componentに”依存性注入(Dependency Injection, DI)”とを行うことでServiceの中の関数を使用できる

- 一般的なAngularの開発
    - Component側にはビュー（見栄え）に関するロジックのみを書く
    - アプリ固有のビジネスロジックはService側に全て切り出し、Componentはそれを呼ぶだけ
        - こうすることで、各ファイルがコンパクトになり可読性が大きく向上する


## ロジックのサービス分割手順　日時取得機能の例
### 想定する状況
- Componentのtsファイルに、以下の様な日時を取得してフォーマットを整形するロジックを書いていたとする
  - 他のComponentでもこの機能を使い回せるように、Serviceとして切り出す

```
  // --日時データを取得--
    // Dateオブジェクトの作成
    const now = new Date();
    // 各日時要素を取得
    const year = String(now.getFullYear()); // 年
    // 1月=0と出るため+
    const month = String( ("00" + (now.getMonth() + 1)).slice(-2)); // 月
    const date = String( ("00" + now.getDate()).slice(-2)); // 日
    const hour = String( ("00" + now.getHours()).slice(-2)); // 時
    const min = String( ("00" + now.getMinutes()).slice(-2)); // 分
    const sec = String( ("00" + now.getSeconds()).slice(-2)); // 秒
    // YYYYMMDDHHMMSS形式にまとめる 
    const time =  year + month + date + hour + min + sec;
    return time;
```

- まずは関数として切り出してみる
  - Component内で関数として切り出せる機能＝Service内の関数として切り出し可能
  - 以下のgetTime()がServiceからの関数呼び出しに変わるイメージ

```
cost t = getTime();

...中略...

  getTime() { // ----ファイル名のprefixに付ける日時データを取得---
    // Dateオブジェクトの作成
    const now = new Date();
    // 各日時要素を取得
    const year = String(now.getFullYear()); // 年
    // 1月=0と出るため+
    const month = String( ("00" + (now.getMonth() + 1)).slice(-2)); // 月
    const date = String( ("00" + now.getDate()).slice(-2)); // 日
    const hour = String( ("00" + now.getHours()).slice(-2)); // 時
    const min = String( ("00" + now.getMinutes()).slice(-2)); // 分
    const sec = String( ("00" + now.getSeconds()).slice(-2)); // 秒
    // まとめる YYYYMMDDHHMMSS
    const time =  year + month + date + hour + min + sec;
    return time;
  }
```

### Serviceを作成
- ng gコマンドで雛形を自動生成可能
    - ~~.spec.tsはテスト用のファイルなので一旦スルーしてOK
```
\src\app\service> ng g service "get-time"
CREATE src/app/service/get-time.service.spec.ts (339 bytes)
CREATE src/app/service/get-time.service.ts (136 bytes)
```


### Serviceの実装
- 先ほどの関数をService側に転記
- Component側のgetTime()関数は消す

get-time.service.ts
```
import { Injectable } from '@angular/core';
@Injectable({
  providedIn: 'root'
})
export class GetTimeService {
  constructor() { }
  // 日時を取得整形する関数  
  getTime() { 
    // Dateオブジェクトの作成
    const now = new Date();
    // 各日時要素を取得
    const year = String(now.getFullYear()); // 年
    // 1月=0と出るため+
    const month = String( ("00" + (now.getMonth() + 1)).slice(-2)); // 月
    const date = String( ("00" + now.getDate()).slice(-2)); // 日
    const hour = String( ("00" + now.getHours()).slice(-2)); // 時
    const min = String( ("00" + now.getMinutes()).slice(-2)); // 分
    const sec = String( ("00" + now.getSeconds()).slice(-2)); // 秒
    // まとめる YYYYMMDDHHMMSS
    const time =  year + month + date + hour + min + sec;
    return time;
  }
}
```

#### Serviceの注入(DI)
Component側でServiceの関数を利用可能にする。以下はcomponentのtsファイルの話

- serviceをimport
  ```
  import { GetTimeService } from '../../service/get-time.service'; // get time logic
  ```
- 依存性注入
    - ここで宣言したtimeServiceという変数にServiceの中身が入るイメージ
    - ”timeService.使いたい関数名”でService内の関数を呼び出せる
  ```
    constructor(
      private timeService: GetTimeService,
    ) {
  ```
- ComponentからServiceの関数を使用する
```
// 日時取得＆整形ロジックの利用
const time = this.timeService.getTime();
// 結果を確認
alart(time);
```


## 参考
### 関連記事
- [[Angular mat-input] バリデーションまとめ](/Angular-mat-input-バリデーションまとめ/)
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)
- [静的ホスティングサービスまとめ(Netlify/S3/Firebase Hosting/Github Pages)](/静的ホスティングサービスまとめ-Netlify-S3-Firebase-Hosting-Github-Pages/)

