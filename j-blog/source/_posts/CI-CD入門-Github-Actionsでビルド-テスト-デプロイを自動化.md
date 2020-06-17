---
title: '[CI/CD入門] Github Actionsでビルド/テスト/デプロイを自動化'
date: 2020-06-15 17:35:02
categories:
- Tool Tips
- CI/CD(Github Actions)
tags: 
- Github Actions
- Github
- CI/CD
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/84656875-59f8aa00-af4e-11ea-8cab-1639fd85a51c.png
---

<!-- toc -->

## CI/CDとは
開発の効率化を目的とした、以下の思想を指します。

- CI (継続的インテグレーション)
    - テスト, ビルド等を自動化して小まめに行う
- CD (継続的デリバリー)
    - 本番環境へのデプロイを自動化して小まめに行う

上記を実現するために様々なツール(ex. jenkins, Circle CI...)があり、Githubと連携させて利用するのが一般的です。2020にGithubが自ら打ち出した新たなサービスがGithub Actionsです。

## Github Actions解説
### 概要
- [Github Actions](https://github.co.jp/features/actions)
  - Githubが提供している機能の一つ
  - 機能：ワークフローの自動化
    - pushやmergeを起点に自動で何らかのアクションを実行
    - ex. テストの自動化、Buildの自動化、本番環境へのデプロイの自動化
  - ymlファイル形式でワークフローを定義して使う

![workflow](https://user-images.githubusercontent.com/41946222/84237526-75ba1580-ab34-11ea-8a58-0c710ef8bf93.png)

- 魅力
    - 工数削減
        - 従来のようにGitと他のツールを連携させる手間が無くなる。設定/学習コストの削減を期待できる
    - 再利用性
        - 作成したWrokflowをGitでそのまま管理して、使いまわせる

- Marketplaceから他のユーザが作成済みのActionを利用可能
  - 試しにAngularで検索してみると以下が表示された
  - [Github Actions Marketplace Angular](https://github.com/marketplace?type=actions&query=Angular)

- S3/Firebase/Github Pages等のHostingサービスへの自動デプロイやテストを実行するActionを一般ユーザが作って公開してくれている
  - ソースを更新してGitにPushしただけで、瞬時に本番環境も更新できるスピーディーな運用をパッと実現できそうです
  - 本番環境(Hostingサービス)への初回のデプロイまでは以下の記事で解説しています
    - [[AWS S3 x Angular] 静的WEBサイトホスティングでSPAを公開/ng build/公開範囲の限定/CI/CD化](/AWS-S3-x-Angular-静的WEBサイトホスティングでSPAを公開-公開範囲の限定/)
    - [Firebase Hosting完全版(Angularで開発したSPAを無料で公開～ダッシュボードで費用管理)](/Firebase-Hosting完全版-Angularで開発したSPAを無料で公開～ダッシュボードで費用管理/)
    - [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)

![Actions](https://user-images.githubusercontent.com/41946222/84236499-aef18600-ab32-11ea-85ad-1c1320084897.png)

### 料金
- Publicリポジトリ
  - 無料
- Privateリポジトリ
  - 従量課金性
![料金](https://user-images.githubusercontent.com/41946222/84237765-e4976e80-ab34-11ea-9cbe-e51ab8749faf.png)

- 大規模開発でなければ、無料で使えそう

### 注意点
- レガシープランユーザの場合は利用できないらしいので、GHE(Github Enterprise)を会社で利用している方は確認してください。リポジトリにActionタブが無い場合はレガシープランかもしれません。
    - 公式説明
    ```
    Github Actionsはレガシープランユーザはご利用できません。
    ```

### ワークフローの定義
- 定義方法
  - リポジトリにworkflowディレクトリを作成
  ```
  .github/workflows/
  ```
  - YAMLファイルを配置
    - ここにワークフローを定義
    - ファイルがワークフローの単位となる
    - ファイルは複数可、並列で実行される



### ワークフロー(YAMLファイル)の構造
```
ワークフロー（YAMLファイル）
  └ jobs:
    └ ジョブ(名前は任意)
       └ steps:
         └ アクション
```

### Job
- 各ジョブは仮想環境の新しいインスタンスで実行される。
- ジョブ間で環境変数やファイル、セットアップ処理の結果などは共有されない。
- ジョブ間の依存を定義して待ち合わせることができる。
- データの受け渡しが必要ならアーティファクト経由

### Step
- Jobが実行する処理の集合。
- 同じJobのStepは同じ仮想環境で実行されるのでファイルやセットアップ処理は共有できる。しかし各ステップは別プロセスなのでステップ内で定義した
- 環境変数は共有できない。
  - jobs.<job_id>.envで定義した環境変数は全Step で利用できる

## 開発フローの検討
分散開発におけるGit flowを検討しました

### 例：S3(AWS)でAPをHostngしているケースの作業
- APを改修して、本番環境に変更を反映するまでに必要な作業
  1. 開発
  2. gitへpush
  3. PullRequest レビュー ⇒ Master Merge
  4. Build
  5. 本番環境へデプロイ

- Masterが更新される度に、Buildしてデプロイする単純作業(4 & 5)を自動化したい

- やり方は２通りありそう
  - Github Actionsでbuildとdeployを実行
  - GithubからはWebhookのみ、AWS上でCode Builderを起動させてBuild、Code Deployでdeployを実行

### Github Actionsを利用した開発フロー 
1. 個人で担当箇所を開発
2. Gitの個人Branchにpush
3. Pull Request レビューを実施
4. Masterへmerge
5. Github Actionsが起動
    - Buildを実行
    - (KarmaによるE2Eテストを実行)
    - 本番環境(etc. S3/Firebase...)へデプロイ
    - Slack or Teamsに通知(ここはWebhookやNotificationを使ってもOK)

--------------------------------

## 実装
### 使い方
- workflowを定義したymlファイルを作ります
    - 以下の例のように配置したymlファイルが読み込まれます
    ```
    your-repository\.github\workflows\sample.yml
    ```
- workflowの生成方法（以下のどちらか）
    - Githubのブラウザ(Actionタブ)から設定
    - Local Repositoryで生成してpush

### Githubブラウザ側で設定
- Githubの任意のリポジトリのActionタブで設定を行う
  - Github Enterpriseを利用中でActionタブが無い場合
    - レガシープランユーザーの可能性があるので、契約の確認が必要
      - 公式説明
      ```
      GitHub Actionsはレガシープランユーザーではご利用できません。
      ```

![Github Actions](https://user-images.githubusercontent.com/41946222/84634978-88b25880-af2d-11ea-915f-78e5029a5d52.png)


- ブラウザでそのままディレクトリとwrokflowファイルを生成できます
    - set up a workflow yourselfを押下すると以下の様画面に飛びます

![Workflow編集画面](https://user-images.githubusercontent.com/41946222/84640899-47be4200-af35-11ea-9203-4ff973598067.png)


- Actionsタブを確認
    - 先ほど生成されたworkflowが実行されていることが分かります
    - New workflowを押下してworkflowを追加していけます

![Actionsタブ](https://user-images.githubusercontent.com/41946222/84642036-e13a2380-af36-11ea-8214-5048fa7e82a8.png)


### Loacalリポジトリで作業する場合
- ディレクトリを作成
```
your-repository> mkdir .github/workflows/
```

- YAMLファイルを作成
  - 名称の規則は無し
```
your-repository.github/workflows/> touch sample.yml
```

- そのままymlファイルを編集してGitにpushすれば、Github Actionsが動くようになります



### Workflowテンプレートの開発
- Buildを実行する設定例
    - 解説を後述します
```
name: Angular CI/CD ## workflow名を設定
on:  ## workflowのTriggerを定義
  push: ## push時に発火 他例) pull request
    branches: ## 条件でmaster branchへのpushに限定
      - master

jobs: ## ここで実行するジョブを指定
  build: ## buildという名目のジョブを定義
    ## 実行環境 Github Actions提供の仮想サーバー
    runs-on: ubuntu-latest ## Ubuntsの最新版環境を指定
    strategy:
      matrix:
        node-version: [12.x]

    steps: ## ここで実行する処理/コマンドを定義

    ## github actionsの環境にリポジトリのソースを持ってくる
    - uses: actions/checkout@v2

    ## 環境構築が必要
    - name: install
      run: npm install -g @angular/cli

    ## -----CI(継続的インテグレーション)----
    ## Buildするアクション
    - name: Build ## アクション名
      ## 実行するコマンド
      run: ng build --prod ## npm run build:prod?

    ## Github Actions環境(仮想サーバー)で処理実行後にpushする必要がある
    - name: Push Build to Releases
      uses: ncipollo/release-action@v1
      with:
        artifacts: "dist/angular-githubaction/*"
        token: ${{ secrets.TOKEN }}
```


#### Triggerを指定
- on: でworkflowが発火するタイミングを指定できます
    - 以下はmaster mergeの実行時を指定
```
on:  ## workflowのTriggerを定義
  push: ## push時に発火 他例) pull request
    branches: ## 条件でmaster branchへのpushに限定
      - master
```


#### 実行環境を設定
- Github Actionsは裏側で仮想サーバーが動きます
    - 今回はLinux (Ubunts)を設定
    - WindowsやMacOS等、一通り揃っています

```
    ## 実行環境 Github Actions提供の仮想サーバー
    runs-on: ubuntu-latest ## Ubuntsの最新版環境を指定
    strategy:
      matrix:
        node-version: [12.x]
```

#### actionの設定
- ワークフローの最小単位
- ２種類ある
  - run
    - コマンドを実行
  - use
    - Githubやサードパーティの公開actionを利用

- 設定の流れ
  - まず公開のアクションがないか探してみる
  - なければ実行したいコマンドをrunで設定

#### checkout
- まずはgithub actionsで利用する仮想サーバーにリポジトリのソースコードを持ってくる必要があります
    - Githubが事前に公開アクション（actions/checkout@v2）を用紙してくれているので、そちらを使います

```
    ## github actionsの環境にリポジトリのソースを持ってくる
    - uses: actions/checkout@v2
```


#### 環境設定
- 今回はAngularのコマンドを実行するためにNode.jsをinstallする必要があります

- 参考
    - [Using Node.js with GitHub Actions](https://help.github.com/en/actions/language-and-framework-guides/using-nodejs-with-github-actions)

```
    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node-version }}
    - run: npm install
```


- runでinstallコマンドを実行させます
```
    ## 環境構築が必要
    - name: install
      run: npm install -g @angular/cli
```


#### build
- runでbuilのコマンドを実行
```
    ## -----CI(継続的インテグレーション)----
    ## Buildするアクション
    - name: Build ## アクション名
      ## 実行するコマンド
      run: ng build --prod
```

#### 処理実行後にpush
- Github Actions環境(仮想サーバー)で処理実行したため、リポジトリへpushして反映する必要がある
```
    - name: Push Build to Releases
      uses: ncipollo/release-action@v1
      with:
        artifacts: "dist/angular-githubaction/*"
        token: ${{ secrets.TOKEN }}
```


### Github Actionsのステータス

- 実行時
![In progress](https://user-images.githubusercontent.com/41946222/84649987-6971f600-af42-11ea-9ce4-5fa4cc1a037f.png)





## 参考
### 関連記事 (本番環境へのデプロイまで)
- [[AWS S3 x Angular] 静的WEBサイトホスティングでSPAを公開/ng build/公開範囲の限定/CI/CD化](/AWS-S3-x-Angular-静的WEBサイトホスティングでSPAを公開-公開範囲の限定/)
- [Github Pages x Hexo運用をGithub Actionsで自動化](/Github-Pages-x-Hexo運用をGithub-Actionsで自動化)
- [Firebase Hosting完全版(Angularで開発したSPAを無料で公開～ダッシュボードで費用管理)](/Firebase-Hosting完全版-Angularで開発したSPAを無料で公開～ダッシュボードで費用管理/)
- [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)


### Github Actions
- [Github Actions Document](https://help.github.com/en/actions)
- [GitHub Actions のコンテキストおよび式の構文](https://help.github.com/ja/actions/reference/context-and-expression-syntax-for-github-actions)


- [GitHubの新機能「GitHub Actions」で試すCI/CD](https://knowledge.sakura.ad.jp/23478/)
- [Github Actionsの使い方メモ](https://qiita.com/HeRo/items/935d5e268208d411ab5a)
- [Github Action の作り方メモ](https://qiita.com/HeRo/items/e2d5e3bc3dbe810f0482)

- [AWESOME ACTIONS　お勧めアクションやツール](https://github.com/sdras/awesome-actions)

- [Building Angular Apps Using GitHub Actions](https://medium.com/better-programming/building-angular-apps-using-github-actions-bf916b56ed0c)
  - buildの記載例


### S3
- [AWS S3にAngularアプリをデプロイする手順](https://qiita.com/BBA/items/4de0132e63a371da4626)
- [Angularで作ったWebアプリをGitHubで管理してS3に自動デプロイする](https://s8a.jp/angular-github-aws-s3-auto-deploy#%E5%8B%95%E4%BD%9C%E7%A2%BA%E8%AA%8D)
- [AWS初心者の私がAmazon S3のStatic website hostingを利用して静的Webページをホスティングしてみました](https://blog.web.nifty.com/engineer/2401)

