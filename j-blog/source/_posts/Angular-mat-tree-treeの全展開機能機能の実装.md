---
title: '[Angular mat-tree] treeの全展開機能機能の実装'
date: 2020-12-09 23:33:41
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

## 概要
- 実装したい機能
  - 複数のtreeを一括で展開/折り畳みする機能
- 実現方法
  - mat-treeにはデフォルトで全展開に利用可能なメソッドが用意されているため、それらを利用する

- 全展開
  - expandAll() メソッド
- 全閉
  - collapseAll() メソッド

- 全展開機能の書式
```
tree.treeControl.expandAll();
```

## 全展開/全閉ボタンの実装
```
<button (click)="tree.treeControl.collapseAll()">collapse All</button>
<button (click)="tree.treeControl.expandAll()">expand All</button>
<mat-tree #tree [dataSource]="dataSource" [treeControl]="treeControl">
  ...
<mat-tree>
```

- mat-iconを利用した例
```
<button mat-icon-button (click)="treeControl.expandAll()"><mat-icon>unfold_more</mat-icon></button><!--全展開ボタン-->
<button mat-icon-button (click)="treeControl.collapseAll()"><mat-icon>unfold_less</mat-icon></button><!--全閉ボタン-->
```

## 参考
### 関連記事
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
- [[Angular Service入門] ロジックを切り出し、複数Componentで再利用可能にする](/Angular-Service入門-ロジックを切り出し、複数Componentで再利用可能にする/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)

### その他
- [Angular Material 6.0.1ツリーがデフォルトで開かれ、すべて展開/折りたたみます](https://www.it-swarm-ja.tech/ja/angular/angular-material-601%E3%83%84%E3%83%AA%E3%83%BC%E3%81%8C%E3%83%87%E3%83%95%E3%82%A9%E3%83%AB%E3%83%88%E3%81%A7%E9%96%8B%E3%81%8B%E3%82%8C%E3%80%81%E3%81%99%E3%81%B9%E3%81%A6%E5%B1%95%E9%96%8B%E6%8A%98%E3%82%8A%E3%81%9F%E3%81%9F%E3%81%BF%E3%81%BE%E3%81%99/838603092/)