---
title: Github Pages x Hexo運用をGithub Actionsで自動化
date: 2020-06-18 00:23:44
ategories:
- Tool Tips
- CI/CD(Github Actions)
tags: 
- Github Actions
- Github
- CI/CD
- Hexo
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/84656875-59f8aa00-af4e-11ea-8cab-1639fd85a51c.png
---

　Github Actionsを用いると、Gitにpushしたタイミングなどで、任意のコマンドを自動で実行させることができます。その機能を活用してGithub Pages x Hexoで生成されたWEBサイトの運用フローを自動化します。

- Github Actionsについては以下の記事で詳しく解説しています
    - [[CI/CD入門] Github Actionsでビルド/テスト/デプロイを自動化](/CI-CD入門-Github-Actionsでビルド-テスト-デプロイを自動化/)


## 今回自動化したい手順
- 前提とするWEBサイトを生成するまでは以下の記事を参照して下さい
    - [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)

- Hexoサイトの更新フロー
    - 記事を生成
    ```
    hexo new '記事名'
    ```
    - 生成されたmdファイルを編集
    - hexo generateを実行 
    - ディレクトリ名を変更
        - Github Pagesはリポジトリ直下に置かれたファイルを読み込みます。そこでHexoディレクトリを読み込むとエラーが出るため、Hexoディレクトリの頭に_を付与して読み込めない状態にします
        ```
        mv your-blog _your-blog
        ```
    - add/commitしてGitにpush

- 以下の単純作業は人間がやる必要が無さそうです
    - hexo generateの実行 
    - ディレクトリ名の変更

## ディレクトリ名の変更を自動化

- Github PagesのActionsタブを選択
    - set up a workflow yourselfを押下

![Github Actions](https://user-images.githubusercontent.com/41946222/84634978-88b25880-af2d-11ea-915f-78e5029a5d52.png)

- エディターが立ち上がります

![Github ACtions Editor](https://user-images.githubusercontent.com/41946222/84920443-1fce0a80-b0fe-11ea-977c-179957db0e00.png)



- 記載例

```
# workflow name
name: Hexo x Github Pages CI

# master push時に発火
on:
  push:
    branches: [ master ]

jobs:
  # This workflow contains a single job called "build"
  changeName:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
    # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
    - uses: actions/checkout@v2

    # Runs a single command using the runners shell
    - name: Change hexo directory name
      run: mv j-blog _j-blog
```

- 記載後に"start commit"を押下


## 関連記事
- [[CI/CD入門] Github Actionsでビルド/テスト/デプロイを自動化](/CI-CD入門-Github-Actionsでビルド-テスト-デプロイを自動化/)
- [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)