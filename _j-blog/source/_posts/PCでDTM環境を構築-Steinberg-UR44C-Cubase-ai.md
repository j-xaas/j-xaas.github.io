---
title: 'PCでDTM環境を構築 [Steinberg UR44C/Cubase ai]'
date: 2020-04-19 02:50:33
category: 
- SmartHome
- DTM
tags: 
- Steinberg UR44C
- DTM
- Cubase ai
toc: true
---

PCでDTM環境を構築するまでを解説します。InterfaceにはSteinberg UR44Cを採用しています。  
基本的にモバイル向けのCubasis leをiPadで使用していますが、[リモートセッションサービス](syncroom.yamaha.com)がPC対応のみであったため、備忘録的にまとめておきます。

![構成](https://user-images.githubusercontent.com/41946222/79885198-61f60b00-8431-11ea-97cc-6298ae1c92a3.png)


<!-- toc -->

## 1. Interfaceについて 
- PCにラインで音を取り込むにはインターフェイスが必要になります
### Steinberg UR-C
- [Steinberg UR44C](https://amzn.to/2JdYsPS)を採用しました
    <a href="https://www.amazon.co.jp/%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%B3%E3%83%90%E3%83%BC%E3%82%B0-Steinberg-USB3-0-%E3%82%AA%E3%83%BC%E3%83%87%E3%82%A3%E3%82%AA%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%BC%E3%83%95%E3%82%A7%E3%82%A4%E3%82%B9-UR44C/dp/B07YLKRH58/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&keywords=Steinberg+ur44C&qid=1585065911&sr=8-1&swrs=3928F2E7A513E4BED97A4C3E596F00B9&linkCode=li3&tag=junjun10804-22&linkId=3bf102532686f968cd4c92202a2f4172&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07YLKRH58&Format=_SL250_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun10804-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun10804-22&language=ja_JP&l=li3&o=9&a=B07YLKRH58" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

- 採用ポイント
    - Input×4 ＆ マイクプリアンプx4
        - 複数の楽器とマイクを繋ぎ、人数分のヘッドホンに出力すれば、屋内でサイレントセッションが可能
        - 元々↑をやりたくて買いました
    - 価格
        - 3万円ちょっとで手頃
            - 今回の条件に当てはまる中で最安（他は5万以上）
            - 更にInputを増やしたければ、予算を上げるかミキサーを買うしかないと思います
        - もう少し節約したい場合
            - [UR22C](https://amzn.to/3bw7rZm) （17000円程度）
                - InputとOutputが減りますが基本的にはほぼ同じ機器
    -  USB 3.0（USB Type-C）対応
        - PCとiPad Proの両方で読み込めます
    - Cubase le(モバイル用)とcaubase ai(PC用)のコードがついてくる
        - [Cubase](https://new.steinberg.net/ja/cubase/)
            - 音楽制作ソフト
            - 扱いやすく機能が一通り揃っているので初級者におすすめらしい
        - 本来は購入する必要があります(6000円程度)
    - ループバック機能により、動画配信も可能

- 周辺機器について
    - インターフェイス ⇔ PC間の接続ケーブル
        - 製品にUSB-C ⇔ USB3.0のケーブルがついてきます
        - USB-C ⇔ USB-Cケーブルはついてきません
            - なので、USB-CポートのみのMacBookやiPadの場合は以下の何れかの対処が必要です
                - ハブで変換する
                    - [USB Type C ハブ USB C ハブ USB3.0 ハブ 6in1 Type-c hub](https://amzn.to/2VqhARF)を使ってます
                - [USB-Cケーブル](https://amzn.to/2VNnGKT)を購入
    - Outputの端子用のアダプタ
        - ステレオ標準プラグ(楽器のシールドを刺す穴)なので、ステレオミニプラグ（イヤホンの端子）はそのままでは刺せません
        - わざわざ対応した端子の機器を買わなくても、変換アダプタ(2つで700円程度)を使えばOKです
            - [UGREEN ステレオミニプラグ（メス）⇒ステレオ標準プラグ（オス） 変換アダプタ 金メッキ 3.5mm ⇒ 6.35mm アンプヘッドホン変換 (2個)](https://amzn.to/2yweumk)

### マルチエフェクターで代用
- マルチエフェクターの中にはインターフェイスとしての機能がついているものもあります
    - ZOOMのG3やB3等
- ポイント
    - CubaseのようなDAWソフトはついてこないので別途購入が必要
    - Inputが少ない
- なので、以下に当てはまる方はインターフェイスを買った方が良いです
    - 複数の楽器やマイクを繋げたい
    - DAWソフトを利用したい

## 2. 導入手順
- PCとSteinberg UR-CでDTM環境を構築する為に必要な作業は以下の三点です

1. TOOLS for UR-Cのインストール
2. Basic FX Suiteのアクティベーション
    - ここまででPCで扱うDAWツールは基本使えます
3. Cubase AI (DAW)のダウンロード

### TOOLS for UR-C
- [公式サイトから](https://japan.steinberg.net/jp/support/downloads/ur44.html)から入れます
    - TOOLS for UR44 V2.2.1 - 278 MBを選択
    - zipファイルがインストールされます
- USBを全部外す
- zipファイルを開いてsetup.exeを実行

![image](https://user-images.githubusercontent.com/41946222/79579880-24c50c80-8103-11ea-8f1e-840e06207758.png)

- 指示にしたがって次へを押していけばOK

![image](https://user-images.githubusercontent.com/41946222/79580134-884f3a00-8103-11ea-9420-0ab5cce0a5f3.png)

- 以下が出たら完了

![image](https://user-images.githubusercontent.com/41946222/79580496-057aaf00-8104-11ea-905c-2bcd2abac4a6.png)


#### インストール後の確認
- steinbergと付属のUSBで接続
    - 電源はUSBCからではなく、アダプターに設定してください

- デバイスマネージャーを開く
    - オーディオの入力及び出力にsteinbergが表示されてればOK
    - PCがインターフェイスからの接続を読み込めるようになりました

![image](https://user-images.githubusercontent.com/41946222/79581133-f5170400-8104-11ea-925c-09d994424d74.png)


### Basic FX Suiteのアクティベーション
- 先ほど一緒にインストールした上記にアクティベーションコードを入れて、ライセンスを入れる必要があります
- eLicencer Control Centerを開く
- アクティベーションコードを入力を選択
- Steinbergを購入した際の説明書に以下の紙が入っています
    - ESSENTIAL PRODUCT LICENSE INFORMATION
        - ここに書いてあるコードを入力

- 以下の画面が表示されます
    - ライセンスをダウンロード
![image](https://user-images.githubusercontent.com/41946222/79582273-833fba00-8106-11ea-955f-f6b2e259e133.png)

- 以上で完了です
    - ここまででPCへのライン音源の取り込みは可能です
    - DAW以外にもリモートセッションツール等で遊べるようになります
        - [[SYNCROOM/NETDUETTO] YAMAHAのリモートセッションツールを試してみた](https://j-xaas.github.io/2020/04/19/YAMAHA%E3%81%AE%E3%83%AA%E3%83%A2%E3%83%BC%E3%83%88%E3%82%BB%E3%83%83%E3%82%B7%E3%83%A7%E3%83%B3%E3%83%84%E3%83%BC%E3%83%ABSYNCROOM%E3%81%AE%E3%83%99%E3%83%BC%E3%82%BF%E7%89%88%EF%BC%9ANETDUET%E3%82%92%E8%A9%A6%E3%81%97%E3%81%A6%E3%81%BF%E3%81%9F/)

### Cubase AIのダウンロード
- [SteinbergのWEBサイト](http://www.steinberg.net/getcabaseai/)でアカウント登録する必要があります
    - 詳細はサイト内に詳しくまとまっています
- ダウンロードの際にコードが必要になります
    - steinbergの説明書についている以下の紙に記載されています
        - CUBASIS AI DOWNLOAD INFORMATION
