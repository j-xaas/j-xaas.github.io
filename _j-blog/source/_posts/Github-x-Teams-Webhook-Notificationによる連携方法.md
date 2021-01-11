---
title: Github x Teams Webhook/Notificationによる連携方法
date: 2020-06-10 13:31:51
categories:
- Tool Tips
- Github
tags: 
- Github.com
- Teams
- Webhook
- CI/CD
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/84231133-6b921a00-ab28-11ea-8c0e-a47fb924b2c2.png
---

TeamsとGithubの連携方法について解説します。GitにpushやPull Requestdを出す度にメンバにメッセージを送る手間を自動化しましょう。
Slackとの連携方法は沢山出てくるのですが、Teamsは古いものしか無かったので纏めておきます。

<!-- toc -->

# 1. TeamsとGithubの連携方法
- 2種類あります
![連携イメージ](https://user-images.githubusercontent.com/41946222/84231004-371e5e00-ab28-11ea-9da6-9ae5cea49718.png)

## 1.1. Webhooks
- Githubの機能
- 任意のEventを起点に発火
    - ex.) masterへのpush, PullRequest...
- 特定のURLに対して通知を飛ばす
    - Teamsの特定チャンネルのコネクタ宛            
- Teams側ではコネクタでリクエストを受け取る
    - ex.) Incoming Webhook or Githubアプリ

## 1.2. Notification
- Githubの機能
- Eventを起点に発火
    - 自分でEventを絞ることはできない
- 特定のメールアドレスに対して通知を飛ばす
    - Teamsの特定チャンネルのメールアドレス宛
    - Teamsのチャンネルにはそれぞれ一意なメールアドレスが与えられている

## 1.3. どちらを使うべきか？
- 社内セキュリティや機能制限でWebhookを使えない or メーリングリスト等にも通知を飛ばしたい
    - Notification
- 特に制限無し
    - Webhook

# 2. WebhookでTeams x Githubを連携
- Gituhubアプリは社内の機能制限で入れられなかったのでIncoming Webhookを利用します
    - TeamsのGithubアプリ
        - Teams App Storeか[ここ](https://appsource.microsoft.com/ja-jp/product/office/WA104381552?src=wnblogmar2018&tab=Overview)で入手可能

## 2.1. Teamsにコネクタを追加
- チャンネル名の右にある・・・からコネクタを選択
![コネクタ](https://user-images.githubusercontent.com/41946222/84217745-f0b90700-ab07-11ea-869b-703f331471d2.png)

- Incoming Webhookの構成を選択
![Incoming Webhook](https://user-images.githubusercontent.com/41946222/84217852-31188500-ab08-11ea-876d-d7873338bcc2.png)

- 接続名
  - Github等判別可能な名前を指定
- イメージを設定
  - Botがメッセージを投稿する際のアイコンになる

![Incoming Webhook](https://user-images.githubusercontent.com/41946222/84218283-1266be00-ab09-11ea-923e-77c477b9e962.png)

- 作成を押下

- 出てきたURLを控えて完了
![URL](https://user-images.githubusercontent.com/41946222/84218434-58238680-ab09-11ea-824d-faa67bdc9b5f.png)

- チャンネルにメッセージが投稿される
![Message](https://user-images.githubusercontent.com/41946222/84218566-aafd3e00-ab09-11ea-8e27-9ecbb4bb194a.png)

## 2.2. GithubのWebhook設定

- 設定画面
    - 任意のリポジトリのページに移動
    - Settings
    - Hooks
    - Add webhook

![Github Webhooks](https://user-images.githubusercontent.com/41946222/84218862-4393be00-ab0a-11ea-9508-4bda344bac08.png)


- Payload URL
  - 送信先のURLを指定
  - 控えておいたTeamsのコネクタのURLをコピペ
- Secret
  - パスワードが必要であれば設定
- Which events would you like to trigger this webhook?
  - 通知を飛ばすイベントを絞りたい場合は ”Let me select individual events”を選択
  - 出てきた選択肢から必要なものをチェック

![Events](https://user-images.githubusercontent.com/41946222/84219238-067bfb80-ab0b-11ea-81ec-f4945dba38ae.png)

- 設定が完了したらAdd webhook
- 以上でpush時にTeamsに通知が投稿されるようになります

# 3. NotificationでTeams x Githubを連携
## 3.1. メールアドレスを取得
- チャンネル名の右にある・・・から”メールアドレスを取得”を選択
![Teams channnel mail address](https://user-images.githubusercontent.com/41946222/84225711-d5a3c280-ab1a-11ea-827b-640736955a92.png)

- 出てきたアドレスをコピー
  - <>で囲まれている箇所がメールアドレス
  - 通常のメールからここに対してメールを送ればTeamsに内容が投稿される

## 3.2. GitのNotificationに設定
- 任意のリポジトリのSettings/Notificationsへ移動
  - Addressに先ほど控えたTeamsチャンネルのURLをコピペ
![image](https://user-images.githubusercontent.com/41946222/84225908-5236a100-ab1b-11ea-8ddf-a68f779997e5.png)


- 以下のようになれば設定完了
![image](https://user-images.githubusercontent.com/41946222/84226090-cec97f80-ab1b-11ea-9162-58ef2d795acd.png)

- 試しにpushをすると、Teamsに投稿されます
![image](https://user-images.githubusercontent.com/41946222/84226892-fcafc380-ab1d-11ea-97f5-5455456895cc.png)


# 4. その他
## Microsoft FlowやAutomationによる自動化
- 通知が来たタイミングでワークフローを実行させることもできるようです
  - Slackのワークフロー機能のようなもの
- 社内の機能制限により試せなかったので、変えさせられたら追記します

## 参考
### 関連記事
- [Github入門 ～入社初日の完全な素人でも分かる優しい説明～](/Github入門-入社初日の完全な素人向けの優しい説明/)
- [[CI/CD入門] Github Actionsでビルド/テスト/デプロイを自動化](/CI-CD入門-Github-Actionsでビルド-テスト-デプロイを自動化/)
- [Github x Teams Webhook/Notificationによる連携方法](/Github-x-Teams-Webhook-Notificationによる連携方法/)
- [Github Pages x Hexo運用をGithub Actionsで自動化](/Github-Pages-x-Hexo運用をGithub-Actionsで自動化)
- [git複数アカウントの使い分け設定](/git複数アカウントの使い分け設定/)
- [[Hexo x Github Pages] 5分以内にブログを自動生成して無料で公開するまで【完全版】](/Hexo-x-Github-Pages-5分以内にブログを自動生成して無料で公開するまで/)
- [git merge & コンフリクトの解消（複数名の編集内容を集約）](/git-merge-コンフリクトの解消（複数名の編集内容を集約）/)
- [Github.comからGithubEnterpriseへの移行手順](/GithubからGithubEnterpriseへの移行手順/)

### その他
- [GitHubの組織へのメンバー変更をMicrosoft Teamsに通知する（by Microsoft Flow）](https://shunsukekawai.hatenablog.com/entry/2018/07/19/230909)


