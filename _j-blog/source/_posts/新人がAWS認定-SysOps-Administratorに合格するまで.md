---
title: 新人がAWS認定 SysOps Administratorに合格するまでの学習方法詳細 (2021年)
date: 2021-04-01 15:38:38
categories:
- Others
- 資格
tags: 
- AWS
toc: true
thumbnail: https://user-images.githubusercontent.com/68212997/116777182-1e07fe00-aaa8-11eb-8088-df57422e17b6.png
---

- AWS認定 SysOps Administrator(SOA)の合格体験記
- SAA(Solution Architect)は一度もAWSを触らず[黒本](https://amzn.to/2PInGNo)と[問題集](https://aws.koiwaclub.com/)だけで合格できたが、SysOpsは専用の教材も無く、格段に難しかったので、学習方法やレベル感について詳細をまとめておきます

<!--toc-->

## AWS認定 SysOps Administrator 概要
![AWS認定](https://user-images.githubusercontent.com/68212997/116777182-1e07fe00-aaa8-11eb-8088-df57422e17b6.png)


- [AWS 認定 SysOps アドミニストレーター – アソシエイト](https://aws.amazon.com/jp/certification/certified-sysops-admin-associate/)
    - AWSアソシエイト資格三種の一つ
    - AWS でのデプロイ、管理、運用に関しての問題が出題される
    ```
    認定によって検証される能力
    ・スケーラブルで、高可用性および高耐障害性を備えたシステムを AWS でデプロイ、管理、運用する
    ・AWS との間のデータフローを実装および制御する
    ・コンピューティング、データ、セキュリティ要件に基づく適切な AWS のサービスを選択する
    ・AWS 運用のベストプラクティスの適切な使用方法を識別する
    ・AWS の使用コストを予測し、運用コストコントロールメカニズムを識別する
    オンプレミスワークロードを AWS に移行する
    ```

- `SAA(Solution Architect Associate)の次`にとる資格としておすすめ

- SAAと比較してかなり実用的
    - SAAは"AWSが得意"ではなく、"最低限の知識があります"程度のレベル感だが、SysOpsを持っていれば、組織内のAWSアカウントの管理や運用中のサービスの運用監視周りの設計をリードすることができる

- 上位資格に挑戦する前のステップになる
    - AWS DevOps Professional合取得にはDeveloperの内容よりも、SysOpsの細かい内容の方が障壁になる
    - SAP(Solution Architect Professional)はSysOpsほど細かくでないが、運用周りの細かい知識を先に固めておくと手堅く点数を稼げる
    - DeveloperとSysOpsのどちらを先に取るべきかは実務経験にもよるが、SysOpsの方が比較的簡単

### 難易度
- SAA取得済みでProfessionalレベルに挑戦する前に受験すると丁度良い

- SAA(AWS Solution Architect)と比較して遥かに難しく、`出題内容が細かい`
    - SAAは[黒本](https://amzn.to/2PInGNo)と[問題集サイト](https://aws.koiwaclub.com/)を1, 2ヶ月集中して取り組んだらAWSを触ることなく余裕を持って合格できたが、SysOpsは厳しかった

- AWSのサービス全般についての一般的な知識があっても、`２択までしか絞れない`ことが多い
    - 実際に手を動かした経験のあるサービスのみ確実に答えることができるレベル
    - 問題集サイトの問題を覚えるのではなく、`説明まで読み込んで深く理解しなければ歯が立たない`

- SysOpsは`完全に机上の学習のみではハードになる`
    - 少なくともCloudFormationと運用監視関係のサービスは、実務で利用する機会が無くとも、自分で触ってみた方が良い
    - 利用したことの無いサービス(KMS, Systems Manager, RDS)も沢山あったが合格できた（経験があるほど難易度が下がる程度）

---

## 合格までの道のり

### 0. 先にSAA(AWS Solution Architect)を取得
- 新卒配属三ヶ月程度で、AWSを一度も触ることなく受験
    - SAA取得後に実務でAWSを触り始めた

- 運用を専門的に極めたいという特殊な希望がなければ、`SysOpsの前に必ずSAAを取得すべき`

### 1. AWS公式の研修を受講
![Systems Operations on AWS](https://user-images.githubusercontent.com/68212997/116775773-bf3e8680-aa9f-11eb-8405-e64a6e1c86fe.png)

- [Systems Operations on AWS](https://www.aws.training/SessionSearch?pageNumber=1&courseId=10020)
    - ３日間の講義
    - AWSのSolution Architectが講師を務めてくれ、質問も沢山できる

- 会社経由で無料だったため受講
    - 会社で受けられない場合は、最低限の実務経験があれば受けなくてもOK
        - 個人では20万以上かかるので注意
    - CloudFormationも運用監視関連のサービスも利用したことない場合は、先にう毛ておくと理解し易い

- `実務経験が豊富な人以外は、これだけでは間違いなく受からない`

### 2. 書籍
<a href="https://www.amazon.co.jp/AWS%E8%AA%8D%E5%AE%9A%E3%82%A2%E3%82%BD%E3%82%B7%E3%82%A8%E3%82%A4%E3%83%883%E8%B3%87%E6%A0%BC%E5%AF%BE%E7%AD%96-%E3%82%BD%E3%83%AA%E3%83%A5%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%82%A2%E3%83%BC%E3%82%AD%E3%83%86%E3%82%AF%E3%83%88%E3%80%81%E3%83%87%E3%83%99%E3%83%AD%E3%83%83%E3%83%91%E3%83%BC%E3%80%81SysOps%E3%82%A2%E3%83%89%E3%83%9F%E3%83%8B%E3%82%B9%E3%83%88%E3%83%AC%E3%83%BC%E3%82%BF%E3%83%BC-%E5%B9%B3%E5%B1%B1-%E6%AF%85/dp/4865941991?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&dchild=1&keywords=AWS+3%E8%B3%87%E6%A0%BC&qid=1619856689&sr=8-1&linkCode=li3&tag=junjun1080c-22&linkId=bc75c0de42bbeb4b8e1fdf593590d143&language=ja_JP&ref_=as_li_ss_il" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=4865941991&Format=_SL250_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=junjun1080c-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=junjun1080c-22&language=ja_JP&l=li3&o=9&a=4865941991" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

- [AWS認定アソシエイト3資格対策 ソリューションアーキテクト、デベロッパー、SysOpsアドミニストレーター ](https://amzn.to/3aTooPe)を購入
    - SysOpsの試験対策になる日本語の書籍は上記のみ(2021時点)
    - SysOps関連の項目のみを読んだ
    - SAAの黒本と比較して、説明が細かく為になったのでおすすめ
    - これを読めば、公式の研修を受講しなくても良さそう

### 3. 実務経験
試験合格には以下の経験が大きく活きた

- CloudFormationテンプレートの開発
    - `CFnを触った経験があると合格に大きく近づく`
    - 業務内でAWSを使っていて、IaCを実践できていなければ、指示されていなくともCFnテンプレート化してみると、仕事での評価も知識も獲得できるのでおすすめ
        - 0からテンプレートを作らなくとも、Former2という実機環境からテンプレートを自動生成できるツールを併用して仕上げてみる（変数化など）と力がつく（手順は以下の記事を参照）
            - [[AWS/IaC] Former2によるCloudFormation/Terraformテンプレートの自動出力方法](/AWS-IaC-Former2によるCloudFormation-Terraformテンプレートの自動出力方法/)
    - 業務内で使う機会が無ければ、個人でWEBに落ちているテンプレートを改造して試してみると良い経験になる

- 実際のサービスの運用
    - CloudFront, S3, WAF, その他運用監視関係
    - `運用監視周りのサービスは実際に手を動かしてみなければ暗記だけでは厳しい`
        - 自分のアカウントを作って簡単に通知設定などを設定してみると良い

### 4. 問題集サイト
![AWS WEB問題集サイト](https://user-images.githubusercontent.com/68212997/116777054-13993480-aaa7-11eb-935a-296fa0800e24.png)


- 有名な[AWS 問題集サイト](https://aws.koiwaclub.com/)を２週(SAAもここで学習)
    - 試験対策の勉強はこれがメイン
    - Udemyなどで学習した人もいるようだが、英語らしい。Linked in LearningのSysOpsの研修は情報が古かった
    - 1ヶ月ちょっと学習。余裕を持つなら二ヶ月必要
        - #88まであり、平日は一日５つ程度(各7問)のペースで取り組んだが、結構大変
    - 他の人の体験記を見ていても`実務経験が浅い人は１周では危険`
    - サイトの問題は定期的に更新されているが、毎年の公式の更新直後は問題がかなり異なっていた
        - 筆者は試験問題更新直後に一度受験し、問題集サイトと全く範囲が違い落ちてしまった...
        - 2度目の受験時はかなりの精度で出題範囲が似通っていた
            - `各問題の説明まで全て読み込むことで合格レベルに達した`
            - SAAほど問題が似ていないので、深い理解が必要

### 5. 模擬試験
- `模擬試験で高得点を取れていても本番の試験の方が難しいので注意`

#### 模擬試験概要
WEBから公式の模擬試験を受験することができる。`時期によっては問題集サイトとAWS認定試験の範囲が異なることもあるため、必ず受験して感触を掴んだ方が良い`

- SOA-P01: AWS 認定 SysOps アドミニストレーター – アソシエイト模擬
    - 費用: 2000円(税込み 2200)
    - 時間: 35分
        - 試験開始前に「受験者行動規範」を読んで確認するため時間が 5 分含まれている
    - 難易度
        - 問題集サイト #88まで一周解説の読み込みまでやっていればほぼ全て解ける
---

#### 申し込み手順
- AWS認定の[認定サイト](https://www.aws.training/certification?banner=exam-prep)から以下を申し込む
    - SOA-P01	AWS Certified SysOps Administrator - Associate Practice

- 試験の申し込みタブ
    - ピアソンVUE(or PSI)で模擬試験をスケジュールする
    - 画面下部の受験資格がある試験欄には模擬試験がないので注意 

- 受けたい試験を選択
    - SOA-P01: AWS 認定 SysOps アドミニストレーター – アソシエイト模擬
- 試験言語を選択
- 試験の詳細が表示され、問題なければ次へ
- 支払情報と請求情報を入力
    - バウチャーコードがあれば入力
        - 本試験のバウチャーは会社から支給されていたが、模擬試験は自腹で払う必要があった...
    - バウチャがなければクレジット情報を入力
    - 住所は英語表記でなければ弾かれるので注意
- 申込事項に問題がなければ予約内容を確定
    - 予約されましたと表示される
- この画面の試験開始ボタンを押すと模擬試験が始まる
    - はじめに説明があるので、身構えなくてOK

- 試験終了後にE メールでスコアのリンクが送られてくる
    - 5分程度待つことになる
    - 認定サイトのピアソンVUE試験の管理からみれる
    - スコアレポートの表示

#### 問題集サイト#88まで一周した後のスコア
- 合計スコア: 90% = (18/20)
    - ミスっていたのは以下のトピックのみなので復習をした
        - Monitoring and Reporting
            - モニタリングレポート 試験比重 22%
        - Security and Compliance
            - セキュリティとコンプライアンス 試験比重18%
    
```
トピックレベルのスコア:
1.0. Monitoring and Reporting     80%
2.0. High Availability            100%
3.0. Deployment and Provisioning  100%
4.0. Storage and Data Management  100%
5.0. Security and Compliance      75%
6.0. Networking                   100%
7.0. Automation and Optimization  100%
```

- 一度目の受験では`模擬試験スコア 85%で余裕だと思ったが、本番で不合格となった`
    - 合格最低点720,スコア700であった
    - 本番の方が圧倒的に難しいので注意


### 6. 試験
- オンラインか、全国の試験センターで受験可能
    - コロナの影響で、2020からオンライン受験が可能になった
    - オンラインの方が先に埋まってしまうので早めに予約した方が良い

- 一度目の受験
    - AWS利用経験ほぼなし、問題集サイトに取組み（説明は読み込まず）、模擬試験を受けたら85%だったので甘くみて即受験
    - 合格点720に対して20点足らず不合格
        - 試験問題更新直後で学習範囲がずれており、理解度も足らないため２択までしか絞れない問題が多かった

- 二度目の受験
    - この間に運用監視サービスやCloudFormationを実務で経験
    - [書籍](https://amzn.to/3vzBQje)の該当範囲を読み込み、問題集サイトを二週実施（説明まで読み込んだ）、間違えた問題は全てノートにまとめて復習も実施
        - 間違えた問題はAWSの公式ページを読んで理解するよう努めた
    - 本番の試験はかなり余裕を持って取り組めた
    - 900/1000点で合格。合格点は720なのでかなり余裕があった
    
- 合格すると認定サイトでスコアレポートとPDFの証明書を確認でき、デジタルバッジも発行可能になる
    - PDFに書かれているValidation Numberは、本人確認のためによく使う機会があるので、控えておくと良い


## 関連記事
- [Angular x AWS SDK for JavaScriptの始め方](/Angular-x-AWS-SDK-for-JavaScriptの始め方/)

- [静的ホスティングサービスまとめ(Netlify/S3/Firebase Hosting/Github Pages)](/静的ホスティングサービスまとめ-Netlify-S3-Firebase-Hosting-Github-Pages/)
- [[Angular ReactiveForm x mat-input] 一つの入力欄のエラーメッセージを出し分ける](/Angular-ReactiveForm-x-mat-input-一つの入力欄のエラーメッセージを出し分ける/)
- [[Angular mat-input] バリデーションまとめ](/Angular-mat-input-バリデーションまとめ/)
- [[Angular] 複数のcheckboxで一つ以上のチェックを必須とするバリデーション](/Angular-複数のcheckboxで一つ以上のチェックを必須とするバリデーション/)
- [[Angular Material入門] mat-inputで生成したフォームから入力値を取得(双方向データバインディング)](/Angular入門-mat-inputで生成したフォームから入力値を取得-双方向データバインディング/)
- [[Angular] mat-selection-list & ngForでcheckboxをリスト表示～選択値を配列として取得](/Angular-mat-selection-listでcheckboxを表示～選択値を配列として取得/)

- [[Angular JavaScript] JSONデータのファイル化と出力 取得したデータを任意の名称で保存するロジック (TypeScript)](/Angular-JSONデータのファイル化と出力-クラウドから取得したデータを任意の名称で保存する/)
- [[Angular JavaScript] JSONファイル(複数)の読み込み](/Angular-JavaScript-JSONファイルの読み込み/)

- [[Angular Schematics] 開閉可能なサイドナビ＆ツールバーを3分で自動生成する](/Angular-Schematics-開閉可能なサイドナビ＆ツールバーを3分で自動生成する/)
- [[Angular] map() & filter() & mat-checkboxを使って選択値を配列に格納するロジック](/Angular-map-fileter-mat-checkboxを使って選択値を配列に格納するロジック/)

