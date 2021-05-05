---
title: '[Ionic入門] Angular/React/VueベースでWeb/iOS/Androidを同時開発可能なクロスプラットフォームフレームワーク'
date: 2021-04-05 21:38:52
category:
- Serverless Application Dev
- Mobile (Ionic)
tags:
- Ionic
- Angular
- iOS
- Android
- クロスプラットフォーム
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/116813817-bda0bb80-ab90-11eb-8b5c-f1ca7fd6977e.png
---

- クロスプラットフォームで動作可能なアプリを開発できるIonic Frameworkの概要と初期開発手順についてのメモ
- モダンフロントエンドフレームワーク(ex. Angular,React,Vue)ベースで開発したAPをモバイル(iOS/Android)に変換することができる
    - Web系のフロントエンドエンジニアが覚えると、ほぼ学習コスト無しで全てに対応可能になる
- Flutterとどちらを採用すべきか？は開発チームのエンジニアによる
    - WEB技術(Typescript)が軸→Ionic、0からDartを学びたい→Flutter

<!--toc-->

## Ionic Framework概要
### クロスアプリケーションフレームワーク
![Ionic概要](https://user-images.githubusercontent.com/68212997/116816769-45d98d80-ab9e-11eb-80e3-8f4d592873e4.png)

- SPA(Webアプリ)ベースのアプリをiOS/Androidで動くネイティブアプリに変換できるフレームワーク
    - Web/iOS/Androidのアプリをハイブリット展開可能
        - １つのIonic Projectから Web/iOS/AndroidのAPをビルドするイメージ
    - 特に、既存のWEB APのモバイル化に迅速に対応できる
- コンポーネントやコード(HTML/Scss/Typescript)を使い回して開発可能
    - Typescriptで書いたロジックをそのまま使い回せる
    - Swift等を使うと、Webとモバイルで同じロジックを別言語で書き直す手間が発生する   

- `WEBフレームワークとはUI開発で一部差分`がある
    - 例えば、Angularで使われるWeb専用のUIコンポーネントは使用できない
       - 一般的にAngularでは[Angular Material](https://material.angular.io/)を活用して、htmlやcssをあまり書かずにUIを実装していく
        - ヘッダー、メニュー、テーブル等の部品がある
            ![Angular Material](https://user-images.githubusercontent.com/68212997/116815730-d2358180-ab99-11eb-9907-783a7e915133.PNG)

    - Ionicでは[モバイル向けのUIコンポーネント](https://ionicframework.com/jp/docs/components)を活用する
        ![Ionic UI Component](https://user-images.githubusercontent.com/68212997/116815750-f6915e00-ab99-11eb-805b-bcd32e3e8bcf.PNG)
        - Ionicの`UI ComponentはiOSとAndroidでそれぞれ異なる最適なマテリアルデザインが表示される`

- `ネイティブ機能のAPIとドキュメントが豊富に公開されている`
    - WEBベースで開発するが、ネイティブデバイスの機能もドキュメントを見てNative APIsを使えばハマらずに実装できる
    - [Ionic Framework - Native APIs](https://ionicframework.com/jp/docs/native)

### Ionicの強み
- Web系のフロントエンドエンジニアが学習コストなく活用可能
- WEB APのロジックをそのまま使いまわせる
    - Flutterを採用する場合はDartで書き直さなくてはならない
- 複数のWEBフロントフレームワークに対応
    - Angular, React, Vue, Nuxt.js
        - 日本ではなぜかReactが流行っているが、海外ではフルスタックなAngularやNuxt.jsが主流になりつつある認識
    - React NativeはReactにしか対応できない為

### Ionic vs Flutter
- 組織のエンジニアのスキルベースで判断
    - 軸がWEB(TS, JS) → Ionic
    - 軸がモバイル & 一からDartを学びたい → Flutter
- WEB/モバイル以外にも対応したい場合はFlutter
    - FlutterはWindowsやmacOS、Linux、車載、テレビ、スマートホーム関連のモジュールに組み込むこともできる
- すでにWEB APが存在していて、追加でモバイル対応したいケース
    - Ionicであれば、ロジックの書き直しが生じない
    - Flutterの場合はDartで書き直すことになる
- 結局は新たにDartの記法を学ぶ必要があるか？が判断ポイント
    - 個人的には、WEBエンジニアはIonicで困ることが特に無い為、Flutterを覚える旨味が特に無いと考える

---
### Ionic UI Component
![Ionic UI Component](https://user-images.githubusercontent.com/68212997/116815750-f6915e00-ab99-11eb-805b-bcd32e3e8bcf.PNG)

- IonicのUI Componentを活用すれば、`iOSとAndroidでそれぞれ異なる最適なマテリアルデザインを表示できる`
    - それぞれデザインする必要は無く、デザイナー無しでも高いUI品質を担保できる

---
### Ionic UI Template
![Ionic FirstApp Temaplate](https://user-images.githubusercontent.com/68212997/116815792-1cb6fe00-ab9a-11eb-8526-0076f79cafe1.png)

- Ionicではテンプレートを活用することで、↑のようなUIを自動生成することができる
    - タブメニューやカメラ機能程度なら自分で実装する必要が無く、手軽にモバイルAPらしさを出せるので、WEB開発になれたエンジニアでも機能開発に集中できる

---
### Ionicの学習方法
- WEBの公式ドキュメントと書籍で基本的に困ることはない

#### 書籍
以下がとても分かりやすかった
- [Ionicで作る モバイルアプリ制作入門[Angular版]<Web/iPhone/Android対応>](https://amzn.to/335cTj9)
- [Ionicで作る モバイルアプリ制作入門〈Web/iPhone/Android対応〉](https://amzn.to/3tbX5pA)

#### WEB
![Ionic Document](https://user-images.githubusercontent.com/68212997/116814948-35251980-ab96-11eb-9e21-59d08d7528bf.png)

- [Ionic Framework - 日本語ドキュメンテーション](https://ionicframework.com/jp/docs/)
    - 活用するWEBフレームワーク(ex. Angular, React...)をタブで選択して、それぞれの説明を確認できる親切なドキュメントになっている

- [Ionic Framework - UIコンポーネント](https://ionicframework.com/jp/docs/components)
    - UI開発は基本的にこちらのコンポーネントの中から使えそうなものを選定して実装していけば良い

- [Ionic Framework - Native APIs](https://ionicframework.com/jp/docs/native)
    - ネイティブデバイスの機能を利用する場合はこちらを参照

後はベースに採用するWEBフレームワークの学習をすればOK

---

## IonicによるAP開発手順
### 環境構築
- Ionicの開発に必要なものは以下
    - Node.js
    - Ionic CLI 

#### Node.js
- インストール方法は以下を参照
    - [Node.jsのインストール](/Angular入門-環境構築～PJの開始-基本コマンド/#11-nodejsのインストール)

- node -vでバージョンを確認
```
> node -v
v14.7.0
```

#### Ionic CLIのインストール
- Ionic CLI
    - これを入れるとIonicコマンドを実行可能になる
- Nodeのバージョンが古い場合は先にバージョンアップ

- Macの場合
```
sudo npm install ionic -g

...
+ ionic@5.4.16
added 225 packages from 149 contributors in 10.316s
```

- Windowsの場合
```
npm install ionic -g
```

- バージョンを確認
```
> ionic -v
5.4.16
```

### モバイルアプリ開発用ツールのインストール
- 実機での動作確認やリリース作業(App Store/Google Play)に以下が必要
    - [Xcode](https://apps.apple.com/jp/app/xcode/id497799835)
        - iOSアプリの実行/リリースに必須の統合開発環境
        - Windowsでは利用不可
    - Android Studio
        - Androidアプリの実行/リリースに必須の統合開発環境
        ![Android Studio](https://user-images.githubusercontent.com/68212997/116815628-5f2c0b00-ab99-11eb-84a3-855fb25c5fae.png)

#### Xcode
- XcodeはApp Storeから入手
    - インストールには20分以上かかった

![Xcode App Store](https://user-images.githubusercontent.com/68212997/116815666-871b6e80-ab99-11eb-9d16-e9192e670a4b.png)

- ランタイムのインストール
```
sudo gem install cocoapods # iOSのパッケージ管理システム
xcode-select --install # Xcodeのコマンドラインツール
pod repo update # CocoaPodsの依存関係を更新
```

#### Android Studioのインストール
- [Android Sudioサイト](https://developer.android.com/studio)からダウンロード
    - インストーラーを起動してインストール
    - 指示にしたがって初期設定を実施

![About Android Studio](https://user-images.githubusercontent.com/68212997/116816577-73720700-ab9d-11eb-8b95-9f840889f0e2.png)

#### Ionic向けのVSCode Pluginをインストール
- VSCodeにはIonic/Angular向けのPluginが豊富にある
    - 必要に応じて開発環境に取り込む

---
### Ionic projectの開始〜テンプレートからのUI自動生成

- Ionic Projectを開始
```
ionic start --type=angular 
```

- Project名を入力
```
Every great app needs a name! 😍

Please enter the full name of your app. You can change this at any time. To bypass this prompt next time, supply
name, the first argument to ionic start.

? Project name: sample-pj
```

- 次にテンプレートを選択する
    - モバイルアプリのUIの大枠は自動構築できる
    - tabs
        - モバイルアプリの定番のタブ型のアプリ
    - sidemenu
        - サイドメニューがハンバーガーで開くタイプ
    - blank
        - Home画面のみのシンプルな画面
    - camera with gallery
        - カメラ機能とデバイスの画像を操作する機能があるタイプ

```
Let's pick the perfect starter template! 💪

Starter templates are ready-to-go Ionic apps that come packed with everything you need to build your app. To
bypass this prompt next time, supply template, the second argument to ionic start.

? Starter template: (Use arrow keys)
❯ tabs         | A starting project with a simple tabbed interface 
  sidemenu     | A starting project with a side menu with navigation in the content area 
  blank        | A blank starter project 
  my-first-app | An example application that builds a camera with gallery 
  conference   | A kitchen-sink application that shows off all Ionic has to offer 
```

- 指定したProject名のディレクトリが生成される
```
> ls
sample-pj
```

- プレビューを起動
    - Angularのng serveと同様にlocalhostで動作確認可能
```
> cd sample-pj
ionic serve
```

- Sidemenuテンプレートを使った場合の初期画面
    - メールアプリ風になっている

![Ionic tabs template](https://user-images.githubusercontent.com/68212997/116816382-a5cf3480-ab9c-11eb-8f49-56fe084980a2.png)

- Tabsテンプレートを使った場合の初期画面
    ![Ionic Tabs Tenplate](https://user-images.githubusercontent.com/68212997/116816334-79b3b380-ab9c-11eb-8fdd-056623b3ffe7.png)


- --labオプション
    - Ionic ComponentはiOSとAndroidでそれぞれ異なるマテリアルデザインを持っている
    - labオプションで同時に確認可能
```
ionic serve --lab
```
![Ionic Serve Lab](https://user-images.githubusercontent.com/68212997/116816408-c1d2d600-ab9c-11eb-8e9c-a3f61be59975.png)


### tabsテンプレートのコンポーネント構成
- sample-pj/src/app
    - ディレクトリ構成は通常のAngular PJと同様
    - 以下のコンポーネントが自動生成されている
        - tabs, tab1, tab2, tab3, explore-container

```
app % ls
app-routing.module.ts   app.component.scss      app.component.ts        explore-container       tab2                    tabs
app.component.html      app.component.spec.ts   app.module.ts           tab1                    tab3
```

### UIの改修例
- 自動生成されたUIの改修手順について
    - タブのアイコンやタイトルの変更

#### tabs component
- デフォルト
    - tabs temlateで自動生成されるコード
```
<ion-tabs>

  <ion-tab-bar slot="bottom">
    <ion-tab-button tab="tab1">
      <ion-icon name="triangle"></ion-icon>
      <ion-label>Tab 1</ion-label>
    </ion-tab-button>

    <ion-tab-button tab="tab2">
      <ion-icon name="ellipse"></ion-icon>
      <ion-label>Tab 2</ion-label>
    </ion-tab-button>

    <ion-tab-button tab="tab3">
      <ion-icon name="square"></ion-icon>
      <ion-label>Tab 3</ion-label>
    </ion-tab-button>
  </ion-tab-bar>

</ion-tabs>
```

- 改修方法
    - アイコンを変更
        - 設計した各画面に適したion-iconを選定
    - タイトルを変更
        - 各画面のComponentのion-titleを変更

- [ion-icon](https://ionicframework.com/docs/v3/ionicons/)について
    - ion-iconのアイコンは以下のマテリアルデザインの名称をname=で指定することで活用できる
    - 各画面のアイコンとしてふさわしいものを[公式ドキュメント](https://ionicframework.com/docs/v3/ionicons/)から探せば良い
    ![ion-icons](./ion-icons.png)


- tabs.page.htmlを以下のように実装
    - タブの一つをホーム画面とする
        - アイコンを家マークに変更
        - アイコン下のタイトルをHomeに変更

```
<ion-tabs>

  <ion-tab-bar slot="bottom">
    <ion-tab-button tab="tab1">
      <ion-icon name="home"></ion-icon>
      <ion-label>Home</ion-label>
    </ion-tab-button>
```

- serveで変化を確認
    - tabsのボタンのアイコンが切り替わっていることを確認
    ```
    ionic serve --lab
    ```

- ion-toolbarについて
    - 以下の書式でツールバーとタイトルを定められる
```
<ion-header [translucent]="true">
  <ion-toolbar>
    <ion-title>
      <!--アプリのタイトルに変更-->
    </ion-title>
  </ion-toolbar>
</ion-header>
```

ここまでで、モバイルAPらしいタブ型の基本的なUIの骨子を開発することができる。この後、使えそうなUI Componentを公式ページから探して、各画面を作り上げていく。


## デバイスのネイティブ機能の実装: Ionic Native API
![Ionic - Native APIs](https://user-images.githubusercontent.com/68212997/117120831-ca125780-adce-11eb-84a0-0b50300ff157.png)

- [Ionic Native APIs](https://ionicframework.jp/docs/native/)
    - こちらを利用すればデバイス本体の機能を活用できる
        - ex. カメラ、センサー、写真フォルダ、3D Touch、クラウド連携...
    ```
    CapacitorやCordovaを使って、どんなIonicアプリにもネイティブデバイスの機能を簡単に追加できるようにするための、オープンソースのコレクションとプレミアムプラグインと統合します。このことによって、ネイティブパワーのアプリ体験を構築することができます。
    ```
    - ベースとするフレームワーク毎の説明がドキュメントとして公開されているので、そこで記法を確認して利用すれば良い
        - この豊富なAPI群により、WEB開発しか経験の無い方でも、大抵のモバイルAPを開発できる

---

その他の実装については、ベースとしたWEBフレームワークやTypeScriptの書式を調査しながら進めれば良い。

- Build方法については以下にまとめた
    - [[Ionic Framework] Web/iOS/AndroidアプリのBuild方法](/Ionic-Framework-Web-iOS-AndroidアプリのBuild方法/)


----
## 参考
### 関連記事
- [[Ionic Framework] Web/iOS/AndroidアプリのBuild方法](/Ionic-Framework-Web-iOS-AndroidアプリのBuild方法/)
- [[Angular入門] AngularCLI環境構築~PJの開始,コンポーネントの生成,初めての関数開発](/Angular入門-AngularCLI環境構築-PJの開始-コンポーネントの生成-初めての関数開発/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
- [Angular x AWS SDK for JavaScriptの始め方](/Angular-x-AWS-SDK-for-JavaScriptの始め方/)
- [[Angular/TypeScript(JavaScript)] 非同期処理/待ち合わせ処理のまとめ (Observable/subscribe/forkJoin/Promise/async/await/then)](/Angular-TypeScript-JavaScript-非同期処理-待ち合わせ処理のまとめ-Observable-Promise-async-await/)
- [[Angular JavaScript] JSONデータのファイル化と出力 取得したデータを任意の名称で保存するロジック (TypeScript)](/Angular-JSONデータのファイル化と出力-クラウドから取得したデータを任意の名称で保存する/)
- [[Angular] ng e2e Test 証明書エラーでハマった際の回避策(unable to get local issuer certificate))](/Angular-ng-e2e-Test-証明書エラーでハマった際の回避策/)
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular Service入門] ロジックを切り出し、複数Componentで再利用可能にする](/Angular-Service入門-ロジックを切り出し、複数Componentで再利用可能にする/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)
- [[Angular JavaScript] JSONデータのファイル化と出力 ~取得したデータを任意の名称で保存するロジック~ (TypeScript)](/Angular-JSONデータのファイル化と出力-クラウドから取得したデータを任意の名称で保存する/)
- [[Angular JavaScript] JSONファイル(複数)の読み込み](/Angular-JavaScript-JSONファイルの読み込み/)
- [Angular x AWS SDK for JavaScriptの始め方](/Angular-x-AWS-SDK-for-JavaScriptの始め方/)
- [静的ホスティングサービスまとめ(Netlify/S3/Firebase Hosting/Github Pages)](/静的ホスティングサービスまとめ-Netlify-S3-Firebase-Hosting-Github-Pages/)
- [[Angular ReactiveForm x mat-input] 一つの入力欄のエラーメッセージを出し分ける](/Angular-ReactiveForm-x-mat-input-一つの入力欄のエラーメッセージを出し分ける/)
- [[Angular mat-input] バリデーションまとめ](/Angular-mat-input-バリデーションまとめ/)
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)

### その他
- [Ionicで作る モバイルアプリ制作入門[Angular版]<Web/iPhone/Android対応>](https://amzn.to/335cTj9)
- [Ionicで作る モバイルアプリ制作入門〈Web/iPhone/Android対応〉](https://amzn.to/3tbX5pA)
- [Ionic Framework - 日本語ドキュメンテーション](https://ionicframework.com/jp/docs/)
- [Ionic Framework - UIコンポーネント](https://ionicframework.com/jp/docs/components)
- [Ionic Framework - Native APIs](https://ionicframework.com/jp/docs/native)