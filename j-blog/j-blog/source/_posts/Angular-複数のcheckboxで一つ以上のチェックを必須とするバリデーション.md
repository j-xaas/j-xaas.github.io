---
title: '[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション'
date: 2020-09-10 23:00:38
categories:
- Serverless Application Dev
- SPA (Angular)
tags: 
- Angular
- 'Angular Material'
- 'mat-checkbox'
- 双方向データバインディング
thumbnail: https://user-images.githubusercontent.com/41946222/85309098-11606400-b4ed-11ea-84a1-69fee72f8208.png
toc: true
---

AngularでWEB APの画面を作る際に、複数のチェックボックスをユーザに選択させることがよくあると思います。その際にcheckboxの未チェックをリアルタイムで検知する仕組みを作ることで、ユーザに余計な画面遷移やストレスを課さずに済み、不正なリクエストも防げます。

- バリデーションとは？
    - 入力内容や記述内容が要件を満たしているか、妥当性を確認すること
    - 例えば、本人の契約意志をchecboxを確認するまで、次へボタンを無効化される仕組みをよく見かけます。必須項目をチェックせずに進んでもエラーが出るだけで、無駄な手間とストレスを生んでしまうためです
    - エラーが起きてからどうこうではなく、そもそもエラーを起こせない仕組みを実現するものというイメージ

- 参考
    - 入力欄(mat-input)のバリデーションについては以下にまとめています
        - [[Angular mat-input] バリデーションまとめ](/Angular-mat-input-バリデーションまとめ/)
    - mat-checkboxを実装し、チェックした項目の値を取得するまでは以下にまとめています
        - [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)
        - [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)

<!--toc-->

## Angularにおけるcheckboxのバリデーション方法

### A(単一のcheckbox向け） XX.invalidの利用

- checkbox要素に以下の変更を加える
    - 属性値にrequiredを追加
        - チェックを必須とするバリデーションチェックが行われる
    - #で名前を付ける
        - ”#で付けた名称.invalid”でバリデーションエラーを検知可能になります
        - つまり、checkをしていなければ、以下のように状態値を確認できます
        ```
        XXX,invalid=true
        ```
- 送信button
    - 属性値 [disabled]= || XX.invalidで条件を追加する
    - XX.invalid=trueの時＝チェックしていない時に、ボタンがdisabled=無効化される

- 単一のチェックボックスのチェックを必須化するケースはこれで問題無さそう
    - Ex. 契約事項の確認画面

### B（複数のチェックボックス向け） checkした項目を取得する配列が空か確認
- checkboxの値を取得する配列が空であれば、ボタンをdisabledにする
- 上記では”一つ以上にチェック”の検知が難しい
    - ngForでループで複数のチェックボックスを表示している場合、全てが必須になってしまう
- 状態値の取り方以外はAと同様

今回は”複数のチェックボックスから最低一つ以上にチェックすること”を強制するため、Bの実装例を以下に示す

## 実装例
### 前提
- checkboxとチェックした項目の値を取得する仕組みは実装済み
    - 未実装であれば、以下を参考にどうぞ
    - [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)
        - [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)

### 実装

- tsファイル側で先にbool値用の変数を宣言
  - check無しの時: true
  - check一つ以上の時: false

- sample.component.ts
```
checkValidation = true;
```

一つ以上のチェックがあれば、上記の値をfalseに変更する仕組みが必要

- buttonの無効化条件に追加
  - 一つでもtrueがあればdisabled（ボタンが無効化される状態）になる

- sample.component.html
```
<!--バリデーション状態によって無効化されるボタン-->
<button mat-raised-button
[disabled]="Id.invalid || passKey.invalid || this.checkValidation">

<!--ついでにエラーメッセージも用意-->
<mat-error *ngIf="checkValidation">最低一項目のチェックが必須</mat-error>

<!--チェック時にtodoLeafItemSelectionToggle(node)という関数が発火する、チェックボックスをループ表示するサンプル-->

    <ngFor...
        <ng-container>
        　<mat-checkbox
            [checked]="checklistSelection.isSelected(node)"
            (change)= "todoLeafItemSelectionToggle(node)"
          >{{node.name}}<!--dashboardにnode.nameの値を渡す (click)="onclickResource(node.name)"-->
            <!--click時にnode.nameを選択値として取得　マーキング--->
          </mat-checkbox>
        </ng-container>
        ...
```

- 今回は配列(this.checklistSelection.selected)にチェックした値が入るように機能を作っている
  - ”this.checklistSelection.selected.length = 0”の時、つまり一つもチェックしていない時に、buttonを無効化すれば良い

- 0をboolean値として扱ってくれるか試したがダメ
  - 0ならtrueにする関数が必要
  - check時にその関数を呼び出して値を変更したい
  - 初期値falseの変数を定義して、check時にtrueに変えるだけでOK

- checkboxのソース
  - check時に値を取るメソッドを呼び出している
    - (change)= "todoLeafItemSelectionToggle(node)"
```
        　<mat-checkbox
            [checked]="checklistSelection.isSelected(node)"
            (change)= "todoLeafItemSelectionToggle(node)"
          >{{node.name}}<!--dashboardにnode.nameの値を渡す (click)="onclickResource(node.name)"-->
            <!--click時にnode.nameを選択値として取得　マーキング--->
          </mat-checkbox>
```

- check時にチェック数に応じてbool値を変更する関数を定義
  - checkする度にchangeCheckboxValidation()を呼び出す

```
    /** Toggle a leaf to-do item selection. Check all the parents to see if they changed */
    todoLeafItemSelectionToggle(node: FlatTreeNode): void {
      this.checklistSelection.toggle(node);
      this.checkAllParentsSelection(node);
      // バリデーション用のbool値を取得する
      this.changeCheckboxValidation();
    }

    // チェックボックスが一つでもチェックされていれば、bool値をfalseに変更するメソッド
    changeCheckboxValidation(){
      // check数が0出なければ、checkboxバリデーション用のbool値を変更
      if (this.checklistSelection.selected.length != 0) {
        this.checkValidation = false;
      } else { // check数が0になったらtrueに
        this.checkValidation = true;
      }
    }
```

以上で”最低一つにチェック”というバリデーションを実現できた

## 参考
### Angular Mateerial関連記事
- [[Angular mat-input] バリデーションまとめ](/Angular-mat-input-バリデーションまとめ/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)