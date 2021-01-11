---
title: Adobe XDによるUIデザイン/プロトタイピング
date: 2021-01-11 21:27:55
categories:
- Tool Tips
- Adobe XD
tags: 
- Adobe XD
- UI
- プロトタイピング
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/103886405-ce7e7a80-5124-11eb-9514-7e08e019b6b2.png
---

Adobe XDによるUI設計についてのメモ。Adobe XDはUIのデザインだけでなく、プロトタイピングもできる。
つまり、インタラクティブに動く(ex. 画面遷移, ポップアップ)APのモックを短時間で用意できる。
本確的な実装に入る前に、ステークホルダーにUXを検証してもらうことで、後の改修作業のリスク低減を見込める。
  
特にデザイナー不在のPJにおいては、ExcelやpptによるUI設計を避けるために、エンジニアや営業も覚えておくと役立つ。
ハッカソンやアイデアソンにも使えそう。

<!--toc-->

<img width="1701" alt="Adobe XD thumbnail design" src="https://user-images.githubusercontent.com/68212997/103886536-038acd00-5125-11eb-83ea-09293e7ef91b.png">


## ダウンロード〜インストール
- AdobeのサイトからXD_installer.dmgをダウンロード
    - [Adobe XD](https://www.adobe.com/jp/products/xd.html)

<img width="1655" alt="Adobe XD" src="https://user-images.githubusercontent.com/68212997/103604624-fd45f680-4f54-11eb-9a93-596187820664.png">

- インストーラーを開く
<img width="434" alt="installer" src="https://user-images.githubusercontent.com/68212997/103604729-501fae00-4f55-11eb-859a-3a69e90a3bb1.png">

- 指示に従ってインストール
<img width="1276" alt="Adobe XD Install" src="https://user-images.githubusercontent.com/68212997/103604877-a68cec80-4f55-11eb-8ae9-1fb204a9fce2.png">

- なければAbodeのユーザ登録が必要
<img width="1274" alt="Adobe ユーザ登録" src="https://user-images.githubusercontent.com/68212997/103605023-07b4c000-4f56-11eb-932f-1c7e5cb2807c.png">

- インストールが完了すると以下の画面が表示されます
<img width="1086" alt="Adobe XD desktop" src="https://user-images.githubusercontent.com/68212997/103606164-1a7cc400-4f59-11eb-84d9-5c783d57c1bc.png">

---

## UI Kitを導入する
<img width="1501" alt="Adobe XD UI Kit" src="https://user-images.githubusercontent.com/68212997/103607514-7137cd00-4f5c-11eb-8539-f5a10259ea36.png">

デザイナー以外が利用する場合は、更の状態から作らずにテンプレを使うのが一般的

- 以下のサイトからUI Kitを導入できる
    - [Adobe XD UI Kit](https://www.adobe.com/jp/products/xd/features/ui-kits.html)

- AngularなどのWEBフレームワークでフロントエンド開発を行うのであれば以下を導入すると良い
    - AngularでUI開発に用いるAngular MaterialはこのGoogle Material Designが元になっている

<img width="1220" alt="Google Material Design" src="https://user-images.githubusercontent.com/68212997/103607039-56188d80-4f5b-11eb-8461-5092eeb79dd3.png">

- ローカルにKitをダウンロードすると、Light ThemeとDark ThemeのXDファイルを入手できる
    - それぞれのファイルを開くと、Adobe XDで以下の様に表示される

- Dark Theme
<img width="1514" alt="Adbo XD Dark Theme" src="https://user-images.githubusercontent.com/68212997/103608198-20c16f00-4f5e-11eb-904c-ee28421e0165.png">

- Light Theme
<img width="1340" alt="Adobe XD Light Theme" src="https://user-images.githubusercontent.com/68212997/103608380-90cff500-4f5e-11eb-9edb-cb9e62e72802.png">


## UIのプロトタイピング
- 新規ドキュメントを作成からデザインしたいタイプを選択してデザインを始められる
    - ex. モバイル, Web, カスタムサイズ

<img width="1086" alt="Adobe XD desktop" src="https://user-images.githubusercontent.com/68212997/103606164-1a7cc400-4f59-11eb-84d9-5c783d57c1bc.png">


## リンク機能
- 共有タブ/右のナビから、作成したデザインやプロトタイプを任意の形式で公開することができる
<img width="1193" alt="Adobe XD make link" src="https://user-images.githubusercontent.com/68212997/104022407-9863f800-5203-11eb-95e9-dbac0715a32e.png">

- 生成されたURLにアクセスすると以下の様に表示される
    - 関係者にコメントを書き込んでもらうことも可能

![AbdobeXD Link Smaple](https://user-images.githubusercontent.com/68212997/104021313-e5df6580-5201-11eb-8d1c-85075b48237a.jpg)

- 表示設定
<img width="387" alt="AdobeXD リンク 表示設定" src="https://user-images.githubusercontent.com/68212997/104022806-20e29880-5204-11eb-961e-ebf09f42fb2e.png">

- 以下の４種から設定可能
    - デザインレビュー
    - 開発
    - プレゼンテーション
    - ユーザーテスト
    - カスタム


- 無料アカウントでは1リンクまでしか公開できないが、作品→公開中のアイテム→完全に削除 より消せば問題なし
    - 仕事で使用する場合は、法人アカウントを作ると良い

- 以下で詳しく解説されていた
{% youtube sW5DJSRIadw %}


### モバイルデバイスのプレビュー
![Mobile Device Preview](https://helpx.adobe.com/content/dam/help/ja/xd/help/preview-mobile/jcr_content/main-pars/image_165997333/preview_prototypes-01.png)

- モバイルにAdobe XDのアプリを入れることでプレビューすることもできる
    - [モバイルデバイスでのプロトタイプのリアルタイムプレビュー](https://helpx.adobe.com/jp/xd/user-guide.html/jp/xd/help/preview-mobile.ug.html)


## 参考
- [Adobe XD](https://www.adobe.com/jp/products/xd.html?sdid=19SCDRPN&mv=search&ef_id=CjwKCAiAi_D_BRApEiwASslbJ7MQ57CoppbwiQKyvL31gHMHGOMurd9ECJa2X9V5P0u865lfKJbrbxoCxI8QAvD_BwE:G:s&s_kwcid=AL!3085!3!380840905165!e!!g!!adobexd!1641270158!61553403526)
- [Adobe XD UI Kit](https://www.adobe.com/jp/products/xd/features/ui-kits.html)
- [Adobe XD基本の使い方](https://udemy.benesse.co.jp/design/web-design/how-to-use-adobexd.html)