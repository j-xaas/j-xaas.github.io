---
title: >-
  [Angular/TypeScript(JavaScript)] 非同期処理/待ち合わせ処理のまとめ
  (Observable/subscribe/forkJoin/Promise/async/await/then)
date: 2021-01-26 23:35:16
categories:
- Serverless Application Dev
- SPA (Angular)
tags: 
- Angular
- TypeScript
- JavaScript
- Observable
- Promise
- async/await
- forkJoin()
- then()
thumbnail: https://user-images.githubusercontent.com/68212997/104157486-8d40e000-542e-11eb-9c5d-786957fff2b4.png
toc: true
---

- Angular等のTypeScriptベースのフレームワークでフロントエンドを開発する際には、待ち合わせ処理が度々問題となる
- 外部通信が基本的に非同期処理であるため、返り値を加工するにはひと手間加える必要がある

<!--toc-->

## 前提知識
### 同期/非同期/待ち合わせ処理とは？
プログラムを書く前にどれが最適化か？を判断する必要がある（書き方が変わってくる）

- 非同期処理
  - 実行完了を`待たずに`次の処理を並列で開始する
  - メリット
    - 処理の高速化を図れる。
    - 一部の処理でエラーが出ても、他の処理は問題無く動く
    - 処理完了まで`ユーザーを待たせない`
  - デメリット
    - 返り値を使って次の処理を行いたい時に問題が発生する
    (返り値無しの状態で次の処理を実行してしまう)

- 同期処理
  - 処理完了まで`他の処理をストップ`する
  - メリット
    - 処理を順に実行できるため、プログラムを書きやすい
  - デメリット
    - 処理完了までユーザーを待たせる事になる

- 待ち合わせ処理
    - `ある条件が満たされるまで処理を待たせる`
      - ex. 返り値が戻ってくるまである処理の開始を待たせる
    - `同期処理とイコールではない`
    - `複数の処理群としてみると非同期でOKだが、個々の処理は待ち合わせて順番に実行させたいケースがある`
        - ユーザーが処理の完了を待つ必要がない → 非同期でOK
        - データ取得後に加工したい → 待ち合わせ処理が必要

- TypeScript(JavaScript)においては、以下のような処理が非同期で実行されることを考慮する必要がある
  - `API通信`
  - `データベース通信`
  - その他の重い処理全般

### TypeScript(JS)の待ち合わせ処理問題とは
上述の非同期で動く特性から、初級者は以下の問題でハマることが多い

- TypeScript(JS)では、`プログラムが書かれた順に動くとは限らない`
  - `基本的に非同期処理`で動く
    - 非同期処理は重い処理の終了を待たずに、次の処理を進められるので、高速化という意味では有効だが、困るシーンもある

- よくあるケース
  - 外部APIと通信してデータを取得する場合、その戻り値が帰ってくる前に次の処理に進んでしまう。そのため、以降の処理がデータ無しで行われてしまう
  - 正しく外部にリクエストできている筈が、データ＝undefinedと出力されてしまう
    - この辺りはChromeの開発モードで出力順を見ると理解し易い


- 回避策: 待ち受け処理/同期処理を実現する（返り値が来た後に次の処理を実行させるように書き換える）
  - 書き方は色々ある
    1. Observable, subscibeを活用して待ち合わせ処理を実現する
    2. Observable, forkJoin()を活用して待ち合わせ処理を実現する
    3. aync/awaitを利用して同期処理化する
    4. Promise, then()を活用して同期処理化する

---

## Observable型を使って待ち合わせ処理を実現するパターン

