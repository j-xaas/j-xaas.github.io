---
title: '[Angular mat-input] バリデーションまとめ'
date: 2020-09-09 23:17:22
category:
- Serverless Application Dev
- SPA (Angular)
tags: 
- Angular
- Angular Material
- mat input
- 双方向データバインディング
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/85309098-11606400-b4ed-11ea-84a1-69fee72f8208.png
toc: true
---

- Anuglarのフォームにおけるバリデーションチェックについてのまとめ
    - Angular Material(mat-input)を用いて、フォームを用意するまでは以前の記事で解説しています
        - [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)

- バリデーションチェックとは？
    - 最近のWEBサービスでよく見受けられる、リアルタイムで入力値をチェックして問題があればエラーを表示してくれる機能などのこと
        - レガシーなWEBサービスでは、半角、全角が間違っていても申請ボタンを押して更新するまで気づくことができず、ユーザがストレスを受けてしまうケースが多くありました
    - 活用例
        - 未入力の項目があれば、申請ボタンを無効化。該当箇所を赤くマーキング
        - 利用可能な型（Ex. 全角、半角、英数）を規定して制限
        - 文字数を制限
            - 悪意を持った攻撃を防ぐ効果があります
        - passwordを＊＊＊でマスク
        - typeを規定
            - emailの~~@~~や郵便番号や電話番号のーが挟まる形式など

こういったバリデーションを用意に実装できるのがAngularの強みの一つ

<!--toc-->


## Angularのmat-inputにおけるバリデーション

Angularにおいては、input要素に様々な属性値を与えることで、バリデーションチェックを行い、〜〜.valid/~~.invalidといった状態値で確認できる。以下に主なバリデーションパターンと解説を示す。


### 文字数制限
- 文字数を強制
  - minlength=""
    - 指定した文字数以下の場合、入力欄がエラー状態になる(赤くなる)
  - maxlength=""
    - 指定した文字数までしか入力できない

### 必須入力
- requiredをつけるだけ

### typeの指定
- type=で設定可能
```
<input type="email"...
```

### パターンチェック

- [PatternValidator]というディレクティブを利用することで、正規表現で詳細な条件を定める事ができる

- 記法：pattern="条件"
  - 以下は英字(小大)に限定している
```
<input name="XXX" ngModel pattern="[a-zA-Z ]*">
```
- 英大文字＋数字
```
<input pattern="[A-Z0-9]*">
```

