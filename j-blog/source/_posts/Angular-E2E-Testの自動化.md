---
title: '[Angular] E2E Testの自動化'
date: 2021-01-11 16:55:28
categories:
- Serverless Application Dev
- SPA (Angular)
tags: 
- Angular
- 'e2e test'
thumbnail: https://user-images.githubusercontent.com/68212997/104157486-8d40e000-542e-11eb-9c5d-786957fff2b4.png
toc: true
---

## E2Eテスト概要
- Angularでは以下の二種のテストをngコマンドで実行可能（Protoractor/Karmaというテストを実行するためのフレームワークが標準で準備されている）
  - E2Eテスト
    - End to End（エンドツーエンド）テストの略
    - WEBブラウザー上でのテストを自動化できる
    - 画面毎のテスト
    - テストフレームワーク
      - Protoractor
    - コマンド
      - ng e2e
  - ユニットテスト
    - 関数ごとのテスト
    - テストフレームワーク
      - Karma
    - コマンド
      - ng test

今回は画面毎のテストを自動化したいので、Protoractorを活用してE2Eテストを実践する

- E2Eテストまでの流れ
  - 1. 操作したい要素に識別子を付与
    - 要素(Ex. ボタン、入力フォーム)を判別可能にする
  - 2. テストを定義
    - テストコードを書く
  - マルブラウザ対応の設定をする
  - テストを実行

## 選択する要素に識別子を付与
E2EテストではAPの画面を自動操作する。
その際にどこを操作するか？(特定のボタン、フォームなど)を判別するために、各Componentのhtmlファイルで各要素に識別子を付与する必要がある。

- 選択する要素を識別する識別子
  - [data属性](https://developer.mozilla.org/ja/docs/Web/HTML/Global_attributes/data-*)を使う
  - 書式
    - 以下を操作したい要素に定義していく
  ```
  [attr.data-test]="'任意の識別子'"
  ```
    - ※要素を識別するために他に使用される属性として、id属性・class属性などもあるが、これらのはデザインの変更などで変更される恐れがあるため勧められていない。

- 一般的なログイン画面で以下の要素に識別子を与えた例
  - ログインIDの入力欄
  - パスワードの入力欄
  - リクエストを送信するボタン

```
<h1>login</h1>
<form>
  <div>
    <mat-form-field>
      <input matInput placeholder="email" [attr.data-test]="'email'"> // ←　要素①： data-*属性で一意となる識別子を振る
    </mat-form-field>
  </div>
  <div>
    <mat-form-field>
      <input type="password" matInput placeholder="passoword" [attr.data-test]="'password'"> // ← 要素②
    </mat-form-field>
  </div>
</form>
<div class="p-button-wrapper">
  <button mat-raised-button class="p-button" color="accent" (click)="login()" [attr.data-test]="'login-button'">Login</button>　 // ← 要素③
</div>
```

この後の工程で、これらの識別子に対するアクションを定義する。
上記の例であれば、IDとパスの識別子に対して入力値を与えて、ボタンの識別子に対して(click)アクションを実行させることになる。

##　テストファイル
- テストを定義するファイルは以下の二種類
  - \angular-pj\e2e\src\＊.e2e-spec.ts
  - \angular-pj\e2e\src\*.po.ts

Angular CLIでプロジェクトを始めると、上記のファイルが自動生成されているはず。

### Page Object
- アプリケーションの画面単位で1オブジェクトを定義するPage Objects としてE2Eテストが作成される
- PageObjects
  - アプリの画面を1つのオブジェクトとして捉えるデザインパターン
- PageObjectのメリット
  - もし、テストケースに直接セレクタを記述する場合、デザイン変更がにより、セレクタを記述している全テストケースの修正が必要
  - Page Objectsとして、*.po.tsに各画面毎の要素選択などの処理を定義することで、*.e2e-spec.tsは実際のテストケースを手続き的に書いていける
    - コードの可読性が向上
    - 再利用性が高まる

## E2Eテストを実行
- テストコードを書き終えたらコマンドでテストを実行する
  - 以下をGithub ActionsやCircle CIなどで自動化するとCI/CDを実現できる

- E2Eテストは以下のコマンドで実行できる
```
ng e2e
```

- ここで証明書エラーにハマった場合は以下を参照
    - [[Angular] ng e2e Test 証明書エラーでハマった際の回避策(unable to get local issuer certificate))](/Angular-ng-e2e-Test-証明書エラーでハマった際の回避策/)

テストコードのより詳細な記述方法については別途まとめます

## 参考
### 関連記事
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
- e2e test  概要
  - [ブラウザーを自動で操作し動作確認できる、「Angular」のe2eテスト](https://codezine.jp/article/detail/11013)
- e2e test マルチブラウザ
  - [【Angular】これからはじめるE2Eテスト（2019）](https://qiita.com/nishiemon/items/d774c17476c9f7d8ad6a)
