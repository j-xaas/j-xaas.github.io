---
title: '[SYNCROOM] リモートセッションのやり方(YAMAHAの遠隔合奏アプリ)'
date: 2020-08-08 02:14:09
category: 
- SmartHome
- DTM
tags: 
- SYNCROOM
- NETDUETTO
- Steinberg UR44C
- DTM
- リモートセッション
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/89672648-f83a3800-d91f-11ea-801a-cef06dc605b2.png
---

7/21にYAMAHAの新しいリモートセッションサービスが公開されたので試してみました。

- 前バージョン(NETDUETTO)を試した際の記事
    - [[NETDUETTO] YAMAHAのリモートセッションツールを試してみた](/YAMAHAのリモートセッションツールSYNCROOMのベータ版：NETDUETを試してみた/)

<!--toc-->

## SYNCROOM概要
![SYNCROOM概要](https://user-images.githubusercontent.com/68212997/89672850-51a26700-d920-11ea-8043-ea6f4f6d3d12.png)

- [SYNCROOM](https://syncroom.yamaha.com/)
    - YAMAHAの遠隔合奏アプリ
    - 基本無料
    - 超低遅延
        - 0.02秒（20msec）程度らしい
            - 有線LANの場合。WiFiでもそこまで悪くはなかったです
        - ZOOM等のサービスでは0.5秒以上はズレが生じます
    - CD音質の通信
        - 非圧縮もしくはロスレスの16bit/48kHz
    - 動作環境（デスクトップ版）
        - Windows 10（64bit）（日本語OS版のみ)
        - macOS Mojave（10.14）
        - macOS Catalina（10.15） （言語設定が日本語のみ動作）
    - モバイル版
        - Android版の[SYNCROOM β](https://play.google.com/store/apps/details?id=jp.co.yamaha.ux.syncroom)が試験的に公開されています
        - まだ通信は不安定らしい
    - 参考
        - [コロナ禍で苦しむミュージシャンの救世主となるか？ヤマハがネット越しのセッションツール、SYNCROOMをリリースする背景](https://www.dtmstation.com/archives/29387.html)

## インストール
- 公式サイトの[ダウンロードページ](https://syncroom.yamaha.com/play/dl/)にアクセス
    - 自分の端末のOSを選択してダウンロード

![SYNCROOM ダウンロード](https://user-images.githubusercontent.com/68212997/89644523-0b82de80-d8f3-11ea-9641-641e1812f352.png)

- ダウンロード後にインストーラを開く
    - SYNCROOM-JP-mac-1.0.1.dmg
    - SYNCROOM

![SYNCROOMインストーラ](https://user-images.githubusercontent.com/68212997/89645256-5bae7080-d8f4-11ea-97bb-9cbbf7c175d2.png)

- 指示にしたがってインストールを実行

## 初期設定
- 音符マークのアイコンを開くと以下の初期画面が立ち上がります

![SYNCROOM初期画面](https://user-images.githubusercontent.com/68212997/89645470-d11a4100-d8f4-11ea-83ca-47e63760e854.png)

- YAMAHA Online Memberへの会員登録が必要
    - 仮登録後に届くメールのURLにアクセス

<img width="1094" alt="YAMAHA Online Member登録" src="https://user-images.githubusercontent.com/68212997/89645591-13438280-d8f5-11ea-8474-4565bced47dd.png">

- 以下が表示されれば完了
<img width="1097" alt="YAMA Online 登録完了" src="https://user-images.githubusercontent.com/68212997/89645796-8e0c9d80-d8f5-11ea-91f7-a30a91a8f3c6.png">

- 改めてアプリに戻り、ログインに進む
- 設定チュートリアルが開始します

![SYNCROOM チュートリアル](https://user-images.githubusercontent.com/68212997/89645980-ddeb6480-d8f5-11ea-8ebe-70b40636538a.png)

- この時点でマイクやインターフェイスは接続しておく必要があります
    - おすすめのインターフェイスと設定方法についてまとめた記事
        - [UR44C](https://amzn.to/33CmqQc), [UR22C](https://amzn.to/2XzxcCX)
        - [PCでDTM環境を構築 [Steinberg UR44C/Cubase ai]](/PCでDTM環境を構築-Steinberg-UR44C-Cubase-ai/)

- 出力先を選択
    - 鳴りをチェックできます

![SYNCROOM出力の設定](https://user-images.githubusercontent.com/68212997/89647203-23109600-d8f8-11ea-975b-bfc02e92236c.png)

- 入力設定
    - 複数のチャンネルを使うこともできるようです

![SYNCROOM 入力設定](https://user-images.githubusercontent.com/68212997/89647671-dda09880-d8f8-11ea-8ab1-a8c506d33791.png)

- ニックネームを設定
![SYNCROOM　ニックネームアイコンの設定](https://user-images.githubusercontent.com/68212997/89647817-29ebd880-d8f9-11ea-8672-6cf8a496d1f3.png)

以上で設定は完了

## 使用方法

1. 機材をセッティング
    - インターフェイスや楽器をPCに繋いでおきます
    - 適当に楽器を弾いて、”インプット”から出力を確認しましょう
        - ここに出てこない場合はインターフェイスのドライバーが入っていないかもしれません

![SYNCROOM インプットのチェック](https://user-images.githubusercontent.com/68212997/89648322-06755d80-d8fa-11ea-9660-98a69f11ed82.png)

2. ルームに集まる(↓のどちらか)
    - ルームを作る
    - 他人が作ったルームに入る
        - ルーム名で検索して入る方法と、ルーム一覧から選択する方法があります

<img width="1020" alt="SYNCROOM ルーム選択" src="https://user-images.githubusercontent.com/68212997/89671349-c7590380-d91d-11ea-945f-d1ebc859fad2.png">


- ルーム一覧は以下のようにブラウザに表示されます
    - フリーセッションの部屋もあれば、仲間内でパスワードを設定して集まってる部屋も
    - スタ練より手軽に集まれるので気が楽

![SYNCROOM ルーム一覧](https://user-images.githubusercontent.com/68212997/89652631-b77ef680-d900-11ea-99f2-5a0a114d1a35.png)


- セッション画面
    - 入室中のメンバーや通信状況を確認できます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/68212997/89652354-535c3280-d900-11ea-9b25-72bfa6de8ab8.png" height="350px" width="500px">
</div>

- ”チャット”から参加メンバーと連絡を取れます
    - 前回のNETDUETTOから追加された機能

    <img width="1014" alt="SYNCROOM チャット" src="https://user-images.githubusercontent.com/68212997/89657116-87872180-d907-11ea-80cd-0636653696fc.png">


- 詳細表示を押すと、各メンバの通信状況や遅延状況を確認できます


<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/68212997/89657401-f95f6b00-d907-11ea-9596-fb5010a4f982.png" height="350px" width="500px">
</div>

- 顔は見れないのでZoomと併用してセッションするといい感じになりました
    - iPadやMacBookでZoomに入り、[popInAladdin](https://amzn.to/2Py2LJ5)（プロジェクター）にミラーリングして写しています
    - 100インチぐらいなので、4人ならほぼ等身大でメンバーが映ります

![SYNCROOM_ZOOM](https://user-images.githubusercontent.com/68212997/89672192-43078000-d91f-11ea-9a23-b129cb89d812.jpg)


## DTM環境
以下のような環境を作っています

- DTM環境を作るところから始める方は↓の記事が参考になると思います
    - [PCでDTM環境を構築 [Steinberg UR44C/Cubase ai]](/PCでDTM環境を構築-Steinberg-UR44C-Cubase-ai/)

[DTM構成](https://user-images.githubusercontent.com/41946222/79885198-61f60b00-8431-11ea-97cc-6298ae1c92a3.png)

[DTM構成②](https://user-images.githubusercontent.com/41946222/77458772-10625c80-6e42-11ea-9998-40235fa4a44c.png)


## 関連記事
- [[SYNCROOM/NETDUETTO] YAMAHAのリモートセッションツールを試してみた](/YAMAHAのリモートセッションツールSYNCROOMのベータ版：NETDUETを試してみた/)
- [PCでDTM環境を構築 [Steinberg UR44C/Cubase ai]](/PCでDTM環境を構築-Steinberg-UR44C-Cubase-ai/)
- [iPad Pro (第4世代)でDTM環境を構築](/iPad-ProでDTM環境を構築/)