- Angularでは適したケースであれば、基本的にPromiseよりもObservableが推奨されている
    - [Observable と 他の技術の比較](https://angular.jp/guide/comparing-observables)

- service側の返り値をobservable型に定義、component側で.subscribe()で受け取り、その中に処理を書き込む

- ポイント
    - `Observableでも待ち合わせ処理は実現できるが、同期処理はできない`
    - 同期処理についてはPromiseを使う
    - ストリームを扱える

### 1. Observable & subscribe()
- Service側
    - 返り値をObservable型で定義
- Component側
    - .subscribe()で返り値を受け取る

- sample.service.ts
    - Observable型で返り値を返す
```
getmethod(){
    return observable = new Observable<number>(observer => {
        // 外部APIとの通信処理

    });
}
```

- sample.component.ts
    - 書式：observable.subscribe()

```

this.sampleService.getMethod().subscribe(value => {

  // 返り値を利用する処理をここに書く

});
```

### 2. Observable & forkJoin()
- forkJoin()を活用すると、`Observableの全ての処理の完了を待って次の処理を実行できる`
  - ユースケース
    - 複数回外部APIにリクエストを実行して、全てのデータが帰ってきてから加工

- 書式
```
// 複数の外部リクエストを実行してobservable型で受けとる
// 配列hogehogeの中身を一つずつ引数として渡して実行する例
const observable = hogehoge.map(hoge => {
    return this.sampleService.getData(hoge).pipe(catchError(e => observableOf({"error": e})))
});

// 待ち合わせ処理
forkJoin(observables).subscribe( response => {
  // 以降に処理を記載

```

- 活用例は以下の記事に記載した
  - [[Angular JavaScript] JSONデータのファイル化と出力 ~取得したデータを任意の名称で保存するロジック~ (TypeScript)](/Angular-JSONデータのファイル化と出力-クラウドから取得したデータを任意の名称で保存する/)

---

## Promise型を使って同期処理化するパターン

### 3. async/awaitを活用する
- asyncをつけた関数の返り値はPromise型となる
- asyncをつけた関数はawaitで待てる

- await
  - Promiseの値が取り出されるまで待つ
- async
  - awaitキーワードを使っている関数の頭に付ける必要がある
- import必要？
- ポイント
  - 同期処理化したい関数の宣言時：頭にasyncを付ける
  - 同期処理化したい関数の実行時：awaitを付ける
  - awaitはasyncを付けた関数内でしか実行できない

#### Serviceで外部API通信を実行 ⇒ Component側の変数に受け取る例
- sample.component.ts
  - awaitを付けた関数の処理を待つ
```
let result; // 返り値を受け取る変数
  async ngOnInit() {
    // Service経由で外部APIからデータを受け取る
    this.result = await this.sampleService.getMethod()
    console.log(this.getData);
  }
```
- sample.service.ts
  - 関数の頭にasyncを付ける
```
async getMethod(){
}
```

#### 複数の関数を順に実行する例
```
// プログラムの大筋
async function main() {
    const x = await getX()
    const y = await getY()
    console.log(x + y)
}
async function getX() {
    return 1
}
async function getY() {
    return 2
}
main()
```

#### アロー関数の場合はasyncをつける箇所が異なる
- Angularの場合は基本的にアロー関数が推奨されているのでこの書き方も覚えた方が良い

```
// functionによる関数宣言
async function sampleFunc() {
    // 処理内容
}
// アロー関数による関数宣言
const sampleFunc = async() => {
    // 処理内容
}
```
---

### 4. Promise, .then()を活用する
- awaitではなくthen()を使っても良い
  - 基本的にawaitの方がすっきり描けるので推奨はしない

- async付きの関数の返り値はPromise型となる
  - Promise型であればthen()関数を利用して同期処理化できる
    https://tech.playground.style/javascript/asynchronous-processing/


- sample.service.ts
    - Promise型で宣言
```
// initiate execution
let promise = new Promise<number>((resolve, reject) => {
  // Executer fn...
});
```

- sample.component.ts
  - Promise型の
```
promise.then(value => {
  // handle result here
});
```


## 参考
### 関連記事
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

### 非同期処理参考
- [Angular 日本語ドキュメント/Observable と 他の技術の比較](https://angular.jp/guide/comparing-observables)
  - ここの[チートシート](https://angular.jp/guide/comparing-observables)が役立つ

- [【JavaScript】非同期処理の完了をpromiseとasync/awaitで待つ方法](https://tech.playground.style/javascript/asynchronous-processing/)
- [Promiseの使い方、それに代わるasync/awaitの使い方](https://qiita.com/suin/items/97041d3e0691c12f4974)