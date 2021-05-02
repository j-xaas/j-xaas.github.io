---
title: Popin Aladdin 2にゲーム(Switch/プレステ)画面を映す方法
date: 2020-12-27 16:27:31
category:
- SmartHome
- Gadget
tags:
- popIn Aladdin2
- Elgato HD 60S
toc: true
draft: true
thumbnail: https://user-images.githubusercontent.com/68212997/103165932-018d5800-4861-11eb-83a0-b7d96077fd74.jpg
---

[Popin Aladdin(プロジェクター)](/popInAladdin-review/)にゲームを映してプレイするまでの備忘録

- 投影するとこんな感じ

{% youtube 8jdAx8B6KyU %}


## やりたいこと・課題
- やりたいこと
    - ゲーム画面(Switch)を[Popin Aladdin 2(プロジェクター)](/popInAladdin-review/)に写して、120インチの大画面でプレイしたい
- 課題：[popin Aladdin](/popInAladdin-review/)にはHDMI端子が無い
    - 他の機器との連携は無線(WiFi/Bluetooth)のみ可能
    - HDMI接続のゲーム全般ができないと公式サイトにも書かれている

## 実現方法
1. Switchの映像をHD 60 S経由でMacBookに映す
2. MacBookの画面をAirPlayでpopin Aladdinに映す

![Switch → Popin Aladdin2](https://user-images.githubusercontent.com/68212997/103164858-d7816900-4853-11eb-9e4a-6d71a892e336.jpg)

- キャプチャーボードという機器を利用することで、PCにSwitchの画面を映せます
- 音声も一緒にPopin Aladdinまで送れました

## 使用機器
- Switch ＆ HDMIケーブル 
- [MacBook Pro](https://amzn.to/3puQsgL)
    - AirでもAirplayの機能を使えればOK
- キャプチャーボード: [Elgato HD 60S](https://amzn.to/2KTmrbk)
    - 元々プロのゲーム配信者などが使っている製品。遅延が非常に小さく専用アプリを入れるだけで利用できました
    - 4Kで出力したい場合は[Elgato HD60 Pro](https://amzn.to/3hkN3xU)でもOK

<a href="https://www.amazon.co.jp/Elgato-Game-Capture-HD60-1GC109901004/dp/B01DRWCOGA/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&crid=257MWUN9KZS35&dchild=1&keywords=elgato%2Bgame%2Bcapture%2Bhd60%2Bs&qid=1609052283&sprefix=Elgato%2B%2Caps%2C268&sr=8-5&th=1&linkCode=li2&tag=junjun1080c-22&linkId=a63c0f685cbc9b18e3a9796483484da1&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B01DRWCOGA&Format=_SL160_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun1080c-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun1080c-22&language=ja_JP&l=li2&o=9&a=B01DRWCOGA" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

- [USB C Hub](https://amzn.to/3prsdQi)
    - MacBookとHD60Sの接続用

- [Popin Aladdin 2](https://amzn.to/3nWIJri)
    - 1や[SE(廉価版)](https://amzn.to/3nSwrjI)でも恐らくOK

## 準備 キャプチャーボード用ソフトのインストール
- Elgato HD60S(キャプチャーボード)を準備
    - [Elgato HD 60S](https://amzn.to/2KTmrbk)

-  Game Capture HDを公式サイトからダウンロード ＆ インストールする
    - https://www.elgato.com/en/gaming/downloads

<img width="1763" alt="Elgato downloadサイト" src="https://user-images.githubusercontent.com/68212997/103148333-ea3b6580-47a1-11eb-8284-8ba603dfa1ea.png">

- 製品(HD 60 S)/OSを選択後にGame Captureをダウンロード
<img width="1688" alt="Elgato Game Capture" src="https://user-images.githubusercontent.com/68212997/103148379-65048080-47a2-11eb-8409-0165a7b6eaf2.png">

- Game Capture HD
    - ダウンロードされたGame Capture HDを以下のように開ればOK
<img width="1222" alt="Game Capture HD画面" src="https://user-images.githubusercontent.com/68212997/103148487-77cb8500-47a3-11eb-9dab-ded5295b2774.png">


 
## SitchからPopin Aladdinに表示するまで

### 1. SwitchからHDMIでHD60 Sに接続
- SwitchのHDMI Outから"IN"と書かれているHDMIに接続

### 2. Game Capture HDを起動
- MacBookでソフトを起動しておく
- 後でHD60を接続すると、デバイスのプルダウンの選択欄でHD60を選択可能になる

### 3. HD60 SとMacBookを接続
![PCに表示したスマブラ](https://user-images.githubusercontent.com/68212997/103153726-f4775700-47d5-11eb-8b34-99cf7babdcc7.png)

- HD60 Sの USBC端子からMacBookの端子に映像を出力
    - OutのHDMI端子では無いので注意
- これでMacBookにSwitchの映像を表示できる
- SwitchとMacBook間のラグはほぼ完全に無し

### 4. AirPlayでpopin aladdin 2に表示

- MacBookとPopin Aladdinは同じWiFiに接続していれば、AirPlayで接続することができる
    - ここのラグはWiFiの強さや接続数で変動する
    - 基本的に全く気にならないレベル
    - スマブラなどのラグにシビアなゲームをする場合は接続数を強い回線が良い

## 使用感
- 普通のゲームは全く気にならずにプレイできるレベル
- WiFiの状況によってはラグが発生する場合もある
    - うちは大量の機器を繋いでいますが、スマブラレベルのシビアな戦いになると少しラグが気になる程度
        - 他の接続を減らすか、それだけMacやモニターを見てプレイすると良いかもしれない
    - 強い回線を使っていれば概ね問題なし

- ちなみにTVやDVDは無線で接続可能な専用のチューナーや機器を購入すれば簡単に映せました
    - [Xit Air BOX](https://amzn.to/2JpDZeQ)

- PopIn Aladdin自体のレビューは以下
    - [PopIn Aladdin2 レビュー 照明型プロジェクター　使用感/専用アプリ/投影サイズ/Alexa連携/設置方法](/popInAladdin-review/)


## 関連記事
- [Popin Aladdin2 レビュー 照明型プロジェクター　使用感/専用アプリ/投影サイズ/Alexa連携/設置方法](/popInAladdin-review/)
- [youtube popin Aladdin 2にSwitchを映してみた](https://youtu.be/8jdAx8B6KyU)
- [【HD60S】macでSwitchのゲーム実況する方法【Catalina】](https://kyoheyblog.com/game-commentary-mac/)
- [popin aladdin 2でSwitchをやってみた（HDMI出力）](https://note.com/my_tips/n/nfe4d5ab15137)
