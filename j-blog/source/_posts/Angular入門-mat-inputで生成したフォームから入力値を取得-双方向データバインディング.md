---
title: '[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)'
date: 2020-06-22 21:26:46
categories:
- Serverless Application Dev
- SPA (Angular)
tags: 
- Angular
- Angular Material
- mat input
- 双方向データバインディング
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/85309098-11606400-b4ed-11ea-84a1-69fee72f8208.png
---

　Angular初級者向けに、簡単なフォームを生成して入力値を取得するまでを解説します。

## 基礎知識
前提となる超基本なので分かっている部分は飛ばしてください。

### Angular Projectの初め方
- Angular Project
    - Angularにおけるアプリの雛形/単位のことです
- Angular PJを生成
```
ng new "ap-name"
```
- Componentを生成
    - APはコンポーネントと呼ばれる一塊で開発していきます
    ```
    ng g component "component-name"
    ```
    - コンポーネントは以下のファイルのセットです
        - HTML/Typescript/Scss/karma
    - HTMLが画面、Typescriptが機能、Scssがデザイン、karmaがテスト用ファイルというイメージです

### Angular Material

- [Angular Material](https://material.angular.io/)
  - Googleが提唱しているMaterial DesignというCSSフレームワークを使うためのツール
    - Google/twitter/Microsoft等は、AngularでWEB APを開発しているので、それらのデザインに使われています
  - 要するにAPの画面の部品を、デザイナーでなくとも簡単に作れるようになるツールです
  - 様々なMaterialを組み合わせてパズルのようにUI(画面のこと)を開発していくのがAngular開発の基本です

- 使用例
  - mat button/mat checkbox
  ![mat-button](https://user-images.githubusercontent.com/41946222/85590466-6633e400-b67f-11ea-9ac4-6a8eb6fc52a6.png)

  - mat toolbarとside-navでよくある開閉するメニューを作れます
  ![mat toolbar & side-nav](https://user-images.githubusercontent.com/41946222/80100008-4feba800-85aa-11ea-8bad-dc2ceb714f16.png)
    - 上記を自動生成するSchematicsというツールの解説記事も書いたので、理解が進んだら学習してみてください
      - [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)

  - mat input (datepicker)
  ![mat input datepicker](https://user-images.githubusercontent.com/41946222/85590634-88c5fd00-b67f-11ea-9887-3e7ee72db362.png)

  - mat table
  ![image](https://user-images.githubusercontent.com/41946222/85592226-f1fa4000-b680-11ea-9b7d-3be0201902f9.png)


- 設定を覚えれば、上記の様なmaterialをhtmlに1行書くだけで表示できます
  - つまり、これらを順にマスターしていけば、APの画面を自在に作れます

### mat-input
- [Angular Material mat Input](https://material.angular.io/components/input/examples)
  - 今風のフォーム（入力欄のこと）を作れます
  - HTMLに`<input mat-input></input>`と一行書くだけで、インターネット黎明期のようなフォームにならずに済みます
  - 他にも色々と便利な機能があります
    - オートコンプリート機能
      - 入力途中で予測表示してくれる機能
      - AngularもGoogleが作っているだけあって、ブラウザの記憶機能も使えます
    - プレイスホルダー機能
      - 入力欄に未記入の状態のみ、うっすら案内を表示してくれる機能
      - 記入例をこれで表示するとイケてるAPっぽく見えます
    - バリデーション機能
      - typeの指定
        - 例えば mail にすれば自動的にmailの英数で@を挟む形式以外入力できなくなります
        - 電話番号や郵便番号であれば、自動的に-を補完してくれたりもします
      - 読み込みを挟まないエラー表示
        - 条件を定めると、入力中にリアルタイムでエラー表示等できます
        - 全入力して送信を押すまでエラーが出ないWEBページでイライラしたことありませんか？こういったUX(ユーザ体験)に配慮することでモダンなAPを開発しましょう。例えば、大文字小文字は自動で変換するか先に警告しましょう。

![mat-input example](https://user-images.githubusercontent.com/41946222/85586437-e48e8700-b67b-11ea-81c5-d07bfbf4bce3.png)

### 双方向データバインディング
- 双方向データバインディング
    - html側とts側で変数を自動的に同期することが可能です
- 記法
```
[(ngModel)]="variable-name"
```

## 実装

### Angular Materialのinstall
- 以下のコマンド一発で利用の準備が整います
```
$your-angular-pj> ng add @angular/material
```
- 質問に対しては全てYesでOK
  - 途中でアプリケーションの主要カラーとサブカラーを聞かれます
  - 左が主要カラー/右がサブカラーです

### html
- 実装例を以下に示します
    - input要素にmatInputを属性として書くだけでangular material仕様のフォームになります
    - [(ngModel)]="key"
        - 双方向データバインディングで、ts側と変数keyを共有しています。入力値が自動的に変数keyに代入されます
    - {{variable-name}}で変数の値を表示できます
    ```
      <mat-form-field>
        <mat-label>KeyId</mat-label>
        <input matInput id="KeyId" key="KeyId" [(ngModel)]="key" min="1" placeholder="Ex. abc..." >
      </mat-form-field>
      {{key}}
    ```

### ts
- 変数を宣言するだけでOK
```
  // 入力値を格納する変数を宣言
  key;
```

## mat-inputにバリデーションを実装する

- 次に今回作ったフォームに”バリデーションチェック”を実装しましょう
  - [[Angular mat-input] バリデーションまとめ](/Angular-mat-input-バリデーションまとめ/)

- バリデーションチェックとは？
    - 最近のWEBサービスでよく見受けられる、リアルタイムで入力値をチェックして問題があればエラーを表示してくれる機能などのこと
        - レガシーなサービスでは、半角全角が間違っていても申請ボタンを押して更新するまで気づくことができず、ユーザがストレスを受けて利用を諦めてしまうことも
    - 活用例
        - 未入力の項目があれば、申請ボタンを無効化。該当箇所を赤くマーキング
        - 利用可能な型（Ex. 全角、半角、英数）を規定して制限
        - 文字数を制限
            - 悪意を持った攻撃を防ぐ効果があります
        - passwordを＊＊＊でマスク
        - typeを規定
            - emailの~~@~~や郵便番号や電話番号のーが挟まる形式など


### おまけ：未入力の場合のみ初期値を表示するロジックを開発する
- ts側に以下の様な判別ロジックを書きます
    - 入力値の有無をif(this.key === undefined)で判断しています
```
  // 入力値を格納する変数を宣言
  key;

    // 判別ロジック
    if ( this.key  === undefined ) {
      this.key = '初期値';
    }
```

以上です。Angular APのUI(画面)開発はAngular Materialのページで必要そうなMaterialを探して、使い方をググりながら進めていくというイメージを持てればある程度自走できるようになると思います。
Angular Materialについて、公式ドキュメントだけでは難しいものは記事に解説をまとめているので、参考にどうぞ。

## 参考
### 関連記事
- [[Angular mat-input] バリデーションまとめ](/Angular-mat-input-バリデーションまとめ/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)

### その他
- [angular material](https://material.angular.io/)
- [angular material components](https://material.angular.io/components/categories)
- [Angular Materialについて](https://note.com/mm_morimori/n/n18abdb6447cf)
- [Angular Materialのインストールから使い始めまで](https://medium.com/@donuzium/angular-material%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%8B%E3%82%89%E4%BD%BF%E3%81%84%E5%A7%8B%E3%82%81%E3%81%BE%E3%81%A7-d1e9c868688c)
- [Angularで「フォーム」の入力値をコンポーネントと同期するには？（双方向バインディング）](https://www.atmarkit.co.jp/ait/articles/1707/14/news134.html)