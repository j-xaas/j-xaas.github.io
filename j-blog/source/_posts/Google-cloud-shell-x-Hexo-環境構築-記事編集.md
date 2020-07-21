---
title: '[Google cloud shell x Hexo] 環境構築&記事編集'
date: 2020-07-22 01:01:48
categories:
- Tool Tips
- Google Cloud Shell
tags: 
- Google Cloud Shell
- Hexo
- Github.com
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/88082269-868e8a00-cbbc-11ea-9f4c-b619ff55a640.png
---

Google Cloud Shell上にHexo環境を構築した際のメモ

<!-- toc -->

## 前提
- 想定するシチュエーション
    - 既に他環境でHexoでWEBページを生成してgitにアップ済み
- Hexoやgithub, cloud shell自体の説明は他記事にまとめてあるので、こちらで補完してください
    - [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)
    - [Github入門 ～入社初日の完全な素人でも分かる優しい解説～](/Github入門-入社初日の完全な素人向けの優しい説明/)
    - [Google Cloud Shellの設定手順](https://j-xaas.github.io/iPad-ProでWEB-AP開発/#google-cloud-shelの設定手順)

## git連携 & リポジトリのclone
- git configでユーザの情報を登録してから、cloneでデータを持ってくる
    - cloud shellにはgit cliが元から入っているので楽
```
git config --global user.name "user-name"
git config --global user.email "e-mail"
git clone your repository
```
- hexoをinstall
    - cloud shellにはNode.jsが元から入っているのでHexoを入れるだけ
```
cd your-repository/your-blog
npm install -g hexo
npm install hexo --save
```

## 記事のプレビュー表示における注意点
- Hexo serverでlocalhostを起動する際に、-pでポートを8080に指定する必要があります
    - デフォの4000はcloud shellでは見れないため
```
hexo server -p 8080
```

- previewは右上のアイコンから開けます
![cloud shell open preview](https://user-images.githubusercontent.com/68212997/88078543-8cce3780-cbb7-11ea-8ad8-eb7ac786d301.png)

- 以上でブラウザでWEBページの画面を確認可能です

## 関連記事
### Google Cloud Shell
- [iPad ProでWEB AP開発 & RPA](/iPad-ProでWEB-AP開発/)
- [Google Cloud Shellの設定手順](https://j-xaas.github.io/iPad-ProでWEB-AP開発/#google-cloud-shelの設定手順)

### Hexo
- [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)
- [Hexoテーマ(theme)変更: icarus](/Hexoテーマ-theme-変更-icarus/)
- [Hexoにコメント欄を追加(Disqus)](/Hexoにコメント欄を追加-Disqus/)
- [icarus(Hexo Theme) Tips Menuの編集](/icarus-Hexo-Theme-Tips-Menuの編集/)
- [Hexo 共有機能のPlugin Add Thisで動的なシェアボタンを追加](/Hexo-share機能の追加-Add-This/)
- [[Hexo] 記事のURLを変更](/Hexo-記事のURLを変更/)
- [Hexo URLのリダイレクトhexo-generator-alias/hexo-generator-redirect](/Hexo-URLのリダイレクト-hexo-generator-alias-hexo-generator-redirect/)
- [Hexoでサイトマップを自動生成 ~ Google Search Consoleへ登録](/Hexoでサイトマップを自動生成-Google-Search-Consoleへ登録/)
- [HexoブログのAMP化【完全版】](/HexoブログのAMP化/)

### Github
- [Github入門 ～入社初日の完全な素人でも分かる優しい解説～](/Github入門-入社初日の完全な素人向けの優しい説明/)
- [[Github入門] branchをLocalで生成～Remoteに反映](/github-branchをLocalで生成～Remoteに反映/)
- [git merge & コンフリクトの解消（複数名の編集内容を集約）](/git-merge-コンフリクトの解消（複数名の編集内容を集約）/)
- [Github x Teams Webhook/Notificationによる連携方法](/Github-x-Teams-Webhook-Notificationによる連携方法/)
- [Github.comからGithubEnterpriseへの移行手順](/GithubからGithubEnterpriseへの移行手順/)
