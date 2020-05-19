---
title: '[Angular入門] 環境構築～PJの開始/基本コマンド'
date: 2020-05-20 00:07:59
category:
- Serverless Application Dev
- SPA (Angular)
tags:
- Angular
toc: true
---
  
WEBアプリケーション開発に用いる[フレームワーク](https://sp.otsuka-shokai.co.jp/words/framework.html)である、Angularの環境構築～アプリの雛形の生成までを解説します。
Angularを使えば、[Single Page Application](https://digitalidentity.co.jp/blog/creative/about-single-page-application.html)と呼ばれるモダンなアプリを高速で開発することができます。例えば、GoogleやTwitter等のWEBアプリはこれで作られています。


![image](https://user-images.githubusercontent.com/41946222/82344822-7da00180-9a2f-11ea-868c-265520c3f38f.png)


<!-- toc -->

## 1. Angular動作環境準備手順

### 1.1. Node.jsのインストール
- インストール済みであれば飛ばしてください

- ダウンロードサイト(https://nodejs.org/download/release/v8.16.0/)から [node-v8.16.0-x64.msi] をクリックします。

![nodejs_install](https://user-images.githubusercontent.com/41946222/68363873-94f05f00-016f-11ea-99d9-f55f07c0cdbd.PNG)

- ダウンロードした [node-v8.16.0-x64.msi] をダブルクリックして、インストールを開始します。
下記画面が表示されるので、表示される内容に従いインストールを進めてください。
（インストール先などは、変更せずに行います。

![nodejs_install2](https://user-images.githubusercontent.com/41946222/68363881-a174b780-016f-11ea-9d15-039b92ae05fb.PNG)

### 1.2. Angular CLIのインストール
- インストール
```
npm i -g @angular/cli
```
- 正常にインストールされれば、ngコマンドを使用可能になります(ngはAngularの略称)

## 2. Angularプロジェクトの作成~実行手順
### 2.1. Angularプロジェクトの作成
- 以下のコマンドでアプリの雛形が生成されます
```
ng new <sample-ap-name>
cd <sample-ap-name>
```
- Angualr APのディレクトリ構成  
    - 以下のセットが自動生成されます
![angular](https://user-images.githubusercontent.com/41946222/68365531-38437300-0174-11ea-9bd1-68cb595c9a80.PNG)


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

## 3. Angular関連のTips
- VSCode拡張機能 
    - [おすすめ](https://marketplace.visualstudio.com/items?itemName=johnpapa.angular-essentials)
        - その他にも沢山あるので随時導入してください
- [Angular公式のチュートリアル](https://angular.jp/start)
- [Angularよく使うコマンド](https://qiita.com/AsatoSa/items/ce7b416dc83522965d72)
