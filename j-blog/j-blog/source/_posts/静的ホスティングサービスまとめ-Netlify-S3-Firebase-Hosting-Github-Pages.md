---
title: 静的ホスティングサービスまとめ(Netlify/S3/Firebase Hosting/Github Pages)
date: 2020-09-16 21:34:23
category:
- Serverless Application Dev
- Hosting Service
tags: 
- S3
- Netlify
- 'Firebase Hosting'
- GithubPages
thumbnail: https://user-images.githubusercontent.com/68212997/93338676-93022c80-f865-11ea-8f30-4bd5989e7939.png
toc: true
---

静的コンテンツのホスティングサービスについてのメモ

## 主流のHostingサービスと利用方法の解説(2020時点)
以下に一通りまとめました。

- Amazon S3
    - [AWS S3 x Angular(静的WEBサイトホスティングでSPAを公開/ng build/公開範囲の限定/CI/CD化)](/AWS-S3-x-Angular-静的WEBサイトホスティングでSPAを公開-公開範囲の限定/)
- Firebase Hosting
    - [Firebase Hosting完全版(Angularで開発したSPAを無料で公開～ダッシュボードで費用管理)](/Firebase-Hosting完全版-Angularで開発したSPAを無料で公開～ダッシュボードで費用管理/)
- Netlify
    - [Netlifyで公開](/ar-js-x-a-frame-WebAR入門/#apを公開)
- Github Pages
    - [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)


## 使い分け
詳細は時間がある時に追記します。

- 試験公開や個人開発者向け
    - 以下は手軽 ＆ 無料だが、アクセス過多ですぐ落ちる
        - Netlify
        - Github Pages
- 本番環境向け
    - 利用するクラウドによって使い分ける
        - AWS S3
        - Firebase Hosting

### Netlify
- 最も簡易的。ブラウザにアップしたいフォルダをドラッグするだけでOK
- 以下の記事で詳しく解説しています
    - [Netlifyで公開](/ar-js-x-a-frame-WebAR入門/#apを公開)

### Github Pages
- GithubにアップしているソースをそのままHostingして公開できるGitの機能
- 以下の記事で詳しく解説しています
    - [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)

### Amazon S3
- 本番環境で最も使われているであろうオンラインストレージ
- セキュリティや運用監視についてもAWSの知識があれば、かなり細かく設定・自動化可能
- 以下の記事で詳しく解説しています
    - [AWS S3 x Angular(静的WEBサイトホスティングでSPAを公開/ng build/公開範囲の限定/CI/CD化)](/AWS-S3-x-Angular-静的WEBサイトホスティングでSPAを公開-公開範囲の限定/)

### Firebase Hosting
- Firebase(GCP)を利用する場合はこれ
- コマンド一発でアップ可能
- 強みはGoogle Analyticsとの連携が初めからされているので、パフォーマンスをチェックしやすいこと

- 以下の記事で詳しく解説しています
    - [Firebase Hosting完全版(Angularで開発したSPAを無料で公開～ダッシュボードで費用管理)](/Firebase-Hosting完全版-Angularで開発したSPAを無料で公開～ダッシュボードで費用管理/)

※時間がある時に、それぞれの詳細と使い分けについて追記します。

## 関連記事
- Netlify
    - [Netlifyで公開](/ar-js-x-a-frame-WebAR入門/#apを公開)
- Github Pages
    - [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)
- AWS S3
    - [AWS S3 x Angular(静的WEBサイトホスティングでSPAを公開/ng build/公開範囲の限定/CI/CD化)](/AWS-S3-x-Angular-静的WEBサイトホスティングでSPAを公開-公開範囲の限定/)
- Firebase Hosting
    - [Firebase Hosting完全版(Angularで開発したSPAを無料で公開～ダッシュボードで費用管理)](/Firebase-Hosting完全版-Angularで開発したSPAを無料で公開～ダッシュボードで費用管理/)