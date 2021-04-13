---
title: '[Angular] (mouseover)/(mouseout)イベントで各ボタンの説明を表示'
date: 2020-12-09 23:51:12
categories:
- Serverless Application Dev
- SPA (Angular)
tags: 
- Angular
- 'Angular Material'
- 'mat tree'
thumbnail: https://user-images.githubusercontent.com/41946222/85309098-11606400-b4ed-11ea-84a1-69fee72f8208.png
toc: true
---


Angularにおけるマウスオーバーイベントについてのメモ

## 概要
Angularでは(click)イベントと同様に、要素に(mouseover)イベントを定めることで、マウスオーバー時に実行するメソッドを容易に指定できる

- 書式
```
<div (mouseover)="sampleMethod()">
```

## 実装例
各ボタンの説明をマウスオーバー時のみ表示する

- 実現方法
  - 説明を表示したい要素に以下のイベントを設定  
    - (mouseover)
    - (mouseout)
  - イベント発生時に状態値を変更
  - ngIfで状態値に応じて説明を表示

- 実装例
  - [[Angular mat-tree] treeの全展開機能機能の実装](Angular-mat-tree-treeの全展開機能機能の実装)で実装したボタンのホバー時に説明を表示するように改修
  - (mouseover)時に状態値：showExpandExplanationの値を変更 → ngIfを設定した要素が表示される 
```
<button mat-icon-button (click)="treeControl.expandAll()" (mouseover)="showExpandExplanation=true" (mouseout)="showExpandExplanation=false"><mat-icon>unfold_more</mat-icon></button><!--全展開ボタン-->
<div *ngIf="showExpandExplanation">全展開ボタン</div>

<button mat-icon-button (click)="treeControl.collapseAll()" (mouseover)="showCollapseExplanation=true" (mouseout)="showCollapseExplanation=false"><mat-icon>unfold_less</mat-icon></button><!--全閉ボタン-->
<div *ngIf="showCollapseExplanation">折り畳みボタン</div>
```

## 参考
### 関連記事
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
- [[Angular Service入門] ロジックを切り出し、複数Componentで再利用可能にする](/Angular-Service入門-ロジックを切り出し、複数Componentで再利用可能にする/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)