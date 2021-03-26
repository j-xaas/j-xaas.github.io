---
title: '[AWS] PJ毎のコスト管理まとめ(Cost Explorer/Cost Anomaly Detection/Budgets/タグ付けルール)'
date: 2021-03-26 16:43:35
categories:
- Serverless Application Dev
- AWS
tags: 
- AWS
- AWS Cost Explorer
- Cost Anomaly Detection
- AWS Budgets
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/112600566-b92c0900-8e54-11eb-89a7-69e5663c4651.png
---

単一アカウント内で複数のPJやサービスが運用されているケースのコスト管理についてのメモ  
マルチアカウント運用の際にもこれらのサービスの設定が必要になる

<!--toc-->

---
## PJ開始時にすべきコスト関連の作業
以下は基本的に無料枠で実施できるため、PJ開始時に対処しておく
（SNS通知に微量だが料金がかかる）

### 1. 運用ルールの定義・周知
- AWSの利用開始時に、タグ付けルールを定めてメンバに周知する
- CFnテンプレートにはタグの入力欄を設けておく

### 2. PJで利用中のリソースに限った総コストの確認/予測
![Cost Explorer](https://user-images.githubusercontent.com/68212997/112601148-71f24800-8e55-11eb-93e3-12505b1be7c4.png)

- Cost Explorerのフィルタリング機能を活用する
- 単一のAWSアカウント内で複数のサービスを運用する場合、タグを付与することで費用を区分できる

### 3. 異常検出
- Cost Anomaly Detectionを活用する
- タグやリソースベースでPJのコストを追跡し、料金の異常発生時に通知させる
    - 例えば、DDoS攻撃による料金の急騰などが挙げられる

### 4. Budgetsで予算を設定、超過時の通知を設定
![AWS Budgets](https://user-images.githubusercontent.com/68212997/112601321-a9f98b00-8e55-11eb-90f5-422197f39b76.png)

- AWS Budgetsを活用して、承認を得た予算に対する状況管理を可能にする

---

## 設定手順

### 運用ルール(タグ付けルール)の定義
単一のAWSアカウント内で複数のサービスを運用する場合、タグを付与することで費用を区分することが可能になる。PJでリソースを作り始める前に、ルールを定めて周知しておく必要がある。

- タグ付けルール例
    - Project
        - Project毎のコスト管理に活用
    - Owner
        - リソースの作成者が後で判別するために必要
    - Enviroment
        - 本番、テスト環境などの判別/コスト管理のために活用

CFnテンプレートを利用する場合は、上記のタグを入力値として用意しておくと、つけ忘れを防止できる


### コスト配分タグのアクティブ化
Cost Explorer内で、タグによるフィルタリングをするには、リソースに付与したタグを Billing and Cost Management コンソールでアクティブ化する必要がある。会社でOrganizationsを利用している場合、部門のアカウントでは設定権限が無いこともあるので注意。

- AWS Console/Billing and Cost Management/コスト配分タグ
  - アクティブ化するタグを選択
  - ここでアクティブ化したタグのみ、Cost Explorerでフィルタリングに活用できる
    - リソースにタグを付与しただけでは活用できない

### PJのコスト確認/予測方法
- AWS Management Console/Cost Explorer
    - 過去６か月間のコスト(＄)がサービス毎に表示される

- 任意の期間に変更
    - 期間が表示されているタブから選択

- PJのリソースのフィルタリング
    - 右側のフィルター欄/タグを押下
    - Project名のタグを選択

![Cost Explorerタグによるフィルタリング](https://user-images.githubusercontent.com/68212997/112600912-1de76380-8e55-11eb-9b68-26053b73d872.png)

- 予測
    - 期間を変更することで、最大12ヵ月先までの予想費用をグラフに表示できる


### Cost Anomaly Detectionでコストの異常検出を設定
- AWS Cost Anomaly Detectionにより、コスト異常検出と原因の分析を自動化できる
  - 無料だが、SNSアラート通知を有効化すると微量の料金がかかる

- AWSConsole/Cost Explorer/コスト異常検出
  - コストモニターを作成
  - モニタータイプを選択
    - コスト配分タグ
      - PJ毎の総費用を追跡する場合はこちらでPJで運用ルールとして付与したタグを選択する
    - 連結アカウント
      - マルチアカウント形式で運用している場合はこちらにアカウントIDを入力
  - アラートサブスクリプションを設定
    - アラート受信者に、開発チームのteamsのチャンネルやメーリスを登録しておくと管理が楽になる
  ```
  コストモニターが異常を検出したときに通知します。アラートの頻度に応じて、E メールまたは Amazon SNS によって指定されたユーザーに通知できます。たとえば、組織の Finance team のサブスクリプションを作成できます。
  ```
  - モニターを作成


## Budgetsによる予算管理
- AWS Management Console/Cost Explorer/Budgetsタブ
    - 予算を作成
    - 事前に承認されたPJのコストを上限値として設定する
    - 設定した予算に対する状況は%で確認できる
        - 引き上げが必要になったら、Cost Explorerの予測を元に算出した値で予算管理者から承認を得て、Budgetsの予算を更新する流れになる


## 参考
### 関連記事
- [Angular x AWS SDK for JavaScriptの始め方](/Angular-x-AWS-SDK-for-JavaScriptの始め方/)

- [[Angular JavaScript] JSONデータのファイル化と出力 ~取得したデータを任意の名称で保存するロジック~ (TypeScript)](/Angular-JSONデータのファイル化と出力-クラウドから取得したデータを任意の名称で保存する/)
- [[Angular JavaScript] JSONファイル(複数)の読み込み](/Angular-JavaScript-JSONファイルの読み込み/)
- [静的ホスティングサービスまとめ(Netlify/S3/Firebase Hosting/Github Pages)](/静的ホスティングサービスまとめ-Netlify-S3-Firebase-Hosting-Github-Pages/)
- [[Angular ReactiveForm x mat-input] 一つの入力欄のエラーメッセージを出し分ける](/Angular-ReactiveForm-x-mat-input-一つの入力欄のエラーメッセージを出し分ける/)
- [[Angular mat-input] バリデーションまとめ](/Angular-mat-input-バリデーションまとめ/)
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)
- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)

### その他
- [Amazon Web Services ブログ コスト配分タグを用いた効率的なコスト管理](https://aws.amazon.com/jp/blogs/news/cost-allocation-tag/)
- [ユーザー定義のコスト配分タグのアクティブ化](https://docs.aws.amazon.com/ja_jp/awsaccountbilling/latest/aboutv2/activating-tags.html)
