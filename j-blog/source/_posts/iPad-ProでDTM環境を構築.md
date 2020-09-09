---
title: iPad Pro (第4世代)でDTM環境を構築
date: 2020-03-25 00:04:42
category: 
- SmartHome
- DTM
tags: 
- iPad Pro
- DTM
- Cubasis LE
- Alexa
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/77458671-eb6de980-6e41-11ea-96b1-b60dd63aef66.png
alias: /2020/03/25/iPad-ProでDTM環境を構築/
---

こんにちは。先日、新しい[iPad 2020(第４世代)](https://amzn.to/2WMiw3M)を購入しました。発売当日に受け取り、即DTM環境を作ってみました（たぶん世界最速）Alexaにより、セッティングも自動化しました。シンプルな構成なので、入門者の方も良かったら参考にしてみてください。  
結論から言うと、PC無しでも問題無く宅録できました。

リモートセッションをやろうとするとPCが必要です。その辺りは以下を参考にどうぞ
- [[SYNCROOM] リモートセッションのやり方(YAMAHAの遠隔合奏アプリ)](/SYNCROOM-リモートセッションのやり方-YAMAHAの遠隔合奏アプリ/)
- [PCでDTM環境を構築 [Steinberg UR44C/Cubase ai]](/PCでDTM環境を構築-Steinberg-UR44C-Cubase-ai/)

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77458671-eb6de980-6e41-11ea-96b1-b60dd63aef66.png" height="370px" width="600px" alt="現在の構成">
</div>

<!-- toc -->

## 1. 機材解説


### Audio Interface
- [Steinberg USB3.0 UR44C](https://amzn.to/2Vcc8RX)を採用しました

    <a href="https://www.amazon.co.jp/%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%B3%E3%83%90%E3%83%BC%E3%82%B0-Steinberg-USB3-0-%E3%82%AA%E3%83%BC%E3%83%87%E3%82%A3%E3%82%AA%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%83%95%E3%82%A7%E3%82%A4%E3%82%B9-UR22C/dp/B07YLKRH58/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&crid=3R27S50YO8Z7B&dchild=1&keywords=steinberg+usb3.0+%E3%82%AA%E3%83%BC%E3%83%87%E3%82%A3%E3%82%AA%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%83%95%E3%82%A7%E3%82%A4%E3%82%B9+ur22c&qid=1592915526&sprefix=Steinberg+USB,aps,248&sr=8-1&th=1&linkCode=li2&tag=junjun1080c-22&linkId=53678cd0f1e21bd319eaea4635b6c3b1&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07YLKRH58&Format=_SL160_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun1080c-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun1080c-22&language=ja_JP&l=li2&o=9&a=B07YLKRH58" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

- 採用ポイント
    -  USB 3.0（USB Type-C）対応
        - タブレットで済ませるにはUSB Cが必要です
    - ジャック×4 ＆ マイクプリアンプx4
        - 複数人の楽器とマイクを繋いで、イヤホンジャック or Bluetoothで出力すれば、サイレントセッションもできます
    - 価格
        - 3万円ちょっとで手頃
            - 今回の条件に当てはまる中で最安でした
                - 他は5万以上
                - 更にジャックを増やしたければ、予算を上げるしかないと思います
            - ジャック×２で構わなければ以下がお勧めです
                - [スタインバーグ Steinberg USB3.0 オーディオインターフェイス UR22C](https://amzn.to/2YWvifI)
    - ios版のCubaseが付随
        - [Cubase](https://new.steinberg.net/ja/cubase/)
            - 初級者でも扱いやすい有名な音楽制作ソフトでプロも使っています
            - こういった機能が豊富なDAWソフトは宅録に必須です
        - 無料
            - 本来は購入する必要があります
        - セットアップが楽
            - Apple StoreでinstallしてUSB-Cケーブルで繋げるだけ
            - 実は今回のハードとCubaseは、どちらもSteinburg社が開発しているので相性が良いです
    - ループバック機能により、動画配信も可能

#### Cubaseのセットアップ手順
- [Cubasis LE2](https://apps.apple.com/jp/app/cubasis-le-2/id989112953)をApp Storeからinstall

- アプリを起動

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77536850-65ed4680-6ee0-11ea-8881-b3321e3be6f7.png" height="370px" width="500px" alt="Cubasis LE2">
</div>


- ”To unlock a Cubasis LE features...”と表示されます
    - 解除方法は以下の二つです
        - 料金を支払う
        - 対象製品と接続する
            - この対象製品が[Steinberg USB3.0 UR44C](https://amzn.to/2JdYsPS)です

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77538172-9fbf4c80-6ee2-11ea-9620-c80d0d4a2065.jpg" height="370px" width="500px" alt="Cubasis Locked">
</div>



- USB-Cケーブルで接続(iPadについていた純正品を使いました)

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77536906-80bfbb00-6ee0-11ea-9379-f4b4599b55ef.jpg" height="370px" width="500px" alt="Cubasis接続状態">
</div>

- unlock完了
    - 以下のように表示されれば連携完了です

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77536766-39392f00-6ee0-11ea-80ca-5b3d533fb640.png" height="370px" width="500px" alt="Cubase_unlocked">
</div>

- Cubaseを初めてすぐに適当にパッドを使って打ち込んでみました（意外とそれっぽくなって楽しい）

{% youtube 8_v6i2ZHF0A %}

### iPad Pro 2020（第4世代）

- [12.9インチモデル](https://amzn.to/3hPHB6l)を採用しました

<a href="https://www.amazon.co.jp/Latest-Apple-iPad-Pro-MXAT2J/dp/B0863KDNQJ/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&dchild=1&keywords=iPad+Pro+2020&qid=1592916056&sr=8-5&linkCode=li2&tag=junjun1080c-22&linkId=dd9afdec97894cc324efe75eaef06da5&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B0863KDNQJ&Format=_SL160_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun1080c-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun1080c-22&language=ja_JP&l=li2&o=9&a=B0863KDNQJ" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

- 採用ポイント
    - USB C対応
        - USB C対応のInterfaceに簡単に繋げます
    - メモリが6Gに増強
        - 音源編集にはある程度パワーが欲しい
    - トラックパッド・マウスに対応
        - Bluetoothマウス等でPCライクな作業が可能に
    - 出力方法にBluetoothを選択可能
    - 値下げ
        - iPad Pro第三世代登場時よりもだいぶ安いです

- コスパ重視の方は[11インチ](https://amzn.to/3183oQB)にすると少し安いです

### Hub
- [USB-C Hub](https://amzn.to/2Vbgitd)を付けました

<a href="https://www.amazon.co.jp/microSD-%E3%82%AB%E3%83%BC%E3%83%89%E3%83%AA%E3%83%BC%E3%83%80%E3%83%BC-%E3%83%98%E3%83%83%E3%83%89%E3%83%9B%E3%83%B3%E3%82%B8%E3%83%A3%E3%83%83%E3%82%AF-Macbook-SAMSUNG/dp/B087DZ9R55/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&dchild=1&keywords=USB+C+Hub&qid=1592916390&sr=8-35&linkCode=li2&tag=junjun1080c-22&linkId=b99ada8f6bdefc79f53c8d5caf97d5e6&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B087DZ9R55&Format=_SL160_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun1080c-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun1080c-22&language=ja_JP&l=li2&o=9&a=B087DZ9R55" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

- 以下のような入出力を実現するために採用してます
    - 入力
        - USB-C
    - 出力
        - HDMI
            - モニター
        - イヤホンジャック
            - 音声確認
        - Bluetooth
            - モニター、プロジェクター、スピーカーとの連携


- [QGeeM USB-C Hub](https://amzn.to/2RmIO9f)
    - より強めのHubが欲しければこちらもおすすめです(こちらはMacbook用に購入したのですが、iPadにも使えました)

<a href="https://www.amazon.co.jp/gp/product/B082WT3FL4/ref=as_li_ss_il?ie=UTF8&psc=1&linkCode=li2&tag=junjun1080c-22&linkId=9c77ad3aecfa8897c8c29c657143b27b&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B082WT3FL4&Format=_SL160_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun1080c-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun1080c-22&language=ja_JP&l=li2&o=9&a=B082WT3FL4" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

### Bluetoothスピーカー＆イヤホン＆カメラ
- [WF1000XM-3](https://amzn.to/2YpRKi6)
    - 立って弾く際にコードが引っかかるのが面倒で、結局生音でしかやらない節があったため採用
    - 専用アプリから細かくイコライザーを弄れて楽しい
    - Bluetoothのノイキャンイヤホンの中で最も音質が良い(2020/03 時点)
        - スケールアップ技術で、音質を勝手に上げてくれる

    <a href="https://www.amazon.co.jp/%E3%82%BD%E3%83%8B%E3%83%BC-SONY-%E3%83%AF%E3%82%A4%E3%83%A4%E3%83%AC%E3%82%B9%E3%83%8E%E3%82%A4%E3%82%BA%E3%82%AD%E3%83%A3%E3%83%B3%E3%82%BB%E3%83%AA%E3%83%B3%E3%82%B0%E3%82%A4%E3%83%A4%E3%83%9B%E3%83%B3-WF-1000XM3-Bluetooth/dp/B07TKGGZ47/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&crid=1GIIHXFFVCEI2&dchild=1&keywords=wf1000x&qid=1592916608&sprefix=WF1000,aps,259&sr=8-1&linkCode=li2&tag=junjun1080c-22&linkId=1772fb6ba494f3f7d3afb4495a946776&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07TKGGZ47&Format=_SL160_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun1080c-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun1080c-22&language=ja_JP&l=li2&o=9&a=B07TKGGZ47" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

- [SRS-XB32](https://amzn.to/2QJ25Sb)
    - 価格が手頃で低音の出が良い
        - これの一つ上のサイズは音量重視で音がこもり気味（Sonyの店員さん談）
    - 複数個を連携させて、別の音を振れる
        - 自分を囲むように配置して、音に包まれてみたかったので使ってます
    - ジャックもさせます
    - 防水なのでお風呂でも聞ける

    <a href="https://www.amazon.co.jp/%E3%82%BD%E3%83%8B%E3%83%BC-SONY-%E3%83%AF%E3%82%A4%E3%83%A4%E3%83%AC%E3%82%B9%E3%83%9D%E3%83%BC%E3%82%BF%E3%83%96%E3%83%AB%E3%82%B9%E3%83%94%E3%83%BC%E3%82%AB%E3%83%BC-SRS-XB32-%E3%83%A9%E3%82%A4%E3%83%86%E3%82%A3%E3%83%B3%E3%82%B0%E6%A9%9F%E8%83%BD%E6%90%AD%E8%BC%89/dp/B07QZ67FTT/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&dchild=1&keywords=SRSXB-32&qid=1592916727&sr=8-2&linkCode=li2&tag=junjun1080c-22&linkId=b20205fcc94e85c963402c151ddff8d9&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07QZ67FTT&Format=_SL160_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun1080c-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun1080c-22&language=ja_JP&l=li2&o=9&a=B07QZ67FTT" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

### WEBカメラ
- 録画・配信用にクリップ型のカメラを繋げています
    - オススメ
        - [Webカメラ HD1080P 30FPS広角](https://amzn.to/2Z5h6B0)

### アンプ
- Fender信者なので、[Fenderのベースアンプ](https://amzn.to/2B4sB3H)を使っています
    - でかくてかっこいいので40Wを購入したものの、近所迷惑になるので音量最低でもライン以外では音出しできないです

    <a href="https://www.amazon.co.jp/Fender-%E3%83%95%E3%82%A7%E3%83%B3%E3%83%80%E3%83%BC-%E3%83%99%E3%83%BC%E3%82%B9%E3%82%A2%E3%83%B3%E3%83%97-RUMBLE-100V/dp/B01D5R6OOK/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&dchild=1&keywords=Fender+%E3%83%99%E3%83%BC%E3%82%B9%E3%82%A2%E3%83%B3%E3%83%97&qid=1592916960&sr=8-1&th=1&linkCode=li2&tag=junjun1080c-22&linkId=415d1a6167de3f943d67dcbc11114016&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B01D5R6OOK&Format=_SL160_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun1080c-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun1080c-22&language=ja_JP&l=li2&o=9&a=B01D5R6OOK" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

### 全体像
- モニターに写すとこんな感じ(追記：今はもう少し整理しています)

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77660576-d8cbef80-6fbc-11ea-9c6e-1374d2e58d04.jpg" height="400px" width="270px" alt="Cubase_unlocked">
</div>

- プロジェクターにミラーリングするとこんな感じ
    - 120インチでかい

![Cubasis@popin aladdin](https://user-images.githubusercontent.com/41946222/85407181-d9652980-b59d-11ea-9bcb-de5c466e36e5.png)


## 2. AlexaでSettingを自動化
- せっかくDTM環境を作っても、セッティングに時間がかかるとやる気が失せくなりそうなので、自動化しました。電気代にも優しくなります。
    - ITの知識が無くても簡単にできるので。良かったら真似てみてください。
    - 機器が増えるほど効果を発揮してくると思います

### 定型アクション
Alexaに「DTM」と言うと、実行される”定型アクション”を設定します

- Trigger
    - 音声：「DTM」
- Action
    - スマート電源タップAをON/OFF
        - Bassアンプを接続
    - スマート電源タップBをON/OFF
        - エフェクターを接続
    - スマートリモコンを起動
        - Monitorのリモコンの電源ボタンを登録

### 解説

- スマートスピーカー
    - 画面付きの[Echo Show](https://amzn.to/3dsT9ZM)を採用

        <a href="https://www.amazon.co.jp/Echo-Show-8-%E3%82%A8%E3%82%B3%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%BC8-HD%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%BC%E3%83%B3%E4%BB%98%E3%81%8D%E3%82%B9%E3%83%9E%E3%83%BC%E3%83%88%E3%82%B9%E3%83%94%E3%83%BC%E3%82%AB%E3%83%BC-with-Alexa-%E3%83%81%E3%83%A3%E3%82%B3%E3%83%BC%E3%83%AB/dp/B07SHC76CH/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&crid=2UQ3Q4HI8MF09&dchild=1&keywords=echoshow5&qid=1592917791&sprefix=Echoshow,aps,244&sr=8-2&linkCode=li2&tag=junjun1080c-22&linkId=2bd14621d315c3b3f6d7e69f14614e31&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07SHC76CH&Format=_SL160_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun1080c-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun1080c-22&language=ja_JP&l=li2&o=9&a=B07SHC76CH" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

- [Smart電源タップ](https://amzn.to/33PCDQo)
    - 電源のON/OFFで操作する機器であれば基本これでOKです
    - 一通り試したことがありますが、一番コスパがいいのは各穴を制御できる[Meross スマート電源タップ](https://amzn.to/33PCDQo)です

    <a href="https://www.amazon.co.jp/Meross-%E3%82%B9%E3%83%9E%E3%83%BC%E3%83%88%E9%9B%BB%E6%BA%90%E3%82%BF%E3%83%83%E3%83%97-Google-%E5%80%8B%E5%88%A5%E3%82%B9%E3%82%A4%E3%83%83%E3%83%81-MSS425FJP-VC/dp/B07Q9S2629/ref=as_li_ss_il?_encoding=UTF8&pd_rd_i=B07Q9S2629&pd_rd_r=dbc7792e-ecc1-4ae6-b606-b8fd672521d3&pd_rd_w=Mx5zc&pd_rd_wg=DcYua&pf_rd_p=fa547a3a-46ca-44b8-a21f-e7113b805747&pf_rd_r=A086P3AQH7A1304FX26D&psc=1&refRID=A086P3AQH7A1304FX26D&linkCode=li3&tag=junjun10804-22&linkId=3cf4b512efc97da93eaabe853e48ffa7&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07Q9S2629&Format=_SL250_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun10804-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun10804-22&language=ja_JP&l=li3&o=9&a=B07Q9S2629" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />    

- Smartリモコン
    - 赤外線リモコンで動くものであれば、Smartリモコンに記憶させて、Alexaから実行できます
    - 大量のリモコンを纏められるのですっきりします
    - 私は[Nature Remo mini](https://amzn.to/3fKVxfK)を使っています
        - 人感センサーまでついているので、勝手にON/OFFさせることもできます
    
    <a href="https://www.amazon.co.jp/Nature-Remo-mini-%E5%AE%B6%E9%9B%BB%E3%82%B3%E3%83%B3%E3%83%88%E3%83%AD-%E3%83%A9-REMO2W1/dp/B07CWNLHJ8/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&dchild=1&keywords=nature+remo&qid=1592918120&sr=8-1-spons&psc=1&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUExNjRNOFVFWVBFM1lYJmVuY3J5cHRlZElkPUEwMjM0NjQ0NzFCSDVKNE1ZMkVUJmVuY3J5cHRlZEFkSWQ9QTNBSEtMOFlLQkNWREcmd2lkZ2V0TmFtZT1zcF9hdGYmYWN0aW9uPWNsaWNrUmVkaXJlY3QmZG9Ob3RMb2dDbGljaz10cnVl&linkCode=li2&tag=junjun1080c-22&linkId=499ec738e5808333a7e33412e5cb8046&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07CWNLHJ8&Format=_SL160_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun1080c-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun1080c-22&language=ja_JP&l=li2&o=9&a=B07CWNLHJ8" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

## 3. その他のもろもろ

### リモートセッション
- ネットを介してセッションできるサービスがあります

- 手段
    - [YAMAHA NETDUETTO](https://www.netduetto.net/)
        - ヤマハの遠隔合奏サービスです
            - 無料でmacOS, Windows10, Androidに対応しています（Androidに対応ということはそのうちiOSにも対応しそう...）
        - 以下の記事で解説してます
            - [[SYNCROOM] リモートセッションのやり方(YAMAHAの遠隔合奏アプリ)](/SYNCROOM-リモートセッションのやり方-YAMAHAの遠隔合奏アプリ/)
        - これを使うのであれば、PCで環境を作らないとダメでした
            - [PCでDTM環境を構築 Steinberg UR44C/Cubase ai](/PCでDTM環境を構築-Steinberg-UR44C-Cubase-ai/)

    - [JamBlaster](https://www.jamkazam.com/products/jamblaster)
        - 米国のスタートアップが最近開発したハードです
            - スマホベースでネットと繋ぐらしい。詳しくは調査中
            - こちらであれば、iPadでのそのうち使えるようになりそう


### モニター部分をプロジェクターに変更
- iPadはBluetoothによって画面をミラーリングすることもできます。プロジェクターには[Popin Aladdin](https://amzn.to/37QVnB0)を使っています

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77458772-10625c80-6e42-11ea-9998-40235fa4a44c.png" height="370px" width="600px" alt="プロジェクターを入れた構成">
</div>

- ミラーリングするとこんな感じで、壁一面にCubasisが映ります
    - これだけでかいと立って引きながらでも見えて楽しいです

![Cubasis@popin aladdin](https://user-images.githubusercontent.com/41946222/85407181-d9652980-b59d-11ea-9bcb-de5c466e36e5.png)


- 採用理由
    - 120インチを表示できるので大きいモニターよりコスパが良い
        - 壁一面に映してリモートセッションをやってみたい...
    - 照明と一体型のプロジェクター
        - 暗くする手間を省ける
        - 場所を取らない
    - 赤外線リモコンが付随しているので、スマートリモコンでAlexaから動かせる
    - 詳しくは以下の記事にまとめています
        - [popIn Aladdin2 レビュー 照明型プロジェクター　使用感/専用アプリ/投影サイズ/Alexa連携/設置方法](/popInAladdin-review/)

    <a href="https://www.amazon.co.jp/popIn-Aladdin-%E3%83%9D%E3%83%83%E3%83%97%E3%82%A4%E3%83%B3%E3%82%A2%E3%83%A9%E3%82%B8%E3%83%B3-%E3%83%97%E3%83%AD%E3%82%B8%E3%82%A7%E3%82%AF%E3%82%BF%E3%83%BC%E4%BB%98%E3%81%8D%E3%82%B7%E3%83%BC%E3%83%AA%E3%83%B3%E3%82%B0%E3%83%A9%E3%82%A4%E3%83%88-%E9%AB%98%E9%9F%B3%E8%B3%AA%E3%82%B9%E3%83%94%E3%83%BC%E3%82%AB%E3%83%BC/dp/B07J5B3VD2/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&crid=1TQT5K8IKNSQN&dchild=1&keywords=popin+aladdin+2&qid=1592918188&sprefix=popin+a,aps,254&sr=8-1&linkCode=li2&tag=junjun1080c-22&linkId=fd93e790d23a399bde4357044d7a1a0e&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07J5B3VD2&Format=_SL160_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun1080c-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun1080c-22&language=ja_JP&l=li2&o=9&a=B07J5B3VD2" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

### AWS Componserの導入
仕事でAWS(クラウドサービス)をよく使っているのですが、去年機械学習用のキーボードが発表され、ずっと気になっています。

- [AWS Composer](https://aws.amazon.com/jp/deepcomposer/)とは
    - 弾いたメロディに対して、AIがアレンジを加えて作曲
    - 学習済みのモデルが複数用意されている
    - そのままSoundCloudにアップ可能
    - 学習モデルはオリジナルでも作成可能

学習モデルは与える音源によって変化していくらしいです。AIに音楽のルーツを持たせられるのが人間らしくて興味深いです。

エンジニア寄りの話になりますが、AWSでの開発もcloud9等のブラウザ型IDEを用いれば、iPadからやれてしまいます。趣味と仕事を結びつけたら楽しそうなので、時間を見つけて挑戦しようと思います。

## まとめ
今回の記事は以上です。タブレットでも音源編集が可能になり、だいぶDTMの敷居が下がったように思います。私も何も知らないまま一から調べて今回の構成を作ったので、ここまでは誰でも真似できると思います。是非実践してみてください。質問やアドバイスなどあればどしどしください。

## 関連記事
- [[SYNCROOM] リモートセッションのやり方(YAMAHAの遠隔合奏アプリ)](/SYNCROOM-リモートセッションのやり方-YAMAHAの遠隔合奏アプリ/)
- [PCでDTM環境を構築 [Steinberg UR44C/Cubase ai]](/PCでDTM環境を構築-Steinberg-UR44C-Cubase-ai/)
- [popIn Aladdin2 レビュー 照明型プロジェクター　使用感/専用アプリ/投影サイズ/Alexa連携/設置方法](/popInAladdin-review/)
- [[SYNCROOM/NETDUETTO] YAMAHAのリモートセッションツールを試してみた](/YAMAHAのリモートセッションツールSYNCROOMのベータ版：NETDUETを試してみた/)