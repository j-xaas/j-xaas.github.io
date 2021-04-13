---
title: draw.ioをVSCode上で使用/図表をGitで一元管理
date: 2020-07-22 21:44:42
category:
- Tool Tips
- Drawing
tags:
- draw.io
- Cloud Craft
- Mind Node
- plantUML
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/82150925-c4a5bf80-9894-11ea-945c-aaf082ed9457.png
---

描画ツールのdraw.ioをVSCodeの拡張機能で利用可能にして、データをgitで一括管理する方法を解説します。
draw.io以外の描画ツールについては以下にまとめてあります
- [描画ツールまとめ [draw.io/Cloud Craft/Mind Node/plantUML]](/描画ツールまとめ-draw-io-Cloud-Craft-Mind-Node-plantUML/)

<!-- toc -->


## 1. 概要

### 1.1. draw.ioとは
- [draw.io](http://draw.io/)
    - WEBブラウザベースの描画ツール
        - Googleアカウントで運用でき、ローカル/Google Drive/Github等にデータを保存可能
    - 大抵の図(ex. フロー, システム構成...)はこれで作成可能です
    - 完成したらpng形式で出力して、markdownやpptに差し込むのが一般的

- テンプレートの例
    - FlowChartやクラス図  
    ![image](https://user-images.githubusercontent.com/41946222/82151236-8066ef00-9895-11ea-973f-b8a317512495.png)
    - Cloudのアーキテクチャ  
    ![image](https://user-images.githubusercontent.com/41946222/82151278-b3a97e00-9895-11ea-8a12-ce12c75db791.png)

- WEBで利用する際の問題
    - 共同開発時の管理の煩雑さ
        - あるメンバが作った図を、別メンバが編集できない
            - レビュー、修正が面倒
        - gitでAPのコードと一緒に一括管理できない
    - セキュリティ
        - 制約の厳しいPJであれば、ツール自体を使えないことが多い

上記の問題を解決するために、ローカル版を利用します

### 1.2. VSCodeの拡張機能

- VS Code用のプラグインがあり、フルの機能を端末上で利用可能です
    - [Draw.io VS Code Integration](https://marketplace.visualstudio.com/items?itemName=hediet.vscode-drawio)


![VSCode Draw.io Local](https://user-images.githubusercontent.com/41946222/82150925-c4a5bf80-9894-11ea-945c-aaf082ed9457.png)

- 拡張子を.drawio.svgにしてファイル作成すると、Draw.ioに関連付けられ、svg形式のファイルを作画＆作成可能
  - ファイルを開くだけでDraw.ioの編集画面VSCode内に立ち上がる
- メリット
  - SVG形式
    - gitで管理可能
      - 共同編集、差分管理が用意
    - 読み込みがpng等と比較して高速
  - セキュリティ上の制約をクリア
- mdファイルには以下のようにパス参照で埋め込む(HTMLからimgタグで参照してもOK)
  ```
  ![](img/image1.drawio.svg)
  ```

## 2. 使用方法

### 2.1. 拡張機能をInstall

- Draw.io IntegrationをInstall
  - VSCodeの左のサイドナビよりExtensionsアイコンを選択（or Ctrl+Shift+X）
  - Draw.ioで検索

![Draw.io Integration](https://user-images.githubusercontent.com/68212997/88175428-d030b080-cc60-11ea-9f3c-9c72e6772069.png)

- アイコン横のInstallボタンを選択
  - Installすると以下の様な説明ページを確認出来ると思います

![Draw.io Integration](https://user-images.githubusercontent.com/68212997/88175610-0cfca780-cc61-11ea-95bd-e598b31d3ac8.png)



### 2.2. ファイルを生成
- .draw.io.svg形式のファイルを生成
  - markdownのドキュメントと同じディレクトリに配置（後でパス参照するため）
```
touch sample.drawio.svg
```

### 2.3. 図表を編集
sample.drawio.svgを開くと自動的にDraw.ioの編集画面が開きます

![drawio.svg](https://user-images.githubusercontent.com/68212997/88176653-8e086e80-cc62-11ea-9641-3fe34e0bb5b8.png)

- WEBのDraw.ioと同様に編集
  - VSCodeのタブの一つで編集できるのが中々便利
- Ctrl+S等で保存
- Exportの必要はありません

### 2.4. Markdwonから参照
- 相対パスで先ほどのsvgファイルを参照
  - 以下で表示された
```
![](./sample.drawio.svg)
```

- プレビューの様子
  - 先ほど編集した図が出ます
![draw.io vscode preview](https://user-images.githubusercontent.com/68212997/88177354-af1d8f00-cc63-11ea-9b9e-bd9ae9cf6bc1.png)


後はGitにまとめて送れば、他メンバも同様に編集できます


## 関連記事
- [描画ツールまとめ [draw.io/Cloud Craft/Mind Node/plantUML]](/描画ツールまとめ-draw-io-Cloud-Craft-Mind-Node-plantUML/)

- [VSCode上のマークダウン とDraw.ioでドキュメントを作成する](https://qiita.com/oruharo/items/f5bdeedad28731da1b11)