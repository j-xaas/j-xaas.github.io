---
title: '[Angular入門] AngularCLI環境構築~PJの開始,コンポーネントの生成,初めての関数開発'
date: 2020-08-07 19:37:38
category:
- Serverless Application Dev
- SPA (Angular)
tags:
- Angular
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/83424662-abda0400-a467-11ea-831a-2b146d29a1c5.png
---

　未経験からモダンなWebアプリ開発を学びたい方向けに、Angularというフレームワークを用いた開発の初歩を解説します。Angularは[公式ドキュメント](https://angular.jp/)が豊富なのですが、基本的にWeb開発の経験者を想定して書かれています。極力丸めた表現で書いたので、この記事で「自分にもアプリ開発できそうだ」と一歩踏み出せれば幸いです。こういった記事や実戦経験で概要を掴んで、詳細は[書籍](https://amzn.to/2XAs0Py)で学習するという進め方がおすすめです。

### 前提条件
- Node.jsのインストール
    - Angularで開発するにはNode.jsが必要です
    - JavaScriptを動かすための土台のようなもの

### CLIのインストール
- angular cliをインストール
    - angularのコマンドを使うためのツールを使えるようにする
```
npm install -g @angular/cli
```

- ngコマンド
    - ngはAngularの略
    - 例えば、ng generate ~~でファイルのセットを自動生成できる 

- ngコマンドのバージョンを見てみる
```
ng version
```
- 以下が表示されれば、問題なく入っている
```
     _                      _                 ____ _     ___
    / \   _ __   __ _ _   _| | __ _ _ __     / ___| |   |_ _|
   / △ \ | '_ \ / _` | | | | |/ _` | '__|   | |   | |    | |
  / ___ \| | | | (_| | |_| | | (_| | |      | |___| |___ | |
 /_/   \_\_| |_|\__, |\__,_|_|\__,_|_|       \____|_____|___|
                |___/
    

Angular CLI: 10.0.5
Node: 14.7.0
OS: darwin x64

Angular: 
... 
Ivy Workspace: 

Package                      Version
------------------------------------------------------
@angular-devkit/architect    0.1000.5
@angular-devkit/core         10.0.5
@angular-devkit/schematics   10.0.5
@schematics/angular          10.0.5
@schematics/update           0.1000.5
rxjs                         6.5.5
```

## プロジェクトを作成
- プロジェクト
    - Angularにおけるアプリの単位

- ng newコマンドでプロジェクトを作成
```
ng new "ap-name"
```
- 上記を実行するといくつか質問をうける

- routing
    - routingとは画面遷移（APの画面の移動）のこと
    - Angularのデフォルトの機能を使うと楽に実装できる
    - yでOK
        - routingの設定ふぁいるが自動生成される。あとでいじる
```
? Would you like to add Angular routing? Yes
```

- stylesheetは何を使う？
    - cssは聞いたことがあるひとも多いと思うが、文字の色などのデザイン面を定義するファイルのこと
    - AngularではScssを利用するのが基本
    - scssはcssの拡張版

```
? Which stylesheet format would you like to use? SCSS   [ https://sass-lang.com/documentation/
syntax#scss
```

- 上記の質問に答えるとAngular PJの自動生成を開始する
    - 以下のようにAngularに必要なファイルのセットがここでできている
```
CREATE ap-name/README.md (1024 bytes)
CREATE ap-name/.editorconfig (274 bytes)
CREATE ap-name/.gitignore (631 bytes)
CREATE ap-name/angular.json (3662 bytes)
CREATE ap-name/package.json (1250 bytes)
CREATE ap-name/tsconfig.base.json (458 bytes)
CREATE ap-name/tsconfig.json (426 bytes)
CREATE ap-name/tslint.json (3184 bytes)
CREATE ap-name/.browserslistrc (853 bytes)
CREATE ap-name/karma.conf.js (1019 bytes)
CREATE ap-name/tsconfig.app.json (292 bytes)
CREATE ap-name/tsconfig.spec.json (338 bytes)
CREATE ap-name/src/favicon.ico (948 bytes)
CREATE ap-name/src/index.html (292 bytes)
CREATE ap-name/src/main.ts (372 bytes)
CREATE ap-name/src/polyfills.ts (2835 bytes)
CREATE ap-name/src/styles.scss (80 bytes)
CREATE ap-name/src/test.ts (753 bytes)
CREATE ap-name/src/assets/.gitkeep (0 bytes)
CREATE ap-name/src/environments/environment.prod.ts (51 bytes)
CREATE ap-name/src/environments/environment.ts (662 bytes)
CREATE ap-name/src/app/app-routing.module.ts (245 bytes)
CREATE ap-name/src/app/app.module.ts (393 bytes)
CREATE ap-name/src/app/app.component.scss (0 bytes)
CREATE ap-name/src/app/app.component.html (25757 bytes)
CREATE ap-name/src/app/app.component.spec.ts (1062 bytes)
CREATE ap-name/src/app/app.component.ts (212 bytes)
CREATE ap-name/e2e/protractor.conf.js (869 bytes)
CREATE ap-name/e2e/tsconfig.json (299 bytes)
CREATE ap-name/e2e/src/app.e2e-spec.ts (640 bytes)
CREATE ap-name/e2e/src/app.po.ts (301 bytes)
✔ Packages installed successfully.
    Successfully initialized git.
```

- 生成されたアプリを見てみる
    - ng newで指定した名前のディレクトリができている
```
ls

ap-name
```
- 中身を見てみる
    - Angularの基本セットが入っている
    - 一個ずつ使い方を理解していけばOK
```
cd ap-name
ap-name % ls
README.md               karma.conf.js           package.json            tsconfig.base.json      tslint.json
angular.json            node_modules            src                     tsconfig.json
e2e                     package-lock.json       tsconfig.app.json       tsconfig.spec.json
```

- ng newで自動生成したAngular Projectの構成
![Angular Project構成](https://user-images.githubusercontent.com/68212997/89703973-ee015380-d98a-11ea-8417-576765f6a06e.png)

- 基本的にいじるのは app-name/src/appの中身
    - 見てみるとapp componentのセットが入っている
    - app componentは最も大きいコンポーネント
```
ap-name % cd src/app 

app % ls
app-routing.module.ts   app.component.scss      app.component.ts
app.component.html      app.component.spec.ts   app.module.ts
```

- APを起動してみる
    - serveでAngularで作ったアプリを起動できる
    - --openでブラウザ（EX. Chrome）
```
ng serve --open
```

- 起動したアプリの画面が表示される
    - ng newで生成したアプリの初期画面
    - コンポーネントの追加やファイルの編集のあとも同様にして、変化を確認するのが開発の流れ

![Angular PJ初期画面](https://user-images.githubusercontent.com/68212997/89630730-b9ce5a00-d8da-11ea-8ffc-eb911d683b2b.png)

- エディタにCloud Shellなどを利用している場合の注意
    - -p 8080と書いてserve
    ```
    ng serve -p 8080
    ```    
    - 画面右上のアイコンを押下”ポート8080でプレビュー”を選択すれば起動したアプリをブラウザで確認できる
        - 詳細は以下の記事にまとめてある
            - [Cloud Shellのプレビュー表示における注意点](https://j-xaas.github.io/Google-cloud-shell-x-Hexo-%E7%92%B0%E5%A2%83%E6%A7%8B%E7%AF%89-%E8%A8%98%E4%BA%8B%E7%B7%A8%E9%9B%86/#%E8%A8%98%E4%BA%8B%E3%81%AE%E3%83%97%E3%83%AC%E3%83%93%E3%83%A5%E3%83%BC%E8%A1%A8%E7%A4%BA%E3%81%AB%E3%81%8A%E3%81%91%E3%82%8B%E6%B3%A8%E6%84%8F%E7%82%B9)
    - 解説
        - portというものの指定が必要
            - デフォルトではhttp://localhost:4200/
                - 端末のport 4200でAPが起動している
        - Cloud Shellはクラウド上のサーバで動いている
            - serveで実行したアプリもクラウド上のサーバで動いている
            - 確認ためにはCloudShellのプレビュー機能が必要
            - プレビュー機能がポート8080にしか対応していない。そのため、serveの実行時にportの指定を行っている

- serveは Ctrl＋Cで止められる

### ファイルを編集して画面の変化を見てみる
- app.component.htmlを開く
    - 何やら大量に書いてある
    ```
    <!-- * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * -->
    <!-- * * * * * * * * * * * The content below * * * * * * * * * * * -->
    <!-- * * * * * * * * * * is only a placeholder * * * * * * * * * * -->
    <!-- * * * * * * * * * * and can be replaced. * * * * * * * * * * * -->
    <!-- * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * -->
    <!-- * * * * * * * * * Delete the template below * * * * * * * * * * -->
    <!-- * * * * * * * to get started with your project! * * * * * * * * -->
    <!-- * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * -->
    <style>
    :host {
        font-family: -apple-system,     BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
        font-size: 14px;
        color: #333;
        box-sizing: border-box;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
    }
    ```
    - 先ほど見た初期画面がここで定義されている

- 全て消して、適当に書いてみる。Command+Sなどで保存
```
<div>初めてのAngular</div>
<br>
<button>スイッチ</button>
```
- serveで変化を確認してみる
```
ng serve --open
```

- 表示されている
![Angular preview](https://user-images.githubusercontent.com/68212997/89632870-e637a580-d8dd-11ea-92de-b49dacf68059.png)


ここまでで、ファイルを編集してserveで確認していくイメージが湧くはず

### コンポーネントを生成

- コンポーネントとは？
    - 画面の中の塊の単位だと考えれば良い
    - Angularではコンポーネントを入れ子構造にして、画面を作っていく
    - コンポーネント毎に以下のファイルの郡のセットが自動生成され、それらをいじっていく
        - ~~component.html
            - 表示される画面
            - 先ほどいじったのは、appコンポーネントのhtml
        - ~~component.scss
            - デザインを担当
            - Ex. サイズや色など
        - ~~component.ts
            - 機能を担当
            - tsはTypescriptの略
            - typescriptはJavaScriptの拡張版
            - 主にここを開発していく
        - ~~somponent.spec.ts
            - テスト用のファイル

- コンポーネントを自動生成する
    - 以下を実行すると上記のセットが生成される
    - 実行する場所はsrc/app配下
```
ng g component "component-name"
```
- 以下のようにファイルが生成される
```
app % ng g component "component-name"
CREATE src/app/component-name/component-name.component.scss (0 bytes)
CREATE src/app/component-name/component-name.component.html (29 bytes)
CREATE src/app/component-name/component-name.component.spec.ts (678 bytes)
CREATE src/app/component-name/component-name.component.ts (307 bytes)
UPDATE src/app/app.module.ts (505 bytes)
```
- lsで確認
    - component-nameというディレクトリを確認できる
```
app % ls
app-routing.module.ts   app.component.scss      app.component.ts        component-name
app.component.html      app.component.spec.ts   app.module.ts
```

- 生成されたコンポーネントの中身を見てみる
    - コンポーネントのファイル郡ができている
```
app % cd component-name
omponent-name % ls
component-name.component.html           component-name.component.spec.ts
component-name.component.scss           component-name.component.ts
```
- component.htmlの中身
    - 初期状態で以下のように書かれている
```
<p>component-name works!</p>
```

- serveをしてもこのコンポーネントはまだ表示されない
- このコンポーネントをappコンポーネントの中に入れ子構造で配置することでAPの画面に現れる

- コンポーネントの挿入方法
    - 親となるコンポーネントのhtmlに以下のように要素（<>でhtmlに書くもの）を記述する
    ```
    <app-component-name></app-component-name>
    ```

- app.component.htmlを開き以下のように記述
```
<!--ここにcomponent-nameコンポーネントを挿入する---->
<app-component-name></app-component-name>

<div>初めてのAngular</div>
<br>
<button>スイッチ</button>
```

- serveで確認
```
ng serve --open
```

- 入れ子構造にしたcomponent-nameコンポーネントの中身が表示されていることがわかる

![Angular preview 2](https://user-images.githubusercontent.com/68212997/89634461-665f0a80-d8e0-11ea-90f3-3384ab6fba3e.png)


### Typescriptで機能を書いてみる

- app.component.tsを開く
    - 初期状態で以下のように書かれている
```
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
  title = 'ap-name';
}
```

- 以下のようにアラートを出す関数（機能）を書く
```
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
  title = 'ap-name';

  firstMethod(){
    alert("初めての関数");
  }
}
```

このままではこの関数は実行されない。トリガーを指定しなければならない

- htmlにclickイベントを指定
    - 以下のように記述
```
<button (click)="firstMethod()">スイッチ</button>
```
- 解説
    - (click)=""でクリックされた際に＝で指定した関数を実行すると定義できる

- serveして確認
```
ng serve --open
```

![firstMethod](https://user-images.githubusercontent.com/68212997/89635960-c8207400-d8e2-11ea-86ae-0f0753bfd165.png)

- 次にAngular Materialを使ってモダンなUIを開発しする手法と入力値の取得方法を学びましょう
    - [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)


本記事では極力丸めた表現をしているので、細かい用語などは[書籍](https://amzn.to/2XAs0Py)での学習がおすすめです。


## 参考記事
- [[Angular入門] 環境構築～PJの開始/基本コマンド](Angular入門-環境構築～PJの開始-基本コマンド/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
- [[Google cloud shell x Hexo] 環境構築&記事編集](https://j-xaas.github.io/Google-cloud-shell-x-Hexo-%E7%92%B0%E5%A2%83%E6%A7%8B%E7%AF%89-%E8%A8%98%E4%BA%8B%E7%B7%A8%E9%9B%86/)
- [[Angular入門] 環境構築～PJの開始/基本コマンド](Angular入門-環境構築～PJの開始-基本コマンド/)


## Angular関連のTips
- VSCode拡張機能 
    - [おすすめ](https://marketplace.visualstudio.com/items?itemName=johnpapa.angular-essentials)
        - その他にも沢山あるので随時導入してください
- [Angular公式のチュートリアル](https://angular.jp/start)
- [Angularよく使うコマンド](https://qiita.com/AsatoSa/items/ce7b416dc83522965d72)
