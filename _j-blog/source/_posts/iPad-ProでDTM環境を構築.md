---
title: iPad Pro (第4世代)でDTM環境を構築
date: 2020-03-25 00:04:42
category: 
- SmartHome
- DTM
tags: 
- iPad Pro
- DTM
- Alexa
toc: true
---

こんにちは。先日、新しい[iPad 2020(第４世代)](https://amzn.to/2WMiw3M)を購入しました。発売当日に受け取り、即DTM環境を作ってみました（たぶん世界最速）。Alexaにより、セッティングも自動化しました。シンプルな構成なので、入門者の方も良かったら参考にしてみてください。  
結論から言うと、DTMをやるだけならMacBook無しでも大丈夫だと思います。

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77458671-eb6de980-6e41-11ea-96b1-b60dd63aef66.png" height="370px" width="600px" alt="現在の構成">
</div>


<!-- toc -->

## 1. 機材解説

### 1.1. Audio Interface
- [Steinberg USB3.0 UR44C](https://amzn.to/2JdYsPS)を採用しました

    <a href="https://www.amazon.co.jp/%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%B3%E3%83%90%E3%83%BC%E3%82%B0-Steinberg-USB3-0-%E3%82%AA%E3%83%BC%E3%83%87%E3%82%A3%E3%82%AA%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%83%95%E3%82%A7%E3%82%A4%E3%82%B9-UR44C/dp/B07YLKRH58/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&keywords=Steinberg+ur44C&qid=1585065911&sr=8-1&swrs=3928F2E7A513E4BED97A4C3E596F00B9&linkCode=li3&tag=junjun10804-22&linkId=3bf102532686f968cd4c92202a2f4172&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07YLKRH58&Format=_SL250_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun10804-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun10804-22&language=ja_JP&l=li3&o=9&a=B07YLKRH58" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

- 採用ポイント
    - ジャック×4 ＆ マイクプリアンプx4
        - 複数人の楽器とマイクを繋いで、イヤホンジャック or Bluetoothで出力すれば、サイレントセッションが可能
            - これがやりたくて買った節があります
    - 価格
        - 3万円ちょっとで手頃
            - 今回の条件に当てはまる中で最安でした
                - 他は5万以上
            - 更にジャックを増やしたければ、予算を上げるしかないと思います
                - まだ初級者なので、必要になったらミキサーでも買おうかなぁというスタンスです
    -  USB 3.0（USB Type-C）対応
        - PCを毎回立ち上げるのは面倒なため、タブレットで済ませたい
    - ios版のCubaseがついてくる
        - [Cubase](https://new.steinberg.net/ja/cubase/)
            - 最も有名な音楽制作ソフトでプロもよく使っています
            - 機能が豊富で、宅録には必須
        - 無料
            - 本来は購入する必要があります
        - セットアップが楽
            - Apple StoreでinstallしてUSB-Cケーブルで繋げるだけ
            - 実は今回のハードもCubaseもSteinburg社が開発しているので相性が良い
    - ループバック機能により、動画配信も可能

### 1.2. iPad Pro 2020（第4世代）

- [12.9インチモデル](https://amzn.to/2wCNvF1)を採用しました

<a href="https://www.amazon.co.jp/Apple-iPad-12-9%E3%82%A4%E3%83%B3%E3%83%81-Wi-Fi-128GB/dp/B0863RN4V2/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&keywords=ipad+pro&qid=1585062568&sr=8-5&linkCode=li3&tag=junjun10804-22&linkId=82ff7b87c17b0b401f6bd8e169725ce2&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B0863RN4V2&Format=_SL250_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun10804-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun10804-22&language=ja_JP&l=li3&o=9&a=B0863RN4V2" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

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

- メモリの増強以外のポイントはiPad Pro（第三世代）でも満たしています
    - コスパ重視の方は[11インチ](https://amzn.to/2QJ0lbB)なら8万程度です

### 1.3. Hub
- [USB-C Hub](https://amzn.to/2vQU5ra)を付けました

<a href="https://www.amazon.co.jp/gp/product/B084WPZ5NR/ref=as_li_ss_il?ie=UTF8&psc=1&linkCode=li3&tag=junjun10804-22&linkId=60b4dc5c8f9005bcc043c7540b3225f4&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B084WPZ5NR&Format=_SL250_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun10804-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun10804-22&language=ja_JP&l=li3&o=9&a=B084WPZ5NR" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

- 以下のような入出力を実現するために採用してます
    - 入力
        - USB-C
    - 出力
        - HDMI
        - イヤホンジャック
        - Bluetooth

### 1.4. Bluetoothスピーカー＆イヤホン
- [WF1000XM-3](https://amzn.to/2UzfvRN)
    - 立って弾く際にコードが引っかかるのが面倒で、結局生音でしかやらない節があったため採用
    - 専用アプリから細かくイコライザーを弄れて楽しい
    - Bluetoothのノイキャンイヤホンの中では一番音質が良い(2020/03 時点)
        - スケールアップ技術で、音質を勝手に上げてくれる

    <a href="https://www.amazon.co.jp/%E3%82%BD%E3%83%8B%E3%83%BC-SONY-%E3%83%AF%E3%82%A4%E3%83%A4%E3%83%AC%E3%82%B9%E3%83%8E%E3%82%A4%E3%82%BA%E3%82%AD%E3%83%A3%E3%83%B3%E3%82%BB%E3%83%AA%E3%83%B3%E3%82%B0%E3%82%A4%E3%83%A4%E3%83%9B%E3%83%B3-WF-1000XM3-Bluetooth/dp/B07TMLFBVQ/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&crid=2K768A5ITIOLH&keywords=wx1000xm3&qid=1585068229&s=diy&sprefix=WX1000,diy,284&sr=8-1&swrs=686EF029BC69DF7325EF480AC3A8F0A1&th=1&linkCode=li2&tag=junjun10804-22&linkId=b60880eac6dedd3eb1c10cdd98c31177&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07TMLFBVQ&Format=_SL160_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun10804-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun10804-22&language=ja_JP&l=li2&o=9&a=B07TMLFBVQ" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

- [SRS-XB32](https://amzn.to/2QJ25Sb)
    - 価格が手頃で低音の出が良い
        - これの一つ上のサイズは音量重視で音がこもり気味（Sonyの店員さん談）
    - 複数個を連携させて、別の音を振れる
        - 自分を囲むように配置して、音に包まれてみたかったので使ってます
    - ジャックもさせます
    - 防水なのでお風呂でも聞ける

    <a href="https://www.amazon.co.jp/dp/B07QZ67FTT/ref=as_li_ss_il?psc=1&pd_rd_i=B07QZ67FTT&pd_rd_w=euzvC&pf_rd_p=6413bd85-d494-49e7-9f81-0e63e79171a9&pd_rd_wg=rkZfv&pf_rd_r=2PVHVEWK3H7VKTF7S6G0&pd_rd_r=3c551492-f885-4d3f-8cde-d860532bf71f&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUEyRTFXRE8yR0VOODEmZW5jcnlwdGVkSWQ9QTA2NTAyMzYxSURGRTY1MUo3U0NZJmVuY3J5cHRlZEFkSWQ9QTE0NlVVNVZVTDE0TUcmd2lkZ2V0TmFtZT1zcF9kZXRhaWwmYWN0aW9uPWNsaWNrUmVkaXJlY3QmZG9Ob3RMb2dDbGljaz10cnVl&linkCode=li3&tag=junjun10804-22&linkId=4855dc59c68bf65179fac7573adb0be9&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07QZ67FTT&Format=_SL250_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun10804-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun10804-22&language=ja_JP&l=li3&o=9&a=B07QZ67FTT" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

### 1.5. アンプ
- Fender信者なので、[Fenderのベースアンプ](https://amzn.to/3dt8j2o)を使っています
    - ”でかくてかっこいい”という理由で40Wを購入したものの、近所迷惑になるので音量最低でもそのまま音を出せない..

    <a href="https://www.amazon.co.jp/Fender-%E3%83%95%E3%82%A7%E3%83%B3%E3%83%80%E3%83%BC-%E3%83%99%E3%83%BC%E3%82%B9%E3%82%A2%E3%83%B3%E3%83%97-RUMBLE-100V/dp/B01D5R6O7M/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&keywords=Fender+%E3%83%99%E3%83%BC%E3%82%B9+%E3%82%A2%E3%83%B3%E3%83%97&qid=1585068032&s=diy&sr=8-1&linkCode=li3&tag=junjun10804-22&linkId=f2bb86e5a18a1cbed8ad9c7ae19161ba&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B01D5R6O7M&Format=_SL250_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun10804-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun10804-22&language=ja_JP&l=li3&o=9&a=B01D5R6O7M" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />


## 2. AlexaでAuto Setting
- せっかくDTM環境を作っても、セッティングに時間がかかるとやる気が失せてしまうので、自動化しました。電気代にも優しくなります。
    - ITの知識が無くても説明に従えば簡単にできるので。良かったら真似てみてください。
    - 機器が増えるほど効果を発揮してくると思います

### 定型アクション
- Alexaに「DTM」と言うと、実行される”定型アクション”を設定しました

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
    - 画面付きの[Echo Show 5](https://amzn.to/3dr24fs)を採用

        <a href="https://www.amazon.co.jp/Echo-Show-5-%E3%82%A8%E3%82%B3%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%BC5-%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%BC%E3%83%B3%E4%BB%98%E3%81%8D%E3%82%B9%E3%83%9E%E3%83%BC%E3%83%88%E3%82%B9%E3%83%94%E3%83%BC%E3%82%AB%E3%83%BC-with-Alexa-%E3%83%81%E3%83%A3%E3%82%B3%E3%83%BC%E3%83%AB/dp/B07KD87NCM/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&keywords=Alexa+Echo&qid=1585070659&s=musical-instruments&sr=1-1-catcorr&linkCode=li2&tag=junjun10804-22&linkId=83bd9d0bfdbe970070a18861c20f4a65&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07KD87NCM&Format=_SL160_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun10804-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun10804-22&language=ja_JP&l=li2&o=9&a=B07KD87NCM" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

- [Smart電源タップ](https://amzn.to/33PCDQo)
    - 電源のON/OFFで操作する機器であれば基本これでOKです
    - 一通り試したことがありますが、一番コスパがいいのは各穴を制御できる[Meross スマート電源タップ](https://amzn.to/33PCDQo)です

    <a href="https://www.amazon.co.jp/Meross-%E3%82%B9%E3%83%9E%E3%83%BC%E3%83%88%E9%9B%BB%E6%BA%90%E3%82%BF%E3%83%83%E3%83%97-Google-%E5%80%8B%E5%88%A5%E3%82%B9%E3%82%A4%E3%83%83%E3%83%81-MSS425FJP-VC/dp/B07Q9S2629/ref=as_li_ss_il?_encoding=UTF8&pd_rd_i=B07Q9S2629&pd_rd_r=dbc7792e-ecc1-4ae6-b606-b8fd672521d3&pd_rd_w=Mx5zc&pd_rd_wg=DcYua&pf_rd_p=fa547a3a-46ca-44b8-a21f-e7113b805747&pf_rd_r=A086P3AQH7A1304FX26D&psc=1&refRID=A086P3AQH7A1304FX26D&linkCode=li3&tag=junjun10804-22&linkId=3cf4b512efc97da93eaabe853e48ffa7&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07Q9S2629&Format=_SL250_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun10804-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun10804-22&language=ja_JP&l=li3&o=9&a=B07Q9S2629" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />    

- Smartリモコン
    - 赤外線リモコンで動くものであれば、Smartリモコンに記憶させて、Alexaから実行できます
    - 大量のリモコンを纏められるのですっきりします
    - 私は[Nature Remo mini](https://amzn.to/2y3uifX)を使っています
        - 人感センサーまでついているので、勝手にON/OFFさせることもできます

    <a href="https://www.amazon.co.jp/Nature-Remo-mini-%E5%AE%B6%E9%9B%BB%E3%82%B3%E3%83%B3%E3%83%88%E3%83%AD-%E3%83%A9-REMO2W1/dp/B07CWNLHJ8/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&crid=37HI5Y32WTKQA&keywords=nature+remo&qid=1585065062&sprefix=Nature%E3%80%80,aps,259&sr=8-4&linkCode=li3&tag=junjun10804-22&linkId=1dcf8efb29ee246be4faa38c3f6f5256&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07CWNLHJ8&Format=_SL250_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun10804-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun10804-22&language=ja_JP&l=li3&o=9&a=B07CWNLHJ8" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />


## 3. 今後の展望
### 3.1. モニター部分をプロジェクターに変更
- iPadはBluetoothによって画面をミラーリングすることもできます。プロジェクターには[Popin Aladdin](https://amzn.to/3aiqKVn)を使う予定です

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77458772-10625c80-6e42-11ea-9998-40235fa4a44c.png" height="370px" width="600px" alt="About Github Pages">
</div>

- 採用理由
    - 120インチを表示できるので大きいモニターよりコスパが良い
        - 壁一面に映してリモートセッションをやってみたい...
    - 照明と一体型のプロジェクター
        - 暗くする手間を省ける
        - 場所を取らない
    - 赤外線リモコンが付随しているので、スマートリモコンでAlexaから動かすことも可能

    <a href="https://www.amazon.co.jp/popIn-Aladdin-%E3%83%9D%E3%83%83%E3%83%97%E3%82%A4%E3%83%B3%E3%82%A2%E3%83%A9%E3%82%B8%E3%83%B3-%E3%83%97%E3%83%AD%E3%82%B8%E3%82%A7%E3%82%AF%E3%82%BF%E3%83%BC%E4%BB%98%E3%81%8D%E3%82%B7%E3%83%BC%E3%83%AA%E3%83%B3%E3%82%B0%E3%83%A9%E3%82%A4%E3%83%88-%E9%AB%98%E9%9F%B3%E8%B3%AA%E3%82%B9%E3%83%94%E3%83%BC%E3%82%AB%E3%83%BC/dp/B07J5B3VD2/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&crid=1LU6UOWAOS3OM&keywords=popin+aladdin&qid=1585071881&sprefix=popin,aps,263&sr=8-5&swrs=D7479C2CDBA3BA084A9A5627C81F989B&linkCode=li3&tag=junjun10804-22&linkId=01dac3ab47999be9201cb441db2a1e02&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07J5B3VD2&Format=_SL250_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun10804-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun10804-22&language=ja_JP&l=li3&o=9&a=B07J5B3VD2" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

### 3.2. AWS Componserの導入
仕事柄AWS(クラウドサービス)の動向を常にチェックしているのですが、去年のSummitで機械学習用のキーボードが発表され、ずっと気になっています。

- [AWS Composer](https://aws.amazon.com/jp/deepcomposer/)とは
    - 弾いたメロディに対して、AIがアレンジを加えて作曲
    - 学習済みのモデルが複数用意されている
    - そのままSoundCloudにアップ可能
    - 学習モデルはオリジナルでも作成可能


学習モデルは与える音源によって変化していくらしいです。AIに音楽のルーツを持たせられるのが人間の様でとても興味深いです。

エンジニア寄りの話になりますが、AWSでの開発もcloud9等のブラウザ型IDEを用いれば、iPadからやれてしまいます。趣味と仕事を結びつけたら楽しそうなので、時間を見つけて挑戦しようと思います。

### 3.3. 電子ドラムを追加
- ここまで揃えれば家でセッションできますね
    - おすすめを教えて頂けると嬉しいです

### 3.4. リモートセッション
- ネットを介してセッションできるサービスがあります
    - 現時点では遅延もあるらしいです

- 手段
    - [YAMAHA NETDUETTO](https://www.netduetto.net/)
        - ヤマハのサービスです
    - [JamBlaster](https://www.jamkazam.com/products/jamblaster)
        - 米国のスタートアップが最近開発したハードです
            - スマホベースでネットと繋ぐらしい。詳しくは調査中

- 5GやWifi6で遅延が減って、完璧なサービスが登場するのが楽しみです
    - とりあえずYAMAHAのNETDUETTOを一緒に試してくれる方を募集してます
    （いきなり知らない人は怖いので）

## まとめ
今回の記事は以上です。タブレットでも音源編集が可能になり、だいぶDTMの敷居が下がったように思います。私も何も知らないまま一から調べて今回の構成を作ったので、ここまでは誰でも真似できると思います。是非実践してみてください。  
質問やアドバイスなどあればどしどしください。