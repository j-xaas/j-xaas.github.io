---
title: '[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得'
date: 2020-05-28 21:25:40
category:
- Serverless Application Dev
- SPA (Angular)
tags:
- 'mat-selection-list'
- 'ngFor'
- Angular
- 'map()'
- 'filter()'
- 'mat-checkbox'
- 'Angular Material'
thumbnail: https://user-images.githubusercontent.com/41946222/83426603-5bb07100-a46a-11ea-8093-bd0d6e517706.png
toc: true
---

- [Angular Material](https://material.angular.io/components/select/overview)を用いたUIの実装について、初級者向けに以下を解説します
    - チェックボックス型の選択項目をリスト表示
    - 選択値を配列にまとめて格納するロジック
- 使いどころ
    - 外部サービスやDBに対するリクエストに含めるパラメータを選択値に応じて変化させる
    - 設定ページに良く使います

<!--画像--->
<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/83153404-e1b27c00-a139-11ea-846c-3df957ebc188.png" height="400px" width="300px" alt="画面イメージ">
</div>

<!-- toc -->

## 1. 基礎知識

### 1.1. Angular Material
- [Angular Material](https://material.angular.io/)
    - メニューやツールバー、ボタン、チェックボックスなどの良く使う部品を提供してくれるAngular公式のUIフレームワーク
    - 基本的に画面は0から作らずにMaterialを組み合わせて作ります
    - まだ導入していなれけば以下を実行すると使えるようになります
        - 聞かれることは全部Yesで問題ないです
    ```
    ng add @angular/material
    ```

- [mat-selection-list](https://material.angular.io/components/list/api)
    - checkbox付きのリストを表示するMaterial
    - 今回はこれを使います (詳細は後述)

## 2. 実装
- コンポーネント(sample-component)を例に解説します
    - sample-component.html
        - Angular Materialを活用したUI
    - sample-component.ts
        - データの宣言、データ加工機能

### 2.1. sample-component.html
- まずAngular Materialを使って画面を作ります

- 実装例
    - mat-selection-list要素の中にmat-list-option要素を並べます
    - 後でそれぞれ丁寧に解説します
```
<!--選択リスト-->
<mat-selection-list *ngFor="let item of Data; index as i">
    <ng-container>
        <mat-list-option
          [(selected)]="item.selected"
          [value]="item.value"><!--こっちに書いても発火しない(selectionChange)="checkRegion(i)"--->
            {{item.value}}<!--{{item[i].value}}--->
        </mat-list-option>
    </ng-container>
</mat-selection-list>

<br>
<!--外へリクエストを送るボタン-->
<button mat-raised-button (click)="submit()">Submit</button>
```

#### 2.1.1. mat-selection-listとmat-list-optionについて

- 書式
    - mat-list-optionが選択肢の単位
```
<mat-selection-list>

    <mat-list-option
        [(selected)]="item.selected"
        [value]="item.value"><!--こっちに書いても発火しない(selectionChange)="checkRegion(i)"--->
        {{item.value}}<!--{{item[i].value}}--->
    </mat-list-option>

</mat-selection-list>
```

- mat-list-optionの属性値
    - selected
        - checkboxの状態を示す
            - true or false
    - value
        - 要素が持つ値
```
    <mat-list-option [(selected)]="item.selected" [value]="item.value">
        {{item.value}}
    </mat-list-option>
```

- mat-selection-listの属性値
    - (selectionChange)イベント
        - checkされた際に発火するイベント
        - 以下の例では何かが選択される度に"checkRegion()"というメソッドが実行される
        ```
        <mat-selection-list (selectionChange)="checkRegion()">
        ```
    - mat-list-option側に書いても動かないので注意


#### 2.1.2. *ngFor="let item of regionsData; index as i"

- ngFor
    - [構造型ディレクティブ](https://angular.jp/guide/structural-directives)と呼ばれるもの
    - ある配列型のデータを元に、ngForを属性値として設定した要素で挟まれた部分をループ表示する
    - 以下をイメージしてください
    ```
    <div *ngFor="..<参照するデータを定義(ループの中で使用する)>..">
        <!--ここに書かれた内容をループ表示--->
    </div>
    ```
    - HTMLで何行も大量にべた書きする必要がある様な項目を
    - 参照したデータ配列を元に、取り出した変数を変えながら楽に表示できる。すっきりかけるというのがポイント

- let item of Data;
    - 書式
    ```
    let <ループ内での変数名> of <参照するデータ配列>;
    ```
    - 意味
        - Data配列をループの中ではitemという名称で扱います
        - 配列から一要素づつ抜き出して、ループ内での変数itemとして使います

    - 使用イメージ
        - tsでデータを定義
        ```
        Data =[Apple, Orange, Pinapple]
        ```
        - html
        ```
        <div *ngFor="let item of Data">
            {{item}} <!--Dataの中身(Apple, Orange, Pinapple)を順に表示-->
        </div>
        ```

    - 参照するデータ
        - 上の例で分かるようにts側(sample-component.ts)で配列を定義する必要があります
            - 後で解説します
        - ts側で定義するので外部から取得した動的なデータを元にすることも可能

- *ngFor="let item of Data; index as i"
    - Dataに含まれる配列を変数itemとして一つづつ取り出しながらループで表示する
    - 配列のカウントをiで行う

- ng-container
    - これで囲んだ要素はセットでngForでループする対象にできる
        - 複数の要素を纏めてループする場合にはよく使います
    ```
    <div *ngFor="..<参照するデータを定義(ループの中で使用する)>..">
        <ng-container>
            <div> XXXX </div>
            <div> XXXX </div>
        </ng-container>
    </div>
    ```

#### 2.1.3. index as i
- ループが何週目か？をiで定義可能
- ”0から始まる”ので注意
- 使用イメージ
    - 実装
    ```
    <div *ngFor="let item of Data">
        {{i+1}}番目: {{item}} 
    </div>
    ```
    - 表示
    ```
    1番目: Apple
    2番目: Orange
    3番目: Pinapple
    ```

### 2.2. sample-component.ts
- ts側には以下を書きます
    - ngForで使うデータソース
    - 選択値を取得するロジック

#### 2.2.1. データソースの準備
- ngForで参照するデータを以下のように定義します

```
// 選択肢として表示するregionデータをハッシュで管理
  Data = [ // 個々のselectedプロパティでチェック状態を保持
    { value: 'us-east-1', selected: false},
    { value: 'us-east-2', selected: false},
    { value: 'us-west-1', selected: false},
    { value: 'us-west-2', selected: false},
    { value: 'ca-central-1', selected: false},
    { value: 'eu-central-1', selected: false},
    { value: 'eu-west-1', selected: false},
    { value: 'eu-west-2', selected: false},
    { value: 'eu-west-3', selected: false},
    { value: 'eu-north-1', selected: false},
    { value: 'ap-northeast-1', selected: true}, // 初めからcheck=true
    { value: 'ap-northeast-2', selected: false},
    { value: 'ap-northeast-3', selected: false},
    { value: 'ap-southeast-1', selected: false},
    { value: 'ap-southeast-2', selected: false},
    { value: 'ap-south-1', selected: false},
    { value: 'me-south-1', selected: false},
    { value: 'sa-east-1', selected: false},
  ];
```

- 使い方
    - ngForでこの配列から一つづつ呼び出す単位は以下です
    ```
    { value: 'us-east-1', selected: false}
    ```
    - つまりそのループの中で us-east-1 を示すには以下のように書きます
    ```
    <div *ngFor="let item of Data">
        {{item.value}}
    </div>
    ```


#### 2.2.2. 双方向バインドによる状態値(selected)の変更
- これを用いるとパラメータの変化を伝えることができます
- Angularにおける双方向バインド
    - 片方の変数の変化を
    ```
    [(変数)]=変数
    ```
- 今回の使い方
    - ユーザがcheckした際にselectedの値が変わります
    - item.selected＝tsで定義したハッシュの該当箇所の選択値が変更されます
        - 後はts側でitem.selectedの値を使って機能を書けます
```
<mat-list-option
    [(selected)]="item.selected"
```

#### 2.2.3. 選択値の取得
- メソッドを用意して任意のタイミングで呼び出します
    - ハッシュ値全体からfilter()とmap()を使って選択値を取り出します
```
  // チェックされたリージョンを判別して配列を作成して返す
  makeCheckList() {

    let checkList = [];
    // selectedからチェック状態を確認→配列に選択値を格納
    return checkList = this.Data // Data＝ハッシュ値
      .filter((d) => d.selected) // checkされているものに絞る
      .map((d) => d.value); // checkされているリージョン名を取得
  }
```

#### 2.2.4. 選択値を用いた外部へのリクエスト
- 外部と通信する機能はServiceに書きましょう
    - Serviceを呼び出す際に、

- 機能連携イメージ
    - ボタンの(click)イベント
    - (click)="submit()"と定めていた、submit()メソッドが発火
    - 選択値を取得

    - 外部連携Serviceを呼び出す
        - この際に引数として、取得した選択値を渡す
    - Service内での外部リクエスト
        - この際に引数として渡した選択値を使う

```
submit() {

    // チェックされたリージョンリストを取得
    const REGIONS = this.makeCheckList();

    // 外部へリクエストするサービスの呼び出し
    let responseData = this.getService(REGIONS);
}
```

## 後書き
今回は以上です。このようなロジックを作るシーンは結構あると思います。
短時間でぱっと作って機能面にかける時間を増やしましょう。

### 関連記事
- [[Angular入門] mat-inputで生成したフォームから入力値を取得～双方向データバインディング](\Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)
    - 普通のcheckboxを使う場合はこっち
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)

