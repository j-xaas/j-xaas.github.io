---
title: "[AWS Budgets x SNS]でPJ毎の運用コストを管理/閾値超過で通知"
date: 2020-06-30 00:50:27
categories:
- Serverless Application Dev
- AWS
tags: 
- AWS
- AWS Budgets
- Amazon SNS
toc: true
thumbnail: https://user-images.githubusercontent.com/41946222/86028091-76c3d000-ba6c-11ea-842b-dde968b2606d.png
---

AWSにおけるコストの管理方法についてです。総コストはCloud Watchでも見れますが、PJ単位でコスト管理するにはAWS Budgetsでタグを使ってフィルタリングする方式が適しています。

## AWS Budgestとは？
- [AWS Budgets](https://aws.amazon.com/jp/aws-cost-management/aws-budgets/)
  - カスタム予算を設定して、コストまたは使用量が予算額や予算量を超えたとき (あるいは、超えると予測されたとき) にアラートを発信できるサービス

![AWS Budgets](https://user-images.githubusercontent.com/41946222/85978157-10619200-ba19-11ea-93b4-c4370132d0b6.png)

- コストレポートで出力して、報告にそのまま使える
![Report](https://user-images.githubusercontent.com/41946222/85979631-d645bf80-ba1b-11ea-8f6d-5d03862ff587.png)


## PJのリソースの総コストをタグでフィルタリング
- まずは各リソースにタグを付ける
  - PJタグを付与するルールをチーム内で設定

- Console/ AWS Budgets
- Cost Explorer
  - ここでコストの詳細を視覚的に確認できる
![Cost Explorer](https://user-images.githubusercontent.com/41946222/85978982-aea22780-ba1a-11ea-83a5-bb25d7017197.png)


- データのグループ化やフィルタリングでPJ毎のリソースのコストを抽出する
  - グループ化の条件/詳細/タグから絞る
  ![AWS Budgets グループ化](https://user-images.githubusercontent.com/41946222/85980248-fe81ee00-ba1c-11ea-9538-42b0f3be8c56.png)

  - データのフィルタ機能で全体から絞る
  ![AWS Budgets データのフィルタ](https://user-images.githubusercontent.com/41946222/85979530-a5658a80-ba1b-11ea-815e-c339ebfe0d8c.png)


## 予算作成 & アラートを設定
- AWS Budgets/予算を作成
- 予算を作成する
  - 予算タイプ
    - コスト予算
  - 予算の設定
    - タグでPJで利用しているリソース群に絞る
  ![予算設定](https://user-images.githubusercontent.com/41946222/85980897-2de52a80-ba1e-11ea-9d45-8273c4a260a9.png)
  - アラートの設定
    - しきい値
      - 80%にしておく 
    - SNS Topicを経由するか、直でメール送信か選べる
      - 今回は連絡電子メールにTeamsの開発チームのチャンネルのアドレスを設定
    ![AWS Budgets alart](https://user-images.githubusercontent.com/41946222/85981522-486bd380-ba1f-11ea-98a5-3cfbd97e5050.png)
  - 予算の確認
    - 問題なければ”作成”

以上で、コストの閾値超過時点でTeamsに通知が飛ぶように設定できた。
