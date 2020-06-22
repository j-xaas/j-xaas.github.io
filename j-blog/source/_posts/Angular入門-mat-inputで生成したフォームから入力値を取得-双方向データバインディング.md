---
title: '[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)'
date: 2020-06-22 21:26:46
categories:
- Serverless Application Dev
- AWS
tags: 
- AWS
- CloudFront
- WAF
- S3
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



### mat-input


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
ng add @angular/material
```
- 質問に対しては全てYesでOK

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

### 未入力の場合のみ初期値を表示するロジックを開発する
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
