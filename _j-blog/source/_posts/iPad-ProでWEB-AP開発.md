---
title: iPad ProでWEB AP開発 & RPA
date: 2020-03-25 22:07:15
category: 
- Tool Tips
- Google Cloud Shell
tags: 
- iPad Pro
- Google Cloud Shell
- Cloud9
- RPA
- ショートカット
- ブラウザ型IDE
toc: true
alias: /2020/03/25/iPad-ProでWEB-AP開発/
thumbnail: https://user-images.githubusercontent.com/41946222/77545699-2c233c80-6eee-11ea-9e2c-fad4fd8dd3f3.png
---

こんにちは。先日[iPad 2020(第４世代)](https://amzn.to/2WMiw3M)を購入しました。発売日の今日受け取ってこの記事を書いています。最近はPCライクな作業の効率がUpしているので、WEB APの開発をできるようにしてみました。一言で開発環境が立ち上がるように自動化もしています。さっき[DTM環境](https://j-xaas.github.io/2020/03/25/iPad-Pro%E3%81%A7DTM%E7%92%B0%E5%A2%83%E3%82%92%E6%A7%8B%E7%AF%89/)も構築しました。
  

{% youtube miNQ61wB8Hg %}


<!-- toc -->


## 概要
- ざっくりのイメージ図を書きました

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77545699-2c233c80-6eee-11ea-9e2c-fad4fd8dd3f3.png" height="300px" width="500px" alt="iPad Dev ">
</div>

- 流行りのブラウザ型IDEを利用して、iPadでの開発を可能にします
- また、iPadのRPAツールとSiriで一言でIDEが立ち上がるようにします

### 基礎知識
- ブラウザ型IDEとは？
  - URLで即アクセスして、ブラウザで利用可能なIDE
    - RDPをするより手軽
      - そもそもiPadからのRDPはできなくもないがまだ厳しい
  - 裏側で動いているのはクラウド上の仮想サーバ
    - Cloud9ならEC2、Cloud ShellならGCEのリソースを利用
      - 意識せずに使えるので安心してください
  - 自端末のOSに依存せず開発可能
    - つまりiPadでも開発可能
      - 普通のWinows PCでも効果が大きいです
    - 社内プロキシ問題に悩まされない
        - 個人的には最重要ポイント
    - 環境構築を省略可能
        - node_module, Git, AWSやGCPのCLI等の便利なものが元から入っている
          - 新設チームや初級者への効果は計り知れないと思います

- お勧めできるブラウザ型IDEは以下の二つです
  - [Google Cloud Shell](https://cloud.google.com/shell/docs?hl=ja)
    - Googleのブラウザ型IDE
    - Googleアカウントで利用
    - 環境構築を省略可能
        - Firebase CLI等のGCP向けのものが入っている
  - [AWS Cloud9](https://aws.amazon.com/jp/cloud9/)
    - AWSのブラウザ型IDE
    - AWSアカウントで利用
    - 環境構築を省略可能
        - AWS CLI等の便利なものが入っている
    - ★複数ユーザーで同時編集可能
        - リモートワークやレビューに便利です
        - やり方は以下にまとめてあります
          - [cloud9による共同編集・リモート開発](https://j-xaas.github.io/2020/03/07/cloud9%E3%81%AB%E3%82%88%E3%82%8B%E5%85%B1%E5%90%8C%E7%B7%A8%E9%9B%86%E3%83%BB%E3%83%AA%E3%83%A2%E3%83%BC%E3%83%88%E9%96%8B%E7%99%BA/)

### 選定基準

- 用途から
  - 手軽さ優先・今すぐ使いたい
    - Google Cloud Shell
      - Googleアカウントさえあれば、5分以内に使い始められます
      - Cloud9はAWSのアカウントが必要なので、権限周りがシビアです
        - 組織で共有している場合、少し面倒なことになります
  - 複数人で同時編集したい
    - AWS Cloud9
      - そのうち、Google Cloud Shellにも同じ機能がつきそうではあります
      - 以下を参考にどうぞ
        - [cloud9による共同編集・リモート開発](https://j-xaas.github.io/2020/03/07/cloud9%E3%81%AB%E3%82%88%E3%82%8B%E5%85%B1%E5%90%8C%E7%B7%A8%E9%9B%86%E3%83%BB%E3%83%AA%E3%83%A2%E3%83%BC%E3%83%88%E9%96%8B%E7%99%BA/)

- 開発するサービスから
  - バックエンドに利用するクラウドが決まっていれば、環境構築の手間を削減できるので、合わせましょう
    - Firebase, GCPを利用する
      - Google Cloud Shell
    - AWS, Amplify, Lambdaを利用する
      - AWS Cloud9

## 周辺機器
### マウス
- iPadとマウスはBluetoothで繋げられます
  - iPad対応で”発信機が不要”なモノにしましょう
    - 安めで、USB充電が可能な[Fenifox](https://amzn.to/33L13KB)を使っています
      - amazonで探していたら、ipadに対応していないにも関わらず、”対応”と書かれているものがあったので、よく確認してください
        - fenifoxの説明には全くないのですが、接続時にパスワードを求められたら"0000","1234","1111"の何れかを入力すればOKです
    - 拘る人には[Apple純正](https://amzn.to/3dwoL1A)のマウスもあります

### Keyboard
- ひとまず[Magic Keyboard](https://amzn.to/2ybfNXL)を使用しています
  - 非常に軽く、登録後は叩くだけでipadと繋がります
  - 打鍵感に拘る人はこれが良いと思います
- 話題のiPadが浮くキーボードは2020/05発売なので待機中です
  - [Apple Magic Keyboard](https://amzn.to/2JeoFxV)
    - イケてるけど、エントリーモデルのipadより高い...
    - 追記：最近購入しましたが、ほぼ完全にPCになるのでお勧めです

    <a href="https://www.amazon.co.jp/Apple-Magic-Keyboard-12-9%E3%82%A4%E3%83%B3%E3%83%81iPad-Pro-%E7%AC%AC3%E4%B8%96%E4%BB%A3%E3%81%A8%E7%AC%AC4%E4%B8%96%E4%BB%A3/dp/B0863JB6GL/ref=as_li_ss_il?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&keywords=keyboard&qid=1585148574&s=computers&sr=1-8&linkCode=li3&tag=junjun10804-22&linkId=db9e7a94a54397fd62458a97a42bf10a&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B0863JB6GL&Format=_SL250_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun10804-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun10804-22&language=ja_JP&l=li3&o=9&a=B0863JB6GL" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

### イヤホン
- 会議用に用意しておくと便利です
- Sony製品が好きなので[WF1000XM-3](https://amzn.to/2UzfvRN)を使っています

## Google Cloud Shellの設定手順
- Google アカウントを作成
- ログインした状態でGCPを開く
  - [Google Cloud Platform](https://console.cloud.google.com/?hl=ja)
- Googleアカウントでそのままログイン
  - GCPコンソールが開きます


<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77556836-54199c80-6efc-11ea-8c6c-46d5084b2f69.png" height="400px" width="600px" alt="GCP Console Top">
</div>

- コンソール上部の赤で囲んだマークを押すと、コマンドラインが現れます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77560233-89c08480-6f00-11ea-9836-b8ac913ad85f.png" height="400px" width="600px" alt="GCP Console Top">
</div>

- コマンドラインが下部に出た状態
  - 次に拡大ボタンを押下しましょう

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77560503-eae85800-6f00-11ea-80ea-147176a449d3.png" height="400px" width="600px" alt="GCP Console Top">
</div>

- 全画面表示になります
  - 次にペンのマークを押下しましょう

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77560762-36026b00-6f01-11ea-8f86-9b4cddea8c5b.png" height="400px" width="600px" alt="GCP Console Top">
</div>

- エディターが現れます
  - 以上でVC Codeのような画面になったと思います
  - このWEBページもGoogle Cloud Shellのエディターで編集して、CLIでhexoのコマンドを打って自動生成しています
    - 画像の処理が楽なのでiPadでやると効率があがります

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77561852-9a71fa00-6f02-11ea-8176-1ad72277b6f4.png" height="150px" width="500px" alt="GCP Console Top">
</div>

- WEB APのlocalhostで実行してブラウザで確認したい時
  - 上の左から二番目のアイコンをクリックすると、portを指定してブラウザを開くことができます
    - クラウドIDEで開発する際に嵌りがちなシーンですが、Cloud Shellなら楽に解決できます

### ショートカット x SiriでIDEを開くまでを自動化
- 『Hey Siri, GCP』と言ったらGoogle Cloud Shellが開くようにします
  - ブラウザ型IDEの欠点として、ブラウザから開く手間があります。自動化して時短＆モチベーション向上を図りましょう

- ショートカット
  - iPhoneやiPadで使える手軽なRPAツール
  - iPadで作業するメリットの一つだと思います

#### 手順
- GCPコンソールのURLを控える
- ”ショートカット”アプリを開く
- ”ショートカットの作成”
- ショートカット名を”GCP”に設定
- 左上の検索欄で"Chrome"と入力
- 候補に出る”Chromeで検索”を選択
  - "ChromeでURLを開く"を選択
- URL欄に先ほどのURLをコピペ

以上で完了です。"HEy Siri, GCP"といえば、自動でIDEが出てきます


{% youtube PVPwo-PMOLI %}

- よく使うサービスは同じように表示までを自動化しておくと便利です
  - 冒頭に動画を載せておきましたが、cloud9も同じようにURL指定で開けるようにしています


### 開発の周辺作業
PCじゃないと無理じゃない？と言われそうな作業の代替え案です

- パワポ
  - Googleスライドを使いましょう
    - クラウド上に保存すれば、どの端末からでも見れるので便利です
    - アクセス制限を付けて任意の人に共有することも可能です
- Excel
  - Googleスプレッドシートを使いましょう

## 関連記事
- [cloud9による共同編集・リモート開発](https://j-xaas.github.io/2020/03/07/cloud9%E3%81%AB%E3%82%88%E3%82%8B%E5%85%B1%E5%90%8C%E7%B7%A8%E9%9B%86%E3%83%BB%E3%83%AA%E3%83%A2%E3%83%BC%E3%83%88%E9%96%8B%E7%99%BA/)
- [Cloud9 x Firebase x AngularでAP開発](https://j-xaas.github.io/2020/03/05/Cloud9-x-Angular-x-Firebase%E3%81%A7AP%E9%96%8B%E7%99%BA%EF%BC%88%E5%B0%8E%E5%85%A5%E7%B7%A8%EF%BC%89/) 
- [[Google cloud shell x Hexo] 環境構築&記事編集](/Google-cloud-shell-x-Hexo-環境構築-記事編集/)
    - このブログ自体もiPadからcloud shellを使って編集しています