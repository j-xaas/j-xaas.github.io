---
title: '[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する'
date: 2020-04-23 22:15:13
category:
- Serverless Application Dev
- SPA (Angular)
tags:
- Angular
- Schematics
- Angular Material
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/84594690-0410fd00-ae8f-11ea-8ad2-efb9a4477226.png
---

今回はAngular開発におけるComponentの自動生成手法について解説します。  
アプリの基本的な画面はこの手法を用いれば、コードを書くまでもなく高速で開発できます。サービスとしての独自性の無い単純作業は極力自動化しましょう。  
  

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/80100008-4feba800-85aa-11ea-8bad-dc2ceb714f16.png" height="250px" width="600px" alt="自動生成したナビゲーション">
</div>

<!-- toc -->

## Angular Schematicsとは？
- [Schematics](https://material.angular.io/guide/schematics)の機能
    1. カスタマイズされたComponentをコマンドで自動生成
        - ありがちなUIは大抵これで自動生成できます
            - 今回は例として画面のサイズに応じて自動的に開閉するサイドナビとツールバーでできたナビゲーションを自動生成します
    2. モジュールの追加
    3. 設定の変更

- 例えば、Angular CLIの以下のコマンドの処理はSchematicsで定義されています
    - ng generate
    - ng add
- @angular/cdkと@angular/materialに含まれています


- Angular CLIのng generateやng add といったコマンドの処理はAngular Schematicsによって定義されている
    - 自分で作ることもできます
    - 既存のSchematicsを拡張することも可能

- 実行コマンド
```
ng generate @angular/material:<schematics-name> <component-name>
```

- 自作可能
    - ありがちな構成を用意しておけば、サービスを高速で量産できます
    - わざわざ手順書を残すよりも遥かに効率的です

## 準備 
### Angular PJを生成
生成済みであればここはスルーしてください

- 1. APの雛形を作成
```
ng new 'sample-ap'
```
    - Angular PJの初期画面はこんな感じです
    ```
    ng serve --open
    ```
    ![image](https://user-images.githubusercontent.com/41946222/79465920-e530ee80-8036-11ea-8ddb-4e21fce97003.png)

- 2. Angularの初期画面をまっさらにします
    - src/app/app.component.html をrouter-outletだけを残して削除
    ```
    <router-outlet></router-outlet>
    ```

### Schematicsをinstall
- ng addコマンドで
- npm installを使って入れると設定ファイルを弄る必要が出たりします

- 1. まずはAngular Materialを入れます
```
ng add @angular/material
```
- 出力
    - 好きなthemeを選択
        - ひとまずIndigo/Pinkにします
    - 他も基本yesでOKです
```
/firebase-sample (master) $ ng add @angular/material
Installing packages for tooling via npm.
Installed packages for tooling via npm.
? Choose a prebuilt theme name, or "custom" for a custom theme: 
❯ Indigo/Pink        [ Preview: https://material.angular.io?theme=indigo-pink ] 
  Deep Purple/Amber  [ Preview: https://material.angular.io?theme=deeppurple-amber ] 
  Pink/Blue Grey     [ Preview: https://material.angular.io?theme=pink-bluegrey ] 
  Purple/Green       [ Preview: https://material.angular.io?theme=purple-green ] 
  Custom 
? Set up HammerJS for gesture recognition? Yes
? Set up browser animations for Angular Material? Yes
UPDATE package.json (1437 bytes)
```

- 2. cdkをinstall
```
ng add @angular/cdk
```
- 出力
    - packege.jsonの中身を自動で改修してくれます
```
Skipping installation: Package already installed
UPDATE package.json (1437 bytes)
```

### 画面表示用のComponentを生成
- top
```
\src\app> ng g component top
```

## ナビゲーションを自動生成

- 実行
```
\src\app> ng g @angular/material:navigation
```

- 出力
```
$ ng generate @angular/material:navigation main-nav
CREATE src/app/navi/navi.component.scss (193 bytes)
CREATE src/app/navi/navi.component.html (938 bytes)
CREATE src/app/navi/navi.component.spec.ts (1234 bytes)
CREATE src/app/navi/navi.component.ts (583 bytes)
UPDATE src/app/app.module.ts (1126 bytes)
```

- src/appの配下にmain-navコンポーネントが自動生成されています
```
src\app\main-nav> ls

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----       2020/04/23     14:59            936 main-nav.component.html
-a----       2020/04/23     14:59            193 main-nav.component.scss
-a----       2020/04/23     14:59           1256 main-nav.component.spec.ts
-a----       2020/04/23     14:59            598 main-nav.component.ts
```

## 画面に反映

### 1.  top componentに埋め込む
- src\app\top\top.component.html
    - app-component_nameで要素を作るだけで入れ子にできます
    
```
<app-main-nav></app-main-nav> <!--main-naviコンポーネントを参照-->
```

### 2. topをルーティングで指定
- 画面起動時にtopコンポーネントが表示されるように設定

- src\app\app-routing.module.ts
```
//routeを定めるコンポーネントをimport
import { TopComponent } from './top/top.component';

const routes: Routes = [
  {path: '', redirectTo: '/top', pathMatch: 'full'},　//デフォルトのパス。AP起動時
  {path: 'top', component: TopComponent }, // top画面のパスを定義

```

### 3. Localhostで画面への反映を確認

- APを起動
```
ng serve --open
```

- 以下のようにナビが表示されます
  - ツールバーには自動的にAngular-PJ名が入ります

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/80067373-a9d37a00-8578-11ea-941e-514a54d91c9d.png" height="250px" width="600px" alt="自動生成したナビゲーション">
</div>

- サイドナビの表示は画面サイズに応じて変化
  - 大
    - デフォルトで表示
  - 小  
    - デフォルトでは非表示/バーガーアイコンの押下で開閉

- Windowを狭めてみる

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/80067621-2403fe80-8579-11ea-9b48-569c4f602ab5.png" height="300px" width="500px">
</div>

- バーガーアイコンを押下
    - Menuが表示されます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/80066424-e2725400-8576-11ea-8782-72c40e3e21e4.png" height="400px" width="500px">
</div>


- あとは各画面を生成してサイドナビのMenuから飛べるようにすれば、アプリの雛形ができます

- この程度の画面はSchematicsを使って3分で実装しましょう
    - 初級者が自力で書くと、ここまででもだいぶ工数を食います

## 自動生成されるコードの解説
- Schematicsで自動生成したデフォルト
- main-nav.component.html
    - angular materialのmat-toolberやmat-sidenaviが使われています
    - WEBアプリだと毎回書きがちなパターン
```
<mat-sidenav-container class="sidenav-container">

  <!--サイドナビ-->
  <mat-sidenav #drawer class="sidenav" fixedInViewport
      [attr.role]="(isHandset$ | async) ? 'dialog' : 'navigation'"
      [mode]="(isHandset$ | async) ? 'over' : 'side'"
      [opened]="(isHandset$ | async) === false">
    <mat-toolbar>Menu</mat-toolbar>
    <mat-nav-list> <!--ここをrouterLink="/pass-name"で各画面に飛べるように改修-->
      <a mat-list-item href="#">Link 1</a>
      <a mat-list-item href="#">Link 2</a>
      <a mat-list-item href="#">Link 3</a>
    </mat-nav-list>
  </mat-sidenav>

  <!--ヘッダー-->
  <mat-sidenav-content>
    <mat-toolbar color="primary">
      <!--sidenavの開閉用ボタン-->
      <button
        type="button"
        aria-label="Toggle sidenav"
        mat-icon-button
        (click)="drawer.toggle()"
        *ngIf="isHandset$ | async">
        <mat-icon aria-label="Side nav toggle icon">menu</mat-icon>
      </button>
      <span>scrab-for-aws</span>
    </mat-toolbar>
    <!-- Add Content Here -->
  </mat-sidenav-content>
</mat-sidenav-container>

```

- Tips
    - ヘッダーに複数の要素を表示して、画面サイズに応じて配置を変化させたい場合
    - 要素の間に以下を挟めばレスポンシブな余白ができます
```
    <span style="flex:auto;"></span><!----余白-->
```

- この後の作業イメージ（画面遷移の実装）
    1. 各画面用のコンポーネントを生成
    ```
    ng g component <component-name>
    ```
    2. app-routing.module.tsでパスを設定
    3. main-nav.component.htmlを改修
        - 各ボタンのrouterLinkに各Componentのパスを設定


## 参考
- 公式
    - [Schematics](https://material.angular.io/guide/schematics)
- Schematicsの作り方
    - [Advent Calender 2018 Schematicsを作ってみよう](https://qiita.com/puku0x/items/462a038133e7233dfaed)
  - [Angular CLI 6.0.0のng addとSchematicsを使ってAngularMaterialを簡単セットアップ](https://qiita.com/daikiojm/items/c8045f9a5cc44c2848b5)