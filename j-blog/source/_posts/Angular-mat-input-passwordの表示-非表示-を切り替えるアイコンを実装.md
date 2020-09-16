---
title: '[Angular mat-input] passwordの表示/非表示(*)を切り替えるアイコンを実装'
date: 2020-09-16 20:55:07
categories:
- Serverless Application Dev
- SPA (Angular)
tags: 
- Angular
- 'Angular Material'
- 'mat input'
thumbnail: https://user-images.githubusercontent.com/68212997/93334304-b1652980-f85f-11ea-9d72-4d5e17a3488c.png
toc: true
---

<!--toc-->

## やりたいこと：　パスワードのマスク(***と出る)を目アイコンでON/OFF
- type=passwordで入力値が**で表示される
    - 覗き見を防止するための基本的な仕様だが、入力値の確認も可能にしたい
    ​
![Passwordフォームイメージ](https://user-images.githubusercontent.com/68212997/93334304-b1652980-f85f-11ea-9d72-4d5e17a3488c.png)

- やり方
  - html側
    - type=変数
      - 条件分岐で 'password' : 'text'に切り替わる
    - 変数の初期値はpassword
    - 入力欄に目アイコンのボタンを追加
      - mat-iconのvisibility、visibility_offを使用
      - matSuffixでbuttonを入力欄の右端に
    - 押下時に入力値が見えるようにする
  - ts側
    - 条件分岐用に変数を定義
    ```
    hide = true;
    ```
​
‐ Angularにおける変数の条件分岐の書き方
  - 以下は変数hideの値がtrueかfalseかで、?以降の:で区切られた値が採用されるという意味
```
[type]="hide ? 'password' : 'text'"
```
​
## 実装例
- htmlの実装
  - input内のtype属性
    - 変数hideの値による条件分岐でpassword or textに切り替わる
    - つまり、表示/非表示(*)が切り替わる
  - buttonを作成
    - mat-iconで目のアイコン(visibility, visibility_off)に
    - mat-suffixで右端に
    - (click)時にhideの値を変更
      - (click)="hide = !hide"
```
      <mat-form-field class="mat-input-margin2">
        <mat-label>secretAccessKey</mat-label>
        <input matInput
          id="secretAccessKey"
          skey="secretAccessKey"
          [(ngModel)]="skey"
          minlength="40"
          maxlength="40"
          pattern="[a-zA-Z0-9!-/:-@¥[-`{-~]*"
          placeholder="Ex. FdOI0..."
​
          [type]="hide ? 'password' : 'text'"
          
          required
          #secretAccessKey="ngModel"
        >
        <!--目のアイコンの押下で入力値の表示を切り替え--->
        <button mat-icon-button
          matSuffix
          (click)="hide = !hide"
          [attr.aria-label]="'Hide password'"
          [attr.aria-pressed]="hide"
        >
            <mat-icon>{{hide ? 'visibility_off' : 'visibility'}}</mat-icon>
        </button>
​
        <mat-error>入力必須</mat-error>
​
      </mat-form-field>
```
​

- [公式のコードサンプル(stackblitz)](https://stackblitz.com/angular/qmnqdvxmban?file=src%2Fapp%2Fform-field-prefix-suffix-example.ts)


## 参考
### ​関連記事
- [[Angular mat-input] バリデーションまとめ](/Angular-mat-input-バリデーションまとめ/)
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)


### その他
- [Angular Material Form field](https://material.angular.io/components/form-field/overview)
​