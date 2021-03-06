---
title: '[Angular入門] 環境構築～PJの開始/基本コマンド'
date: 2020-05-20 00:07:59
category:
- Serverless Application Dev
- SPA (Angular)
tags:
- Angular
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/83424662-abda0400-a467-11ea-831a-2b146d29a1c5.png
---
  
WEBアプリケーション開発に用いる[フレームワーク](https://sp.otsuka-shokai.co.jp/words/framework.html)である、Angularの環境構築～アプリの雛形の生成までを解説します。
Angularを使えば、[Single Page Application](https://digitalidentity.co.jp/blog/creative/about-single-page-application.html)と呼ばれるモダンなアプリを高速で開発することができます。例えば、GoogleやTwitter等のWEBアプリはこれで作られています。


![image](https://user-images.githubusercontent.com/41946222/82344822-7da00180-9a2f-11ea-868c-265520c3f38f.png)


<!-- toc -->

## 1. Angular動作環境の準備手順

### 1.1. Node.jsのインストール
- インストール済みであれば飛ばしてください
- Node.jsとは
    - JavaScriptが動くために必要な土台のようなものです
    - Angularで開発する際には、TypeScriptというJavaScriptの拡張版の言語でコードを書きます
        - TypeScriptは最終的にJavaScriptに変換されて動くので、Node.jsが必須となります

- [Node.js Download](https://nodejs.org/en/download/)
    - こちらから自身のOSに合ったものをダウンロードしてください

- ダウンロードしたnode-vX.XX.X-xXX.msiをダブルクリックして、インストールを開始します。
下記画面が表示されるので、表示される内容に従いインストールを進めてください。

![nodejs_install2](https://user-images.githubusercontent.com/41946222/68363881-a174b780-016f-11ea-9d15-039b92ae05fb.PNG)

### 1.2. Angular CLIのインストール
- インストール
```
npm i -g @angular/cli
```

- 正常にインストールされれば、ngコマンドを使用可能になります(ngはAngularの略称) 
    - ngコマンドを使うことで、一からコードを書かずにAPの一部を自動生成できます
    - このように開発工数を短縮する為に使う便利な道具がフレームワークだと思ってください

## 2. Angularプロジェクトの作成~実行手順
### 2.1. Angularプロジェクトの作成
- 以下のコマンドでアプリの雛形が生成されます
```
ng new <sample-ap-name>
cd <sample-ap-name>
```
- Angualr APのディレクトリ構成  
    - 以下のセットが自動生成されます
![Angular Project構成](https://user-images.githubusercontent.com/68212997/89703973-ee015380-d98a-11ea-8417-576765f6a06e.png)

### 2.2. ビルドと実行
- 次のserveコマンドで先の手順で生成したAP（の雛形）が起動します。
```
ng serve --open
```
- 上記コマンドの機能詳細
    - 開発用のWEBサーバが起動し、作成したAPを起動
    - TypeScriptコンパイラを、監視モードに
    - ソースコードが変更される度に、ブラウザを更新
    - --openはブラウザも同時に起動するオプション

- 以下のようにAngular PJの初期画面がブラウザに表示されれば成功です
    
![image](https://user-images.githubusercontent.com/41946222/82355091-b98d9380-9a3c-11ea-8f54-cd5358e6b441.png)

- ここまででWEB AP開発のスタートラインに立てました。続いて生成されたファイルを弄ってUI(画面のこと)や簡単な機能を開発してみましょう。
    - [[Angular入門] AngularCLI環境構築~PJの開始,コンポーネントの生成,初めての関数開発](/Angular入門-AngularCLI環境構築-PJの開始-コンポーネントの生成-初めての関数開発/)
    - [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)


## 3. Angular関連のTips
- VSCode拡張機能 
    - [おすすめ](https://marketplace.visualstudio.com/items?itemName=johnpapa.angular-essentials)
        - その他にも沢山あるので随時導入してください
- [Angular公式のチュートリアル](https://angular.jp/start)
- [Angularよく使うコマンド](https://qiita.com/AsatoSa/items/ce7b416dc83522965d72)


### 関連記事
- [[Angular入門] AngularCLI環境構築~PJの開始,コンポーネントの生成,初めての関数開発](/Angular入門-AngularCLI環境構築-PJの開始-コンポーネントの生成-初めての関数開発/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)