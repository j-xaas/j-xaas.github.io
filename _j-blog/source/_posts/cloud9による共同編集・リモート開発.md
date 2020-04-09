---
title: cloud9による共同編集・リモート開発
date: 2020-03-07 01:47:37
categories:
- Tool Tips
- AWS Cloud9
tags:
- cloud9
- 共同開発
- テレワーク
toc: true
---

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/76097639-63977b00-600b-11ea-8586-cf30b3608a1f.png" height="100%" width="100%">
</div>

  
　こんにちは。最近ウィルス対策の影響でテレワークの利用が広がっているようです。IT界隈の方達は慣れているとは思いますが、「トラシューやコードレビュー、ペアプロ等は対面でないと困る」という声をちらほら聞きます。新入社員が先輩に横で教えてもらうためにわざわざ出社するケースもあるようです。そこで、今回は開発の遠隔・同時編集を可能にするTipsを紹介します。利用するのはAWSのブラウザ型IDEであるcloud9のワークスペース共有機能です。

<!--- toc -->

## 前提知識
- [cloud9のワークスペース共有機能](https://docs.aws.amazon.com/ja_jp/cloud9/latest/user-guide/share-environment.html)
    - 画像の様に複数メンバでエディタ＆CLIを同時に遠隔操作できます
        - 右側のタブに参加メンバが表示されます
    - 初期設定だけ完了すれば、ブラウザでURLを共有してログインしてもらうだけです
        - テレワーク時に「ちょっとここが分からないので、アドバイスを頂けますか？」と開発環境に入ってもらうと助けてもらいやすいです
            - 直接コードやコマンドを書き込んでもらうこともできます
            - Slack等のチャットだけでコードの話をするのは実際辛いです
        - 開発チームを監督する側も状況を理解しやすく、皆が幸せになれます

<div style="text-align:center;">
<img src="https://user-images.githubusercontent.com/41946222/77328583-14b64900-6d60-11ea-9aed-3f0414313d5e.png" height="250px" width="500px" alt="共同開発のイメージ">
</div>


- 対抗：[VSCode LiveShare](https://qiita.com/taichi0514/items/95aca7428376a7b41f90)
    - Microsoftのエディターでもプラグインによって似たようなことができます
    - 個人的な使用感としては以下の差異があります
        - 自分のLocal環境を共有
            - APを起動すると恐ろしく重くなり、落ちてしまうことも...
        - 各メンバ全員がプラグインを設定する必要があり、手間取る
            - ホスト側が参加メンバ毎に権限付与する工程が毎回ある
    - VSCodeの豊富なプラグインが必要なケースもあるので、私は場合によって使い分けています。個人の好みにもよると思います。

## 前提条件
- AWSアカウントを作成済み or 作成可能
- cloud9 開発環境を作成済み
    - 公式ドキュメントを参考にしてください
        - [AWS Cloud9 で 環境 を作成する](https://docs.aws.amazon.com/ja_jp/cloud9/latest/user-guide/create-environment.html)
- 読者は以下を想定
    - 開発環境のオーナー
    - IAMユーザの作成権限を持つ

## cloud9環境共有手順
- 以下の二点が必要です
    - AWSのIAM Userの作成
    - Cloud9への参加メンバの登録

- 右上の"Share"を押下
    <div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/41946222/76101952-96913d00-6012-11ea-9004-ab8ae6dd81ea.png" height="100%" width="100%">
    </div>

- 共有機能の設定画面が出ます
    <div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/41946222/76102262-17e8cf80-6013-11ea-9f0c-55a8f0889a29.png" height="100%" width="100%">
    </div>

### Cloud9への参加メンバの登録
- 共有機能の設定画面の”Invite Members”にIAM User名を入力
- タブで権限を設定
    - R: Readのみ
    - RW: Read & Write
- まだ未作成の場合は"create a new user"よりAWS Consoleに飛びましょう

### IAMユーザの追加
- ★cloud9で共同編集したいだけのメンバ向けの権限についてです。最小権限の原則に従いましょう
    - アクセス許可の設定の”既存のポリシーを直接アタッチ”を選択
    - 検索欄に"cloud9"と打つ
    - AWSCloud9EnvironmentMemberを選択・付与

## まとめ

　以上で複数メンバでの共同編集ができます。テレワークにおける情報伝達のもどかしさを解消して気持ちよく開発しましょう。ハンズオン研修・学習用途にも便利だと思います。
    
## 関連記事
- [cloud9 x Angular x FirebaseでWEB AP開発](https://j-xaas.github.io/2020/03/05/Cloud9-x-Angular-x-Firebase%E3%81%A7AP%E9%96%8B%E7%99%BA%EF%BC%88%E5%B0%8E%E5%85%A5%E7%B7%A8%EF%BC%89/)

画像ファイル
D:\Users\0000011349117\Desktop\memo_article\cloud9member



## 補足：Angular開発環境の構築手順(cloud9)

- 以下を実行するだけでOKです
```
npm install @angular/.....
ng add @angular/material
```
- 自動で入るとは思いますが、もし利用中にCDKがないというエラーが出たら以下も実行してください
```
$ ng add @angular/CDK
```


