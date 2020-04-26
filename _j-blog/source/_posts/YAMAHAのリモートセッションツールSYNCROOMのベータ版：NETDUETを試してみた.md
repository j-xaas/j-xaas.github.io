---
title: '[SYNCROOM/NETDUETTO] YAMAHAのリモートセッションツールを試してみた'
date: 2020-04-19 02:53:21
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
---

先日4/6にYAMAHAより新しい遠隔合奏サービス「SYNCROOM」が発表されました。  
6月～のリリースに備えて、既に公開中のβ版: NET DUETを使ってみます。（仕様はほぼそのままなので慣れとくといいかも）

## YAMAHAの遠隔セッションサービス（SYNCROOM/NETDUETTO）

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79638131-f573d580-81be-11ea-8b4b-16d9d6b1f351.png" height="400px" width="650px">
</div>

### SYNCROOM
- 6月頃より正式リリース
- 無料
- 超低遅延
    - 0.02秒（20msec）程度らしい
    - ZOOM等のサービスでは0.5秒以上はズレが生じます
- CD音質の通信
    - 非圧縮もしくはロスレスの16bit/48kHz
- 参考
    - [コロナ禍で苦しむミュージシャンの救世主となるか？ヤマハがネット越しのセッションツール、SYNCROOMをリリースする背景](https://www.dtmstation.com/archives/29387.html)

### NETDUETTO
- 上記のβ版で公開中（※2020/04時点）
- 無料
- 接続可能な人数
    - 一つのルームに5人まで
    - 2つのルームを繋ぐことで最大10人まで
- SYNCROOMとの差分
    - 予定されている追加機能
        - メトロノーム機能
        - レコーディング機能
    - ユーザ登録
- 利用条件
    - デバイス
        - PC (iPad Proでは無理でした)
            - SYNCROOMでタブレットにも対応してくれたら嬉しいですが、当分無理そう
    - OS
        - macOS 10.15 Catalina以降
        - Windows 10(64bit)
    - ネット回線
        - 基本的に光回線前提
            - NURO光等
            - softbank光やDOCOMO光を利用している場合
                - 料金変更無しで追加申請可能な回線強化の手段があるらしい（熟練者談）
        - できれば有線LAN
            - WiFiでもできますが、LANケーブルを刺せるPCがあるとベター
        - WiFi 6なら無線でも大丈夫かも？
            - モバイルの5Gのように、WiFiも最近6世代目になり高速化しました

## セッティング
- ざっくりイメージ

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79886594-2577de80-8434-11ea-8c7e-aae072ecea45.png" height="450px" width="600px">
</div>


### 機材
- インターフェイス
    - 楽器からのライン音源をPCに取り込むにはインターフェイスとPC側で読み込む為のドライバーのインストールが必要です
    - 初級者はSteinbergの以下のどちらかでOKだと思います
        - [Steinberg UR22C](https://amzn.to/3auxhvH)：￥17000ぐらい
        - [Steinberg UR44C](https://amzn.to/3bp6fqt)：￥27000ぐらい
    - 使用しているSteinbergのURシリーズについては、別記事で初期設定の手順をまとめておきました
        - [PCでDTM環境を構築 [Steinberg UR44C/Cubase ai]](https://j-xaas.github.io/2020/04/19/PC%E3%81%A7DTM%E7%92%B0%E5%A2%83%E3%82%92%E6%A7%8B%E7%AF%89-Steinberg-UR44C-Cubase-ai/)
    - Outputの端子
        - ステレオ標準プラグ(楽器のシールドを刺す穴)なので、ステレオミニプラグ（イヤホンの端子）はそのままでは刺せません
        - 安めの変換アダプタ(2個で700円程度)を買いました
            - [UGREEN ステレオミニプラグ（メス）⇒ステレオ標準プラグ（オス） 変換アダプタ 金メッキ 3.5mm ⇒ 6.35mm アンプヘッドホン変換 (2個)](https://amzn.to/2yweumk)
    - マルチエフェクターの中にはインターフェイス機能が付随したものもあるので、そちらで代用してもOK

- 有線LAN
    - WiFiで代用可能ですが遅延します。1000円程度なので有線LANも準備しておきましょう
        - 以下を使用しています
            - [Linkinperk CAT8 LANケーブル 2m 40Gbps 2000MHz 超高速](https://amzn.to/3eDlCOd)
                - 40Gbps 2000MHzでほぼ最速です
- Portは有線LANを指す穴です
    - 家のコンセントのどれかに必ずあるので、探してみてください
    - ここからネットワークに接続できます

機材が準備できたら、早速PCにNETDUETTOを導入してみましょう

## NET DUET導入手順
### Download
- [YAMAHAのNETDUETTOラボ](https://www.netduetto.net/download/)から自身のPCのbit数に合わせてダウンロード
    - SYNCROOMから64bit対応に限定されるらしいです

- ダウンロードしたzipファイルを開くとこんな感じ


<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79574690-7bc6e380-80fb-11ea-8560-263877a2af18.png" height="100px" width="500px">
</div>

### インストール
- インストーラーを起動
    - 次へを押していきます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79574870-cc3e4100-80fb-11ea-9b91-98e120cdeb20.png" height="450px" width="500px">
</div>

- インストール先
```
C:\Program Files\Yamaha\NETDUETTO2\
```

- 以下のようにインストールが始まります

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79575060-20492580-80fc-11ea-978b-98512afea090.png" height="450px" width="500px">
</div>

- 数分でインストールできます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79575151-4f5f9700-80fc-11ea-9a9e-ffdc2b2e82ca.png" height="450px" width="500px">
</div>

## 初期設定
- アイコンを開きます
    - チュートリアル画面が出てきます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79575320-8df55180-80fc-11ea-8098-14f3c7194776.png" height="350px" width="500px">
</div>


- 次へを押すと、以下の画面がでます
    - ”ASIOに対応したオーディオデバイスが見つかりました”と出ればOK
        - 事前にインターフェイスの準備が必要です
            - 今回はsteinberg ur44cからUSBでPCに接続しています
            - 別記事で解説します

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79639967-e9414580-81c9-11ea-81f6-14b408acb2f4.png" height="350px" width="500px">
</div>

- 入力の設定

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79640090-a2a01b00-81ca-11ea-8a93-7ae5997e1c2b.png" height="350px" width="450px">
</div>


- 遅延が無いかモニタリングできます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79640130-d2e7b980-81ca-11ea-9131-30179d08ff84.png" height="350px" width="450px">
</div>

- バッファサイズの設定

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79640174-16dabe80-81cb-11ea-91b3-7ec42f89fa50.png" height="350px" width="450px">
</div>

- コントロールパネルを押すと自分が利用しているインターフェイスに応じたドライバの設定が出てきます
    - ”Input Latency”を見ながら遅延が低くなるよう設定してみました

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79640221-586b6980-81cb-11ea-8229-003b912fc716.png" height="370px" width="450px">
</div>

- ニックネームとアイコンを設定
<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79640334-06771380-81cc-11ea-8264-4eac336cf860.png" height="370px" width="450px">
</div>
  
- 以上で完了です

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79640354-2575a580-81cc-11ea-85ba-24c906b2d3c2.png" height="370px" width="450px">
</div>

## 使い方
使い方はシンプルです

1. 機材をセッティング
    - インターフェイスや楽器をPCに繋いでおきます
    - 適当に楽器を弾いて、”インプット”から出力を確認しましょう
        - ここに出てこない場合はインターフェイスのドライバーが入っていないかもしれません

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79640629-818cf980-81cd-11ea-9deb-f80988ee4175.png" height="350px" width="500px">
</div>


2. ルームに集まる(↓のどちらか)
    - ルームを作る
    - 他人が作ったルームに入る

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79640397-666dba00-81cc-11ea-8515-c00653eb12b7.png" height="350px" width="500px">
</div>

- ルームは以下のように表示されます
<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79640717-0841d680-81ce-11ea-83b0-c0ea28e91862.png" height="500px" width="400px">
</div>

- セッション画面

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79641519-f9115780-81d2-11ea-95a9-ed03d2f740fc.png" height="350px" width="500px">
</div>

- 詳細表示を押すと、各メンバの通信状況や遅延状況を確認可能です

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79642425-53f97d80-81d8-11ea-871a-5a9469fe4e76.png" height="350px" width="500px">
</div>

## Twitter連携
- "Twitter連携"を押すと以下のようにウィンドウが出ます
<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79640867-0e848280-81cf-11ea-86be-80b98190e483.png" height="350px" width="500px">
</div>

- 認証
<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79640842-dda44d80-81ce-11ea-9e5e-00a4c34c647c.png" height="500px" width="400px">
</div>

- 出てきたPINコードを入力
    - アイコンとユーザ名が出ます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79640967-b26e2e00-81cf-11ea-9028-16ceb10c7d95.png" height="400px" width="500px">
</div>
