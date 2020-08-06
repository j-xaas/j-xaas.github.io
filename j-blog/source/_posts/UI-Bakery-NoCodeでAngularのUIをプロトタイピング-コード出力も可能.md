---
title: "[UI Bakery] NoCodeでAngularのUIをプロトタイピング(コード出力も可能なツール)"
date: 2020-08-06 23:01:55
categories:
- NoCode(LowCode)
- UI Bakery
tags:
- NoCode
- LowCode
- UI Bakery
- Angular
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/89525922-ed00e280-d821-11ea-9e23-375a7c31139b.png
---

モダンなWebアプリの画面をコードを書かずに開発して、ソースコードとして出力するツールについて解説します。
特徴はソース自体を出力できるため、通常の開発と遜色ない柔軟性を保ったまま時短を図れることです。
他のNoCodeツールと比べて、後から出力したソースに機能を追加していく前提なのでエンジニア向き。

<!--toc-->

## UIBakery概要
### UI Bakeryとは
- [UI Bakery](https://academy.uibakery.io/)
    - NoCode(Low-Code)プロトタイピングツール
        - WebアプリのUI(画面)をコードを書かずに開発することができるWebサービス
    - Angularベースでソースコードを出力可能
        - 最重要ポイント
        - 柔軟性に優れている
            - 他のNoCodeツールのデメリットは、”そのツールでカバーする範囲はできない”ことにある
                - 他のサービスとの連携や高度な機能を足そうとすると、結局作り直しになる
        - どのプラットフォームにも展開可能
            - 他のツールの場合、”ツール内の機能でしか”Webに公開できない。大抵そこで料金がかかる
                - アクセス数に応じて更に料金が加算される仕組みが多く、作れてもビジネスとして微妙
                - マネタイズの手法としては賢いと思うがあまり使いたくない
            - 例えば、Github PagesやAWSのS3、Firebase HostingなどのPFを要件に応じて選定できる
                - つまりインフラの知見を持つエンジニアであれば、費用を最適化してサービスを展開することができる
    - 基本無料
        - 大抵のNoCodeツールで目につく欠点がないように思える

- テンプレートが豊富で、使える物があればかなり楽に実装できます
![UI bakery Template](https://user-images.githubusercontent.com/68212997/89523216-782ba980-d81d-11ea-9ca9-cd0971aee361.png)


### ユースケース
要件定義や開発の初期段階で実際に操作可能な画面を作ることで、認識のギャップを低減することが出来ます。後の改修リスクを極力減らし、早めに機能面の実装に入ることができます。
こういった実働するモデル（プロトタイプ）を早期に製作する手法およびその過程をプロトタイピングと言います。

- UI Bakeryを利用したAP開発の流れ
    - UI BakeryでUIを一日でプロトタイピング
    - 顧客やプロジェクトオーナーに対してデモを実施
        - 指摘や要望があれば修正を加える
        - ここでニーズと実装のギャップを防ぐ
    - UI BakeryよりAngularのソースコードを出力
    - gitなどに加える
    - 出力したソースに機能を加えていく

## 使用方法
### アカウントを作成~ログイン
- [UI Bakery Login](https://app.uibakery.io/auth/login)
    - 今回はGoogleアカウントでログインしてみます
![UI_Bakery_login](https://user-images.githubusercontent.com/68212997/89510035-064a6480-d80b-11ea-9462-72a1a138bc5e.png")

- Confirm
![UI Bakery Registration](https://user-images.githubusercontent.com/68212997/89510449-883a8d80-d80b-11ea-9390-a934816429dc.png)

- 質問に回答してSubmit
![UI Bakery question](https://user-images.githubusercontent.com/68212997/89510776-fbdc9a80-d80b-11ea-9d2d-12532d519d0b.png)

- チュートリアル画面に飛びます
    - Stay Your Journeyから続けると基本的な操作方法が分かります

![UI Bakery Tutorial](https://user-images.githubusercontent.com/68212997/89510939-35ada100-d80c-11ea-804c-69a9ed515557.png)

### 使用方法/機能解説
基本的にWEBブラウザ上でログインして操作します

#### 1. Working Area
- 作成中のAPの画面を確認できるスペース

![UI Bakery Working Area](https://user-images.githubusercontent.com/68212997/89511491-f5025780-d80c-11ea-9556-6cfc90d49d9e.png)

#### 2. Component Picker
- +ボタンを押すと、コンポーネント（簡単に言うとAngularにおける画面の構成要素の単位）
- 使用方法
    - Working Area内の任意のスペースをクリック
    - 中に出てくるグレーの＋をクリック
    - 追加したいコンポーネントの種類を選択
        - Menuや認証画面、表、Chartなどのよく使う物が用意されています
        - めちゃくちゃ便利...
    - コンポーネントが追加される

![UI Bakery Components](https://user-images.githubusercontent.com/68212997/89512771-ab1a7100-d80e-11ea-8757-e97f5b22e490.png)

![UI Bakery Widgets](https://user-images.githubusercontent.com/68212997/89512806-b5d50600-d80e-11ea-87a7-d05b81a60972.png)

#### 3. Configuration Panel
- 右に表示されるパネルから、コンポーネントの設定を行うことが出来ます
![UI Bakery Configuration Panel](https://user-images.githubusercontent.com/68212997/89513120-22500500-d80f-11ea-90f4-7cc277028e8f.png)

#### 4. Toolbar
上のバーでプレビューやソースコードの出力が可能です

- 真ん中のアイコン
    - PCやモバイルなどでどのように表示されるかPreviewを確認できます
- Undo/Redo
    - 戻る/進む
- GET CODE
    - ここからAngularのソースコードを出力可能
    - UI Bakeryで素早く作ったUIを出力したコードに、自分で機能（TypeScript）を書いていく流れが良さそうです
- beta
    - まだベータ版だが、API連携もできるらしい
        - [UI Bakery/Data & API Connection](https://uibakery.io/data-connection)
    - 現時点ではUI開発のみの用途がメインだが、機能面も面倒なところを肩代わりしてくれるようになるかも
        - 細かいところは自分でいじれるままであれば嬉しい

![UI Bakery Toolbar](https://user-images.githubusercontent.com/68212997/89514224-8c1cde80-d810-11ea-9f4c-347c6a445a07.png)

#### 5. Page Management
- ページやレイアウトの設定を行うパネル
- サイドバーやナビゲーションバーもここで設定
- 使用方法
    - 右上のアイコンをクリック
    - ページの追加やレイアウト変更を実施

![UI Bakery Page Management](https://user-images.githubusercontent.com/68212997/89514642-149b7f00-d811-11ea-8a03-74ea87ee196c.png)

#### 6. Tool Sidebar
左のナビ。UI Bakeryのツールを実行できる

- Painter
    - 流行りのダークテーマなどにパパッと変更できます
- Video
    - アプリの紹介動画やデモに使えそう
- Documentation
    - UI Bakeryの公式ドキュメントをみれます

![UI Bakery Tool Sidebar](https://user-images.githubusercontent.com/68212997/89515707-75778700-d812-11ea-9d38-68693df4aea3.png)

#### 学習方法
会員登録をしたら、まずはチュートリアルをやると理解が深まると思います。
他のNoCodeと比較して圧倒的に分かり易く、特にハマる場面もありませんでした。
動画と説明文がセットになっている親切設計。

![UI Bakery Tutorial Sample](https://user-images.githubusercontent.com/68212997/89522042-8678c600-d81b-11ea-9b7c-8e63edca841b.png)

#### Projectの作成
Tutorialが終わったらCreate ProjectからUIを自由にプロトタイピングしてみましょう。
Angularではアプリの単位をProjectと呼びます。

- Create Projectを押下
- PJの名称を入力してCREATE

![UI Bakery Create Project](https://user-images.githubusercontent.com/68212997/89522963-0d7a6e00-d81d-11ea-88eb-3b316630654f.png)

#### テンプレートの選択
テンプレートを使用することでデザインの手間を大幅に短縮できます。よくあるパターンは用意されているので、テンプレートをもとにアレンジを加えるくらいですむことも多いと思います。
不慣れなメンバーが多いチームではこのレベルの画面を作るだけで、2,3週はかかりそう。

![UI bakery Template](https://user-images.githubusercontent.com/68212997/89523216-782ba980-d81d-11ea-9ca9-cd0971aee361.png)

![UI Bakery Template 2](https://user-images.githubusercontent.com/68212997/89523531-fbe59600-d81d-11ea-8e3d-6e619aa20013.png)

- Previewから実際の画面を操作することも可能です
![UI Bakery Template Preview](https://user-images.githubusercontent.com/68212997/89523828-7f06ec00-d81e-11ea-9ae5-b587bedee476.png)

- Templateを実際に利用するとこんな感じ
![UI Bakery Making](https://user-images.githubusercontent.com/68212997/89525922-ed00e280-d821-11ea-9e23-375a7c31139b.png)

操作が直感的なので、実際に試しながら理解を深めていける良いツールでした。
私も引き続き試して、ハマるポイントがあれば追加で記事書こうと思います。API連携の機能も試してみたい。

## 参考
- [UI Bakery Academy](https://academy.uibakery.io/)
- [Angularコードもダウンロードできるプロトタイプツール「UI Bakery」](https://itnews.org/news_contents/product-ui-bakery)
- [UI Bakery/Data & API Connection](https://uibakery.io/data-connection)