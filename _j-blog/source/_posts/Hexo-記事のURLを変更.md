---
title: '[Hexo] 記事のURLを変更'
date: 2020-06-13 15:36:17
category:
- WEB Page Dev
- Hexo
tags:
- Hexo
toc: true
thumbnail: https://media.github-enterprise-9911a.paas1.nec-cloud.com/user/11/files/dd1dd280-ad8b-11ea-8807-e89852f3b1cc
---

<!-- toc -->

- 変更方法
    - Hexoディレクトリ直下の設定ファイル(_config.yml)のpermalinkを変更するだけです

- デフォルト
    - 年月日が入って長ったらしい
    ```
    permalink: :year/:month/:day/:title/ 
    ```
- ここを変更します
    - site-url/:title
    ```
    permalink: :title/ 
    ```
    - site-url/:category/:title/
    ```
    permalink: :category/:title/
    ```

- URLの変化を確認
    ```
    hexo server
    ```
    - 本番環境ではlocalhostがsite-urlに変わります
    ![image](https://media.github-enterprise-9911a.paas1.nec-cloud.com/user/11/files/d8f0b580-ad89-11ea-9383-719c966598e3)

- URLはWEBページのSEOに影響するので余計な箇所は削りましょう

- URLを変更したらリダイレクトの設定も一緒に行いましょう。Google検索でも古い方のURLが出てしまうためです。
    - [Hexo URLのリダイレクト (hexo-generator-alias/hexo-generator-redirect](/Hexo-URLのリダイレクト-hexo-generator-alias-hexo-generator-redirect/)