---
title: '[AWS] MFA(多要素認証)の有効化/IAM運用の基本'
date: 2020-06-16 16:24:28
categories:
- Serverless Application Dev
- AWS
tags: 
- AWS
- IAM
- MFA
- MS Authentication
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/84744456-413fd100-afee-11ea-9c64-4fa09212375a.png
---

　AWS運用における基本的なセキュリティ対策として、強い権限をもつIAMユーザにはMFAの設定が必須です。

<!-- toc -->

## 基礎知識

### MFA(Multi-Factor Authentication)とは
- MFA = 多要素認証
    - 本人確認のための要素を複数、ユーザーに要求する認証方式
        - 例えば、スマホのSMSでパスワードを送る方式や、一時的なパスワードを入力する方式を体験したことのある方が多いと思います
            - 高いセキュリティを求められるサービスではデファクトになりつつあります
        - 万が一、ユーザ名＆パスワードが流出しても悪用を防げることがメリット
    - AWSではIAMユーザ毎にMFAを設定可能

### AWSのアカウント管理について

詳細な説明は省きますが、AWSのIAM運用の基本的な考え方を以下に示します。この辺りを理解するには、AWS Solution Architect等の資格の取得がお勧めです。

- IAM運用の基本的な構成
    - Master Account
        - 危険なので基本的に使用禁止
        - MFAを必ず有効化
    - AWS管理者用：Admin権限をもつIAMユーザ
        - 危険なので常用はしない。使用するのはユーザ追加の時等。
        - MFAを有効化して、重要な作業を実施する際にだけ利用
    - 作業者用：PJメンバのIAMユーザ
        - より上位のIAMユーザを持っているメンバも基本的にこちらを使って作業

より大きな枠組みのマルチアカウント構成やOrganizationの話は省いておりますが、その辺りはAWS　SysOps Administratorを取得すれば理解できるので興味があれば勉強してみるといいと思います。

## MFA設定手順
- 前提
    - IAMを弄る権限を持つIAMユーザで設定
    - 今回はスマホのMFAアプリを使います
        - 社用スマホがあれば基本的にこの手法でOK

- AWSコンソールのサービス一覧より、IAMを選択
- 左のナビゲーションペインよりユーザーを選択
- MFAを有効化したいユーザを選択
- 認証情報タブを選択
![IAM User概要](https://user-images.githubusercontent.com/41946222/84742689-d097b500-afeb-11ea-8fc3-bdcfe69836cf.png)

- MFAデバイスの割り当ての右の”管理”を押下
- 次へ
![image](https://user-images.githubusercontent.com/41946222/84742841-105e9c80-afec-11ea-9f92-80547ffca02a.png)

- ”QRコードの表示”を押下
![image](https://user-images.githubusercontent.com/41946222/84742968-3b48f080-afec-11ea-8f3c-ef6d40bb8c76.png)

- 好きなMFAアプリでQRコードを読み込む 
    - 今回はMS Authenticatorを使いました
        - ”2段階認証のセットアップ”からQRコードを読み込めます
- MFAアプリに表示されるワンタイムパスワードを入力
- MFAの割り当てを押下
- 以下のように表示されればOKです


### 設定後の認証の流れ
- AWS Consoleのログイン画面にアクセス
- Account/IAM　User名/Passwordを入力
- 連携させたMFAアプリに表示されるワンタイムパスワードを入力