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

[Github Actions](https://github.co.jp/features/actions)というサービスを活用して、開発フローの一部を自動化する手法を勉強したので、まとめておきます。今後のデファクトになりそうな便利ツールなので覚えて損はないと思います。

<!--
![CI/CD開発のイメージ](https://user-images.githubusercontent.com/41946222/85951701-e455fa80-b99f-11ea-9b16-f3c7ac73bf33.png)
-->

<!-- toc -->

## CI/CDとは
開発の効率化を目的とした、以下の思想を指します

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
    - 従来のようにGitと他のツールを連携させる手間が無くなる。学習コストや設定にあける工数を削減可能
  - 再利用性
    - 作成したWrokflowをGitでそのまま管理して、使いまわせる
    - 公開アクションが充実しており、よくあるフローは大抵用意されている

### 公開アクションの利用方法
- Marketplaceから検索することで利用可能
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
- Github Enterpeise Cloudには対応済みだが、Github Enterprise Serverは未対応(2020\06時点)
  - Github社に問い合わせたところ、2020後半に対応予定とのこと

- 契約プラン
  - レガシープランユーザの場合利用できないらしいので、会社で利用している方は要確認。リポジトリにActionタブが無い場合はレガシープランかもしれません。
    - 公式説明
    ```
    Github Actionsはレガシープランユーザはご利用できません。
    ```

### 実行環境
- 裏側でVM（仮想サーバー）が立ち上がり、そこで処理を実行してくれます
- OS
  - Linux/Windows/macOSから選んで利用可能
    - これもテンプレートで設定します
  - LinuxやWindowsについては、MicorosoftがGithubを買収した経緯もあり、Azure上で動くようです

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
![CI/CD開発のイメージ](https://user-images.githubusercontent.com/41946222/85951701-e455fa80-b99f-11ea-9b16-f3c7ac73bf33.png)

1. 個人で担当箇所を開発
2. Gitの個人Branchにpush
3. Pull Request レビューを実施
4. Masterへmerge
5. Github Actionsが起動
    - Buildを実行
    - (KarmaによるE2Eテストを実行)
      - テストについての解説は今回は省略します
    - 本番環境(etc. S3/Firebase...)へデプロイ
    - Slack or Teamsに通知(ここはWebhookやNotificationを使ってもOK)

--------------------------------
## 実装
### 使い方
- workflowを定義したymlファイルを作ります
    - 以下のように配置したymlファイルが読み込まれます
    ```
    your-repository\.github\workflows\sample.yml
    ```
- workflowの生成方法（以下のどちらか）
    - Githubのブラウザ(Actionタブ)から設定
    - Local Repositoryでディレクトリ毎生成してpush

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
- AngularアプリをBuildして、AWSのS3にデプロイするワークフローの例
    - 解説は後述します
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

    steps: ## 以降に実行する処理/コマンドを定義

    ## github actionsの環境にリポジトリのソースを持ってくる
    - uses: actions/checkout@v2

    ## 環境構築  
    - name: Angular Github Actions
      # 公開アクションでangular動作環境を構築 
      uses: mayurrawte/github-angular-actions@latest

    ## CI: Build
    - name: Build ## アクション名
      ## 実行するコマンド
      run: ng build --prod

    ## CD: S3にデプロイ
    - name: S3 site action 
      uses: erangeles/s3-site-action@v1.0

    ## Github Actions環境で処理後のソースをGitに返す
    # Runs a single command using the runners
    - name: change directory name & return for git
      run: |
        git config user.name "<git-user-name>"
        git config user.email "<git-user-e-mail>"
        git remote set-url origin https://:${{ secrets.GITHUB_PASS }}@github.com/XXXXXXXX/XXXXXXXX.github.io
        git add *
        git commit -m "Generate & Change directory name!"
        git push origin master
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
- Github Actionsは裏側でVM(仮想サーバー)が動きます
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
  - uses
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
- Angularのコマンドを実行するためにNode.jsやAngular CLIをinstallする必要があります
  - 今回は公開アクションで両方終わらせていますが、それぞれ設定する際の例を以下に示します
    - 効果アクションはバージョンによって動かなくなるリスクもあるので念のため

- 参考
    - [Using Node.js with GitHub Actions](https://help.github.com/en/actions/language-and-framework-guides/using-nodejs-with-github-actions)

- Nodejs動作環境を公開アクションで構築
```
    - name: Setup Node.js environment
      uses: actions/setup-node@v1.4.2
```
- angular CLIをrunでinstall
```
    ## 環境構築が必要
    - name: install
      run: npm install -g @angular/cli
```

#### build
- runでbuildコマンドを実行
```
    ## -----CI(継続的インテグレーション)----
    ## Buildするアクション
    - name: Build ## アクション名
      ## 実行するコマンド
      run: ng build --prod
```

#### 処理実行後にpush
- Github Actions環境(仮想サーバー)で処理したため、リポジトリへpushで反映する必要がある
```
    ## Github Actions環境で処理後のソースをGitに返す
    # Runs a single command using the runners
    - name: change directory name & return for git
      run: |
        git config user.name "<git-user-name>"
        git config user.email "<git-user-e-mail>"
        git remote set-url origin https://:${{ secrets.GITHUB_PASS }}@github.com/XXXXXXXX/XXXXXXXX.github.io
        git add *
        git commit -m "Generate & Change directory name!"
        git push origin master
```


### Github Actionsのステータス
- Actionsタブのjob名から実行結果を確認出来ます
    - ここに出ているのは、Github Actionsで立ち上がった仮想サーバーの中のできごとです
        - GithubをMicrosoftが買収したこともあって、裏はAzureの仮想サーバー(Linux)です

- 実行時
![In progress](https://user-images.githubusercontent.com/41946222/84649987-6971f600-af42-11ea-9ce4-5fa4cc1a037f.png)

- workflowを開発のデバッグ例
    - Github Actionsの中でgitコマンドを実行させようとしていますが、アカウント名などを設定できていないために失敗しており、git configも実行させないとダメそうだ、と分かります

![Debug workflow](https://user-images.githubusercontent.com/41946222/84923468-35ddca00-b102-11ea-8626-b012415e8e61.png)


### 所感
他の方もおっしゃっているように、Github Actionsはworkflowの書式が簡単で、即日で使いこなせる”手軽さ”が魅力的でした (jenkins等はもっと学習コストがかかるイメージ)
ディレクトリ毎コピーするだけで簡単に再利用できるため、開発チーム内で貯めていけば、より効果を発揮しそうです。

- その他: Github Actions活用例
  - [Github Pages x Hexo運用をGithub Actionsで自動化](/Github-Pages-x-Hexo運用をGithub-Actionsで自動化/)

## 参考
### 関連記事 (本番環境へのデプロイまで)
- [[AWS S3 x Angular] 静的WEBサイトホスティングでSPAを公開/ng build/公開範囲の限定/CI/CD化](/AWS-S3-x-Angular-静的WEBサイトホスティングでSPAを公開-公開範囲の限定/)
- [Github Pages x Hexo運用をGithub Actionsで自動化](/Github-Pages-x-Hexo運用をGithub-Actionsで自動化)
- [Firebase Hosting完全版(Angularで開発したSPAを無料で公開～ダッシュボードで費用管理)](/Firebase-Hosting完全版-Angularで開発したSPAを無料で公開～ダッシュボードで費用管理/)
- [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)
- [Github入門 ～入社初日の完全な素人でも分かる優しい説明～](/Github入門-入社初日の完全な素人向けの優しい説明/)
- [[CI/CD入門] Github Actionsでビルド/テスト/デプロイを自動化](/CI-CD入門-Github-Actionsでビルド-テスト-デプロイを自動化/)
- [Github x Teams Webhook/Notificationによる連携方法](/Github-x-Teams-Webhook-Notificationによる連携方法/)
- [Github Pages x Hexo運用をGithub Actionsで自動化](/Github-Pages-x-Hexo運用をGithub-Actionsで自動化)
- [git merge & コンフリクトの解消（複数名の編集内容を集約）](https://j-xaas.github.io/2020/04/08/git-merge-%E3%82%B3%E3%83%B3%E3%83%95%E3%83%AA%E3%82%AF%E3%83%88%E3%81%AE%E8%A7%A3%E6%B6%88%EF%BC%88%E8%A4%87%E6%95%B0%E5%90%8D%E3%81%AE%E7%B7%A8%E9%9B%86%E5%86%85%E5%AE%B9%E3%82%92%E9%9B%86%E7%B4%84%EF%BC%89/)
- [Github.comからGithubEnterpriseへの移行手順](https://j-xaas.github.io/2020/02/09/Github%E3%81%8B%E3%82%89GithubEnterprise%E3%81%B8%E3%81%AE%E7%A7%BB%E8%A1%8C%E6%89%8B%E9%A0%86/)

### Github Actions
- [Github Actions Document](https://help.github.com/en/actions)
- [GitHub Actions のコンテキストおよび式の構文](https://help.github.com/ja/actions/reference/context-and-expression-syntax-for-github-actions)


- [GitHubの新機能「GitHub Actions」で試すCI/CD](https://knowledge.sakura.ad.jp/23478/)
- [Github Actionsの使い方メモ](https://qiita.com/HeRo/items/935d5e268208d411ab5a)
- [Github Action の作り方メモ](https://qiita.com/HeRo/items/e2d5e3bc3dbe810f0482)

- [AWESOME ACTIONS　お勧めアクションやツール](https://github.com/sdras/awesome-actions)

- [Building Angular Apps Using GitHub Actions](https://medium.com/better-programming/building-angular-apps-using-github-actions-bf916b56ed0c)
  - buildの記載例


- [GitHubの新機能「GitHub Actions」でワークフローを自動化しよう](https://codezine.jp/article/detail/11450)

### S3
- [AWS S3にAngularアプリをデプロイする手順](https://qiita.com/BBA/items/4de0132e63a371da4626)
- [Angularで作ったWebアプリをGitHubで管理してS3に自動デプロイする](https://s8a.jp/angular-github-aws-s3-auto-deploy#%E5%8B%95%E4%BD%9C%E7%A2%BA%E8%AA%8D)
- [AWS初心者の私がAmazon S3のStatic website hostingを利用して静的Webページをホスティングしてみました](https://blog.web.nifty.com/engineer/2401)

