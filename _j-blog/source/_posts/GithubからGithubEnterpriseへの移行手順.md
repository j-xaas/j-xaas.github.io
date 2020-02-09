---
title: Github.comからGithubEnterpriseへの移行手順
date: 2020-02-09 22:52:47
tags: 
- Github.com
- GithubEnterprise
- GithubPages
---

## 概要
- リポジトリ移行(Github⇒GHE)の備忘録です
- 想定する状況
    - Organizationごと中身をまるまる移行したいパターン
    - Commitログ等は特に移行する必要無く、シンプルな手法で手早く終わらせたい
- 事前調査 
    - organization単位の以降はできない
    - リポジトリ単位の移行は可能
    - GHE内に同じ名称のOrganizationやリポジトリを作成可能
        - ★一意性のチェックがGithub側とは隔絶されている

## 手順

### 1. GHEでOrganizationを作成
- Githubと同様の名称で問題無し
    - URLは前半が企業ごとのものに変わる
    - SiteAdmin権限が必要
        - GUIで操作

### 2. GHEでリポジトリを作成
- Github.com側と同様の名称で問題無し
    - 空のリポジトリを一通り作成
        - GUIで操作

### 3. Local端末で移行対象のリポジトリのデータを退避
- 各リポジトリの中身をデスクトップ等へコピー
- Gitリポジトリの管理場所:Dドライブから削除
    - この後、GHEから同様の名称のリポジトリをCloneするため
    - Dドライブでlsコマンドを打って削除されたことを確認

### 4. GHEからLocalへClone
- 各リポジトリ（空）を端末へClone
    - URLはリポジトリのページの"Clone or download"タブより確認
```
git clone <リポジトリのURL>
```

### 5. Cloneした空リポジトリに退避データをコピー
- GHEからCloneした空リポジトリへ、退避場所からコピー

### 6. GHEのRemoteにデータを登録
- 各リポジトリで以下の作業を実施する
- 変更内容を保存
```
git add *
git commit "Repository Migration"
```
- Remoteへ送信
```
git push
```
#### Github Pagesが含まれている場合
- _config.ymlに設定する公開URLを変更する必要がある
- GHEにおけるGithub PagesのURLは以下のようになる
```
https://pages.<gheのURL>/<Organization名 or ユーザ名>/<リポジトリ名>.io
```

### 7. GHEで確認
- GHEの問題無くデータが移行できていることを確認
- Github pagesが含まれていたため、ブラウザで表示されるようにする

- リポジトリのSettingsより、以下のようにGithub pagesを有効にしてSave

![githubpages_setting](https://media.github-enterprise-16ae0.paas1.nec-cloud.com/user/16/files/e0a2e680-49b2-11ea-802c-f247b17a1e04)

- GHEにおけるGithub PagesのURLは以下
```
https://pages.<gheのURL>/<Organization名 or ユーザ名>/<リポジトリ名>.io
```

### 8. Github.comより移行元を削除
- リポジトリを削除
- Organizationを削除
    - 管理者権限が必要
