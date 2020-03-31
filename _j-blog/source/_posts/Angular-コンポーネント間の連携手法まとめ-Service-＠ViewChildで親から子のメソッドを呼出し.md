---
title: Angular コンポーネント間の連携手法まとめ(Service/＠ViewChildで親から子のメソッドを呼出し)
date: 2020-03-31 23:23:54
category:
- Serverless Application Dev
- SPA (Angular)
tags:
- Angular
- Service
- '@ViewChild'
toc: true
---

初級者向けに説明した際の備忘録です。

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/78031914-28d0fa80-739f-11ea-83a7-ed40134c8aa0.png" height="300px" width="500px" alt="Component_Cooperation_image">
</div>

<!-- toc -->

## 概要
- AngularにおけるComponent間連携
    - 別々のComponent間ではそのままデータを受け渡したり、メソッドを呼び出すことができません
    - Material等を用いた画面の作り方を覚えて、機能開発を始めたレベルの初級者が最初にハマるポイントだと思います

- 連携手法
    1. Serviceとして機能を外に取り出し、複数のComponent側から参照
        - 基本的にこちらが推奨
    2. 親子構造のコンポーネント間で直接参照する
        - @ViewChildデコレータというものを使えば手軽にできます

上記の連携手法x2について解説します。

## ① Serviceを活用する場合の手順
- Serviceに機能を取り出して参照する場合の手順を以下に示します
    - 親子間で連携させたい場合は飛ばしてください

### Serviceを作成して機能を取り出す
- 作成
```
ng g service 'service-name'
```
- service-name.service.tsを編集
    - 共有したい機能は基本的にここに書く
    - Component側に書くのはUI関係に絞る

### Serviceをimport
- 以降はServiceを取り込むComponentのtsファイルを編集します
```
import { ServiceNameService } from '../service-name.service';
```
### Serviceを注入
- constructorにserviceを注入することで、Service内のメソッドを参照可能になります
    - private "このComponent内でServiceを扱う名称": "Serviceのクラス名"
```
export class TestComponent implements OnInit {

  // Serviceを注入インスタンス化
  constructor(private TestService: ServiceNameService) {
    console.log('サービスを注入');
  }

```

### Serviceのメソッドを実行
- 上記で定めたTestService."メソッド名" で実行できます
```
this.TestService.functionA();
```


## ② @ViewChildを活用する場合の手順
- 親子間でメソッドを呼ぶ際の手順を示します

### @ViewChildと対象の子Componentをimport
- ※対象が複数の場合はViewChildrenを用いる
```
// 親子間の連携にはViewChildが必要
import { Component, ViewChild } from '@angular/core';
// 子のComponentをimport
import { ChildComponent } from '../child/child.component';
```

### @ViewChildデコレータを設定
- ※Angular8以上は@ViewChildの引数が二つ
- {static: false}を第二引数に指定すればOK
    ```
    export class ParentComponent {
        // 子コンポーネントをプロパティ:childとして設定
        @ViewChild( ChildComponent, {static: false} )
        private child: ChildComponent;
    ```

### 子のメソッドを実行
```
child.functionA();
```

- 今回の解説は以上です
    - Angularを扱う際は、なるべく細かいComponentに分けて疎結合な設計にしましょう