- 英(小,大)＋数字(int)＋記号
```
<input pattern="[a-zA-Z0-9!-/:-@¥[-`{-~]*">
```

- [★正規表現を可視化してまとめたチートシート](https://qiita.com/grrrr/items/0b35b5c1c98eebfa5128)

### エラーメッセージの表示
- エラーメッセージの表示
  - mat-errorを使用
  ```
  <mat-error *ngIf="条件"
  ```

ngIfで定める条件（バリデーションチェックに合格しているか？）は次項目を使うことが多い。

### バリデーション状態の取得
- テキストボックス(input)に#~~で名前を付ける事で以下の値を条件式で利用可能になる
  - バリデーション成功時：~~.valid
  - バリデーション失敗時：~~.invalid
- エラーメッセージや最終的な送信ボタンにngIF="~~.valid"を条件付けすることで、入力値に問題がある状態でのリクエストを未然に防ぎ、UXを向上させることができる

- 条件が成り立たない時に無効化
  - ngIFではなく[disabled]=条件式を利用する
  ```
  <button [disabled]="~~.invalid">ラベル</button>
  ```

- 複数のバリデーションが通っている場合のみ有効化
  - 論理和演算子記号"||"で"または"とする
  ```
  <button [disabled]="aaa.invalid||bbb.invalid">
  ```

- 上記では各テキストボックスをひとつずつ条件付けしているが、form要素自体に以下のようにフォーム名を定めれば、全体のバリデータの状態をチェックできる(フォーム名.valid, フォーム名.invalid)
  - 項目が多い場合にはこちらの方が有効
```
<form #フォーム名="ngForm">
```

## 実装例
IDとパスワードを入力して送信ボタンの押下で認証を行う、基本的なフォームの例を以下に示す。

### 今回のサンプルにおける条件詳細
- Id詳細
    - 未入力禁止
        - required
    - 最低文字数
        - minlength="20"
    - 最大文字数
        - maxlength="20"
    - 例の表示
        - placeholder
    - patternチェック
        - 英大文字＋数字
        ```
        pattern="[A-Z0-9]*"
        ```
    - エラーメッセージの表示
        - mat-error要素を使用
- Password詳細
    - 未入力禁止
        - required
    - 最低文字数
        - minlength="40"
    - 最大文字数
        - maxlength="40"
    - 例の表示
        - placeholder
    - type
        - passwordで情報を秘匿
    - patternチェック
        - 英小大文字＋数字＋記号
        ```
        pattern="[a-zA-Z0-9!-/:-@¥[-`{-~]*"
        ```
    - エラーメッセージの表示
        - mat-error要素を使用

- Submitボタン
    - 各バリデートの中でエラーが一つでもあればボタンを無効化
    ```
    [disabled]="Id.invalid || passKey.invalid"
    ```

### 実装
```
<!--フォームとバリデーション-->

    <mat-form-field class="mat-input-margin1">
      <mat-label>ID</mat-label>
      <input matInput
        id="Id"
        akey="Id"
        [(ngModel)]="akey"
        minlength="20"
        maxlength="20"
        pattern="[A-Z0-9]*"
        placeholder="Ex. BSCJJSH7333..."
        required
        #Id="ngModel"
        >
      <!--全バリデーションチェック成功時にId.validが成り立つ--->
      <mat-error>入力必須</mat-error>
    </mat-form-field>

    <mat-form-field class="mat-input-margin2">
      <mat-label>Password</mat-label>
      <input matInput
        id="passKey"
        skey="passKey"
        [(ngModel)]="skey"
        minlength="40"
        maxlength="40"
        pattern="[a-zA-Z0-9!-/:-@¥[-`{-~]*"
        placeholder="Ex. Hy2D3..."
        type="password"
        required
        #passKey="ngModel"
        >
      <mat-error>入力必須</mat-error>
    </mat-form-field>

<!--上記のフォームの入力値のバリデーションチェック成功時に有効化される送信ボタン-->

    <button mat-raised-button
      (click)="submit()"
      [disabled]="Id.invalid || passKey.invalid"
    >Submit</button>
```


## 参考
### 関連記事
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)

### その他
- [angular material](https://material.angular.io/)
- [angular material components](https://material.angular.io/components/categories)
- [Angular Materialについて](https://note.com/mm_morimori/n/n18abdb6447cf)
- [Angular Materialのインストールから使い始めまで](https://medium.com/@donuzium/angular-material%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%8B%E3%82%89%E4%BD%BF%E3%81%84%E5%A7%8B%E3%82%81%E3%81%BE%E3%81%A7-d1e9c868688c)
- [Angularで「フォーム」の入力値をコンポーネントと同期するには？（双方向バインディング）](https://www.atmarkit.co.jp/ait/articles/1707/14/news134.html)

- [Angularで文字列を大文字／小文字に変換するには？（lowercase／uppercase）](https://www.atmarkit.co.jp/ait/articles/1703/28/news148.html)
- [angularjsはテキストボックスに大文字を強制します](https://www.it-swarm.dev/ja/javascript/angularjs%E3%81%AF%E3%83%86%E3%82%AD%E3%82%B9%E3%83%88%E3%83%9C%E3%83%83%E3%82%AF%E3%82%B9%E3%81%AB%E5%A4%A7%E6%96%87%E5%AD%97%E3%82%92%E5%BC%B7%E5%88%B6%E3%81%97%E3%81%BE%E3%81%99/1072025818/)

- [AngularのReactiveFormとAngularMaterialを組み合わせる](https://qiita.com/lightstaff/items/4b34fee8c9510f37a6d6)
- [Angular Formの実装](https://note.com/mm_morimori/n/n4cc7f3de9115)
- [Angular mat-input](https://material.angular.io/components/input/overview)

