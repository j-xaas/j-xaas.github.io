---
title: '[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】'
date: 2020-03-23 23:24:25
category:
- WEB Page Dev
- Hexo
tags:
- WEB Page Dev
- Hexo
toc: true
---

こんにちは。今回は静的WEBサイトジェネレータの[Hexo](https://hexo.io/)を用いたWEBページの自動生成手法と、[Github Pages](https://help.github.com/ja/github/working-with-github-pages/about-github-pages)を用いた公開手法について解説します。”３分でできる”等という触れ込みを見て始めたのですが、当時素人だった私は相当ハマり諦めました...。初級者向けに説明が欠けている所を重点的にまとめようと思います。  
  
昨今はWord PressやNote等のサービスを使って記事を書く方が多いと思いますが、自由度が低く、クオリティを追求すると有料になってしまう事が多いです。
  
今回の手法であれば、運用費を1円もかけず、ソースコードを完全に自由にいじれます。エンジニア界隈で流行っている手法ですが、素人や学生でも使えるレベルです。GithubやWEBの基礎の勉強にもなるので、是非挑戦してみてください。  
  
ざっくりのイメージ（まだ分からなくてOKです）

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77337436-3ae1e600-6d6c-11ea-919a-51893f317e71.png" height="300px" width="700px" alt="About Github Pages">
</div>

<!---toc--->

## 1. 関連技術概要
- 早く進めたい人は手順に飛んでOKです

### 1.1. Hexo
- [静的WEBサイトジェネレータ](https://ferret-plus.com/9413)
    - 静的WEBサイト
        - HTML + CSS + JavaScriptで構成されるWEBページ
    - 上記をコマンド一つで自動生成するツールです
    - Markdown形式のファイルを自動的にビジュアライズして表示可能
        - [Markdown](https://www.asobou.co.jp/blog/bussiness/markdown)とは文章を簡単に記述するための記法です
            - ハイフンで箇条書きを表現したり、#で見出しの文字を表現したりできます
                - HTMLで書くよりはるかに簡単なので覚えましょう
                - 慣れれば普段のメモもこの記法が楽です

- テーマが300種類近くあります
    - [Hexo Theme](https://hexo.io/themes/index.html)
        - ブログや企業サイト、デザイナーのポートフォリオ等、なんでも作れます
    - Themeの変更方法やカスタマイズ方法は他の記事で解説しているので、ページ作成後にご覧ください
        - [Hexoテーマ(theme)変更: icarus](https://j-xaas.github.io/2020/03/13/Hexo%E3%83%86%E3%83%BC%E3%83%9E-theme-%E5%A4%89%E6%9B%B4-icarus/)

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77341636-5c45d080-6d72-11ea-8ac7-85d6d86a9141.png" height="400px" width="600px" alt="Hexo Theme">
</div>

### 1.2. Github Pages
- [GitHub](https://github.co.jp/)([バージョン管理ツール](https://tracpath.com/works/development/git-mercurial-subversion/))が提供しているホスティングサービス
    - 静的なWEBページを公開可能
    - 運用費は無料
    - 一定数のリクエストを超えると落ちてしまう制限がある
        - 10万PV以上/月
            - 初めは無視してOKです

- [Github](https://github.co.jp/)の知識があれば学習コスト0で利用可能
    - Githubとは
        - 簡単に言うと、ソースコードを置いておく場所です
        - 開発に必須とも言えるツールであるため、覚えておくに越したことはないです
        - Github Pagesが優れている点は、このソースの置き場所でそのまま公開できる手軽さにあります

- Github Pagesで生成可能なページの種類
    - 1. ユーザーページ（https://ユーザー名.github.io）
        - Githubユーザ名のrepository(名前固定)で公開
        - Githubユーザ1名につき、1つまで
        - Point
            - Organization(組織)のリポジトリである場合は上記の”ユーザ名”を”Organiztation名”に読み替えて利用できます
                - つまりOrganizationを量産すれば、その数だけユーザページを作成可能です

    - 2. プロジェクトページ（https://ユーザー名.github.io/リポジトリ名/）
        - repositoryを作成すれば、無制限にサイトを作成可能

- Angularで開発したSingle Page Applicationのホスティングも可能
    - WEBアプリの公開にも使えます
        - 10万PVで落ちるという制約があるため、ビジネスの現場ではそこまで使われません
        - 個人で開発したものや、開発途中のものを公開するにはとても便利です
    - Angularの解説は以下にまとめてます
        - [Category/Serverless Application Dev/Angular](https://j-xaas.github.io/categories/Serverless-Application-Dev/SPA-Angular/)
    - フロントエンドのスキルを深めていくのであれば、次のStepとしてAngularによるWEB AP開発に取り組むのが良いと思います。書籍等で勉強してみてください（[おすすめ](https://amzn.to/2WHzoJ4)）

- Markdownやasciidoc形式のファイルをそのままおいて、ビジュアライズ表示も可能
    - push直後には失敗していた。数分後に確認したところ、確認できた。
    - 編集内容の反映までに初めはリードタイムが必要

## 2. 手順
- 初級者向けに環境構築から書きます

### 2.1. 前提/環境構築
- エディターを用意 (以下がお勧めです)
    - Local端末の場合
        - VS Code
            - [【ゼロから！】Visual Studio Codeのインストールと使い方](https://eng-entrance.com/texteditor-vscode)
    - クラウド型IDEの場合 (GoogleアカウントがあればCloud Shellが楽です)
        - Google Cloud Shell
            - [【GCP入門編・第9回】 Cloud Shell で、いつでもどこでも Google Cloud Platform (GCP) が操作可能に！](https://www.topgate.co.jp/gcp09-how-to-use-cloud-shell)
        - AWS Cloud9
            - [Cloud9の使い方と便利機能！最強プログラミング開発環境（IDE）](https://www.sejuku.net/blog/385)
- Node.jsを
    - Hexoを動かすために必要です
    - まだ入れていなければ、インストールしましょう
        - 参考
            - [【Node.js入門】各OS別のインストール方法まとめ(Windows,Mac,Linux…)](https://www.sejuku.net/blog/72545)
    - Google Cloud ShellやAWS Cloud9等のクラウドIDEであれば、初めから入っています
        - 面倒な環境構築を避けたい方は利用してみましょう

### 2.2. github pagesの公開

- 準備
    - ユーザ直下に作成する場合
        - 特になし
    - Organizationで作成する場合
        - Organization名がユーザ名の代わりになります
        - URLにそのままなるので、作りたいWEBページに合わせて決めましょう

- repositoryを作成
    - 以下のように名称を設定すると中においた静的ソースコードがWEBページとして公開される
        - user-name.github.io
    - 今回のrepository名
        - user-name.github.io

- 最終的に以下のURLで表示されます
    - https://user-name.github.io

#### Markdownファイルをおいてみる
- repositoryをコピー
    - PS D:\> git clone https://github.com/user-name/user-name.github.io.git
- 適当なmarkdownファイルを作成してreporitoryにpush
- ブラウザで確認
    - https://github.com/user-name/user-name.github.io

### 2.3. 静的サイトジェネレーターを利用してサイトをビジュアライズ

- 作成したgithub pagesのrepository配下にhexoを入れます

- Hexoをインストール
    - この時点でエラーが出たらNode.jsのバージョンを確認してください
```
$ npm install -g hexo
```
- Hexoでプロジェクトを作成
```
$ hexo init your-blog
```

- hexoで起動して、ページを確認
```
$ cd your-blog
$ hexo server
```
- Localhostで以下の画面を確認
    - 環境構築が事前に済んでいたので、ここまで3分程度

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/69618328-10fa0a80-107d-11ea-85cf-e491879b5b38.png" height="400px" width="500px" alt="Hexo Default">
</div>


- ポイント
    - README.mdは干渉するので削除
    - まだgitにLocalで作ったブログをpushしてもWEBサイトには出ません
        - ここでハマったので詳しく解説していきます

- generate
    - HTMLをpublicフォルダ内に生成するコマンド
```
hexo generate
```

- publicというディレクトリが作られ、配下に以下が生成される
    - \user-name.github.io\your-blog\public

```
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       2019/11/26     19:03                2019
d-----       2019/11/26     19:03                archives
d-----       2019/11/26     19:03                css
d-----       2019/11/26     19:03                fancybox
d-----       2019/11/26     19:03                js
-a----       2019/11/26     19:03           6589 index.html

```

- 以下でindex.htmlを確認しましょう
    - file:///D:/user-name.github.io/your-blog/public/index.html

#### Trouble Shooting
- ここでpushをすると、以下のエラーに悩まされます
    - ググっても関係のない対処法ばかり出て初級者は詰むと思います
    - 私もSSH鍵の設定を確認したりだいぶ迷走しました

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/69620310-887d6900-1080-11ea-9574-dadb2f4a7a0c.png" height="100px" width="400px" alt="Hexo Default">
</div>

```
GitHub Pages failed to build your site.
The value '{}' was passed to a date-related filter that expects valid dates in /_layouts/default.html or one of its layouts.
```

#### 回避策
- エラーの原因
    - Github pagesはリポジトリ内のルートディレクトリを読み込もうとする
    - HEXOでgenerateされたWEBページはpublicディレクトリにある
        - 読み込めずにエラーが出ている
    - generateされたファイルがrepository直下におかれるように設定を編集
        - hexoディレクトリ直下の設定ファイルを編集することでできました

- _config.ymlを編集
```
旧：public_dir: public
新：public_dir: ../
```

- 上記の状態でgenerateコマンドを打つと、public配下に生成されていたファイル群が、repository直下に生成される

```
    ディレクトリ: D:\user-name.github.io

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       2019/11/26     21:36                2019
d-----       2019/11/26     21:36                archives
d-----       2019/11/26     21:36                css
d-----       2019/11/26     21:36                fancybox
d-----       2019/11/24     13:00                your-blog
d-----       2019/11/26     21:36                js
-a----       2019/11/26     21:36           6834 index.html
```

- your-blog（ソースファイル）をgithub pagesが読み込んでエラーを起こさないように名称を変更
    - 余計なディレクトリがあるとGithub PagesはWEBページを表示できません
        - 初級者は個々でもはまると思います
    - 元々のHexoディレクトリを_で読み込めなくするだけでOKです
```
PS D:\user-name.github.io> mv your-blog _your-blog
```

- ここまでやってからpush
- WEBブラウザでページの生成を確認
```
https://user-name.github.io/
```


### 2.4. 新しい記事を追加
- hexo new '記事名'
    - 概要ページを作ってみましょう

```
PS D:\user-name.github.io\your-blog> hexo new "about-mob"
INFO  Created: D:\user-name.github.io\your-blog\source\_posts\about-mob.md
```

- source/_posts/<記事名>.mdが生成

- localhostでチェック
```
PS D:\user-name.github.io\your-blog\source\_posts> hexo server
INFO  Start processing
INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
```
- 記事が増えている

![image](https://user-images.githubusercontent.com/41946222/69628408-08aacb00-108f-11ea-82fb-5baa34dad1a1.png)

- 生成されたMarkdownファイルを確認

```
PS D:\user-name.github.io\your-blog\source\_posts> ls

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----       2019/11/26     20:51             57 about-mob.md
-a----       2019/11/26     18:42            838 hello-world.md
```

- abouto_mob.mdを編集
- 編集内容がLocal内で反映されていることを確認

### 2.5. 下書き記事を追加
```
hexo new draft "article-name"
```
- 下書きディレクトリ source/_drafts にmdファイルが生成される
- 本番環境やhexo serverでは表示されない
    - 表示したい時は以下で--draftsを付ける
    ```
    hexo server --drafts
    ```

### 2.6. 固定ページを追加

```
$ hexo new page "dir-name"
```
- source/"dir-name2/index.md が生成される


### まとめ
- 以上で解説は終了です
- 以下ができる状態になったと思います
    - 静的サイトジェネレーターでWEBサイトを自動生成
    - 静的WEBサイトホスティングで公開

## Hexoサイトのカスタマイズ
- 次はテーマを変更してみましょう
    - [Hexoテーマ(theme)変更: icarus](https://j-xaas.github.io/2020/03/13/Hexo%E3%83%86%E3%83%BC%E3%83%9E-theme-%E5%A4%89%E6%9B%B4-icarus/)

- その他のカスタマイズ
    - [Category/Hexo](https://j-xaas.github.io/categories/WEB-Page-Dev/Hexo/)
