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
    - uses: actions/checkout@vでリポジトリの中身にアクセス可能になります
    - 次にgitコマンドを使う為にgit configを実行
    - git mvでHexoディレクトリの名称を変更
    - git add/commit後にpushでリポジトリに変換
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
    - name: change directory name & return for git
      run: |
        git config user.name "<git-user-name>"
        git config user.email "<git-user-e-mail>"
        git remote set-url origin https://:${{ secrets.GITHUB_PASS }}@github.com/<organization-name>/<your-repository-name>
        git mv j-blog _j-blog
        git add *
        git commit -m "Change directory name!"
        git push origin master
```

- 記載後に"start commit"を押下
    - workflowを定義したymlファイルが生成されます
- 一旦ローカルに反映
```
git pull
```
- 以後masterにpushする度にworkflowが自動で実行されます
- Actionsタブから確認出来る実行結果にチェックマークがついていれば成功です

![actions result](https://user-images.githubusercontent.com/41946222/84927346-aa673780-b107-11ea-8f8a-e723c521004a.png)


## リポジトリにパスワードを埋め込む
- リポジトリのセキュリティ制約が厳しい場合
    - Github Actionsの仮想サーバー上からリポジトリと通信するには、gitのパスワードが必要になります。workflowに直接書き込んでしまうと、Public Repositoryなら誰でも見れてしまうので、Settingsに登録して参照させます

![image](https://user-images.githubusercontent.com/41946222/84924555-a6391b00-b103-11ea-9945-072e61ef03b6.png)

- 参照方法
    - github actionsのworkflowに以下のように書けば値を引っ張れます
    ```
    ${{ secrets.GITHUB_PASS }}
    ```

## Github Actionsでworkflowを開発する際のデバッグ
- Actionsタブのjob名から実行結果を確認出来ます
    - ここに出ているのは、Github Actionsで立ち上がった仮想サーバーの中のできごとです
        - GithubをMicrosoftが買収したこともあって、裏はAzureの仮想サーバー(Linux)です

- 例
    - Github Actionsの中でgitコマンドを実行させようとしていますが、アカウント名などを設定できていないために失敗していることが分かります。git configも実行させないとダメそうだ、と改善ポイントが定まりました。

![Debug workflow](https://user-images.githubusercontent.com/41946222/84923468-35ddca00-b102-11ea-8626-b012415e8e61.png)


## hexo generateを自動化
- [Github actions marketplace hexo](https://github.com/marketplace?type=actions&query=Hexo)
    - こちらの公開アクションを利用します

- 時間ができた時に追記します


## 関連記事
- [[CI/CD入門] Github Actionsでビルド/テスト/デプロイを自動化](/CI-CD入門-Github-Actionsでビルド-テスト-デプロイを自動化/)
- [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)
## 参考
- [【GitHub Actions】CIを使って毎日自動でGitHubに草を生やそうｗｗｗ](https://qiita.com/ykhirao/items/65fee829ee0478187027)
- [Github actions marketplace hexo](https://github.com/marketplace?type=actions&query=Hexo)