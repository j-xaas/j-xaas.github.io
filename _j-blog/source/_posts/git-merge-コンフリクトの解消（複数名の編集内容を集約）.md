---
title: git merge & コンフリクトの解消（複数名の編集内容を集約）
date: 2020-04-08 00:58:26
category:
- Tool Tips
- Github
tags:
- Github.com
toc: true
---

Githubで初級者がハマりがちなポイントについてです。  
複数メンバの編集内容をGitで効率的にまとめる手法を解説します。  

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/78698206-f1cd8c80-793c-11ea-8887-0d93b944b50d.png" height="170px" width="400px" alt="git merge image">
</div>

<!-- toc -->

## 基礎知識
### githubの分散開発
- 基本的な進め方
  1. 開発メンバ数のbranchを作成
  2. 別々のbranchを各メンバが編集
  3. 作業完了後にmerge
    - ※本記事で解説する内容

### git mergeとは
- githubのコマンド
- 機能
  - 複数branchのmerge
  - つまり各メンバの編集内容を反映して合わせることができます

- メリット
  - 1. 差分の可視化
    - githubであれば、編集箇所がマーカーで表示されます
      - 編集日時・編集者も一目で判別可能
    - 目視でソースの変化を比較するにはすさまじい労力を伴います
  - 2. 変更の反映・集約の自動化
    - わざわざコピペする手間を削減可能
    - 複数名が別々の編集をしたファイル
        - 開くと各メンバの変更箇所が並んで表示されます
        - ボタン一つでどれを採用するか決定可能です
      - ※詳細は手順内で説明
  
mergeの楽さと過去の改修内容を時系列で遡れることが、gitを活用して分散開発を行う主な理由です

## 手順
- 他人が編集していた別branchの変更内容を、自分のLocal端末上でmergeするまでを書きます
### merge
- 1. リモートから別branchを取得
  - Localに未登録であれば実行(git branch -aで別branchが出ない場合)
  ```
  git fetch
  ```
- 2. 追加されていることを確認
```
git branch -a
```
- 3. branchへ移動して内容を確認
```
git checkout <branch-name>
```
- 4. 元のbranchに戻る
```
git checkout master
```
- ※Commitが未実行のファイルが無いか確認
  - そのままmergeすると変更の取り込みが漏れてしまうため
  - 以下のように出ればOK
```
git status

On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

- 5. merge
```
git merge <branch-name>
```
- 別々のファイルを改修していた場合は、以上で完了です
    - merge実行時に"CONFLICT"が出力されれば、次の処理も必要です

### コンフリクトの解消
- コンフリクトとは
  - 編集内容の衝突のこと
  - それぞれのbranchで同じファイルを編集していた際に発生する
  - どちらの編集内容を採用するか決める必要がある
  ```
  CONFLICT (content): Merge conflict in <conflict-file>
  ```

- コンフリクトの解消方法
  - 1. 上記のコンフリクトが発生した各ファイルを開く
  - VS Codeの場合
    - コンフリクト箇所をマーカー表示
      - 緑のマーカー
        - 以下から始まる箇所
        ```
        <<<<<<< HEAD (Current Change)
        ```
        　
        - 現在のbranch側の編集内容
      - 青マーカー
        - 以下から始まる箇所
        ```
        >>>>>>> branch-name (Incoming Change)
        ```

        - 現在のbranchにmergeしようとしているbranch側の編集内容
    - マーカー上部に表示されるメニュー
      - Accept Current Change
      - Accept Incoming Change
      - Accept Both Changes
      - Comppare

  - 2. マーカー表示箇所のどちらを採用するか決定
    - 方法は二つ
      - マーカー上部のメニューから選択
        - 例：Accept Current Changeを選択
            - Current branchの内容(緑マーカー)が採用され、Incoming branch(青マーカー)の内容が消去されます
            - つまり、差分を一目で確認して、一押しで取捨選択可能です
      - 要らない方のマーキング箇所を手動で消去

- ファイルの編集後
    - addまでしてコンフリクトが解消された状態になります
    ```
    git add <conflict-file>
    ```

- 確認
  - 赤字でコンフリクト中のファイルが表示されなければOK
  ```
  git status
  ```
- Remoteに登録
```
git commit
git push
```
  
　今回の解説は以上です。pull, commit, push等のgitの基礎と今回の内容を覚えれば、一先ずチームで分散開発を始めることができると思います。ここで手間取ると初動からPJが遅延するので、未修得のメンバがいれば放置せずに教えてあげましょう。細かい機能は使わないものも多いので、走りながら必要に応じて覚えていけば大丈夫です。