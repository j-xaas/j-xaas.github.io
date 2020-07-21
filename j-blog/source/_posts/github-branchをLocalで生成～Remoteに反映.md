---
title: '[Github入門] branchをLocalで生成～Remoteに反映'
date: 2020-06-10 17:29:34
categories:
- Tool Tips
- Github
tags: 
- Github.com
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/84248013-d00ea280-ab43-11ea-8ce0-6a161632c3ea.png

---

<!-- toc -->

![Github Flow](https://user-images.githubusercontent.com/41946222/84247849-950c6f00-ab43-11ea-81db-a863d49e8688.png)


## 基礎知識
- branch(ブランチ)とは
    - 履歴の流れを分岐して記録していくためのもの
        - 直訳すると枝/分岐
    - なぜ必要か？
        - githubで開発する際には、リポジトリの中のソースコードを弄っていくわけですが、複数のメンバがいる場合には、同時進行になります。そこで、各メンバ毎にbranchという単位で作業場所を枝分かれさせる必要があります
    - 分岐したbranchの中身は後で合流(merge)させることができます
    - 開発メンバ毎にbranchを作成するのが一般的です

- 複数人での分散開発のざっくりイメージ
    - メンバ毎のbranchで開発
    - 担当範囲の開発完了後にレビュー(Pull Request)を経て、各branchで編集した内容を合わせる(merge)
    - 各メンバの編集内容をmergeした完成版をmaster branchに置く

## 手順
- branchの生成
```
git branch <branch-name>
```
- branchが作成されたことを確認
  - 下記のコマンドでbranch一覧を確認可能
  ```
  git branch -a
  ```
  - *がついているのがカレントブランチ(今いるbranch)
  - VSCodeの場合赤字で表示されるbranch(remotes/origin/...)がremote上(Github上)のbranch
    - 先ほど作成したbranchはLocalにはあるが、remoteにはまだ存在しないことが分かる

- branch間の移動
```
git checkout <branch-name>
```

- remoteにLocalのbranchを反映
```
git push origin <branch-name>
```

- remote側にもbranchができたか確認
  - 赤字で"remotes/origin/branch-name"が表示されればOK
```
git branch -a
```

- 以降の作業はgit checkoutでbranchを移動しながら行います
- branch間の変更内容を合わせる際には以下の学習も必要です
    - git mergeやgit rebase等のコマンド
    - Pull Request
    - そのうちまとめます

### Trouble Shooting
- 誤って生成したbranchの削除
```
git branch -d <branchname>
```
