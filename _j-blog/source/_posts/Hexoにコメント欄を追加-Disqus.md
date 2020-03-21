---
title: Hexoにコメント欄を追加(Disqus)
date: 2020-03-21 02:42:04
category:
- WEB Page Dev
- Hexo
tags: 
- Hexo
- Disqus
- icarus
toc: true
---

こんにちは。今回はHexoで生成したWEBサイトにコメント欄を追加する手法について解説します。他の説明記事がどれも間違っていて（恐らくバージョンの問題）だいぶハマったのでまとめておきます。

<!--toc-->

## 1. Disqusとは
- [Disqus](https://help.disqus.com/en/articles/1717053-what-is-disqus)
    - コメント機能を拡張するサービス
    - 様々なPlatformに対してプラグインを提供
        - Hexoの[theme](https://hexo.io/themes/index.html)はDisqusの活用を前提としたものが多く、簡単に導入可能です
        - ※ themeに対して設定する為、選定後の対処をお勧めします
            - 関連記事：[Hexoテーマ(theme)変更: icarus](https://j-xaas.github.io/2020/03/13/Hexo%E3%83%86%E3%83%BC%E3%83%9E-theme-%E5%A4%89%E6%9B%B4-icarus/)
    - 無料で利用を開始可能です

## 2. 導入手順
### 2.1. Disqusに登録

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77195868-adad5000-6b25-11ea-8c25-6a432ad7edb3.png" height="400px" width="500px">
</div>

- [Disqus](https://disqus.com/profile/signup/)のSignupを実施
    - Twitter, FB, Googleアカウントの何れかを利用可能です
    - 私はGoogleアカウントを利用しました
    - 認証後にSignupを押下

- "I want to install Disqus on my install"を選択

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77196716-3aa4d900-6b27-11ea-984d-5d60050732ee.png" height="300px" width="500px">
</div>

- Create a new Site
    - 以下の欄を入力
        - Website Name
        - Category
    - CreateSiteを選択
<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77197201-1e556c00-6b28-11ea-9f55-d0d1807250c7.png" height="400px" width="500px">
</div>

- 以下の順に選択
    - "Got it. Let’s get started!"
    - "I don't see my platform listed, install manually with Universal Code"

- 以上でDisqusのサイト用ページが作成されました

### 2.2. Disqus設定

- 以下が設定ページです

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77198283-0a126e80-6b2a-11ea-9783-437f2ac38657.png" height="400px" width="600px">
</div>

- Installation
    - Comment機能非対応のthemeの場合はこちらのスクリプトをサイトに貼り付けましょう
        - Hexoデフォルトのlandscapeであれば必要
            - 該当のejsとプレフィックスにつくファイルを編集してください
            - ファイル所在(landscapeの例)
            ```
            Your-blog\theme\Your-Theme\layout\_partial
            ```
    - 本サイトに採用しているicarusであれば、この工程はスルーしてOK
- "Configure"を押下
- URLを入力
- "Complete Setup"を押下

- 上部のSettingタブを選択
- 左のリストのGeneralを選択
    - "shortname"の値を控える
        - 後でhexoに設定します
    - 各項目に入力して"Save"を押下

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77200711-7db67a80-6b2e-11ea-9610-d8f568082bce.PNG" height="600px" width="600px">
</div>

- 以上でDisqus側の設置は完了です
    - 細かく設定可能なので

### 2.3. Hexoに設定

- shortnameをtheme側の_config.ymlに記述
    - 元の状態
    ```
    comment:
    # Name of the comment plugin
    type: 
    ```
- 改修
    - typeにdisqusを設定
    - shortnameにdisqusで定義された値を設定
    ```
    comment:
        type: disqus
        shortname: <from_disqus>
    ```

- Comment機能があるthemeの場合は以上でOK(icarus等)
    - 以下のディレクトリがあるかで判断してください
        - Your-Blog\themes\Your-Theme\layout\comment

#### Trouble Shooting
- ※Comment機能がないthemeの場合(デフォルトのLandscape等)の対応も載せておきます
    - 1. Hexo側の設定ファイルの改修も必要です
        - Your-Blog/_config.yml
            ```
            # disqus
            disqus_shortname: https-j-xaas-github-io
            ```
    - 2. Disqus/Setting/Installationで表示されるScriptを該当するejsファイルにコピペしてください
        - ejsファイルとは？
            - Tour-Blog/theme/Your-Theme/layoutの配下にあります
            - 例えばheader.ejsはHexoで各記事をgenerateする際に共通の設定を与えてくれるファイルです
            - つまり、headerに共通して与えたいスクリプトがあれば、ここにコピペするだけでOKです

- ※旧バージョンの場合
    - 他の記事はどれも以下のように設定するよう書かれていましたがエラーになります
    ```
    comment:
        disqus: [shortname]
        duoshuo: [shortname]
    ```
    - comment機能の設定ファイルを探して確認
        - 以下にありました
            - j-blog\themes\icarus\layout\comment\disqus.ejs
        - ”comment.shortname”とあることから、設定ファイルのcommentのshortnameの値を参照していることが分かります
            - themeによって特殊な設定が必要なケースも予想されるため、同じように確認してみてください
    ```
    (function() {
        var d = document, s = d.createElement('script');  
        s.src = '//' + '<%= get_config('comment.shortname') %>' + '.disqus.com/embed.js';
        s.setAttribute('data-timestamp', +new Date());
        (d.head || d.body).appendChild(s);
    })();
    ```

## 3. 利用テスト
### 3.1. Localでチェック
- 画面への反映をチェックしましょう
```
hexo server
```
- 記事の下部にコメント欄を確認できます
    - 仕様はDisqusのSettingで細かく変更可能です

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77222316-678fd500-6b95-11ea-8d26-318bd1970bc4.png" height="500px" width="600px" alt="comment_example">
</div>

- コメントの投稿は本番環境でないとエラーになります

- generate
```
hexo generate
```
- 本番環境へデプロイ

### 3.2. 投稿
- WEBから実際にコメントを投稿してみましょう
- ”ログイン”よりGoogleアカウントでログイン
    - Disqusのアカウント登録に利用したアカウントを用いれば、管理者のコメントとして認識されます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77222434-64491900-6b96-11ea-966d-499ff3bfd4ac.png" height="200px" width="500px" alt="comment_example">
</div>

- コメントを投稿してみましょう
    - 投稿者名の横に”管理者”が表示されます
        - Googleアカウントが実名なので、Twitterで登録しておけばよかったかも...

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77222841-6d3be980-6b9a-11ea-8863-11b3f7af74c8.png" height="200px" width="500px" alt="comment_example">
</div>

### 3.3. アカウント登録

- 記事の訪問者をイメージして、Twitterアカウントで試してみます
    - 事前に管理者アカウントから”ログアウト”しておきます
- 投稿欄の下のTwitterマークを選択
    - 認証画面に飛びます


- アカウント設定
    - 各入力項目を埋めて"Sign Up"を押下
<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77223016-0d464280-6b9c-11ea-9eb7-8908b20b49a3.PNG" height="300px" width="400px" alt="Disqus_Twitter認証画面">
</div>

- すると、以下のように規約が表示されます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77223009-f56ebe80-6b9b-11ea-8f60-c66a375029e2.PNG" height="300px" width="400px" alt="disqus_allert">
</div>


- 設定したアドレスに以下のmailが届きます
    - 青字を押下

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77223011-03244400-6b9c-11ea-8bf3-4676ca9b7960.PNG" height="300px" width="400px" alt="disqus_mail">
</div>

- 認証が完了しました

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77223019-1cc58b80-6b9c-11ea-956d-ce5fe2f311bd.PNG" height="250px" width="400px" alt="email_verified">
</div>

- WEB上でログイン
- コメントを投稿
    - アイコン画像はTwitterのものがそのまま反映されるようです

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77223048-6a41f880-6b9c-11ea-9aae-38987fd1bf52.PNG" height="250px" width="500px" alt="twitter_comment">
</div>


- 以上でDisqusの解説は終了です。手軽なので是非導入してみましょう。