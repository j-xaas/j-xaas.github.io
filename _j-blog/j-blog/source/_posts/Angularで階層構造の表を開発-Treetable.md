---
title: Angularで階層構造の表を開発(Treetable)
date: 2020-03-17 21:49:37
category:
- Serverless Application Dev
- SPA (Angular)
tags:
- Angular
- Treetable
thumbnail: https://user-images.githubusercontent.com/41946222/76859307-64969b00-689c-11ea-94de-3b1dac94ed56.png
alias: /2020/03/11/Angularで階層構造の表を開発-Treetable/
---
  
　こんにちは。今回は表題の内容を解説します。
[Angular Material](https://material.angular.io/)の[Mat-table](https://material.angular.io/components/table/overview)と[Mat-tree](https://material.angular.io/components/tree/overview)を組み合わせれば実現可能ですが、初級者には難しく、コードが複雑になります。そこで、簡単に階層型の表を生成可能なOSS:[Treetable](https://github.com/supamax/ng-material-treetable)を利用します。


<div style="text-align:center;">
<img alt="Treetable" src="https://user-images.githubusercontent.com/41946222/76859307-64969b00-689c-11ea-94de-3b1dac94ed56.png" height="150px" width="400px">
</div>


<!-- toc -->

## Treetableの導入手順

### 1. install
- 以下を実行
```
npm i ng-material-treetable --save
```

- Angular Materialパッケージがinstallされていることを確認
    - 既に入れていればスルー
    ```
    npm i @angular/material @angular/cdk @angular/animations --save
    ```

### 2. app.module.tsでModuleをimport

- src/app/app.module.tsを改修
    - TreetableModuleをimport
```
import { TreetableModule } from 'ng-material-treetable';
 
@NgModule({
    ...
  imports: [
    ...
    TreetableModule
  ],
  ...
})
export class AppModule { }
```


### 3. Component側で使用
#### 3.1. HTML
- sample.component.htmlを改修
    - treetable要素を加えるだけでOKです
```
<treetable [tree]="yourTreeDataStructure"></treetable>
```
#### 3.2. TS
- sample.component.ts
    - データソースとinterface（データの型）を規定します
        - GET等で外部APIからデータを持ってくる場合は、べた書きしているデータ部分を返り値に差し替えてください
        - データ構造は自由です。無くてもOK

```
import { Component, OnInit } from '@angular/core';

// 追加
import { Node, Options } from 'ng-material-treetable';


@Component({
  selector: 'app-tree-table',
  templateUrl: './tree-table.component.html',
  styleUrls: ['./tree-table.component.scss']
})
export class TreeTableComponent implements OnInit {

  // データソースを定義
  // 省略部分はJSONデータ　表２のデータソース　class内の先頭に必要であった
  arrayOfNodesTree: Node<Task>[] = [
    {
      value: {
        name: 'Tasks for Sprint 1',
        completed: true,
        owner: 'Marco'
      },
      children: [
        {
          value: {
            name: 'Complete feature #123',
            completed: true,
            owner: 'Marco'
          },
          children: []
        },
        {
          value: {
            name: 'Update documentation',
            completed: true,
            owner: 'Jane'
          },
          children: [
            {
              value: {
                name: 'Proofread documentation',
                completed: true,
                owner: 'Bob'
              },
              children: []
            }
          ]
        }
      ]
    },
    {
      value: {
        name: 'Tasks for Sprint 2',
        completed: false,
        owner: 'Erika',
      },
      children: [
        {
          value: {
            name: 'Fix bug #567',
            completed: false,
            owner: 'Marco'
          },
          children: []
        },
        {
          value: {
            name: 'Speak with clients',
            completed: true,
            owner: 'James'
          },
          children: []
        }
      ]
    }
  ];

  constructor() { }

  ngOnInit() {
  }

}

// 表のデータ構造の定義
export interface Task {
  name: string;
  completed: boolean;
  owner: string;
}

```

### 4. Optionを設定

- sample.component.tsの改修
```
// 追加の必要有
import { Node, Options } from 'ng-material-treetable';
```
- sample.component.htmlの改修
    - [options]による指定で適用できます
```
<treetable
  [tree]="yourTreeDataStructure"
  [options]="yourOptions">
</treetable>
```

### Eventの設定
- sample.component.html
```
<treetable
  [tree]="yourTreeDataStructure"
  (nodeClicked)="logToggledNode($event)">
  <!--Clickで開閉-->
</treetable>
```
- sample.component.ts
    - nodeClickedで動くメソッドを定義
```
logToggledNode(node: Node<SomeNodeType>): void {
  console.log(node);
}
```
  
　以上で階層型の表が完成しました。後は、データソースを直接編集するか、外部から取得したデータに差し替えるだけです。手順の解説は以上です。WEB AP開発の初級者は台帳APでも試しに作ってみると良いかもしれません。(Excelはやめましょう)  
おまけとして、StackBlitzに挙がっていたサンプルソースの解説も置いておきます。

## サンプルソースの解説
- [StackBlitz](https://stackblitz.com/edit/angular-qnlruj)
- 
  - 以下の書き方で今回の要件は満たせそう
  ```
  arrayOfNodesTree: Node<Task>[] = 外部からGETしてきたデータ
  ```

- app.component.ts
```
import { Component } from '@angular/core';

// 追加の必要有
import { Node, Options } from 'ng-material-treetable';

@Component({
  selector: 'my-app',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {

  // 表１のOption
  treeOptions: Options<Report> = {
    capitalisedHeader: true,
    customColumnOrder: [// 列名を定義？
      'owner', 'name', 'backup', 'protected'
    ]
  };

// 省略部分はJSONデータ　表１のデータソース
  singleRootTree: Node<Report> = {
    value: {
      name: 'Reports',
      owner: 'Eric',
      protected: true,
      backup: true
    },
    children: [
      {
          中略
      }
    ]
  };

// 省略部分はJSONデータ　表２のデータソース
  arrayOfNodesTree: Node<Task>[] = [
    {中略
    }
  ]

  logNode(node: Node<Report>) {
    console.log(node);
  }

}

// 表のデータ型　列の要素を定義

// 表1
export interface Report {
  name: string;
  owner: string;
  protected: boolean;
  backup: boolean;
}

// 表２
export interface Task {
  name: string;
  completed: boolean;
  owner: string;
}

```

- 次にHTML側を確認
  - かなりシンプルに書ける
    - treetable要素の中にデータソースとして定義したものを指定するだけ
  ```
  <treetable
    [tree]="TS側で定めたデータソース名"
    (nodeClicked)="logNode($event)"> // Clickで開閉
  >
  </treetable>
  ```

```
// 表１
<h2>Tree as Single Root Node</h2>
<treetable
  [tree]="singleRootTree"
  [options]="treeOptions"
  (nodeClicked)="logNode($event)">
</treetable>

// 表２
<h2>Tree as Array of Nodes</h2>
<treetable
  [tree]="arrayOfNodesTree" // tsでデータソースを定めたやつ
  (nodeClicked)="logNode($event)"> 
</treetable>

```

## 自分でサンプルを開発した際のメモ
- tree-table Componentを新たに生成
```
\src\app> ng g component tree-table
```

- 上の階層のComponentでtree-tableを参照するように変更
  - serveで画面への反映を確認。問題無し
```
        </mat-card-header>
        <mat-card-content>
          <div>

            <app-tree-table></app-tree-table>
            <!--tree tableコンポーネントを挿入---->

          </div>
        </mat-card-content>
```

### サンプルデータで表を出す
- tree-tableコンポーネントを改修

- HTMLの改修
```
<treetable
  [tree]="arrayOfNodesTree"
  (nodeClicked)="logNode($event)">
</treetable>
```
- tsの改修
  - import, データソース, interfaceを定義
  - 列を減らしてみる

```
import { Node, Options } from 'ng-material-treetable';

  // 省略部分はJSONデータ　表２のデータソース
  arrayOfNodesTree: Node<Task>[] = [
    {
      value: {
        name: 'Tasks for Sprint 1',
        completed: true,
        owner: 'Marco'
      },
      children: [
        {
          value: {
            name: 'Complete feature #123',
            completed: true,
            owner: 'Marco'
          },
          children: []
        },
        {
          value: {
            name: 'Update documentation',
            completed: true,
            owner: 'Jane'
          },
          children: [
            {
              value: {
                name: 'Proofread documentation',
                completed: true,
                owner: 'Bob'
              },
              children: []
            }
          ]
        }
      ]
    },
    {
      value: {
        name: 'Tasks for Sprint 2',
        completed: false,
        owner: 'Erika',
      },
      children: [
        {
          value: {
            name: 'Fix bug #567',
            completed: false,
            owner: 'Marco'
          },
          children: []
        },
        {
          value: {
            name: 'Speak with clients',
            completed: true,
            owner: 'James'
          },
          children: []
        }
      ]
    }
  ]

  logNode(node: Node<Report>) {
    console.log(node);
  }

}

// 表のデータ構造を決定
export interface Task {
  name: string;
  completed: boolean;
  owner: string;
}

```


- AP画面への反映を確認
```
ng serve --open
```
- 以下のように表示された

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/76863288-9ca0dc80-68a2-11ea-88a6-e7bb1c46a3c0.png" height="100px" width="400px">
</div>

- エラーが発生
  - class内で初めにデータの定義が必要
  - 順番を入れ替えればOK

- 最終的なtsファイル
```
import { Component, OnInit } from '@angular/core';

// 追加
import { Node, Options } from 'ng-material-treetable';


@Component({
  selector: 'app-tree-table',
  templateUrl: './tree-table.component.html',
  styleUrls: ['./tree-table.component.scss']
})
export class TreeTableComponent implements OnInit {

  // データソースを定義
  // 省略部分はJSONデータ　表２のデータソース　class内の先頭に必要であった
  arrayOfNodesTree: Node<Task>[] = [ 
    {
      value: {
        name: 'Tasks for Sprint 1',
        completed: true,
        owner: 'Marco'
      },
      children: [
        {
          value: {
            name: 'Complete feature #123',
            completed: true,
            owner: 'Marco'
          },
          children: []
        },
        {
          value: {
            name: 'Update documentation',
            completed: true,
            owner: 'Jane'
          },
          children: [
            {
              value: {
                name: 'Proofread documentation',
                completed: true,
                owner: 'Bob'
              },
              children: []
            }
          ]
        }
      ]
    },
    {
      value: {
        name: 'Tasks for Sprint 2',
        completed: false,
        owner: 'Erika',
      },
      children: [
        {
          value: {
            name: 'Fix bug #567',
            completed: false,
            owner: 'Marco'
          },
          children: []
        },
        {
          value: {
            name: 'Speak with clients',
            completed: true,
            owner: 'James'
          },
          children: []
        }
      ]
    }
  ];

  constructor() { }

  ngOnInit() {
  }

}

// 表のデータ構造の定義
export interface Task {
  name: string;
  completed: boolean;
  owner: string;
}

```

- 参考ページ
    - [Angular Material TreeTable Component](https://www.npmjs.com/package/ng-material-treetable)
    - [TreeTable Component画面](http://ng-material-treetable.surge.sh/)
  
- Mat-treeとMat-tableを組み合わせて開発する場合の参考
  - [任意のネスト、キー名をTreeで表示するコードサンプル](https://stackblitz.com/edit/mat-tree-with-drag-and-drop?file=app%2Ftree-flat-overview-example.html)
