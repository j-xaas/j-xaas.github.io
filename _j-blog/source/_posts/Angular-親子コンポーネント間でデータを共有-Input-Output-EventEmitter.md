---
title: '[Angular]親子コンポーネント間でデータを共有(@Input/@Output/EventEmitter)'
date: 2020-04-10 01:22:31
category:
- Serverless Application Dev
- SPA (Angular)
tags:
- Angular
- '@Input'
- '@Output'
- '@EventEmitter'
toc: true
---

[Angular](https://angular.jp/)のコンポーネント間ではデータをそのまま共有することはできません。初級者が躓くポイントの一つだと思うので、解説をまとめておきます。  

<!-- toc -->

## 基礎知識
- Angularにおける親子間のデータ共有にはデコレーターを利用します
- デコレーター
    - 構成情報を付与する仕組み
        - 対象
            - クラス/プロパティ/メソッド/引数
    - 構文
        - @name
        - ex.) @NgModule

|データの流れ|使用するデコレーター|
|----|----|
| 親 ⇒ 子|@Inputデコレーター|
| 子 ⇒ 親|@Outputデコレーター|

- 例えば、子A ⇒ 親 ⇒ 子B の順にデータを共有することも可能です
    - ※関係性の遠いコンポーネント間のデータ共有にはServiceを介しましょう

それぞれ解説していきます。

## 1. 親⇒子 (@Inputを使用)
- Component構成は以下の入れ子構造を想定
    - 親
        - parent.component
    - 子
        - child.component

### 1.1. 親側
- 選択肢を表示
#### parent.component.ts
- 選択値を

```
// 選択値を保存する変数
selected: string;

// データを定義
data = [
    {name = "選択肢A"}
    {name = "選択肢B"}
    {name = "選択肢C"}
]

// click時に選択値をselectedに格納
"onClick(select_name){
    this.selected = select_name;
}

```

#### parent.component.html
- 定義したデータ型からbuttonを繰り返し表示
    - click時に選択値を引数としてonClick()を実行

```
<div *ngFor="let d of data">
    <button (click)="onClick(d.name)">{{d.name}}</button>
</div>

<!--childコンポ―ネントを入れ子で表示--->
<app-child [item]="selected"></app-child>
```

### 1.2. 子側

#### child.component.ts
- @Inputでitemを受け取ります
    - childから渡した選択値が入ります
```
// Inputが必要
import {Component , Input} form '@angular/core'

// @Inputで
export class ChildComponent{
    @Input() item: string;
}
```

#### child.component.html
- 受け取った値を表示
```
<!--ngIfでデータが空じゃなければ表示-->
<div *ngIf="item"> 
    <li>{{item}}を選択しました</li>
</div>

```

## 2. 子⇒親 (@Output/EventEmitterを使用)
- Component構成は以下の入れ子構造を想定
    - 子
        - child.component
            - 選択肢をbuttonで表示
    - 親
        - parent.component
            - 選択値によって変化

- @Outputでデコレーターを活用して、子コンポーネントで発生したイベントを親に通知

### 2.1. 子側

#### child.component.ts
1. import
2. @Outputで受け渡すイベントを定義
3. (click)でイベントを発生させるメソッドを定義
    - 共有したいデータを引数として受け渡す

- emitメソッド
    - イベントをここで発生させる
    - 構文
        - event: イベント名
        - data: $eventオブジェクトとして渡すデータ
    ```
    this.event.emit(data)
    ```

- 記載例
```
// 1. OutputとEventEmitterを利用するためにimport

import { Component, Output, EventEmitter } from '@angular/core';

export class ResourceTreeComponent {

  // 2. イベントを宣言 発生時に$eventオブジェクトとして<>で定義した型のデータを親へ受け渡す

  @Output() selected = new EventEmitter<string>();

  // 3. (click)で共有したいデータを引数としてイベントを発生させる

  onclick(data) {
    // 選択したリソースタイプをselectedへ格納
    this.selected.emit(data);
  }
```

#### child.component.html
- 想定する状況
    - 事前に定義したデータ：nodeをtreeとbuttonで選択肢を表示
    - 選択値: node.nameを親に渡したい
    - (click)でonclickを発火させる

```
  <!--mat-treeで階層型の選択肢を表示--->
  <mat-tree-node *matTreeNodeDef="let node" matTreeNodeToggle matTreeNodePadding>

    <!--click時にnode.nameを選択値として取得--->
    <button mat-button (click)="onclick(node.name)">
      {{node.name}}
    </button>
```

### 2.2. 親側
#### parent.component.html
- 子から受け取ったdataを変数selectに入れて表示
    - (selected)="onSelect($event)"
        - childでselectedイベントが発生したタイミングでonSlect()メソッドを実行
        - $eventオブジェクト
            - emitメソッド経由で渡されるdata
```
    <div>{{select}}</div>
    <!--子コンポーネントの要素にイベントバインディングを設定--->
    <app-child (selected)="onSelect($event)"></app-child>
```

#### parent.component.ts
```
export class ParentComponent implements OnInit {

  // データを受け取る変数を宣言
  select: string;

  // resourcce-treeからデータを受け取る
  onSelect($event) {
    this.select = $event;
    return this.select;
  }
```

解説は以上です。