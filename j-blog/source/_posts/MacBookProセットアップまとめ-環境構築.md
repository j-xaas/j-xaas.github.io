---
title: MacBookProセットアップまとめ/環境構築
date: 2020-08-06 21:47:47
category: 
- Tool Tips 
- MacBook Setup
tags:
- MacBook Pro 16
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/89536696-458cab80-d833-11ea-9aa1-6bffaa39196a.png
---

Macbook Proを買った最初にした一通りの作業のメモ

<!-- toc -->

## ソフトのインストール
- Chrome
    - 拡張機能は引き継ぎ
- VSCode
- Spactacle
- Slack
- Teams
- Zoom
- mmhmm
    - WEB会議用
- [DeepL](https://www.deepl.com/ja/app/)
    - 翻訳アプリ
- SparkAR
    - ARフィルター開発用
- MindNode
- Draw.io

## 本体設定
### キーのリピートを最速化
- 左上のリンゴマーク/Keyboard
- KeyRepeat, Delay Until Repeatのバーを一番右に変更
- タイピングが高速になる

### 画面分割(Spactacle)
- Macはそれぞれのアプリのウィンドウの大きさを調整しないと画面に複数のウィンドウを表示して同時に見ることができない
- 以下からSpectacleをダウンロードして，Applicationsに入れる
    - [Spactacle](https://www.spectacleapp.com/)
- 以下のショートカットを利用可能になる
![Spactacle 1.2](https://user-images.githubusercontent.com/68212997/89248828-a48fcc00-d64b-11ea-9f51-70868d39d604.png)


- 使用例：Left Half
    - Option + Command + ←
    - Windows Key + ←でやっていたことと同じことが可能になる

- 自動で使えるように設定
    - Spectacleを開く
    - Launch Spactacle at loginにチェック

## 環境構築
- git
    - CLI上でgit --versionを打ったら「Developperツールを利用しますか？」と聞かれ自動でMacが入れてくれた
- Homebrew
- Node.js
    - 先にHomebrewrew, Nodebrewを入れる
        - [Homebrew](https://brew.sh/index_ja.html)
            - Mac用のパッケージマネージャ。ツールのインストールとか諸々を管理してくれる
            - Nodebrewを入れるために使う
            - こちらのコマンドをコピペして実行
        - [Nodebrew]()
            - Node.jsのバージョン管理ツール。複数のバージョンのNode.jsをインストールしたり、切り替え等々が可能
            - 以下を実行するだけ
            ```
            brew install nodebrew
            ```
        - Node.jsをインストール
            - インストール可能なバージョンを確認
            ```
            nodebrew ls-remote
            ```
            - インストール
                - 最新版
                ```
                nodebrew install-binary latest
                ```
                - バージョン指定する場合
                ```
                nodebrew install-binary {version}
                ```
            - 警告が出てインストールに失敗した場合は以下を先に実行
            ```
            mkdir -p ~/.nodebrew/src
            ```
        - インストールされたNodeを有効化
        ```
        nodebrew use v14.7.0
        ```
            - 以下を実行して、Current: v~と表示されればOK
            ```
            nodebrew ls
            ```
        - Nodeの実行パスを通す
            - bashで使う場合
            ```
            echo 'export PATH=$HOME/.nodebrew/current/bin:$PATH' >> ~/.bash_profile
            ```
            - zshで使う場合
                - MacOS Catalinaからは標準で起動するターミナルがzsh
            ```
            export PATH=$PATH:/Users/hiro/.nodebrew/current/bin
            ```
        - 確認
            - 以下を問題なく実行できればOK
        ```
        node -v
        npm -v
        ```
- Hexo
    - 以下をHexoディレクトリで実行
    ```
    npm install -g hexo
    npm install hexo --save
    ```
- Angular CLI
- Firebase CLI
- AWS CLI

## VSCode設定
以下は最低でも設定しておくと生産背が上がります

- codeコマンドでファイルを開けるようにする
    - Shift + Command + pでコマンドパレットを開く
    - shell commandで検索
        - Shell Command: Install 'code' command in PATHを選択してInstall
        - 以下が表示されればOK
        ```
        Shell command 'code' successfully installed in PATH
        ```

- Command ＋ `　でエディターとCLIを移動可能にする

- Control + Shift + Tab
    - エディタータブの移動
- Shift + Option + Command + ↑
    - マルチカーソル入力
- Control + Shift + G
    - git

## 参考
- [快適にプログラミングするためのMac設定まとめ【これだけはやっておけ】](https://datawokagaku.com/programming_mac_setting/)
- [MacにNode.jsをインストール](https://qiita.com/kyosuke5_20/items/c5f68fc9d89b84c0df09)
- [MAcにNode.jsをインストールしてnpmを使えるようにする（Nodebrew利用）](https://hirooooo-lab.com/development/nstall-node/)
- [VSCodeのマルチカーソル練習帳](https://qiita.com/TomK/items/3b1f5be07d708d7bd6c5)
- [VSCodeの秘伝のワザを大公開！](https://dev.classmethod.jp/articles/vscode-awesome-things/)