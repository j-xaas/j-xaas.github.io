---
title: Angularアプリの開発支援ツールAugury(Chrome拡張機能)
date: 2020-02-20 18:12:39
categories:
- Serverless Application Dev
- Single Page Application
tags:
- Angular
- Augury
toc: true
---

- 今回はAngularでWEBアプリを開発する際に簡単に利用できるツールを紹介します

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/74917091-5030c100-540a-11ea-9060-2d05fc6c6c23.png" height="100%" width="100%">
</div>

<!-- toc -->

## Auguryと は？
- Angularの開発支援ツール
- Google Chromeの拡張機能
- Chromeをメインブラウザとして利用しているデベロッパーには必須とも言えるツール

## できること
- 標準のデベロッパーツールに以下の機能を追加できます
  - Componentの階層構造やプロパティ値の図示
  - ルーティング情報のグラフ化
  - import済みモジュール構成のリスト表示

## 導入方法
- [Chrome Webstore](https://chrome.google.com/webstore/search/Augury)にアクセス
- "Chromeに追加"をClick
- ブラウザの右上にAuguryの丸いアイコンが追加されます

  <div style="text-align:center;">
  <img src="https://user-images.githubusercontent.com/41946222/74912170-74d46b00-5401-11ea-956d-93801c3cce13.png" height="100%" width="100%">
  </div>


## 利用方法
- Angular APの起動
  - まずは以下のコマンドでAPの画面をChromeに表示しましょう
```
ng serve --open
```
- F12キーで開発者ツールを起動
- ブラウザの右側に表示される開発者ツールに追加されているAuguryタブを選択

- "Component Tree"タブのInjection Graph
    - コンポーネントとサービスの依存関係をグラフ表示
        - 個人的に一番利用する機能です

  <div style="text-align:center;">
  <img src="https://user-images.githubusercontent.com/41946222/74917567-0b595a00-540b-11ea-8dc3-5a257af3bd45.png" height="100%" width="100%">
  </div>

- "Router Tree"タブ
    - ルート構造をツリー表示

  <div style="text-align:center;">
  <img src="https://user-images.githubusercontent.com/41946222/74918209-19f44100-540c-11ea-9953-c4734a0685ed.png" height="100%" width="100%">
  </div>


- "NgModule"タブ
    - import済みのモジュール構成を確認可能
  <div style="text-align:center;">
  <img src="https://user-images.githubusercontent.com/41946222/74917975-c2ee6c00-540b-11ea-89d0-e1fbd9b7195c.png" height="100%" width="100%">
  </div>


## 備考
- 以下も便利そうでした。APの構成管理やドキュメント作成の手間は極力自動化していきましょう。
    - [compodocでAngularプロジェクトのビジュアルなドキュメントを自動生成する](https://one-it-thing.com/3254/)