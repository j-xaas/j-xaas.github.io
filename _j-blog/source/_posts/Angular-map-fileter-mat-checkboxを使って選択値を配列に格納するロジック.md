---
title: '[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック'
date: 2020-04-13 23:26:33
category:
- Serverless Application Dev
- SPA (Angular)
tags:
- Angular
- 'map()'
- 'filter()'
- 'mat-checkbox'
toc: true
---

- 複数のチェックボックスの中で選択項目を判別して、それぞれの固有値を配列に格納するロジックの作り方を解説します。
    - 以下のSample画面で言うと、チェックされたJapanの値を配列に格納します
- 使いどころ
    - checkした項目に応じて外部サービスに対するリクエストに含めるパラメータを変化させる


<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79117116-346fe880-7dc5-11ea-9065-ded3d7d9e307.png" height="90px" width="300px" alt="画面イメージ">
</div>

<!-- toc -->

## 基礎知識
- すぐに実装に取り掛かりたい方は飛ばしてOK

### Angularにおけるデータ加工ロジック
- 基本的にmapやfilterを利用する
    - アンチパターン
        - 何でもfor,if文
    - 何故か?
        - mapは並列処理
            - for文と比較して処理が高速
        - 書式がシンプル
            - 開発期間短縮
            - 可読性向上
            - リファクタリングの簡略化

### map
- [map()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
    - 処理
        - 引数として与えられた配列の各要素に対して、一つづつ関数を実行
        - カウンタが不要な配列向けのfor文というイメージでOKです
    - 返り値
        - 実行結果から新たな配列を生成

### filter
- [filter()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
    - 処理
        - 引数として与えられたテスト関数を各配列要素に対して実行
        - 配列向けのif文というイメージでOKです
    - 返り値
        - テスト関数の条件に合格した要素からなる新しい配列を生成


- 利用イメージ
```
// 配列を宣言
const array = ['AA', 'BB', 'CC', 'DDD', 'EEEE', 'FFFFF'];

// 配列arrayを引数として、条件式3文字以上の要素を抽出して、配列として返す
const result = array.filter(array => array.length >= 3);

console.log(result);
// output: Array ["DDD", "EEEE", "FFFFF"]
```

- 構文は以下
    - value: 配列の要素
    - index: インデックス
    - array: 操作中の配列本体
```
var newArray = arr.filter(callback(element[, index[, array]])[, thisArg])
```

- 他の利用例は以下を参考にどうぞ
    - [MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)をご覧ください
    - [その名の通りのfilter()で絞り込み。（配列とかおれおれAdvent Calendar2018 – 12日目）](https://ginpen.com/2018/12/12/array-filter/)


## 実装
ざっくりイメージは以下
- tsファイル
    - 役割
        - データ構造の定義
        - 判別ロジック
            - (click)イベントで発火
- html
    - 役割
        - checkboxの表示
        - tsファイルで指定したデータの参照
        - (click)イベントの指定

### データ構造の定義

- 配列dataに以下のようにデータ構造を定義します
    - valueを読み替えて使用してください
- sample.component.ts
```
// dataにcheckboxの値とチェック状態の初期値：selectedを定義
data = [
    { value: 'Japan', selected: false}, // 初期値 true
    { value: 'America', selected: false},
    { value: 'China', selected: false},
    { value: 'England', selected: false},
]
```

### 状態判別ロジック
- 状態を判別するmethodを宣言します
- ざっくりイメージ
    - filter()
        - data全体からチェック済みのもの(Selected=true)に絞る
    - map()
        - 残ったdataのvalueで配列を生成
    - Listに格納
- sample.component.ts
```
    makeList() {
        // 格納先の配列を宣言
        let countryList = [];

        // selectedからチェック状態を確認して、配列にを格納していく
        countryList = this.data 
            .filter((d) => d.selected) // checkされているものに絞る
            .map((d) => d.value); // valueを取得
        return countryList; //
    }
```

### 配列データを引数としてリクエストを送る際の例
- 選択値を使って外部サービスへリクエストを行うケースを想定
    - service側で受け取った変数に応じた分岐処理を用意しておくイメージです
- sample.component.ts
```
submit(){
    // 選択値を受け取る
    const selectedCountries = this.makeList();

    // 任意のServiceのメソッドを実行
    getService.requesrMethod(selectedCountries)
}
```


### tsに定めたdataに応じてチェックボックスのリストを表示するHTML

#### 基礎知識
- [ngFor](https://angular.jp/api/common/NgForOf#ngforof)
    - 指定した配列をループ表示させる[構造ディレクティブ](https://angular.jp/guide/structural-directives)
        - べた書きするよりもすっきり書けます
        - 今回は以下のように配列:dataをループさせています
            - ngForのループの中でdataを変数itemとして使うという意味
        ```
        *ngFor= "let item of data; index as i"
        ```
    - ng-container x *ngFor
        - 組み合わせると、ng-containerの中の要素全てを一まとまりでループさせることが可能です
- [mat-checkbox](https://material.angular.io/components/checkbox/overview)
    - 通常のcheckboxはイケてないので[Angular Material](https://material.angular.io/)を使います
        - 通常のcheckboxを使う場合は"mat-checkbox"を"input"に読み替えてください

- Angularのフォーム
    - 二種類あります
        - テンプレート駆動型
            - 書式がシンプルで手軽に実装可能であるため採用
            - #myForm="ngForm" と設定することでこちらになる
        - モデル駆動型 (Reactive駆動型)
            - コードは冗長になりがち
            - より柔軟に複雑な要件を満たせる
#### 記載例
- sample.component.html
    ```
            <div>Choose Country</div>
            <!---formで選択したリージョンの情報を纏めてからリクエスト------>
            <form #myForm="ngForm" (ngSubmit)="submit()">

              <!---定義した配列dataをngForでループ表示 要素をiでカウント---->
              <ng-container *ngFor= "let item of data; index as i" >

                <!---inputで通常のCheckboxに変えてもOK--->
                <mat-checkbox type="checkbox" name = "country{{i}}"
                [(ngModel)]="data[i].selected"
                [value]="item.value"
                (change)="checkCountry(i)"
                >{{data[i].value}}</mat-checkbox> 

              </ng-container>

              <!---チェックボックスから得たデータを纏めてリクエストするボタン--->
              <button mat-raised-button color="primary">Get Data</button>
              <!--(ngSubmit)=で設定されたsubmit()が実行される-->
            </form>
    ```

### 画面で確認
- 以下のように表示されます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79117116-346fe880-7dc5-11ea-9065-ded3d7d9e307.png" height="90px" width="300px" alt="画面イメージ">
</div>


### おまけ: for文で書く場合
- for文でも判別ロジック書いたので参考までにどうぞ
    - だいぶ冗長になってしまいます

```
    // 格納先を宣言
    let regionList = [];

    // for文で書く場合
    var num=0;
    for ( let c = 0; c <= this.data.length - 1 ; c++) {
      if (this.data[c].selected) { // Checkされているか判定 trueなら実行
          // 配列に要素を追加 .push()
          countryList.push(this.data[c].value); // リージョン名を格納
          console.log(countryList[num] + ' checked'); // デバッグ用
          num++;
      } else {
        console.log('Not Check'); // デバッグ用
      }
    }
    console.log(countryList);
```
