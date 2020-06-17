---
title: 
    '描画ツールまとめ [draw.io/Cloud Craft/Mind Node/plantUML]'
date: 2020-05-15 03:02:12
category:
- Tool Tips
- Drawing
tags:
- draw.io
- Cloud Craft
- Mind Node
- plantUML
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/82147139-b3ef4c80-9888-11ea-865b-15bb4ea2186c.png
---

　個人的にイケてると思う描画ツールのまとめです。システム構成図や業務フロー図を何故かExcelで作る風潮があるようですが、そもそも描画ツールではないので、微妙な絵しか起こせません。以下のツールを導入して、レガシーな現場をあなたの手で改善しましょう。新人であれば最初の小さな成果になるかもしれません。（厳しい上司に当たると、Excelを使うだけで情弱扱いされます）

![Cloud Craft](https://user-images.githubusercontent.com/41946222/82147139-b3ef4c80-9888-11ea-865b-15bb4ea2186c.png)

<!-- toc -->

## 1. 描画ツール一覧
- UML(ex. システム構成/クラス図/シーケンス図...)
    - [draw.io](http://draw.io/)
    - [cloud Front](https://cloudcraft.co/)
    - [PlantUML](https://plantuml.com/ja/)
- ツリー
    - [MindNode](https://apps.apple.com/jp/app/mindnode-mind-map/id1218718027)
    - [MindMaster](https://www.mindmaster.io/?gclid=EAIaIQobChMI-v-d4IO76QIVZJ_CCh3boQPcEAAYASAAEgLsavD_BwE)

## 2. 各ツールについて
### 2.1. draw.io
![image](https://user-images.githubusercontent.com/41946222/82151353-1d298c80-9896-11ea-9fad-d17c2a5ed987.png)


- [draw.io](http://draw.io/)
    - WEBブラウザベースの描画ツール
        - Googleアカウントで運用でき、ローカル/Google Drive/Github等にデータを保存可能
    - 用途：万能
        - フローやシステム構成等の大抵の図は全てこれで作成可能です

- テンプレートの例
    - FlowChartやクラス図  
    ![image](https://user-images.githubusercontent.com/41946222/82151236-8066ef00-9895-11ea-973f-b8a317512495.png)
    - Cloudのアーキテクチャ  
    ![image](https://user-images.githubusercontent.com/41946222/82151278-b3a97e00-9895-11ea-8a12-ce12c75db791.png)



- VS Code用のプラグインもあります
    - [Draw.io VS Code Integration](https://marketplace.visualstudio.com/items?itemName=hediet.vscode-drawio)

![image](https://user-images.githubusercontent.com/41946222/82150925-c4a5bf80-9894-11ea-945c-aaf082ed9457.png)




### 2.2. cloud Front
![image](https://user-images.githubusercontent.com/41946222/82147538-85727100-988a-11ea-8cf8-8ef5c11462bb.png)


- [cloud Front](https://cloudcraft.co/)
    - WEBベースの描画ツール
    - 用途
        - クラウドアーキテクチャのシステム構成図作成
            - AWSに特化しており、draw.ioのようにGCPのテンプレはありません
        - AWS上の環境情報のリアルタイム取得（有料）
            - 構成図の自動描画
            - 構成や設定情報の表示
        - 見積もりの自動生成
    - AWSを活用しているチームであれば今すぐ導入を勧めます

- 以下のような図を数分で作成できます
![image](https://user-images.githubusercontent.com/41946222/82147757-e0a46380-988a-11ea-8fb8-498717103e2a.png)

- リアルタイムデータの確認機能 
    - 任意のコンポーネントをクリックすると、現在のコンフィグとコストを確認できます。AWS Web Consoleに直接ジャンプして、Liveリソースとタグを確認できます
- Collaborate機能
    - ダイアグラムやAWSリソースに直接ドキュメントを追加可能です。チーム全体で図をオンラインで共有・編集したり、ドキュメント、Wiki、プレゼンテーションにエクスポートしたりすることができます。


![image](https://user-images.githubusercontent.com/41946222/82147783-10ec0200-988b-11ea-96c8-ab51ed7dd2d0.png)

- 運用費の見積もりは上記のように出せます。
- 2次元の図も作成可能です
    - 優秀なエンジニアのドキュメントをみると、これかdraw.ioで作っている方が多いです
    - pptで綺麗に作るとだいぶ時間を食うので避けましょう

- 現実の環境から図を自動生成することまでできます
    - 公式説明訳
    ```
    公開してすぐに古くなってしまう静的なドキュメントを作成する時間を無駄にしないでください。
    
    Cloudcraft Liveは、AWS環境のすべてのサービス関係を瞬時に分析し、完全なシステムアーキテクチャ図をリバースエンジニアリングします。
    
    強力な自動レイアウト機能や高度なAWSアカウントスキャン機能もAPIとしてご利用いただけます
    ```

- 有料版について
    - 単なる構成図の描画であれば、無料版で可能
    - AWSとAPI連携して情報を取得するのは有料

![image](https://user-images.githubusercontent.com/41946222/82148412-fb78d700-988e-11ea-87e0-152e94834073.png)



### 2.3. PlantUML
![image](https://user-images.githubusercontent.com/41946222/82151531-dd16d980-9896-11ea-9094-dd5c053cb460.png)

- [PlantUML](https://plantuml.com/ja/)
    - 用途
        - コードによるUMLの作成
            - draw.ioと同様に大抵の図に対応可能
    - コーディングに慣れた上級者向けではありますが、テキストなのでGitで差分管理可能
    - PlanUMLの実行には以下のインストールも必要です
        - Java
        - Graphviz
- 基本的にVS Codeにプラグインを入れて使います
    - Visual Studio MarketplaceからInstall

![image](https://user-images.githubusercontent.com/41946222/82151768-3fbca500-9898-11ea-9937-8686f6fce877.png)


- 書き方
    - [PlantUML Cheat Sheet](https://qiita.com/ogomr/items/0b5c4de7f38fd1482a48)

### 2.4. MindNode

![image](https://user-images.githubusercontent.com/41946222/82149119-5dd2d700-9891-11ea-89c5-9e19c796cc9a.png)


- [MindNode](https://apps.apple.com/jp/app/mindnode-mind-map/id1218718027)
    - iPad用のアプリ
    - 用途
        - ツリー状の情報のまとめ
            - アイデアの整理によく使っています

- スキルマップを5分程度で整理した例
<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/79894561-9b824280-8440-11ea-89ba-4267b6508103.png" height="750px" width="900px">
</div>


### 2.5. MindMaster

- [MindMaster](https://www.mindmaster.io/?gclid=EAIaIQobChMI-v-d4IO76QIVZJ_CCh3boQPcEAAYASAAEgLsavD_BwE)
    - MindNodeのWEB版だと思ってください
    - downloadしてAPとしても利用可能です
    - 用途
        - MindNodeと同様

- 以下の様にテンプレを選択して利用を開始できます
    - pptやExcelで必死にレイアウトを調整する手間は
![image](https://user-images.githubusercontent.com/41946222/82148925-36c7d580-9890-11ea-88c3-d4990144e10d.png)

今回の解説は以上です。知っておくだけで生産性が向上するものが多いので、是非同僚に教えて上げてください。
良いツールを見つけたら追記します。お勧めがあれば教えて頂けると嬉しいです。