---
title: icarus(Hexo Theme) Tips Menuの編集
date: 2020-03-17 00:54:02
categories:
- WEB Page Dev
- Hexo
tags:
- Hexo
- icarus
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/76772659-9fd59300-67e4-11ea-96da-e010eb0ac211.png
alias: /2020/03/17/icarus-Hexo-Theme-Tips-Menuの編集/
---


こんにちは。今回はHexoのthemeにicarusを採用した際のTipsを解説します。

<!-- toc -->

- icarusを適用すると以下のようなメニューがヘッダーに表示されます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/76772659-9fd59300-67e4-11ea-96da-e010eb0ac211.png" height="80px" width="650px">
</div>

- ポイント
    - Aboutページは自分で作成する必要がある
    - Menuは追加・削除可能

## Aboutページを追加

### 1. Hexoの通常の記事として生成
- About-This-Blog.mdを生成
```
ng new 'About-This-Blog'
```
- source/_posts内に生成されたファイルを編集

### 2. _config/ymlを改修

- path: your-blog\themes\icarus>
    - About: 移行を生成した記事のパスに変更
    - Hexoの記事のパスは "/YYYY/MM/DD/Article_name" です

```
# Navigation bar link settings
navbar:
    # Navigation bar menu links
    menu:
        Home: /
        Archives: /archives
        Categories: /categories
        Tags: /tags
        About: /2020/02/04/About-This-Blog/ # /aboutから変更
```

## Menuを追加

- メニューの追加も同様の箇所を改修するだけで簡単にできます
- 以下を追加します
    - ポリシーページ
    - 自身のECサイトへのリンク

- 作業は二点です
    - menuの項目を増やす
    - pathを指定する
        - 外部サイトはURLそのままでOKです

- _config.yml
```
# Navigation bar link settings
navbar:
    # Navigation bar menu links
    menu:
        Home: /
        Archives: /archives
        Categories: /categories
        Tags: /tags
        About: /2020/02/04/About-This-Blog/ #/about
        Policy: /2020/03/17/Policy
        EC: https://sheeps.official.ec/

```

- 確認
```
hexo server
```
- 以下のように反映されます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/76773559-00b19b00-67e6-11ea-993b-8d5ebdabd07b.png" height="60px" width="650px">
</div>


- 外部サイトに飛べるか確認
    - メニューの"EC"を押下
    - 以下のように画面が切り替わりました
        - 私が書いたマスコットキャラ：シープ君のグッズ一覧が出てきます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/76774924-f42e4200-67e7-11ea-9f57-f9c5f28d1e20.png" height="600px" width="500px">
</div>

- icarusは柔軟にメニューを弄れるのでカスタマイズ性が高いですね
    - 自身のWEBページに合わせて自由に編集してみてください
    - 個人で勉強ついでに開発しているWEB APが増えたら一覧ページをメニューに足そうと思います

## おまけ：ロゴの変更
- オリジナルのロゴ
    - 所在：\themes\icarus\source\images
        - logo.svg 

- 1. オリジナルのロゴを作成して同様のディレクトリに格納
    - original_logo.pngを作成して配置
- 2. _config.ymlを編集
    - icarus側の設定ファイルを改修
        - themes\icarus\_config.yml
    - 旧
    ```
    favicon: /images/favicon.svg
    
    # Path or URL to the website's logo to be shown on the left of the navigation bar or footer
    logo: /images/logo.svg
    ```
    - 新
        - faviconをコメントアウトしないと上に被さって上手く表示できません
    ```
    #favicon: /images/favicon.svg

    # Path or URL to the website's logo to be shown on the left of the navigation bar or footer
    logo: /images/original_logo.png
    ```
- これだけです
    - 画面への反映を確認
    ```
    hexo server
    ```

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/78388593-3a730600-761c-11ea-936e-0cc0f4a9ce2e.png" height="60px" width="400px" alt="ロゴ変更後">
</div>

- pngをSVGファイルに変換しておくと見栄えも読み込みも良くなります
    - ↓でぱっと変換できます
    - [PNG SVG 変換 - 画像ファイルをオンラインで変換する](https://www.aconvert.com/jp/image/png-to-svg/)