---
title: HexoブログのAMP化【完全版】
date: 2020-03-15 18:00:22
toc: true
category:
- WEB Page Dev
- Hexo
tags:
- Hexo 
- AMP
- hexo-generator-amp
alias: /2020/3/15/HexoブログのAMP化/
thumbnail: https://user-images.githubusercontent.com/41946222/84779139-fe4a2180-b01e-11ea-8a05-d10dfd35891f.png
---

　こんにちは。今回はHexoで生成したWEBページのAMP化の手法について解説します。簡単にいうとWEBページを高速表示可能な形式にします。同様の記事も数件見つけたのですが、所々でハマったので、完全版としてまとめました。

<!-- toc -->

## 基礎知識
### AMP(Accelerated Mobile Pages)とは？
- [AMP HTML](https://amp.dev/documentation/guides-and-tutorials/learn/spec/amphtml/?referrer=ampproject.org&utm_source=search_console&utm_campaign=sc-amp-test)
    - モバイルでのコンテンツ表示を高速化させるための手法
        - GoogleとTwitterが協同で立ち上げたプロジェクトです
        - ページ読み込みに3秒以上かかる場合、53%のユーザーが離脱してしまうと言われています
        - 検索結果で並んでいるサイトに、稲妻マークが小さくついているものを見つけたことはありませんか？
            - 実はあのマークはAMPサイトであるサインです
    - AMP化するには規格に則った記法でサイトを構築する必要があります
        - 自力での対応は難しいので、自動生成する手法を今回解説します

### hexo-generator-ampとは
- [hexo-generator-amp](https://github.com/tea3/hexo-generator-amp)
    - Google AMP (Accelerated Mobile Pages) を自動で生成してくれるプラグイン
    - 各記事のHTMLに対して、AMP HTMLを生成
        - hexo generate実行時に通常のHTMLとAMP版のHTMLの双方ができます

## 導入手順

### 1. hexo-generator-ampのinstall

- Hexoディレクトリ直下で実行
```
npm install hexo-generator-amp --save
```

### 2. headタグにAMP HTMLのパスを指定

- head.ejs
    - 自動生成するHTMLのheaderの設定を規定するファイルです
    - ファイルの所在は以下
        ```
        \themes\theme-name\layout\
        ```
        - 私が利用しているテーマ:icarusの場合は一階層深い所にありました
        ```
        \themes\icarus\layout\common\
        ```

- 以下を追記
```
<% if (is_post() && config.generator_amp){ %>
    <link rel="amphtml" href="<%= config.url %>/<%= page.path %>/amp/index.html">
<% } %>
```

- WEBページを作成する中で、度々headerにscriptを追加するシーンがあると思われます
    - 基本的にhead.elsに同様に追記することで目的を達成可能です。覚えておきましょう

### 3. ./_config.ymlの改修
- _config.ymlにPluginの設定を追加します
    - theme配下ではなくHexoディレクトリ直下のファイルです

- 以下を追記
    - Gitの公式ページの説明はpathの記法が少し間違っていました
```
# hexo-generator-amp

generator_amp:
  templateDir: amp-template
  assetDistDir: amp-dist
  logo:
    path: sample/sample-logo.png
    width: 600
    height: 60
  substituteTitleImage:
    path: sample/sample-substituteTitleImage.png
    width: 1024
    height: 800
  warningLog: false
```

- hexo serverで確認


#### Trouble Shooting
- 以下のエラーが出た場合

```
[00:44:36.025] [hexo-generator-amp] hexo-generator-amp's template has been copied to
the your project.
Please check the following file.
-> j-blog\amp-template\sample
[00:44:36.040] [hexo-generator-amp] error:  Not found the hexo-generator-amp's asset
files. If you do not have the file please download the sample file from the following site. ( see: https://github.com/tea3/hexo-generator-amp/tree/master/template )
Please check the following file.
-> j-blog\amp-template\sample-logo.png
```

- 上記はpathの指定が間違っている
    - sample配下にsample-logo.png等が格納されているため、sample/を頭に付けましょう

### 4. Markdown記法の修正(画像)

- hexo server実行時にAMPの書式チェックがプラグインによって実行されます
- 恐らく以下のErrorが大量に出る人が多いと思われます
  - markdownの画像のURL指定の表記がAMPでは通用しない為です
  ```
  This plugin can not acquire the width and height of such url images.
  Please change the URL to HTTP or HTTPS, or add height and width. 
  ```
- エラーメッセージについて解説
  - このプラグインは、URL画像の幅と高さを取得できない
  - URLをHTTPまたはHTTPSに変更するか、高さと幅を追加してください

- どういうこと？
  - AMPの規格では画像のheightとwidthを設定する必要があります。
  - そのため、各記事のURL指定の画像に対してエラーが出ています


- とりあえず以下のErrorが出ている記事の画像を修正してみます
```
-> _posts/About-This-Blog.md
[18:39:10.984] [hexo-generator-amp] error:  This plugin can not acquire the width and height of such url images.
Please change the URL to HTTP or HTTPS, or add height and width. 
img path: https://user-images.githubusercontent.com/41946222/73755094-dd3c0f00-47a8-11ea-9ec5-e1e537559054.png
Please check the following file.
-> _posts/About-This-Blog.md
[18:39:10.985] [hexo-generator-amp] error:  Error: connect ETIMEDOUT 151.101.108.133:443
This plugin checks whether the image URL exists. 
img path: https://user-images.githubusercontent.com/41946222/74917091-5030c100-540a-11ea-9060-2d05fc6c6c23.png
Please check the following file.
```

- 現在のmarkdown記法
  - heightとwidthを定められない
  ```
  ![image_title](image_url)
  ```

- 回避策
    - 以下のようにHTMLの記法で修正
    ```
    <div style="text-align:center;">
    <img src="image_url" height="500px" width="500px">
    </div>
    ```

- 各記事のURL指定の画像に対して同様の修正を実施します
  - 割と大変なので、記事が少なめの段階で対処した方がいいです

- 確認
  - 以下を実行
  ```
  hexo server
  ```
  - エラーの解消を確認

- 起動したAPをブラウザで確認
    - 以下のように記事を開いている状態で、URLの後ろにampを付ければ生成されたAMP HTMLを確認可能です
    ```
    http://localhost:4000/article_name/amp
    ```
    - 各記事で確認しましょう


<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/76687185-ee5c2380-6664-11ea-8e75-f699f17a498a.png" height="300px" width="600px">
</div>


#### Trouble Shooting
- AMP画像が表示されない
    - heightとwidthの%指定はダメです
    - pxでかっちり定めなければAMPでは表示されません


### 5. WEB上でのチェック

- 本番環境にデプロイ⇒WEBから確認
    - 以下のように各記事のURLに/ampを足すとAMP HTML版のページの表示を確認可能です
        - AMP版と通常版の両方のページをWEBから確認出来るようになっている状態です


#### AMPテスト
- [GoogleのAMPテスト](https://search.google.com/test/amp)
    - 指定したURLがAMPに対応しているかチェック可能です
        - AMP版のURLを指定してください


<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/76698001-1e490c80-66e1-11ea-90d2-12f01b0c5b78.png" height="200px" width="600px">
</div>



- 以上でHexoで生成したWEBページのAMP化は完了です
- 各種設定やTipsについては別記事で解説します