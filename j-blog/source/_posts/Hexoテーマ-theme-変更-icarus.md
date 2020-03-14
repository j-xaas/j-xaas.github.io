---
title: 'Hexoテーマ(theme)変更: icarus'
date: 2020-03-13 23:01:02
categories:
- WEB Page Dev (Hexo)
tags:
- Hexo
- icarus
toc: true
---

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/76524916-3c7af680-64ae-11ea-9a71-81530f93167c.png" height="100%" width="100%">
</div>


<!-- toc -->


## 概要
- Hexoにはテーマのテンプレートが数百種類用意されており、簡単に変更することができます
    - デフォルトはlandscape
        - シンプルでいいのですが、デザインとして物足りなく、プロフィール欄も欲しいので変更します  


    <div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/41946222/76521887-038c5300-64a9-11ea-8dfd-8454b8ce0dd2.png" height="100%" width="100%">
    </div>


## 変更手順
- 1. テーマの選定
- 2. Githubからダウンロード
- 3. 設定(_config.yml)を変更

### 1. テーマの選定
- まずは採用したいテーマを決めましょう
- Hexoのテーマ一覧は以下より確認できます
    - [Themes | Hexo](https://hexo.io/themes/index.html)
    - 295種(2020/03/12時点)あります
- [Star数のランキング](https://github.com/search?o=desc&q=hexo-theme&s=stars&type=Repositories)はこちらです
    - 自身のWEBページの方向性に合わせて選定しましょう

- 個人的にイケてると感じたテーマを紹介します
    - シンプル
        - [icarus](https://ppoffice.github.io/hexo-theme-icarus)
            - プロフィール欄がデフォルトである
            - 各記事がサムネで表示される
                - 手間は少し増えてしまいそう
            - サイドのメニューと記事一覧を、画面サイズに応じて１～３列で表示
        - [NexT](https://theme-next.org/)
            - プロフィール欄がデフォルトである
            - モノトーン
            - １～２列で表示
        - [Material](https://github.com/viosey/hexo-theme-material)
        - [indigo](https://github.com/yscoder/hexo-theme-indigo)
        - [Tranquilpeak](http://louisbarranqueiro.github.io/hexo-theme-tranquilpeak)
            - 最近見つけたので追加。そのうちこちらに変更するかも

    - ダイナミック
        - [React](https://github.com/NoahDragon/hexo-theme-react)       
            - 会社サイト向け
                - デザインが作りこまれています
            - 別の機会に利用してみようと考えています
        - [Ochuunn](http://ochukai.me/)
            - サイトを開いた時の動作がおしゃれです
        - デザイナー向け
            - [MiccallTheme](http://miccall.tech/)
            - [Zhaoo](https://www.izhaoo.com/)
        - [TKL](https://go.kieran.top/)
            - 一枚目から始まるタイプですが記事欄は質素です

#### 今回採用したテーマ
- [icarus](https://github.com/ppoffice/hexo-theme-icarus)
    - GitのStar数ランク5位で人気のテーマです

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/76520745-d8086900-64a6-11ea-9d0d-d10ef9bad261.png" height="100%" width=100%">
</div>


- 採用理由
    - tech記事らしいシンプルさ
    - 各記事がサムネで一覧表示される
    - サイドメニューに以下が表示される
        - Profile
        - Categories
        - Tag
        - RECENT
        - ARCHIVES

### 2. Githubからダウンロード
- 2.1. Gitの[hexo-theme-icarus](https://github.com/ppoffice/hexo-theme-icarus)のソースをhexo-pj-name/themes直下に配置します

- 以下のコマンド一発で入ります
    - hexoディレクトリ直下で実行
    ```
    git clone https://github.com/ppoffice/hexo-theme-icarus.git themes/icarus
    ```

- gitが入っていない方は[hexo-theme-icarus](https://github.com/ppoffice/hexo-theme-icarus)の"clone or download"よりLocalにダウンロードしてthemes配下に展開してください
    - その際にフォルダ名を"icarus"に変更することを忘れないでください
    

### 3. 設定(_config.yml)を変更
- _config.ymlをthemeにicarusを利用するように改修
    - HEXOディレクトリ直下にあります

```
## Themes: https://hexo.io/themes/
theme: icarus
```

- icarusを適用
    - 以下を実行します
    ```
    hexo s
    ```
    - 出力
        - icarusが出てきます
        ```
        j-blog> hexo s
        INFO  =======================================
         ██╗ ██████╗ █████╗ ██████╗ ██╗   ██╗███████╗
         ██║██╔════╝██╔══██╗██╔══██╗██║   ██║██╔════╝
         ██║██║     ███████║██████╔╝██║   ██║███████╗
         ██║██║     ██╔══██║██╔══██╗██║   ██║╚════██║
         ██║╚██████╗██║  ██║██║  ██║╚██████╔╝███████║
         ╚═╝ ╚═════╝╚═╝  ╚═╝╚═╝  ╚═╝ ╚═════╝ ╚══════╝
        =============================================
        INFO  Checking dependencies
        INFO  Validating the configuration file
        WARN  themes\icarus\_config.yml is not found. We are creating one for you...
        INFO  themes\icarus\_config.yml is created. Please restart Hexo to apply changes.
        ```
- 画面を確認
```
hexo server
```

- 以下のようにテーマの変更を確認

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/76524367-4c460b00-64ad-11ea-876c-2c64cdaaf093.png" height="100%" width=100%">
</div>


- 後はいつも通りgenerateして本番環境へデプロイしましょう
```
hexo generate
```

#### Trouble Shooting
- 本番環境で表示が崩れている場合
    - 前のテーマからgenerateされていたファイルが邪魔をしています（私もハマりました）
        - generateで生成されるファイル群を削除すれば、修正できます

-  cheerioがないというエラーが出た場合
    ```
    ERROR Package cheerio is not installed.
    ERROR Please install the missing dependencies from the root directory of your Hexo site.
    ```
    - 以下を実行
        - Hexoディレクトリ直下で
        ```
        npm install cheerio --save
        ```
    - hexo sを実行できます


## icarus設定Tips
- ハマる場面もあったので各種設定についても解説しておきます

### ウィジェットを変更
- icarus/layout/widget内のファイルを改修することで、左右の表示を変更できます

#### プロフィールを変更
- [icarusのwiki](https://github.com/ppoffice/hexo-theme-icarus/wiki/Theme)を参考に改修します
- 上記には書いてありませんでしたが、改修する設定ファイルは以下になります
    - themes\icarus\_cofig.yml

- 中を確認すると以下のようにprofileの初期設定がされていることが分かります
```
# https://ppoffice.github.io/hexo-theme-icarus/categories/Widgets/
widgets:
    -
        # Widget name
        type: profile
        # Where should the widget be placed, left or right
        position: left
        # Author name to be shown in the profile widget
        author: Your name
        # Title of the author to be shown in the profile widget
        author_title: Your title
        # Author's current location to be shown in the profile widget
        location: Your location
        # Path or URL to the avatar to be shown in the profile widget
        avatar: 
        # Email address for the Gravatar to be shown in the profile widget
        gravatar: 
        # Whether to show avatar image rounded or square
        avatar_rounded: false
        # Path or URL for the follow button
        follow_link: 'https://github.com/ppoffice'
```

- 変更
    - profile画像は以下に格納しましょう
        - \themes\icarus\source\images
            - [おしゅし](https://kai-you.net/word/%E3%81%8A%E3%81%97%E3%82%85%E3%81%97%E3%81%A0%E3%82%88) のフリー素材として公開されている画像を用意しました
- 改修例
```
widgets:
    -
        # Widget name
        type: profile
        # Where should the widget be placed, left or right
        position: left
        # Author name to be shown in the profile widget
        author: J
        # Title of the author to be shown in the profile widget
        author_title: IT Specialist
        # Author's current location to be shown in the profile widget
        location: Tokyo Japan
        # Path or URL to the avatar to be shown in the profile widget
        avatar: images/oshushi.png
        # Email address for the Gravatar to be shown in the profile widget
        gravatar: 
        # Whether to show avatar image rounded or square
        avatar_rounded: true

```

- Profileの変更結果  
  

    <div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/41946222/76530468-1d349700-64b7-11ea-8cae-6ed0407cc359.png" height="50%" width=50%">
    </div>

### 背景の変更

- 以下からできます
https://github.com/highlightjs/highlight.js/tree/master/src/styles

```
# Article display settings
article:
    # Code highlight settings
    highlight:
        # Code highlight themes
        # https://github.com/highlightjs/highlight.js/tree/master/src/styles
        theme: atom-one-light
```




### 目次を自動作成するプラグインの導入
- hexo-tocをインストール
```
$ npm install hexo-toc --save
```
- 目次を入れたい箇所に下記を追記すればOK。

```
<!-- toc -->
```

オプションとしてthemes/icarus/config.ymlに下記を追加します。

```
toc:
  maxdepth: 3
  class: toc
  slugify: transliteration
  decodeEntities: false
  anchor:
    position: after
    symbol: '#'
    style: header-anchor
```
- 実際に記事に入れて見ましょう
```
---
title: Hexoでサイトマップを自動生成 ~ Google Search Consoleへ登録
thumbnails: /gallery/sitemap_thumnail.png
date: 2020-03-10 23:23:23
toc: true
tags:
- Hexo
- Sitemap
- google search console
---
<!-- toc -->
## 概要
```

- 画面を確認すると以下のように反映されていました。

    <div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/41946222/76619886-46186300-656f-11ea-979d-878690e12d0c.png" height="100%" width="100%">
    </div>