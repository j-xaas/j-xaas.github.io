---
title: git複数アカウントの使い分け設定
date: 2020-07-21 22:10:39
categories:
- Tool Tips
- Github
tags: 
- Github.com
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/84248013-d00ea280-ab43-11ea-8ce0-6a161632c3ea.png
---

社内開発/他社との共同開発/個人開発等でgitのアカウントを使い分けたい人向けのメモ

<!-- toc -->

## gitアカウント情報の設定方法
端末上のLocalリポジトリとgit上のRemoteリポジトリ間でデータをやり取りするには以下の情報が必要です

- gitの認証に必要な情報
    - gitのユーザ名
    - 登録したメールアドレス

上記の情報の設定方法は以下の二種があり、利用頻度によって使い分けるのが一般的です

- git認証情報の設定方法
    - グローバル変数として設定
        - メインのアカウントを登録
        - リポジトリ毎の設定が無い場合には、ここに設定した情報が使われる
    - リポジトリ毎の設定
        - サブのアカウントを登録
        - 設定したリポジトリ内でのみ影響

## メインアカウントの設定

### global(~/.gitconfig)に設定
業務で良く使うアカウントは、globalに設定する

- git config --globalで設定
```
$ git config --global user.name "account-name"
$ git config --global user.email "account-mailaddress"
```
- 確認
```
$ cat ~/.gitconfig

[user]
    name = account-name
    email = account-mailaddress
```

## サブアカウントの設定
サブのアカウントは、リポジトリ単位でアカウント情報を設定する

### リポジトリ内の./.git/configに設定

- git config --localで設定
```
cd repository
git config --local user.name "subaccount-name"
git config --local user.email "subaccount-mailaddress"
```
- ./.git/configを確認
```
repository> cat ./.git/config

[user]
    name = subaccount-name
    email = subaccount-mailaddress

```

- git logで確認
  - それぞれのアカウントでgit commitsした後にgit logで反映されているか確認可能
  - サブアカウントを登録したリポジトリ以外はメインアカウントの情報が出るはず
```
git log

commit XXXXXXXXXXXXX
Author: subaccount-name <subaccount-mailaddress>
Date:   Xxx Xxx 00 00:00:00 2020 +0000
```

## 参考
### 関連記事
- [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)
- [Github入門 ～入社初日の完全な素人でも分かる優しい解説～](/Github入門-入社初日の完全な素人向けの優しい説明/)
- [[Github入門] branchをLocalで生成～Remoteに反映](/github-branchをLocalで生成～Remoteに反映/)
- [git merge & コンフリクトの解消（複数名の編集内容を集約）](/git-merge-コンフリクトの解消（複数名の編集内容を集約）/)
- [Github x Teams Webhook/Notificationによる連携方法](/Github-x-Teams-Webhook-Notificationによる連携方法/)
- [Github.comからGithubEnterpriseへの移行手順](/GithubからGithubEnterpriseへの移行手順/)

### その他
- [同一環境上で複数のGitアカウントを切り替えるための1アイデア](https://msyksphinz.hatenablog.com/entry/2018/10/23/040000)
