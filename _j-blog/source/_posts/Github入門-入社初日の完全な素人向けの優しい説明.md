---
title: 'Github入門 ～入社初日の完全な素人でも分かる優しい説明～'
date: 2020-05-19 00:33:17
categories:
- Tool Tips
- Github
tags: 
- Github.com
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/83426237-d200a380-a469-11ea-8dd5-6fe92939fc8c.png

---
  
　今回はGithubというツールについて解説します。学生や新入社員等、これからアプリ開発を始める方が初めに覚えるツールなのですが、専門用語を並び立てた説明だらけで戸惑う方が多いため、極力丸めた言葉で優しく書きました。  
　Githubはエンジニアに取っての読み書きのようなものです。当然の前提知識として暗黙知にされることが多く、アプリ開発を学ぶ中では必ず使うシーンがあります。あくまで作業を楽にするための便利なツールなので、食わず嫌いをせずに早めに覚えましょう。

![image](https://user-images.githubusercontent.com/41946222/82224547-ea979680-995e-11ea-88fc-1274230cf1d0.png)

<!-- toc -->

## 1. Githubとは
- 猫のようなキャラはマスコットのOctocatです
![image](https://user-images.githubusercontent.com/41946222/82229837-e327bb80-9965-11ea-8879-c5d3f7ac7156.png)

- ソースコード(アプリを構成するファイル群の事)を保管/共有する場所
    - アプリケーションはコードを書いたファイルの集まりで構成されます
    - 複数人で開発するには、各メンバが編集したソースコードを集めて合体(mergeと言います)させ、最新版を保管しておく共通の場所が必要
- バージョン管理ツール
    - 時系列に沿ったソースコードの編集内容の変化を管理可能です
        - 差分の視覚化
            - 他メンバが開発した箇所がマーキングされて表示されます
                - ソースを1行づつ見比べる手間を省略できます
        - 任意のタイミングに戻せる
            - 例えば、誤ってアプリを壊してしまった際に、直前のタイミングに戻せます
- コードレビュー（評価）
    - Pull Requestという機能を利用すると、自身の編集内容と共にメンバーに確認依頼を飛ばせます
    - 差分が一目で分かるため、レビューが効率的です
        - 指摘箇所のコードとコメントをセットで登録でき、対話形式で解決できます
    - 昔はExcelにレビュー表を作って一つ一つチェックしていたようです
        - このファイルのXX行目の～～がと毎回書いて照らし合わせる作業は恐ろしく非効率的なのでやめましょう
- 連携機能
    - WEBhook
        - Slack等のチャットツールに通知を飛ばす機能です
    - Github Actions
        - 自動化機能
            - 例えば、Gitに編集したコードをアップした際に本番環境（アプリが動くサーバー等）に自動的に送ってくれたりします（デプロイという作業） 
        - ある程度開発に慣れたら使ってみましょう
            - CI/CDやDevOpsというワードと一緒に勉強してみてください

## 2. Githubの利用に必要な準備
- まずはアカウントを作成して、自分のPC上でGitのコマンドを実行できるようにインストールする必要があります

### 2.1. アカウントの作成
- [gitub](https://github.com/)でアカウント登録を行いましょう
    - 必要事項を入力してSign up
    - 通知メールから承認

![image](https://user-images.githubusercontent.com/41946222/82223880-33028480-995e-11ea-9889-630f1f88ec8f.png)

### 2.2. IDEの準備
- 開発を行うにはIDEというものが必要です
- IDEは以下の二つの要素でできています
    - エディター
        - コードを編集する場所
    - CLI
        - コマンドを打つ場所
            - 基本的にここでgithubを操作するコマンドを打つので、用意が必要です

- 以下をPCにインストールしましょう
    - [Visual Studio Code](https://azure.microsoft.com/ja-jp/products/visual-studio-code/)
        - この記事もVS Codeで書いています
    ![image](https://user-images.githubusercontent.com/41946222/82232629-d7d68f00-9969-11ea-9bb1-3977d2b9d25a.png)

- 以降は上記のIDEのCLI(VS Codeの画面下部)にコマンドを打つという前提で説明を書きます

### 2.3. インストール
- 以下を参考にGithubをPCに入れましょう
    - [Gitのインストール](https://git-scm.com/book/ja/v2/%E4%BD%BF%E3%81%84%E5%A7%8B%E3%82%81%E3%82%8B-Git%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)

## 3. リポジトリ作成～クローン
- リポジトリとは
    - ソースコードを格納する一つの箱のようなもの
    - 基本的にアプリ単位で作成します
    - これをGithub上と自分のPC上に対になる形で配置します(clone)
        - 中身のファイルを編集して、コマンドで行き来させます
            - ざっくりイメージ
                1. pullコマンドで自分のPCに持ってくる
                2. 編集
                3. pushコマンドでGithubに返す

### 3.1. リモートリポジトリの作成
- githubにログインして"New Repository"から作成してみましょう

### 3.2. リポジトリをLocal端末へのClone
- リポジトリ作成後の画面を開く
- "Clone or download"を押下
    - 戻りのボタンです
    - 出てきたURLを保存します

- 以下のコマンドをCLIで実行
    - Github上に作ったリポジトリが自分のローカル端末(PC)にコピーされます
```
git clone <url>
```

- ここまででGit ⇔ PC間で、データを行き来させることが可能になりました

### 4. 編集した内容をリモートに送るまで/基本コマンド

基本的な流れを以下に示します

- Github上から最新のソースコードを取得
```
git pull
```

- 開発
    - ファイルを開いて編集
        - リポジトリ生成時にReadme.mdというファイルができていると思います
            - リポジトリの説明を書くファイル
            - ここに開発のルール等をメモしてメンバと共有するのが一般的です
    - VS Codeなら以下のコマンドで開けます
    ```
    code <ファイル名>
    ```

- add: リモートに送るファイルを指定
    - 厳密にはCommitに含めるファイルを指定
```
git add <編集したファイル名>
```
- commit: 編集の完了を宣言
    - commitのタイミング毎にバージョン管理がされます
        - 好きなタイミングに戻れるようにこまめにcommitしましょう
```
git commit
```
- リモートに編集内容を送る
    - ここで送られるのはcommitに含まれるファイルのみ
```
git push
```

- ここまでで、個人開発はできます。次は複数人が関わるパターンを勉強しましょう。ブランチという概念の理解が必要です。

### 4. Githubを用いた分散開発
- 次に、複数名で開発を行うために必要なbranchやmergeを習得しましょう
    - [git merge & コンフリクトの解消（複数名の編集内容を集約）](/git-merge-コンフリクトの解消（複数名の編集内容を集約）/)

### Cheat Sheat
- [Git Cheat Sheat](https://www.softantenna.com/wp/software/git-cheat-sheet/)

- ![image](https://user-images.githubusercontent.com/41946222/69409586-94012500-0d4c-11ea-9ff0-f7fe87911596.png)

## 関連記事
- [Github入門 ～入社初日の完全な素人でも分かる優しい説明～](/Github入門-入社初日の完全な素人向けの優しい説明/)
- [[CI/CD入門] Github Actionsでビルド/テスト/デプロイを自動化](/CI-CD入門-Github-Actionsでビルド-テスト-デプロイを自動化/)
- [Github x Teams Webhook/Notificationによる連携方法](/Github-x-Teams-Webhook-Notificationによる連携方法/)
- [Github Pages x Hexo運用をGithub Actionsで自動化](/Github-Pages-x-Hexo運用をGithub-Actionsで自動化)
- [git複数アカウントの使い分け設定](/git複数アカウントの使い分け設定/)
- [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)
- [git merge & コンフリクトの解消（複数名の編集内容を集約）](/git-merge-コンフリクトの解消（複数名の編集内容を集約）/)
- [Github.comからGithubEnterpriseへの移行手順](/GithubからGithubEnterpriseへの移行手順/)
